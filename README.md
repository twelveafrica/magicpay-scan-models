# magicpay-scan-models

Model weights for [magicpay_scan](https://github.com/twelveafrica/magicpay-scan) —
the SDK that reads M-PESA payment details from a camera frame.

**No weights live in the repository tree.** They ship as release assets: git is
not built for binaries, and 20 MB per release would be cloned by everyone who
came to read a README. The tree holds text only — this file, the licences and
the release notes.

## How the SDK fetches them

Two levels, and the split is the whole point.

| | address | holds | mutable |
|---|---|---|---|
| pointer | `releases/download/manifest-1/manifest.json` | release version, sha256 and URLs | yes, on every release |
| release | `releases/download/models-<version>/<asset>` | the weights themselves | never |

The SDK reads the pointer, checks the sha256 of every file and downloads from
the URLs it lists. Two rules hold this up:

1. **Release assets are never replaced.** New weights mean a new tag. Replacing
   an asset at an existing address breaks exactly the sha256 check the manifest
   exists for.
2. **The pointer is the only mutable place.** It is small, its contents are
   verified by hashes, and rolling a release back means re-uploading the
   previous pointer.

File integrity comes from sha256 in the pointer; the integrity of the pointer
itself comes from TLS to github.com. The manifest is not signed — that is a
stated limitation, not an oversight.

## Publishing a release

From the SDK repository, one command:

```sh
MP_MODELS_REPO=twelveafrica/magicpay-scan-models ./tools/publish_models.sh 1.0.1
```

It builds the manifest with hashes, uploads the weights under a new tag and
overwrites the pointer. Rolling back is re-uploading a previous
`manifest.json` to the `manifest-1` release: apps pick the older release up on
their next check, and its assets are still there.

## What a release contains

| asset | path inside the SDK | what it is |
|---|---|---|
| `ocr-det.onnx` | `ocr/det.onnx` | text line detector, PP-OCR, fine-tuned on Kenyan receipts |
| `ocr-rec.onnx` | `ocr/rec.onnx` | line recogniser, same fine-tuning |
| `ocr-cls.onnx` | `ocr/cls.onnx` | line angle classifier, 0/90/180/270 |
| `ocr-dict.txt` | `ocr/dict.txt` | character dictionary of the recogniser |
| `contour-u2netp.onnx` | `contour/u2netp.onnx` | subject mask for the outline |
| `label-clip_image_encoder.onnx` | `label/clip_image_encoder.onnx` | subject labelling |
| `label-clip_manifest.json` | `label/clip_manifest.json` | label vocabulary |
| `label-vocab.f32` | `label/vocab.f32` | vectors for that vocabulary |
| `face-face_reid_0095.tflite` | `face/face_reid_0095.tflite` | face embedder |

The reading core (`ocr/*`) is also bundled inside the SDK package and works
with no network at all. It is published here as well, so a fixed recogniser can
reach installed apps without a new app build.

## Licences

See [NOTICE.md](NOTICE.md).
