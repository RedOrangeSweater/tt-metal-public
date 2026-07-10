# Transfer control — tt-metal-public

Staging for mechanical tip transfer into `shutovilyaep/tt-metal`.

## Branches

| Branch | SHA | Notes |
| --- | --- | --- |
| `release/10-ttnn-shutov` | `96e4b712338ef7fc3ad7b9d1ac551dc8e0eb3938` | exact ROS Metal tip |
| `release/10-ttnn-shutov.dev` | +1 `PR_BODY.md` only | vitrine PR body |
| `transfer-control` | this file | operator notes |

## Do not

- Replay failed ROS PRs #6–#9 as separate PRs on shutovilyaep
- Upload PEP 440 local versions (`+g…`) to TestPyPI/PyPI

## Proven

- none: https://github.com/RedOrangeSweater/ML.TT.Metal/actions/runs/29074577044
- TestPyPI: https://github.com/RedOrangeSweater/ML.TT.Metal/actions/runs/29080934432
- version: `ttnn-shutov==0.65.0.dev20251204`
- pin: `8dfb324099a1bf6b8839cffd5740e22a4d621385`

## Operator

1. Disable noisy workflows on shutovilyaep/tt-metal
2. `git push --force-with-lease origin 96e4b712338ef7fc3ad7b9d1ac551dc8e0eb3938:main`
3. Actions `publish_target=none` then `pypi`
