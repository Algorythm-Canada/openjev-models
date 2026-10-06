<a name="openjev-models"></a>

# OpenJev Models: Core ML packages for iOS and macOS

Core ML model packages used by [OpenJevSwift](https://github.com/Algorythm-Canada/OpenJevSwift)
for typed AI decisions on iOS and macOS (iPhone and Mac). Nothing here is trained by this organisation: each package is a
conversion of another author's checkpoint, published under that checkpoint's license. The
weights are the authors' own, stored as float16 in Apple's Core ML format.

You do not need to download anything by hand. OpenJevSwift's `OpenJevEncoders` library, and the
`openjev` server built on it, download the package a backend needs on first use, check every
file's SHA-256 against the digests compiled into the library, and keep the package in
`~/Library/Application Support/OpenJevSwift/encoders`. To run without downloads, set
`OPENJEV_ENCODER_MODELS` to a folder of converted packages, as OpenJevSwift's
`docs/deployment.md` describes.

Each release holds one package version. Its assets are the files of the `.mlpackage`, uploaded
under their paths with `/` replaced by `--`, so a device downloads them one by one with no
archive to unpack. How the packages are converted, checked and published is documented in
OpenJevSwift under `Tools/encoders/` and decision D-033.

<a name="packages"></a>

## Core ML model packages

| Release | Model | Runs on | Input shapes | Size |
|---|---|---|---|---|
| `verdict-m18-fp16-v1` | `verdict-1.4`, Verdict by Heman10x | Mac (GPU) and iPhone (Neural Engine) | one function per shape: 1 or 16 questions by 128, 256 or 512 tokens | 306 MB |
| `laya-m18-fp16-v1` | `laya-1.0`, Laya by Nandakishor M / Convai Innovations | Mac (GPU) | one function per shape: 1 or 16 questions by 128, 256, 512 or 1,024 tokens | 849 MB |
| `laya-f18-b1s128-fp16-v1` | `laya-1.0` | iPhone (Neural Engine) | one question by 128 tokens | 843 MB |
| `laya-f18-b1s256-fp16-v1` | `laya-1.0` | iPhone (Neural Engine) | one question by 256 tokens | 843 MB |
| `laya-f18-b1s512-fp16-v1` | `laya-1.0` | iPhone (Neural Engine) | one question by 512 tokens | 843 MB |
| `laya-f18-b1s1024-fp16-v1` | `laya-1.0` | iPhone (Neural Engine) | one question by 1,024 tokens | 845 MB |

An iPhone app fetches only the Laya lengths it asks for (`LayaBackend.prefetch(lengths:)`), at
least the 128-token package before its first read.

A release name reads as model, layout and target, weights, then version:

- `m` is a multifunction package, one function per input shape; `f` is a fixed-shape package
  holding one program (`b1s128`: batch 1, that is one question, by 128 tokens).
- `18` is the Core ML target, iOS 18 (macOS 15 on a Mac).
- `fp16` is float16 weights.
- `v1` is the first release of that package. A changed package gets a new release; an existing
  one is never replaced, since OpenJevSwift pins every file's digest.

## Credits

- **Verdict** is by Heman10x ([Verdict-open-jev](https://github.com/Heman10x-NGU/Verdict-open-jev)):
  a ModernBERT-base encoder with a GLiClass head, calibrated with RLCD. The package here is a
  float16 Core ML conversion of the author's checkpoint,
  [heman10x/rlcd-modernbert-151m](https://huggingface.co/heman10x/rlcd-modernbert-151m) at
  `8af2496` (Apache-2.0), with one function per input shape for iOS 18 and macOS 15. The author's
  tokenizer and calibration files are not re-hosted; OpenJevSwift reads them from the checkpoint
  on Hugging Face at the pinned revision.
- **Laya** is by Nandakishor M / Convai Innovations ([laya](https://github.com/NandhaKishorM/laya)):
  a ModernBERT-large model (421M parameters) fine-tuned on the typed-decisions workflows. The
  packages here are float16 Core ML conversions of the author's checkpoint,
  [convaiinnovations/laya-typed-decisions](https://huggingface.co/convaiinnovations/laya-typed-decisions)
  at `1a793eb` (Apache-2.0): `laya-m18-fp16` for a Mac's GPU, and four fixed-shape packages for
  the iPhone's Neural Engine, which does not load Laya's multifunction package. The author's
  tokenizer and `rl_agent_config.json` are not re-hosted; OpenJevSwift reads them from the
  checkpoint on Hugging Face at the pinned revision.

The packages are provided as is, under the Apache License 2.0 of the checkpoints they come from.
