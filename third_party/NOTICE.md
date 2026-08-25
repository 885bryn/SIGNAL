# Third-Party Notice Policy

SIGNAL intends to reuse upstream software only with explicit provenance and license tracking.

Before copying, vendoring, forking, or adapting upstream code:

1. Verify the exact upstream repository and selected commit.
2. Record the exact license and required notices in `third_party/UPSTREAMS.md`.
3. Preserve required copyright/license notices.
4. Store applicable license texts under `third_party/LICENSES/`.
5. Record imported files/components and local modifications.
6. Keep GPL-derived code isolated from components that are intended to remain separately licensed unless a deliberate licensing decision is approved.
7. Do not depend on an unpinned moving `main` branch for production/reproducible builds.

This file is process documentation, not a substitute for the actual upstream license texts.
