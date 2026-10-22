<p align="center">
  <img src="../Recursos/gh_portada_Apple.macOS.Terminal.png"
       alt="Apple macOS Terminal"
       width="100%">
</p>

# Conversión de imágenes

### Introducción

Convierte imágenes PNG a formato JPEG directamente desde Terminal, sin necesidad de instalar aplicaciones adicionales.

### Comando

```diff
+ sips -s format jpeg ~/Desktop/imagen.png --out ~/Desktop/imagen.jpg
```

<sub><i><strong>Importante:</strong> El símbolo <code>+</code> se utiliza exclusivamente para resaltar el comando en verde dentro de GitHub. No forma parte de su sintaxis y debe omitirse al ejecutarlo.</i></sub>

### Resultados

```text
/Users/usuario/Desktop/imagen.png
  /Users/usuario/Desktop/imagen.jpg
```

### Verificación del archivo

Comprueba el formato y las dimensiones de la imagen convertida.

```bash
sips -g format -g pixelWidth -g pixelHeight ~/Desktop/imagen.jpg
```

### Resultados de la verificación

```text
/Users/usuario/Desktop/imagen.jpg
  format: jpeg
  pixelWidth: 1920
  pixelHeight: 1080
```

### Consideraciones

- El archivo `imagen.png` debe encontrarse en el Escritorio.
- La imagen convertida se guardará como `imagen.jpg` en el mismo lugar.
- El archivo PNG original permanecerá intacto.
- La conversión utiliza compresión con pérdida de calidad.
- No requiere instalar aplicaciones adicionales en macOS.

[← Volver al inicio](../README.md)
