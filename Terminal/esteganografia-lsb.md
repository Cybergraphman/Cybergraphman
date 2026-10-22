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

[← Volver al inicio](../README.md)
