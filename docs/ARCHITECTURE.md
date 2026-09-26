# Architecture

## Data model

Image → Object → Instance → Context → Visibility → Condition → Policy → Edit → Diff → Evidence

An instance represents a physical component, not merely a connected pixel region. A partially hidden component keeps one instance identity with visible and occluded regions.

## Occlusion rule

An occluded region is evidence of non-visibility, not evidence of the hidden geometry. No pixels are fabricated during segmentation.

## Reconstruction rule

Inpainting/reconstruction may be performed only as a derived artifact. It receives its own mask and provenance and can never overwrite the original image or OBSERVED mask.

## Editing rule

authorized_change = proposed_change ∩ MUTABLE

Any changed pixel outside MUTABLE, or any changed pixel intersecting IMMUTABLE, is a policy violation.

## Evidence object

Minimum fields:

- source image hash
- segmentation/mask hash
- policy hash
- configuration hash
- execution timestamp
- operation identifier
- changed pixel count
- protected pixel changes
- decision: PASS / FAIL / BLOCKED
- provenance for derived/reconstructed content
