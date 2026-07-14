# Шаг 10 — отключить шумные Actions на shutovilyaep/tt-metal

## 1. Цель

После force-with-lease tip принесёт upstream workflows. Нужно, чтобы не стартовали сотни job’ов. Оставить только release workflow для `ttnn-shutov`.

## 2. Где выполнять

Терминал (`gh`) предпочтительно. UI — запасной путь.

## 3. Команды (предпочтительный путь)

```bash
set -euo pipefail
cd ~/shutov-release-2026-07
gh auth status
REPO=shutovilyaep/tt-metal

gh workflow list --repo "$REPO" --all > workflows-before.txt
wc -l workflows-before.txt
cat workflows-before.txt

while read -r name; do
  [ -z "$name" ] && continue
  echo "disable: $name"
  gh workflow disable "$name" --repo "$REPO" || true
done < <(gh workflow list --repo "$REPO" --all --json name --jq '.[].name')

gh workflow list --repo "$REPO" --all > workflows-after-disable.txt
```

После шага `20-force-with-lease` (когда на `main` появится новый YAML) **включи только**:

```bash
gh workflow enable "Release ttnn-shutov (from-source at Merge pin)" --repo shutovilyaep/tt-metal
# или:
gh workflow enable release-ttnn-shutov-from-source.yaml --repo shutovilyaep/tt-metal
gh workflow list --repo shutovilyaep/tt-metal --all | tee workflows-final.txt
```

### UI (резерв)

1. https://github.com/shutovilyaep/tt-metal/actions
2. Для каждого workflow → `...` → Disable workflow
3. После tip-push: Enable только `Release ttnn-shutov (from-source at Merge pin)`

## 4. Ожидаемый результат

- До tip-push: все старые workflows Disabled.
- После tip-push + enable: активен только `release-ttnn-shutov-from-source.yaml`.

## 5. STOP

- Нет прав admin/maintain на Actions → сначала почини права.
- После tip-push оставил включёнными post-commit/nightly → отключи до запуска release.

## 6. Записать в RELEASE_LOG.md

- `workflows-before.txt` / `workflows-final.txt`
- точное имя единственного enabled workflow
