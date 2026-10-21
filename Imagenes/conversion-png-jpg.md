# Conversión de imágenes

### Introducción

Convierte imágenes PNG a formato JPEG directamente desde Terminal, sin necesidad de instalar aplicaciones adicionales.

### Comando

```diff
+ sips -s format jpeg ~/Desktop/imagen.png --out ~/Desktop/imagen.jpg
```

**Importante:** El símbolo `+` se utiliza exclusivamente para resaltar el comando en verde dentro de GitHub. No forma parte de su sintaxis y debe omitirse al ejecutarlo.

### Resultados

```text
/Users/usuario/Desktop/imagen.png
  /Users/usuario/Desktop/imagen.jpg
```

### Consideraciones

- El archivo `imagen.png` debe encontrarse en el Escritorio.
- La imagen convertida se guardará como `imagen.jpg` en el mismo lugar.
- El archivo PNG original permanecerá intacto.
- La conversión utiliza compresión con pérdida de calidad.
- No requiere instalar aplicaciones adicionales en macOS.

[← Volver al inicio](../README.md)
