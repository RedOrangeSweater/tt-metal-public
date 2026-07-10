# tt-metal-public

Staging for mechanical transfer of the verified `ttnn-shutov` packaging tip into `shutovilyaep/tt-metal`.

## Branches

| Branch | Role |
| --- | --- |
| `transfer-control` | this README + manifest |
| `release/10-ttnn-shutov` | exact ROS Metal tip (pin + packaging) |
| `release/10-ttnn-shutov.dev` | same tip + `PR_BODY.md` only |

Do **not** replay failed intermediate ROS PRs #6–#9 as separate vitrine PRs.

## Exact tip

- ROS `main`: `96e4b712338ef7fc3ad7b9d1ac551dc8e0eb3938`
- Pin: `8dfb324099a1bf6b8839cffd5740e22a4d621385`
- Package: `ttnn-shutov==0.65.0.dev20251204`
