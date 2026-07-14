# Шаг 20 — guarded force-with-lease tip → shutovilyaep/tt-metal:main

## 1. Цель

Положить на `main` ровно tip `96e4b712338ef7fc3ad7b9d1ac551dc8e0eb3938` (полный tree из `release/10-ttnn-shutov`).

## 2. Где выполнять

Терминал на машине с push-доступом.

## 3. Команды

```bash
set -euo pipefail
cd ~/shutov-release-2026-07/tt-metal

EXPECTED_TIP=96e4b712338ef7fc3ad7b9d1ac551dc8e0eb3938
EXPECTED_OLD=$(cat ../metal-old-main.sha)

git remote remove ros 2>/dev/null || true
git remote add ros git@github.com:RedOrangeSweater/ML.TT.Metal.git
git fetch ros main
git fetch origin

test "$(git rev-parse ros/main)" = "$EXPECTED_TIP"
test "$(git rev-parse origin/main)" = "$EXPECTED_OLD"

# ТОЛЬКО force-with-lease. Не --force. Не reset --hard.
git push --force-with-lease=refs/heads/main:${EXPECTED_OLD} \
  ros "${EXPECTED_TIP}:refs/heads/main"

git fetch origin
test "$(git rev-parse origin/main)" = "$EXPECTED_TIP"
test "$(git rev-parse ${EXPECTED_TIP}^{tree})" = "$(git rev-parse origin/main^{tree})"
echo "METAL_TIP_OK $(git rev-parse origin/main)"
```

Альтернативный source (тот же SHA):

```bash
git fetch git@github.com:RedOrangeSweater/tt-metal-public.git release/10-ttnn-shutov
test "$(git rev-parse FETCH_HEAD)" = "$EXPECTED_TIP"
```

После успешного push — вернись к шагу 10 и **enable** только `release-ttnn-shutov-from-source.yaml`.

## 4. Ожидаемый результат

- `origin/main` = `96e4b712338ef7fc3ad7b9d1ac551dc8e0eb3938`
- tree SHA совпадает с source tip
- есть `.github/workflows/release-ttnn-shutov-from-source.yaml`

## 5. STOP

- `ros/main` ≠ expected tip
- `origin/main` ≠ archived old SHA → обнови lease SHA осознанно
- push rejected → не переключайся на `--force`

## 6. Записать в RELEASE_LOG.md

- old/new/tree SHA, URL commit на shutovilyaep
