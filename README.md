# VME Benchmark

**Visual Masking & Evidence Benchmark**

Reproducible pipeline for construction-site image segmentation, instance/context mapping, visibility/occlusion handling, constrained image editing, and pixel-level evidence.

## Core invariant

Pixels classified as IMMUTABLE must not change. Pixels that are OCCLUDED are not treated as observed geometry. RECONSTRUCTED pixels are always stored separately from OBSERVED evidence.

## Pipeline

1. Source image — immutable original evidence.
2. Object detection — classes/components.
3. Instance segmentation — one mask per physical instance.
4. Context map — relationship and scene role.
5. Visibility map — VISIBLE / OCCLUDED / UNKNOWN.
6. Condition map — e.g. corrosion, deterioration.
7. MUTABLE / IMMUTABLE policy masks.
8. Optional reconstruction — explicitly RECONSTRUCTED, never merged into source evidence.
9. Authorized edit.
10. Pixel diff and evidence package.

## Evidence

Every run records input/mask/config hashes, policy decision, changed-pixel counts, protected-region changes, and PASS/FAIL/BLOCKED status.

## Status vocabulary

- OBSERVED
- DERIVED
- INFERRED
- RECONSTRUCTED
- UNKNOWN
- MUTABLE
- IMMUTABLE
- BLOCKED

## Initial component taxonomy

Condensadoras, estrutura metálica, tela, dutos, tubulações frigoríficas, infraestrutura elétrica, portão, base/piso, fundo/contexto e condições associadas como corrosão.
