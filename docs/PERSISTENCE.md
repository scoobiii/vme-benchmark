# Persistence model

VME persistence is append-only for evidence and immutable for source images.

## Canonical artifact tree

```
datasets/
  <dataset_id>/
    images/
      <image_id>.original.<ext>
    masks/
      <image_id>/
        object.<ext>
        instance.<ext>
        context.<ext>
        visibility.<ext>
        condition.<ext>
        mutable.<ext>
        immutable.<ext>
        reconstructed.<ext>
    metadata/
      <image_id>.json
    evidence/
      <run_id>.json
    diffs/
      <run_id>.diff.<ext>
```

## Persistence invariants

1. The original image is write-once and content-addressed by SHA-256.
2. Every mask is a separate artifact; masks are never baked destructively into the source image.
3. Instance IDs are stable within a dataset/version.
4. OCCLUDED and UNKNOWN are persisted explicitly; absence of pixels does not mean zero.
5. RECONSTRUCTED pixels have independent provenance and never replace OBSERVED pixels.
6. MUTABLE and IMMUTABLE policies are persisted as explicit masks plus a policy/config hash.
7. Evidence records are append-only and reference exact artifact hashes.
8. An edit produces a new artifact/version; the original is never overwritten.
9. A failed or BLOCKED operation is persisted as evidence too.
10. Any artifact referenced by evidence must be retrievable by its content hash.

## Minimum metadata

```json
{
  "dataset_id": "string",
  "image_id": "string",
  "source_sha256": "sha256",
  "schema_version": "v1",
  "instances": [
    {
      "instance_id": "string",
      "class": "string",
      "provenance": "OBSERVED",
      "visibility": "VISIBLE|OCCLUDED|UNKNOWN"
    }
  ],
  "mask_refs": {},
  "policy_ref": "sha256",
  "created_at": "RFC3339"
}
```

## Versioning

Use immutable dataset/version identifiers. Corrections create a new annotation version instead of silently replacing an older annotation. Evidence always points to the exact versions used by an execution.

## Storage boundary

Git stores schemas, code, manifests and small deterministic fixtures. Large photographs, masks and generated artifacts should use object storage or Git LFS; the manifest stores their hashes and locations.

## Integrity check

A persistence validator must verify:

- referenced artifacts exist;
- SHA-256 matches;
- required mask layers exist;
- source image hash is unchanged;
- evidence references the exact mask/policy/config versions;
- no reconstructed artifact is classified as OBSERVED;
- no edit changed IMMUTABLE pixels.
