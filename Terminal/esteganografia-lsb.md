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

[← Volver al inicio](../README.md)
