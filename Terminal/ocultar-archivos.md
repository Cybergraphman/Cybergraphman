# Ocultación de archivos en macOS

## Objetivo
Ocultar un archivo del Escritorio de macOS mediante la Terminal, usando el atributo `hidden` del sistema de archivos, y verificar que sigue existiendo.

## Entorno de la práctica
- **Equipo:** iMac
- **Sistema operativo de referencia:** macOS Golden Gate 27
- **Interfaz:** Terminal
- **Archivo de prueba:** `Cybergraphman.PNG`
- **Ubicación:** `~/Desktop/Cybergraphman.PNG`

## 1. Identificar el archivo
**Comando:**
```bash
ls -l ~/Desktop
```

**Resultado relevante observado:**
```text
-rw-------@ 1 cybergraphman staff 273749 Feb  4  2026 Cybergraphman.PNG
```
El archivo estaba visible en el Escritorio antes de la práctica.

## 2. Ocultar el archivo
**Comando:**
```bash
chflags hidden ~/Desktop/Cybergraphman.PNG
```

**Resultado observado:** el archivo dejó de mostrarse en el Escritorio de Finder. El comando terminó sin presentar un error.

## 3. Verificar el atributo
**Comando:**
```bash
ls -lO ~/Desktop/Cybergraphman.PNG
```

**Resultado observado:**
```text
-rw-------@ 1 cybergraphman staff hidden 273749 Feb  4  2026 /Users/cybergraphman/Desktop/Cybergraphman.PNG
```
El atributo `hidden` confirma que macOS lo ha marcado como oculto, aunque el archivo permanece en su ruta original.

## 4. Registrar la huella digital
**Comando:**
```bash
shasum -a 256 ~/Desktop/Cybergraphman.PNG
```

**Resultado observado:**
```text
a579e7edca4908b81a248302779ff920f9a14fd27f40179fb5be2ef26ee34783  /Users/cybergraphman/Desktop/Cybergraphman.PNG
```

La huella SHA-256 documenta el contenido del archivo al finalizar la práctica. No se obtuvo un hash previo, por lo que este resultado por sí solo no demuestra una comparación de integridad antes/después.

## Visualización de archivos ocultos en Finder

macOS permite alternar la visualización de archivos ocultos en Finder mediante el atajo de teclado:

**`Command (⌘) + Shift (⇧) + .`**

Este atajo cambia únicamente la visualización de los archivos ocultos, sin eliminar el atributo `hidden` ni modificar su contenido.

## Conclusión
Se ocultó `Cybergraphman.PNG` mediante `chflags hidden`, y su existencia y atributo se comprobaron con `ls -lO`. El procedimiento **no cifra** el archivo ni impide que lo localicen usuarios con permisos y herramientas adecuadas. Es una técnica de ocultación visual mediante atributos del sistema de archivos, no esteganografía ni protección criptográfica.

> Práctica realizada con un archivo propio en macOS. Se utiliza macOS Golden Gate 27 como referencia editorial; no se identifica públicamente la versión instalada durante el ejercicio. Esta documentación se limita al procedimiento de ocultación, sin instrucciones de recuperación.
