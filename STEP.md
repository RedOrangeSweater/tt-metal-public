# Шаг 50 — Actions artifact-only (`publish_target=none`) + local smoke

## 1. Цель

Собрать `ttnn-shutov==0.65.0.dev20251204` на shutovilyaep без upload, скачать artifact, проверить L0/L1 без HW.

## 2. Где выполнять

GitHub Actions / `gh`; smoke — терминал + Docker с OpenMPI 5 ULFM.

## 3. Команды

```bash
set -euo pipefail
REPO=shutovilyaep/tt-metal
gh workflow run "Release ttnn-shutov (from-source at Merge pin)" \
  --repo "$REPO" --ref main \
  -f publish_target=none \
  -f publish_version=0.65.0.dev20251204 \
  -f metal_sha=8dfb324099a1bf6b8839cffd5740e22a4d621385
sleep 5
gh run list --repo "$REPO" --workflow=release-ttnn-shutov-from-source.yaml --limit 3
# gh run watch <RUN_ID> --repo "$REPO" --exit-status
```

После green:

```bash
cd ~/shutov-release-2026-07
RUN_ID=<вставь>
mkdir -p metal-none-$RUN_ID && cd metal-none-$RUN_ID
gh run download "$RUN_ID" --repo shutovilyaep/tt-metal
sha256sum $(find . -name 'ttnn_shutov-*.whl') | tee sha256.txt
python3 -m pip install -U twine
python3 -m twine check $(find . -name 'ttnn_shutov-*.whl')
```

Smoke: host apt OpenMPI 4 недостаточен. Используй image с `/opt/openmpi-v5.0.7-ulfm`:

```bash
WHEEL=$(find "$PWD" -name 'ttnn_shutov-0.65.0.dev20251204-*.whl' | head -1)
docker run --rm -v "$PWD":/w -w /w \
  -e LD_LIBRARY_PATH=/opt/openmpi-v5.0.7-ulfm/lib \
  <MANYLINUX_OR_TT_IMAGE> \
  bash -lc '
    set -euo pipefail
    python3.10 -m venv /tmp/v && . /tmp/v/bin/activate
    pip install -U pip && pip install "$1"
    python -c "import ttnn; assert \"site-packages\" in ttnn.__file__; print(\"TTNN_ARTIFACT_SMOKE_OK\", ttnn.__file__)"
  ' bash "$WHEEL"
```

## 4. Ожидаемый результат

Run green; `twine check` OK; маркер `TTNN_ARTIFACT_SMOKE_OK`; нет SIGSEGV / missing tracy / missing `MPIX_Comm_revoke`.

## 5. STOP

Run red; версия с `+g`/`+local`; импорт из git checkout.

## 6. Записать в RELEASE_LOG.md

run URL, artifact IDs, SHA256 artifact wheel, маркер smoke
