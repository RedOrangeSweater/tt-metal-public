**PR title (copy into the title field):** Publish matching ttnn-shutov 0.65.0.dev20251204 from pin 8dfb324099a

## Summary

From-source `ttnn-shutov` at metal pin `8dfb324099a1bf6b8839cffd5740e22a4d621385`, PyPI-legal version `0.65.0.dev20251204` (no PEP 440 `+local` — rejected by TestPyPI/PyPI). Copy-only tracy bundler; OpenMPI 5 ULFM as runtime (`MPIX_Comm_revoke`).

Do **not** replay failed ROS PRs #6–#9 as separate vitrine PRs. Forensic timeline: #6–#9 → fixes #10/#11/#12 → green none `29074577044` → TestPyPI `29080934432`.

## Tip

- Code branch: `release/10-ttnn-shutov`
- Tip SHA: `96e4b712338ef7fc3ad7b9d1ac551dc8e0eb3938`
- Binary pin: `8dfb324099a1bf6b8839cffd5740e22a4d621385`
- Destination baseline before force-with-lease: `63ca6ee0ab221360c161d426f865ffc70d1b6362`

## Verified on RedOrangeSweater

| Stage | Link | Result |
| --- | --- | --- |
| Metal none | https://github.com/RedOrangeSweater/ML.TT.Metal/actions/runs/29074577044 | green |
| Metal TestPyPI | https://github.com/RedOrangeSweater/ML.TT.Metal/actions/runs/29080934432 | green |
| Local TestPyPI | `TESTPYPI_TTNN_OK` | import `ttnn` |
| Torch TestPyPI (pair) | https://github.com/RedOrangeSweater/ML.TT.PyTorchTtnn/actions/runs/29093775367 | green |

## Wheel SHA256 (TestPyPI reference)

```
6328c55d12db443b53356ddfac85972d0bd775dbf13e4c7922fc3272855e9a92  ttnn_shutov-0.65.0.dev20251204-cp310-cp310-manylinux_2_34_x86_64.whl
```

Production SHA256 will differ (rebuild on shutovilyaep); record after prod publish.

## Runtime

Actions-built `ttnn` needs OpenMPI 5 ULFM on `LD_LIBRARY_PATH` (or inside manylinux image `/opt/openmpi-v5.0.7-ulfm/lib`). Host apt OpenMPI 4.x is insufficient.

## Operator steps

Follow numbered branches `ops/00` … `ops/70` in this repo (each `.dev` has `STEP.md`). Index: `transfer-control` / `TRANSFER_CONTROL.md`.
