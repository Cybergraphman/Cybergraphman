# Esteganografía LSB en macOS: ocultar un mensaje en una fotografía

> **Práctica didáctica, parte 1 (hasta el paso 9).** Se documenta la preparación y ocultación del mensaje. La extracción y el análisis forense se añadirán después.

La esteganografía LSB (Least Significant Bit) inserta información en los bits menos significativos de los canales RGB de una imagen. En esta práctica se utiliza Terminal, Python 3 y Pillow en macOS Catalina. **Ocultar no equivale a cifrar ni garantiza anonimato**: el mensaje podría descubrirse y recuperarse mediante análisis.

## 4. Verificar la fotografía PNG

La fotografía original debe estar en el Escritorio bajo el nombre `fotografia.png`. Si parte de un JPEG, conviértalo primero con `sips`:

```bash
sips -s format png ~/Desktop/fotografia.jpg --out ~/Desktop/fotografia.png
```

Consulte sus dimensiones:

```bash
sips -g pixelWidth -g pixelHeight ~/Desktop/fotografia.png
```

## 5. Crear el mensaje desde Terminal

```bash
cat > ~/Desktop/mensaje.txt <<'EOF'
La esteganografía digital utilizando Terminal, es lo único que necesitas para esconder ese mensaje que requiere entregarse de manera casi anónima, porque si el adversario conoce las técnicas, lo puede descubrir antes de ser entregado
EOF
```

Compruebe el contenido:

```bash
cat ~/Desktop/mensaje.txt
```

**Nota:** el mensaje mostrado es únicamente un ejemplo didáctico; no incluya información confidencial en repositorios públicos.

## 6. Comprobar e instalar Pillow

```bash
python3 --version
python3 -m pip show Pillow
```

Si Pillow no está instalado:

```bash
python3 -m pip install --user Pillow
python3 -m pip show Pillow
```

## 7. Crear el programa con Nano

En Terminal:

```bash
nano ~/Desktop/esteganografia.py
```

Pegue el código del punto 8. Para guardar, presione **Control + O**, después **Enter**, y para salir **Control + X**.

## 8. Programa Python: ocultar mensaje LSB

```python
from PIL import Image
from pathlib import Path

# Archivos de trabajo (macOS Catalina)
escritorio = Path.home() / "Desktop"
original = escritorio / "fotografia.png"
mensaje = escritorio / "mensaje.txt"
carpeta_salida = escritorio / "fotografia secreta"
salida = carpeta_salida / "fotografia.png"

if not original.is_file():
    raise FileNotFoundError("No existe la fotografia original.")
if not mensaje.is_file():
    raise FileNotFoundError("No existe el archivo mensaje.txt.")

carpeta_salida.mkdir(parents=True, exist_ok=True)

with Image.open(original) as foto:
    imagen = foto.convert("RGB")

datos = mensaje.read_bytes()
contenido = len(datos).to_bytes(4, "big") + datos

bits = [
    (byte >> posicion) & 1
    for byte in contenido
    for posicion in range(7, -1, -1)
]

capacidad = imagen.width * imagen.height * 3
if len(bits) > capacidad:
    raise ValueError("La imagen no tiene capacidad suficiente.")

pixeles = list(imagen.getdata())
valores = [canal for pixel in pixeles for canal in pixel]

for i, bit in enumerate(bits):
    valores[i] = (valores[i] & 254) | bit

nuevos_pixeles = [
    tuple(valores[i:i + 3])
    for i in range(0, len(valores), 3)
]

imagen.putdata(nuevos_pixeles)
imagen.save(salida, format="PNG")

print("Mensaje ocultado correctamente.")
print("Archivo generado:", salida)
print("Dimensiones:", imagen.size)
print("Bits utilizados:", len(bits))
```

**Funcionamiento:** Se guarda una cabecera de 4 bytes que expresa la longitud del mensaje; después se insertan los bits del contenido en los canales RGB. Se genera una imagen PNG nueva, sin alterar la fotografía original. Si ya existe un archivo de salida con el mismo nombre, se reemplaza.

## 9. Ejecutar y comprobar la imagen generada

Cree o compruebe la carpeta de salida (el programa también la crea automáticamente):

```bash
mkdir -p ~/Desktop/"fotografia secreta"
```

Ejecute:

```bash
python3 ~/Desktop/esteganografia.py
```

Salida esperada (dimensiones y bits varían según la imagen y el mensaje):

```text
Mensaje ocultado correctamente.
Archivo generado: /Users/TU_USUARIO/Desktop/fotografia secreta/fotografia.png
Dimensiones: (ancho, alto)
Bits utilizados: cantidad
```

### Estructura de archivos

```text
Desktop/
├── fotografia.png             # Imagen original
├── mensaje.txt
├── esteganografia.py
└── fotografia secreta/
    └── fotografia.png         # Imagen modificada (mismo nombre)
```

Las dos imágenes se llaman `fotografia.png`, pero están en carpetas distintas. El cambio de contenido puede verificarse con SHA-256 en una etapa posterior. **No se publica aquí la fotografía con el mensaje ya oculto**: cada participante debe generarla.

**Continuará:** creación de `extraer_mensaje.py`, recuperación del mensaje, comparación binaria y análisis de los píxeles modificados.
