# Análisis de hexdump y descompresión múltiple

## Introducción

En este nivel se introducen dos conceptos clave en administración de sistemas y análisis forense: el manejo de **hexdumps** y la **descompresión encadenada de archivos**. El archivo proporcionado no contiene directamente texto legible, sino una representación hexadecimal de datos comprimidos varias veces.

---
| Objetivo | Comando |
|----------|----------|
| Crear directorio temporal seguro | `mktemp -d` |
| Copiar archivos | `cp archivo destino` |
| Mover o renombrar archivos | `mv origen destino` |
| Convertir hexdump a binario | `xxd -r archivo` |
| Ver tipo de archivo | `file archivo` |
| Extraer archivos tar | `tar -xf archivo.tar` |
| Descomprimir gzip | `gzip -d archivo.gz` |
| Descomprimir bzip2 | `bzip2 -d archivo.bz2` |

## Problema

La contraseña del siguiente nivel se encuentra en el archivo `data.txt`, el cual es un **hexdump de un archivo que ha sido comprimido repetidamente**.

Esto implica que no se puede leer directamente y es necesario reconstruir el archivo original paso a paso.

---

## Metodología

Se creó un entorno de trabajo temporal para evitar modificar archivos del sistema. Luego se fue reconstruyendo el archivo original de forma iterativa:

### 1. Crear directorio temporal

```bash id="7q1d8p"
mktemp -d
```

### 2. Copiar el archivo

```bash id="zqk2lm"
cp data.txt /tmp/<directorio_temporal>/
cd /tmp/<directorio_temporal>
```

---

### 3. Convertir el hexdump a binario

```bash id="n4x8aa"
xxd -r data.txt > file
```

* `xxd -r`: revierte el hexdump a su forma binaria original.

---

### 4. Identificar el tipo de archivo

```bash id="p9k2jd"
file file
```

Esto es clave para saber qué tipo de compresión aplicar en cada paso.

---

### 5. Descompresión iterativa

Dependiendo del tipo detectado con `file`, se aplican los comandos adecuados:

| Tipo detectado   | Comando                              |
| ---------------- | ------------------------------------ |
| gzip compressed  | `mv file file.gz && gzip -d file.gz` |
| bzip2 compressed | `bzip2 -d file.bz2`                  |
| tar archive      | `tar -xf file.tar`                   |

Este proceso se repite varias veces:

```bash id="k2j8ss"
file file
```

Cada iteración revela un nuevo nivel de compresión hasta llegar al archivo final legible.

---

## Resultado

```text id="q8w1ld"
<contraseña_del_siguiente_nivel>
```

> Sustituye `<contraseña_del_siguiente_nivel>` por el resultado final obtenido.

---

## Conceptos aprendidos

* Uso de `mktemp -d` para crear entornos seguros temporales.
* Conversión de hexdumps con `xxd -r`.
* Identificación de tipos de archivo con `file`.
* Descompresión encadenada con `gzip`, `bzip2` y `tar`.
* Manejo de archivos intermedios en procesos de análisis.

---

## Aplicación en Pentesting

Este tipo de técnicas es común en análisis forense y CTFs, donde los datos pueden estar ocultos en múltiples capas de compresión o codificación. La capacidad de identificar el tipo de archivo en cada paso es clave para recuperar información oculta.

---

## Conclusión

El nivel demuestra la importancia de combinar herramientas de análisis (`file`, `xxd`) con utilidades de compresión y descompresión. La inspección iterativa permite reconstruir archivos complejos a partir de representaciones aparentemente ilegibles.
