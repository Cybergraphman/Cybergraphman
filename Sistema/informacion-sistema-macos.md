# Información del sistema

### Introducción

Puedes consultar las características técnicas de tu Mac directamente desde Terminal, sin instalar aplicaciones adicionales.

### Comando

```diff
+ system_profiler SPHardwareDataType SPStorageDataType SPSerialATADataType SPSoftwareDataType
```

### Resultados

```text
Hardware:

    Hardware Overview:

      Model Name: Mac Studio
      Chip: Apple M5 Ultra
      Total Number of Cores: 36
      Memory: 512 GB
      Serial Number (system): [REDACTED]
      Hardware UUID: [REDACTED]

Storage:

    Macintosh HD - Data:

      Free: 12.4 TB
      Capacity: 16 TB
      Mount Point: /System/Volumes/Data
      File System: APFS
      Writable: Yes
      Physical Drive:
          Media Name: APPLE SSD
          Medium Type: SSD
          Protocol: Apple Fabric
          Internal: Yes

    Macintosh HD:

      Capacity: 16 TB
      Mount Point: /
      File System: APFS
      Writable: No

SATA/SATA Express:

    No SATA devices found.

Software:

    System Software Overview:

      System Version: macOS 27
      Kernel Version: Darwin 26.0.0
      Boot Volume: Macintosh HD
      Boot Mode: Normal
      Computer Name: Mac Studio
      User Name: [REDACTED]
      Secure Virtual Memory: Enabled
      System Integrity Protection: Enabled
```

[← Volver al inicio](../README.md)
