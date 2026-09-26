# Mask specification

Each source image should have independent layers:

- object: semantic class
- instance: physical component identity
- context: scene/relationship label
- visibility: VISIBLE, OCCLUDED, UNKNOWN
- condition: observed condition such as corrosion
- mutable: pixels authorized for an edit
- immutable: protected pixels
- reconstructed: generated pixels, if reconstruction is explicitly requested

## Example

A refrigeration pipe visible before disappearing behind another component remains one instance. The hidden segment is marked OCCLUDED. Its geometry is not guessed. If later reconstructed, that output receives RECONSTRUCTED provenance and a separate mask.

## Pixel isolation benchmark

For each class/instance, generate an isolated view by masking all non-target pixels while preserving the original target pixels. Isolation must not modify the source evidence.
