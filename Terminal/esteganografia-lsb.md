<p align="center">
  <img src="../Recursos/gh_portada_Apple.macOS.Terminal.png"
       alt="Apple macOS Terminal"
       width="100%">
</p>

# Esteganografía digital

### Introducción

La esteganografía digital es una técnica que permite ocultar información, como mensajes de texto, fotografías, archivos de audio o videos, dentro de otros archivos digitales que funcionan como portadores. Su objetivo es disimular la existencia de la información oculta sin producir alteraciones fácilmente perceptibles en el archivo portador.

El procedimiento permitirá generar dos fotografías con el mismo nombre y apariencia prácticamente idéntica, pero con diferente contenido binario, comprobable mediante sus huellas digitales SHA-256.

### Preparación de la fotografía

Para realizar el procedimiento de esteganografía utilizaremos una fotografía en formato PNG. La fotografía deberá encontrarse en el Escritorio con el nombre `fotografia.png`.

### Verificación de la fotografía

Para comprobar que la fotografía existe y verificar sus dimensiones, ejecutamos el siguiente comando en Terminal:

```bash
sips -g format -g pixelWidth -g pixelHeight ~/Desktop/fotografia.png
```

### Resultados

La fotografía tiene unas dimensiones de 1920 × 1272 píxeles, equivalentes a 2,442,240 píxeles.

```text
/Users/cybergraphman/Desktop/fotografia.png
  format: png
  pixelWidth: 1920
  pixelHeight: 1272
```

### Creación del mensaje

Utilizaremos Terminal para crear un archivo de texto llamado `mensaje.txt`, que contendrá el mensaje que posteriormente ocultaremos dentro de la fotografía.

El archivo `mensaje.txt` quedará almacenado en el Escritorio.

### Comando

```bash
cat > ~/Desktop/mensaje.txt <<'EOF'
La esteganografía digital utilizando Terminal, es lo único que necesitas para esconder ese mensaje que requiere entregarse de manera casi anónima, porque si la fotografía es interceptada por un sujeto que conoce las técnicas, lo puede descifrar antes de ser entregado.
EOF
```

### Verificación

Para comprobar que el archivo se creó correctamente y contiene el mensaje, ejecutamos:

```bash
cat ~/Desktop/mensaje.txt
```

### Resultados

Terminal mostrará el contenido completo del archivo:

```text
La esteganografía digital utilizando Terminal, es lo único que necesitas para esconder ese mensaje que requiere entregarse de manera casi anónima, porque si la fotografía es interceptada por un sujeto que conoce las técnicas, lo puede descifrar antes de ser entregado.
```

### Instalación de Pillow

Para ocultar el mensaje dentro de la fotografía utilizaremos **Pillow**, una biblioteca de Python que permite procesar imágenes digitales y modificar los valores de sus píxeles.

### Verificación

Primero comprobaremos si Pillow está instalado. Ejecutamos en Terminal:

```bash
python3 -m pip show Pillow
```

### Resultados

Si Pillow está instalado, Terminal mostrará información similar a la siguiente (la versión dependerá de la instalación):

```text
Name: pillow
Version: 13.0.0
Summary: Python Imaging Library (fork)
Home-page: https://python-pillow.org
```

### Instalación

Si Pillow no está instalado, ejecutamos:

```bash
python3 -m pip install --user Pillow
```

Una vez finalizada la instalación, repetimos el comando de verificación para confirmar que la biblioteca está disponible.

### Creación del programa de esteganografía mediante Nano

Utilizaremos **Nano**, un editor de texto integrado en macOS, para crear el programa Python que ocultará nuestro mensaje dentro de la fotografía mediante la técnica LSB (*Least Significant Bit*).

El programa se guardará en el Escritorio con el nombre `esteganografia.py`.

### Comando

Abrimos Terminal y ejecutamos:

```bash
nano ~/Desktop/esteganografia.py
```

### Resultados

