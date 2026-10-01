# openjev-models

Core ML conversions of the models that [OpenJevSwift](https://github.com/Algorythm-Canada/OpenJevSwift)
serves on iOS and macOS. Nothing here is trained by this organisation: each package is a
conversion of another author's checkpoint, published under that checkpoint's license.

Each release holds one package version. Its assets are the files of the `.mlpackage`, uploaded
under their paths with `/` replaced by `--`, so a device downloads them one by one with no
archive to unpack. OpenJevSwift's `OpenJevEncoders` library embeds the SHA-256 of every file and
refuses a file that does not match. How the packages are converted, checked and published is
documented in OpenJevSwift under `Tools/encoders/` and decision D-033.

## Packages

| Release | Model | Converted from | License |
|---|---|---|---|
| `verdict-m18-fp16-v1` | `verdict-1.4`, Verdict by Heman10x | [heman10x/rlcd-modernbert-151m](https://huggingface.co/heman10x/rlcd-modernbert-151m) at `8af2496` | Apache-2.0 |

## Credits

- **Verdict** is by Heman10x ([Verdict-open-jev](https://github.com/Heman10x-NGU/Verdict-open-jev)):
  a ModernBERT-base encoder with a GLiClass head, calibrated with RLCD. The package here is a
  float16 Core ML conversion of the author's checkpoint, with one function per input shape for
  iOS 18 and macOS 15. The author's tokenizer and calibration files are not re-hosted; OpenJevSwift
  reads them from the checkpoint on Hugging Face at the pinned revision.

The packages are provided as is, under the Apache License 2.0 of the checkpoints they come from.
