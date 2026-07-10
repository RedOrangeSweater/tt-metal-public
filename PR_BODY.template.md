**PR title (copy into the title field):** release: build ttnn-shutov from Merge pin 8dfb324 (copy-only tracy)

## Summary

Single packaging PR for the verified `ttnn-shutov` channel on pin `8dfb324099a`.
Builds a manylinux cp310 wheel via cibuildwheel, copy-only-bundles `libtracy` (no auditwheel, no patchelf), gates on ELF invariant + manylinux smoke, retags to `manylinux_2_34_x86_64`, and optionally publishes.

## Why not auditwheel / patchelf

On this pin both corrupt ELF (`.init`/`.plt` leave executable LOAD) and `import ttnn` SIGSEGV.
Proven on ROS artifacts: raw missing tracy → ImportError; patchelf wheel → exit 139; copy-only → green.

## Forensic timeline (ROS)

| PR / run | Result |
| --- | --- |
| #6–#8 | scaffolding (no auditwheel, cp310 smoke, upload) |
| #9 / run 29046417280 | patchelf bundler → SIGSEGV |
| #10 / #11 | copy-only tracy + UNREPAIRED persist |
| run 29074577044 | `ELF_INVARIANT_OK` + `MANYLINUX_SMOKE_OK` |

## Stack pointers

- **Base:** `8dfb324099a1bf6b8839cffd5740e22a4d621385`
- **Tip:** `96e4b712338ef7fc3ad7b9d1ac551dc8e0eb3938`

## Operator buttons

1. `publish_target=none` → download artifacts → local smoke
2. `publish_target=testpypi` or `pypi` after verification
