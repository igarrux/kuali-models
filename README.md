# Kuali models

**English** · [Español](README.es.md)

Downloadable GGML weights for [Kuali](https://github.com/igarrux/kuali), kept
separate from the application executable.

## Whisper Large v3 Q8

Reproducible Q8_0 quantization of the official
[`ggml-large-v3.bin`](https://huggingface.co/ggerganov/whisper.cpp/blob/main/ggml-large-v3.bin)
weight published by `whisper.cpp`. This keeps all 32 encoder and decoder layers
while reducing the on-disk size from 3,095,033,483 to 1,656,538,283 bytes.

| File | Format | Bytes | SHA-256 |
| --- | --- | ---: | --- |
| `ggml-large-v3-q8_0.bin` | Q8_0 | 1,656,538,283 | `24bc434f372355688ab9a623077a63e5361a1c41f4d8d648977e39f9b060f09e` |

### Provenance

- Source: official `ggml-large-v3.bin`, 3,095,033,483 bytes, SHA-256
  `64d182b440b98d5203c4f9bd541544d84c605196c4f7b845dfa11fb23594d1e2`.
- Quantizer: `whisper.cpp` v1.8.3, commit
  `2eeeba56e9edd762b4b38467bab96c2517163158`.
- Quantization type: `q8_0`, quantization format version 2.

### Reproduce the quantization

```bash
git clone --branch v1.8.3 https://github.com/ggerganov/whisper.cpp.git
cmake -S whisper.cpp -B whisper.cpp/build \
  -DWHISPER_BUILD_TESTS=OFF \
  -DWHISPER_BUILD_EXAMPLES=ON
cmake --build whisper.cpp/build --target whisper-quantize

whisper.cpp/build/bin/whisper-quantize \
  ggml-large-v3.bin \
  ggml-large-v3-q8_0.bin \
  q8_0
```

## Whisper Large v3 Turbo LatAm

Reproducible conversion of
[`marianbasti/whisper-large-v3-turbo-latam`](https://huggingface.co/marianbasti/whisper-large-v3-turbo-latam)
for [`whisper.cpp`](https://github.com/ggml-org/whisper.cpp). The source model
fine-tunes Whisper Large v3 Turbo for Latin American varieties of Spanish and
is released under the MIT License.

| File | Format | Bytes | SHA-256 |
| --- | --- | ---: | --- |
| `ggml-large-v3-turbo-latam.bin` | F16 | 1,624,555,275 | `b2e3f5e5b159a6978164d237f981fe95335693abb716fb1c229507b235ace540` |
| `ggml-large-v3-turbo-latam-q5_0.bin` | Q5_0 | 574,041,195 | `1f6261540cb4bdb81cb5821ea40e1dc8ae3041fa4a73f00249abe3e65738dcc4` |

### Provenance

- Source model revision:
  `e22f0ff16b5ec1bfb79b225e1fd3c108d1e15dde`.
- `model.safetensors`: 3,235,581,408 bytes, SHA-256
  `a561d7139bdcc69298f1dc481aa35ec18bc5f62529efd70795592feece67d2ed`.
- Converter and quantizer: `whisper.cpp`
  `306c88f4d1286aec1bf96e544632897886af5501`.
- Tokenization resources: OpenAI Whisper
  `5f86d1d86363843179951550570367b37c5d6f78`.
- Conversion environment: CPython 3.11.14, PyTorch 2.13.0,
  Transformers 5.14.1, Safetensors 0.8.0, and NumPy 2.4.6.

### Reproduce the conversion

```bash
python whisper.cpp/models/convert-h5-to-ggml.py \
  whisper-large-v3-turbo-latam openai-whisper output

whisper-quantize \
  output/ggml-large-v3-turbo-latam.bin \
  output/ggml-large-v3-turbo-latam-q5_0.bin \
  q5_0
```

Always verify SHA-256 before loading a weight. Kuali checks it after every
download and whenever the model directory changes.

## License and attribution

This repository and its converted artifacts are distributed under the
[MIT License](LICENSE). Whisper was created by OpenAI. The Large v3 Q8 file is a
quantized derivative of the official Whisper weight. The LatAm fine-tune was
published by Marian Basti, and its GGML files are modified conversions of that
fine-tune. Consult each source model card for its dataset, limitations, and
reported evaluation results.
