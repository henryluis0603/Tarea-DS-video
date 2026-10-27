# Del texto a tu voz: el Reporte de Inflación en un reel

Tarea de la Sesión 15 de Data Science con Python. Convertí el **Reporte de Inflación de setiembre de 2026 del BCRP**
(188 páginas, unas 82 mil palabras) en un reel vertical de 48 segundos, narrado con mi voz clonada.

| Paso | Herramienta | Archivo |
|---|---|---|
| Texto → guion | Claude, con el prompt de `guion.py` | `prompt.txt` → `respuesta.txt` → `salidas/guion.json` |
| Revisión contra la fuente | yo | ver abajo |
| Voz clonada | Qwen3-TTS 0.6B | `salidas/reel.wav` (no se sube) |
| Subtítulos y control de calidad | Whisper large-v3-turbo | `salidas/reel.srt` |
| Video y posts | Pillow + ffmpeg | `salidas/reel.mp4`, `salidas/posts.md` |

## Cómo reproducirlo

```bash
uv venv .venv
uv pip install --python .venv/bin/python qwen-tts faster-whisper pillow imageio-ffmpeg soundfile jiwer num2words pypdf
.venv/bin/python guion.py --texto informe.pdf --respuesta respuesta.txt
.venv/bin/python hacer_reel.py --voz mi_voz.m4a --autor "Henry Llupton" --fuente "BCRP, Reporte de Inflación set. 2026" \
    --tts Qwen/Qwen3-TTS-12Hz-0.6B-Base --device mps --velocidad 1.08
```

El PDF (`informe.pdf`) y la grabación de voz no se versionan. El PDF se descarga de bcrp.gob.pe.

## Revisión del guion contra la fuente

Las 7 cifras del video pasaron el control automático (`cifra_en_texto: true`). También revisé cada frase a mano:

| Escena | Cifra | Dónde está en el reporte |
|---|---|---|
| 1 | Ago. 2026 | p. 4: inflación y tipo de cambio con información a agosto |
| 2 | 4,44 % | Resumen vii (p. 9): sube de 3,91 % en mayo a 4,44 % en agosto; rango meta de 1 % a 3 % (Prólogo) |
| 3 | +15,7 % | Cuadro 47 (p. 176): transporte local, de enero a agosto |
| 4 | 1,8 % | Resumen vii: la subyacente sin transporte es 1,8 %; párrafo 70: «choques de oferta de carácter transitorio» |
| 5 | 4,25 % | Resumen vi: tasa de referencia en 4,25 % desde setiembre de 2025 |
| 6 | 4,2 % | Resumen vii: 4,2 % para 2026, con mayor inflación a fin de año por El Niño |
| 7 | 2,0 % | Resumen vii: 2,0 % para 2027, en el centro del rango meta |

### Qué corregí y por qué

1. **Pollo.** El resumen dice que la inflación subió entre mayo y agosto por el pescado fresco, los huevos y la carne de pollo.
   Pero el Cuadro 47 muestra que el pollo **bajó** 1,5 % entre enero y agosto. Las dos cosas pueden ser ciertas en periodos
   distintos, pero si el video dice «el pollo sube» sin aclarar el periodo, induce a error. Lo saqué de la frase.
2. **«Extraordinario».** En el reporte, El Niño Costero extraordinario es el escenario *más probable*, no un hecho. Un borrador
   decía «con un Niño costero extraordinario». Lo cambié a «por El Niño costero».
3. **«Por eso».** Un borrador decía «por eso el BCRP mantiene su tasa». El reporte no presenta la tasa como consecuencia
   directa de la inflación sin transporte. Lo cambié a «mientras tanto».
4. **«Sin transporte».** El 1,8 % es la inflación subyacente *sin transporte*, no la subyacente completa (que es 4,50 %). La frase
   y la etiqueta lo dicen explícitamente.
5. **Duración.** La primera versión, de 7 escenas, duró 69.8 s porque la voz clonada habla más pausado que las 2.5 palabras por
   segundo que supone el prompt. Probé sacando la escena de la tasa de referencia (47.9 s), pero al final la volví a poner
   porque es la respuesta de política monetaria. Para compensar, subí la velocidad a 1.08 (`--velocidad 1.08`). También acorté
   la etiqueta de la escena 2, que se partía en dos líneas.
6. **Escena 3.** «Empujan el pescado fresco, los huevos y el transporte local» sonaba a lista de compras y ponía los tres
   rubros al mismo nivel. Lo cambié a «los precios de algunos alimentos y, sobre todo, el transporte local», porque el
   transporte local es el rubro que más contribuyó a la inflación de enero a agosto (1,38 puntos, Cuadro 47).

<!-- Henry: añade aquí lo que tú cambies al leer el guion o al ver el video. -->

## Reglas

Solo uso mi propia voz. El video y los tres posts dicen que la voz fue generada con IA.
