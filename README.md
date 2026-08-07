# Modelos de Kuali

Pesos GGML descargables para [Kuali](https://github.com/igarrux/kuali),
separados del binario de la aplicación.

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
Whisper es obra de OpenAI y el ajuste LatAm fue publicado por Marian Basti.
Los ficheros GGML son conversiones modificadas de ese ajuste. Consulta también
la tarjeta del modelo de origen para conocer sus datos, limitaciones y
resultados declarados.