Terminal abrirá el editor Nano, donde introduciremos el código Python encargado de modificar los bits menos significativos de los píxeles de la fotografía.

### Código Python

```python
from PIL import Image
from pathlib import Path

# Archivos de trabajo
escritorio = Path.home() / "Desktop"

original = escritorio / "fotografia.png"
mensaje = escritorio / "mensaje.txt"

carpeta_salida = escritorio / "fotografia secreta"
salida = carpeta_salida / "fotografia.png"

# Comprobar archivos necesarios
if not original.is_file():
    raise FileNotFoundError("No existe la fotografia original.")

if not mensaje.is_file():
    raise FileNotFoundError("No existe el archivo mensaje.txt.")

# Crear carpeta de salida si no existe
carpeta_salida.mkdir(parents=True, exist_ok=True)

# Abrir fotografia y preparar mensaje
with Image.open(original) as foto:
    imagen = foto.convert("RGB")

datos = mensaje.read_bytes()

# Guardar longitud del mensaje en 4 bytes
contenido = len(datos).to_bytes(4, "big") + datos

# Convertir contenido en bits
bits = [
    (byte >> posicion) & 1
    for byte in contenido
    for posicion in range(7, -1, -1)
]

# Comprobar capacidad de la imagen
capacidad = imagen.width * imagen.height * 3

if len(bits) > capacidad:
    raise ValueError("La imagen no tiene capacidad suficiente.")

# Obtener valores RGB
pixeles = list(imagen.getdata())

valores = [
    canal
    for pixel in pixeles
    for canal in pixel
]

# Insertar mensaje mediante LSB
for i, bit in enumerate(bits):
    valores[i] = (valores[i] & 254) | bit

# Reconstruir pixeles
nuevos_pixeles = [
    tuple(valores[i:i + 3])
    for i in range(0, len(valores), 3)
]

# Guardar imagen con mensaje oculto
imagen.putdata(nuevos_pixeles)
imagen.save(salida, format="PNG")

print("Mensaje ocultado correctamente.")
print("Archivo generado:", salida)
print("Dimensiones:", imagen.size)
print("Bits utilizados:", len(bits))
```

### Guardar y cerrar Nano

Una vez introducido el código Python, utilizaremos las siguientes combinaciones de teclas para guardar el programa y cerrar el editor Nano.

- **Control + O:** Guardar el archivo.
- **Enter:** Confirmar el nombre.
- **Control + X:** Salir de Nano.

### Resultados

El programa `esteganografia.py` ha quedado guardado en el Escritorio.


### Ejecución del programa de esteganografía

Una vez guardado el programa `esteganografia.py`, ejecutamos el siguiente comando en Terminal para ocultar el mensaje de texto dentro de la fotografía mediante la técnica LSB.

### Comando

```bash
python3 ~/Desktop/esteganografia.py
```

### Resultados

Si el procedimiento se ejecuta correctamente, Terminal mostrará un resultado similar al siguiente:

```text
Mensaje ocultado correctamente.
Archivo generado: /Users/cybergraphman/Desktop/fotografia secreta/fotografia.png
Dimensiones: (1920, 1272)
Bits utilizados: 2088
```

Se ha creado automáticamente la carpeta `fotografia secreta` en el Escritorio, que contiene la fotografía con el mensaje oculto.

Ambas fotografías conservan el nombre `fotografia.png`, pero están ubicadas en carpetas diferentes:

- **Fotografía original:** `~/Desktop/fotografia.png`
- **Fotografía con mensaje oculto:** `~/Desktop/fotografia secreta/fotografia.png`

La fotografía original permanece sin modificaciones.


### Verificación de las fotografías mediante SHA-256

Utilizaremos Terminal para calcular las huellas digitales SHA-256 de ambas fotografías y comprobar si sus contenidos binarios son diferentes.

### Comando

```bash
shasum -a 256 ~/Desktop/fotografia.png ~/Desktop/"fotografia secreta"/fotografia.png
```

