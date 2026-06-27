# File Discovery

## Introducción

Durante varios niveles de OverTheWire Bandit fue necesario localizar archivos utilizando diferentes atributos como tamaño, permisos, propietario, tipo o legibilidad. Estas técnicas son ampliamente utilizadas durante procesos de enumeración en auditorías de seguridad y administración de sistemas Linux.

---

# Identificación de archivos por tipo

## Problema

Dentro del directorio `inhere` existían múltiples archivos y únicamente uno contenía la contraseña.

El reto indicaba que el archivo era:

* Human-readable.
* No ejecutable.

## Metodología

En lugar de abrir cada archivo manualmente, se utilizó el comando `file` para identificar el tipo de contenido de cada uno.

## Comandos utilizados

```bash
file ./-file*
```

Posteriormente se leyó únicamente el archivo identificado como texto ASCII.

```bash
cat ./-file07
```

> **Nota:** El número del archivo puede variar en futuras versiones del laboratorio. Lo importante es la metodología empleada y no el nombre específico del archivo.

## Conceptos aprendidos

* Identificación del tipo de archivos con `file`.
* Uso de comodines (`*`).
* Enumeración eficiente de múltiples archivos.
* Importancia de validar el contenido antes de abrir archivos de forma indiscriminada.

## Conclusión

El comando `file` permite identificar rápidamente el tipo de un archivo sin necesidad de inspeccionar manualmente su contenido, siendo una herramienta útil durante procesos de enumeración en Linux.
