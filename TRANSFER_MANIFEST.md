# tt-metal-public transfer manifest

## Destination

- Repo: `shutovilyaep/tt-metal` (after ROS verification)
- Staging: `RedOrangeSweater/tt-metal-public` (create after Metal TestPyPI green)

## Exact tip to transfer

| Field | Value |
| --- | --- |
| Pin base | `8dfb324099a1bf6b8839cffd5740e22a4d621385` |
| ROS Metal tip | `96e4b712338ef7fc3ad7b9d1ac551dc8e0eb3938` (merge PR #11) |
| Package | `ttnn-shutov==0.65.0.dev20251204+g8dfb324099` |
| Proof none run | https://github.com/RedOrangeSweater/ML.TT.Metal/actions/runs/29074577044 |
| Artifacts | raw `8221303673`, repaired `8221304544`, shutov `8221315574` |

## Wheel SHA256 (run 29074577044)

```
7aa84db2c3d30da63566beead1ee01f9b5e0e8f08991d7e346cd3a01d506d370  raw
51aaf608f5603de778d017d85dca42021819b9e2d906fe3e033eff19915a7e6a  repaired
da515f5bcb6d47e5f527b0c7a69ed8a50c6abe953f7cffe0a871f0e75903704b  ttnn-shutov manylinux_2_34
```

## Vitrine PR shape (do NOT replay failed #6–#9 as separate PRs)

One code branch `release/10-ttnn-shutov` = pin + final packaging tip.
One `.dev` branch adds only `PR_BODY.md`.

Forensic timeline (reference only in PR body):
- #6 no-auditwheel, #7 smoke cp310, #8 upload wheelhouse
- #9 patchelf bundler → SIGSEGV (run 29046417280)
- #10 copy-only tracy, #11 UNREPAIRED persist → green none (29074577044)

## Guarded mirror commands

```bash
git fetch ros main
EXPECTED=96e4b712338ef7fc3ad7b9d1ac551dc8e0eb3938
test "$(git rev-parse ros/main)" = "$EXPECTED"
# disable noisy workflows in shutovilyaep first
git push --force-with-lease origin "$EXPECTED:main"
test "$(git rev-parse origin/main)" = "$EXPECTED"
```
