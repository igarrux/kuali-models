# Modelos de Kuali

[English](README.md) · **Español**

Pesos GGML descargables para [Kuali](https://github.com/igarrux/kuali),
separados del binario de la aplicación.

## Whisper Large v3 Q8

Cuantización Q8_0 reproducible del peso oficial
[`ggml-large-v3.bin`](https://huggingface.co/ggerganov/whisper.cpp/blob/main/ggml-large-v3.bin)
publicado por `whisper.cpp`. Conserva las 32 capas del codificador y del
decodificador, y reduce el tamaño en disco de 3.095.033.483 a 1.656.538.283 bytes.

| Archivo | Formato | Bytes | SHA-256 |
| --- | --- | ---: | --- |
| `ggml-large-v3-q8_0.bin` | Q8_0 | 1.656.538.283 | `24bc434f372355688ab9a623077a63e5361a1c41f4d8d648977e39f9b060f09e` |

### Procedencia

- Origen: `ggml-large-v3.bin` oficial, 3.095.033.483 bytes, SHA-256
  `64d182b440b98d5203c4f9bd541544d84c605196c4f7b845dfa11fb23594d1e2`.
- Cuantizador: `whisper.cpp` v1.8.3, commit
  `2eeeba56e9edd762b4b38467bab96c2517163158`.
- Tipo de cuantización: `q8_0`, versión 2 del formato de cuantización.

### Reproducir la cuantización

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

Conversión reproducible de
[`marianbasti/whisper-large-v3-turbo-latam`](https://huggingface.co/marianbasti/whisper-large-v3-turbo-latam)
para [`whisper.cpp`](https://github.com/ggml-org/whisper.cpp). El modelo de
origen es un ajuste de Whisper Large v3 Turbo para variedades latinoamericanas
del español y se distribuye bajo MIT.

| Archivo | Formato | Bytes | SHA-256 |
| --- | --- | ---: | --- |
| `ggml-large-v3-turbo-latam.bin` | F16 | 1,624,555,275 | `b2e3f5e5b159a6978164d237f981fe95335693abb716fb1c229507b235ace540` |
| `ggml-large-v3-turbo-latam-q5_0.bin` | Q5_0 | 574,041,195 | `1f6261540cb4bdb81cb5821ea40e1dc8ae3041fa4a73f00249abe3e65738dcc4` |

### Procedencia

- Modelo de origen: revisión
  `e22f0ff16b5ec1bfb79b225e1fd3c108d1e15dde`.
- `model.safetensors`: 3,235,581,408 bytes, SHA-256
  `a561d7139bdcc69298f1dc481aa35ec18bc5f62529efd70795592feece67d2ed`.
- Conversor y cuantizador: `whisper.cpp`
  `306c88f4d1286aec1bf96e544632897886af5501`.
- Recursos de tokenización: OpenAI Whisper
  `5f86d1d86363843179951550570367b37c5d6f78`.
- Entorno de conversión: CPython 3.11.14, PyTorch 2.13.0,
  Transformers 5.14.1, Safetensors 0.8.0 y NumPy 2.4.6.

### Reproducir la conversión

```bash
python whisper.cpp/models/convert-h5-to-ggml.py \
  whisper-large-v3-turbo-latam openai-whisper output

whisper-quantize \
  output/ggml-large-v3-turbo-latam.bin \
  output/ggml-large-v3-turbo-latam-q5_0.bin \
  q5_0
```

Comprueba siempre el SHA-256 antes de cargar un peso. Kuali lo hace al
terminar cada descarga y después de cambiar la carpeta de modelos.

## Licencia y atribución

Este repositorio y sus artefactos derivados se distribuyen bajo la
[licencia MIT](LICENSE).
Whisper es obra de OpenAI. El archivo Large v3 Q8 es un derivado cuantizado del
peso oficial. El ajuste LatAm fue publicado por Marian Basti y sus archivos GGML
son conversiones modificadas de ese ajuste. Consulta la tarjeta de cada modelo
de origen para conocer sus datos, limitaciones y resultados declarados.
