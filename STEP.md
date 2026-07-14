# Шаг 70 — clean production PyPI smoke (без Tenstorrent HW)

## 1. Цель

Чистый venv → `pip install` с pypi.org → import (максимум без HW).

## 2. Где выполнять

Терминал **вне** checkout. Предпочтительно Docker с OpenMPI 5 ULFM.

## 3. Команды

```bash
set -euo pipefail
cd /tmp
rm -rf ttnn-pypi-l0
python3.10 -m venv ttnn-pypi-l0
source ttnn-pypi-l0/bin/activate
python -m pip install -U pip

export LD_LIBRARY_PATH=/path/to/openmpi-v5.0.7-ulfm/lib${LD_LIBRARY_PATH:+:$LD_LIBRARY_PATH}

python -m pip install --no-cache-dir \
  --index-url https://pypi.org/simple/ \
  'ttnn-shutov==0.65.0.dev20251204'

python - <<'PY'
import ttnn
assert "site-packages" in ttnn.__file__
assert "tt-metal" not in ttnn.__file__
print("TTNN_PYPI_OK", ttnn.__file__)
PY
python -m pip show ttnn-shutov | tee /tmp/ttnn-pypi-show.txt
python -m pip check
```

## 4. Ожидаемый результат

Маркер `TTNN_PYPI_OK`; Version `0.65.0.dev20251204`; `pip check` чистый.

## 5. STOP

Host OpenMPI 4 без ULFM; импорт из checkout; ошибка загрузки `.so`.

## 6. Записать в RELEASE_LOG.md

`TTNN_PYPI_OK`, prod SHA256, пометка: **L2 HW eager forward не выполнялся**.

Дальше — Torch по `CHECKLIST_RU.md`.
