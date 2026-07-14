# Шаг 30 — GitHub Environment `pypi` на shutovilyaep/tt-metal

## 1. Цель

Prod publish job использует `environment: pypi` + OIDC. Имя environment должно быть **ровно** `pypi`.

## 2. Где выполнять

https://github.com/shutovilyaep/tt-metal/settings/environments

## 3. Нажатия

1. Settings → Environments → **New environment**
2. Name: `pypi`
3. Deployment branches: ограничить `main`, если UI позволяет
4. Secrets для prod не нужны (Trusted Publishing / OIDC)
5. Save

```bash
gh api repos/shutovilyaep/tt-metal/environments --jq '.environments[].name'
# ожидается: pypi
```

## 4. Ожидаемый результат

Environment `pypi` существует.

## 5. STOP

- Имя `PyPI` / `prod` / другое
- Нет прав создать environment

## 6. Записать в RELEASE_LOG.md

URL environments page, имя `pypi`, дата
