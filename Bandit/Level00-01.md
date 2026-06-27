# Bandit Level 0 → Level 1

## Objetivo

Encontrar la contraseña del siguiente nivel almacenada en un archivo llamado `readme` ubicado en el directorio personal.

## Metodología

Se realizó una enumeración básica del directorio personal para identificar los archivos disponibles. Posteriormente, se leyó el contenido del archivo `readme` utilizando herramientas nativas de Linux.

## Comandos utilizados

```bash
ls
cat readme
```

## Resultado

El archivo `readme` contenía la contraseña necesaria para autenticarse como `bandit1`.

> **Nota:** La contraseña se omite siguiendo las reglas de OverTheWire.

## Conceptos aprendidos

* Navegación básica en Linux.
* Listado de archivos con `ls`.
* Lectura del contenido de archivos mediante `cat`.
* Acceso remoto utilizando SSH.

## Conclusión

Este nivel introduce el uso de comandos básicos de Linux y demuestra la importancia de la enumeración inicial para localizar información relevante dentro de un sistema.
