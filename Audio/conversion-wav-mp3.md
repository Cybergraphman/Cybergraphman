# Conversión de audio

### Introducción

Convierte archivos de audio WAV a formato MP3 directamente desde Terminal utilizando FFmpeg, una herramienta de procesamiento multimedia.

### 1. Instalación

Instala FFmpeg mediante el paquete `imageio-ffmpeg` de Python.

```bash
python3 -m pip install --user imageio-ffmpeg
```

### 2. Localización

Consulta la ruta donde se encuentra el ejecutable de FFmpeg.

```bash
python3 -c "import imageio_ffmpeg; print(imageio_ffmpeg.get_ffmpeg_exe())"
```

### 3. Verificación de la instalación

Comprueba la versión de FFmpeg instalada.

```bash
python3 -c "import imageio_ffmpeg; print(imageio_ffmpeg.get_ffmpeg_version())"
```

### 4. Conversión

Convierte el archivo `audio.wav`, ubicado en el Escritorio, a formato MP3.

```diff
+ ffmpeg -i ~/Desktop/audio.wav -codec:a libmp3lame -qscale:a 2 ~/Desktop/audio.mp3
```

<sub><i><strong>Importante:</strong> El símbolo <code>+</code> se utiliza exclusivamente para resaltar el comando en verde dentro de GitHub. No forma parte de su sintaxis y debe omitirse al ejecutarlo.</i></sub>

### Resultados

**Ejemplo de salida de Terminal:**

```text
Input #0, wav, from '/Users/usuario/Desktop/audio.wav':
  Duration: 00:02:07.02, bitrate: 1411 kb/s
  Stream #0:0: Audio: pcm_s16le, 44100 Hz, stereo, s16

Stream mapping:
  Stream #0:0 -> #0:0 (pcm_s16le -> mp3)

Output #0, mp3, to '/Users/usuario/Desktop/audio.mp3':
  Stream #0:0: Audio: mp3, 44100 Hz, stereo

size=3256KiB time=00:02:07.02 bitrate=210.0kbits/s speed=35.5x
```

### 5. Verificación del archivo

Comprueba que el archivo MP3 generado puede decodificarse correctamente.  
El comando procesa el archivo sin generar otro archivo. Si finaliza sin errores de decodificación, la comprobación se considera satisfactoria.

```bash
ffmpeg -i ~/Desktop/audio.mp3 -f null -
```

### Consideraciones

- Requiere Python 3 y FFmpeg.
- El paquete `imageio-ffmpeg` proporciona un ejecutable de FFmpeg, pero no lo incorpora automáticamente al PATH de Terminal.
- Los comandos `ffmpeg` requieren que el ejecutable esté disponible en el PATH.
- El archivo WAV original permanece intacto.
- El archivo MP3 se guarda en el Escritorio.
- MP3 utiliza compresión con pérdida de información.
- La opción `-qscale:a 2` establece una calidad variable alta (VBR).

[← Volver al inicio](../README.md)