### Resultados

```text
ce2a501425c8a0607fbc8c456bd46db99ff06b120e1346b358db15eb1a47ec6f  /Users/cybergraphman/Desktop/fotografia.png
b172723fcab76c52ca52adbda98b52bd0d60046bafbd3a42fb358727376110c8  /Users/cybergraphman/Desktop/fotografia secreta/fotografia.png
```

**Verificación satisfactoria.** Las fotografías presentan huellas digitales SHA-256 diferentes, lo que confirma que sus contenidos binarios no son idénticos.

Aunque ambas conservan el mismo nombre y una apariencia prácticamente idéntica, la segunda contiene el mensaje oculto mediante la técnica LSB.


### Recuperación del mensaje oculto

Utilizaremos **Nano** para crear un segundo programa Python que permitirá extraer el mensaje oculto dentro de la fotografía mediante la técnica LSB.

El programa se guardará en el Escritorio con el nombre `extraer_mensaje.py`.

### Comando

Abrimos Terminal y ejecutamos:

```bash
nano ~/Desktop/extraer_mensaje.py
```

### Código Python

Terminal abrirá el editor Nano, donde introduciremos el siguiente código:

```python
from PIL import Image
from pathlib import Path

# Archivos de trabajo
escritorio = Path.home() / "Desktop"

imagen = escritorio / "fotografia secreta" / "fotografia.png"
salida = escritorio / "mensaje_recuperado.txt"

# Comprobar que existe la fotografía
if not imagen.is_file():
    raise FileNotFoundError("No existe la fotografia con el mensaje oculto.")

# Abrir fotografía
with Image.open(imagen) as foto:
    imagen_rgb = foto.convert("RGB")
    pixeles = list(imagen_rgb.getdata())

# Extraer bits menos significativos
bits = [
    canal & 1
    for pixel in pixeles
    for canal in pixel
]

# Reconstruir bytes
def leer_byte(inicio):
    valor = 0

    for bit in bits[inicio:inicio + 8]:
        valor = (valor << 1) | bit

    return valor

# Recuperar longitud del mensaje
longitud = 0

for i in range(4):
    longitud = (longitud << 8) | leer_byte(i * 8)

# Comprobar longitud
if (longitud + 4) * 8 > len(bits):
    raise ValueError("Longitud del mensaje no valida.")

# Recuperar mensaje
datos = bytes(
    leer_byte(32 + i * 8)
    for i in range(longitud)
)

# Guardar mensaje recuperado
salida.write_bytes(datos)

print("Mensaje recuperado correctamente.")
print("Archivo:", salida)
print()
print(datos.decode("utf-8"))
```

### Guardar y cerrar Nano

Una vez introducido el código, utilizaremos las siguientes combinaciones:

- **Control + O:** Guardar el archivo.
- **Enter:** Confirmar el nombre.
- **Control + X:** Salir de Nano.

### Resultados

El programa `extraer_mensaje.py` ha quedado guardado en el Escritorio.


### Ejecución del programa para recuperar el mensaje oculto

Utilizaremos Terminal para ejecutar el programa `extraer_mensaje.py` y recuperar el mensaje de texto oculto dentro de la fotografía mediante la técnica LSB.

### Comando

Ejecutamos:

```bash
python3 ~/Desktop/extraer_mensaje.py
```

### Resultados

Terminal mostrará el mensaje recuperado:

```text
Mensaje recuperado correctamente.
Archivo: /Users/cybergraphman/Desktop/mensaje_recuperado.txt

La esteganografía digital utilizando Terminal, es lo único que necesitas para esconder ese mensaje que requiere entregarse de manera casi anónima, porque si la fotografía es interceptada por un sujeto que conoce las técnicas, lo puede descifrar antes de ser entregado.
```

El archivo `mensaje_recuperado.txt` ha quedado guardado en el Escritorio y contiene el mensaje extraído de la fotografía.


[← Volver al inicio](../README.md)
