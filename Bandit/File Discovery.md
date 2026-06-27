# File Discovery

## Introducción

Durante varios niveles de OverTheWire Bandit fue necesario localizar archivos utilizando diferentes atributos como tamaño, permisos, propietario, tipo o legibilidad. Estas técnicas son ampliamente utilizadas durante procesos de enumeración en auditorías de seguridad y administración de sistemas Linux.

---

| Objetivo                    | Comando                |
| --------------------------- | ---------------------- |
| Buscar archivos             | `find . -type f`       |
| Buscar directorios          | `find . -type d`       |
| Buscar por tamaño           | `find . -size 1033c`   |
| Buscar por usuario          | `find . -user usuario` |
| Buscar por grupo            | `find . -group grupo`  |
| Buscar por permisos         | `find . -perm -4000`   |
| Buscar archivos ejecutables | `find . -executable`   |
| Excluir ejecutables         | `find . ! -executable` |
| Buscar texto dentro de un archivo | `grep "texto" archivo`|
| Buscar una palabra en varios archivos | `grep "texto" *`|



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




## Búsqueda de archivos por propietario y tamaño

### Problema

La contraseña se encontraba en un archivo con las siguientes características:

* Propietario: `bandit7`
* Grupo: `bandit6`
* Tamaño: **33 bytes**

### Metodología

En lugar de recorrer manualmente todo el sistema de archivos, se utilizó `find` para localizar archivos que cumplieran simultáneamente con los atributos indicados.

### Comando utilizado

```bash
find / -user bandit7 -group bandit6 -size 33c 2>/dev/null
```

### Explicación

* `-user bandit7`: busca archivos cuyo propietario sea `bandit7`.
* `-group bandit6`: filtra por grupo propietario.
* `-size 33c`: busca archivos de exactamente 33 bytes (`c` significa bytes).
* `2>/dev/null`: oculta los mensajes de error de permisos denegados.

### Conceptos aprendidos

* Búsqueda avanzada de archivos con `find`.
* Filtrado por propietario.
* Filtrado por grupo.
* Filtrado por tamaño.
* Redirección de errores estándar (`stderr`) hacia `/dev/null`.

### Aplicación en Pentesting

Durante una auditoría de seguridad es habitual utilizar `find` para localizar archivos sensibles, binarios SUID, claves SSH, archivos de configuración, copias de respaldo y otros recursos de interés. Dominar los filtros de `find` permite realizar una enumeración más rápida y eficiente.


## Conclusión

El comando `file` permite identificar rápidamente el tipo de un archivo sin necesidad de inspeccionar manualmente su contenido, siendo una herramienta útil durante procesos de enumeración en Linux.




## Búsqueda de archivos por tamaño

### Problema

La contraseña se encontraba almacenada en un archivo que cumplía las siguientes características:

* Legible para humanos (human-readable).
* Tamaño de **1033 bytes**.
* No ejecutable.

### Metodología

En lugar de recorrer manualmente todos los directorios, se utilizó el comando `find` para localizar únicamente los archivos que cumplieran con las características indicadas.

### Comando utilizado

```bash
find . -type f -size 1033c ! -executable
```

### Explicación

* `.`: inicia la búsqueda desde el directorio actual.
* `-type f`: limita la búsqueda únicamente a archivos.
* `-size 1033c`: busca archivos cuyo tamaño sea exactamente 1033 bytes.
* `! -executable`: excluye archivos con permisos de ejecución.

El comando devolvió la siguiente ruta:

```text
./maybehere07/.file2
```

Posteriormente se visualizó su contenido mediante:

```bash
cat ./maybehere07/.file2
```

### Conceptos aprendidos

* Búsqueda de archivos utilizando atributos específicos.
* Filtrado por tamaño.
* Exclusión de archivos ejecutables.
* Enumeración eficiente mediante `find`.

### Aplicación en Pentesting

Durante una auditoría de seguridad es frecuente localizar archivos sensibles utilizando atributos como tamaño, permisos, propietario o tipo. Dominar `find` permite reducir considerablemente el tiempo de enumeración y encontrar información relevante de manera eficiente.
