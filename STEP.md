# Шаг 60 — production publish ttnn-shutov на PyPI

## 1. Цель

Загрузить `ttnn-shutov==0.65.0.dev20251204` на pypi.org (рядом со старой `0.62.0.dev20250916`).

## 2. Где выполнять

Только после green шага 50 и настроенных шагов 30–40.

## 3. Команды

```bash
curl -sL https://pypi.org/pypi/ttnn-shutov/json | python3 -c '
import json,sys
d=json.load(sys.stdin)
print(sorted(d["releases"]))
assert "0.65.0.dev20251204" not in d["releases"], "already published — STOP"
print("PRECHECK_OK")
'

REPO=shutovilyaep/tt-metal
gh workflow run "Release ttnn-shutov (from-source at Merge pin)" \
  --repo "$REPO" --ref main \
  -f publish_target=pypi \
  -f publish_version=0.65.0.dev20251204 \
  -f metal_sha=8dfb324099a1bf6b8839cffd5740e22a4d621385
# gh run watch <RUN_ID> --repo "$REPO" --exit-status
```

Подтвердить индекс и скачать **с PyPI** (не artifact):

```bash
curl -sL https://pypi.org/pypi/ttnn-shutov/json | python3 -c '
import json,sys
d=json.load(sys.stdin)
assert "0.65.0.dev20251204" in d["releases"]
print("PYPI_INDEX_OK")
'
cd ~/shutov-release-2026-07 && mkdir -p metal-pypi-wheel && cd metal-pypi-wheel
python3.10 -m pip download --no-deps --no-cache-dir \
  --index-url https://pypi.org/simple/ -d . 'ttnn-shutov==0.65.0.dev20251204'
sha256sum ttnn_shutov-*.whl | tee PROD_SHA256.txt
```

Production SHA256 не обязан совпадать с TestPyPI.

## 4. Ожидаемый результат

Publish job green; JSON содержит версию; `PROD_SHA256.txt` заполнен.

## 5. STOP

OIDC 403 → шаги 30–40; версия уже была до запуска; publish red при green build.

## 6. Записать в RELEASE_LOG.md

production run URL, PROD_SHA256, https://pypi.org/project/ttnn-shutov/0.65.0.dev20251204/
