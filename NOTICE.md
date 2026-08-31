# Licences and provenance

One row per file, because each one gets asked about separately.

| file | base model | licence of the base | modified by us |
|---|---|---|---|
| `ocr-det.onnx` | PP-OCR (PaddleOCR) | Apache-2.0 | YES: fine-tuned on our own set of receipt and sticker photos |
| `ocr-rec.onnx` | PP-OCR (PaddleOCR) | Apache-2.0 | YES: fine-tuned, dictionary reduced to ASCII |
| `ocr-cls.onnx` | PP-OCR (PaddleOCR) | Apache-2.0 | no, ONNX conversion only |
| `ocr-dict.txt` | ours | — | ours |
| `contour-u2netp.onnx` | U²-Netp | Apache-2.0 | no, ONNX conversion only |
| `label-clip_image_encoder.onnx` | OpenVision ViT-tiny | TO BE CONFIRMED before publishing | no, ONNX conversion only |
| `label-clip_manifest.json`, `label-vocab.f32` | ours, computed with that encoder | — | ours |
| `face-face_reid_0095.tflite` | Intel face-reidentification-retail-0095 | Apache-2.0 | no, TFLite conversion only |

Apache-2.0 requires keeping the copyright notice and stating modifications; for
the fine-tuned files the modification is stated in the last column.

## What ships together with the weights

The fine-tuned `ocr-det` and `ocr-rec` are derived from our own dataset. The
dataset is not published, but weights trained on it are published here. That is
a deliberate decision and should be taken with eyes open: the original photos
cannot be reconstructed from the weights, but the weights are intellectual
property, and in a public repository they are available to anyone, competitors
included.
