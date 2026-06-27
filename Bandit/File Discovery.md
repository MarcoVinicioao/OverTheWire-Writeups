# File Discovery

## Introducción

Durante varios niveles de **OverTheWire Bandit** fue necesario localizar archivos utilizando diferentes atributos como tamaño, permisos, propietario, tipo, legibilidad o contenido. Estas técnicas son ampliamente utilizadas durante procesos de enumeración en auditorías de seguridad y administración de sistemas Linux.

---

## Comandos de referencia

| Objetivo                              | Comando                |
| ------------------------------------- | ---------------------- |
| Buscar archivos                       | `find . -type f`       |
| Buscar directorios                    | `find . -type d`       |
| Buscar por tamaño                     | `find . -size 1033c`   |
| Buscar por usuario                    | `find . -user usuario` |
| Buscar por grupo                      | `find . -group grupo`  |
| Buscar por permisos                   | `find . -perm -4000`   |
| Buscar archivos ejecutables           | `find . -executable`   |
| Excluir ejecutables                   | `find . ! -executable` |
| Buscar texto dentro de un archivo     | `grep "texto" archivo` |
| Buscar una palabra en varios archivos | `grep "texto" *`       |
| Ordenar líneas de un archivo          | `sort archivo`         |
| Mostrar líneas únicas                 | `uniq -u`              |
| Contar repeticiones de líneas         | `uniq -c`              |

---

# Identificación de archivos por tipo

## Problema

Dentro del directorio `inhere` existían múltiples archivos y únicamente uno contenía la contraseña.

El reto indicaba que el archivo debía ser:

* Legible para humanos (*human-readable*).
* No ejecutable.

## Metodología

En lugar de abrir cada archivo manualmente, se utilizó el comando `file` para identificar el tipo de contenido de todos los archivos.

### Comando utilizado

```bash
file ./-file*
```

Posteriormente se leyó únicamente el archivo identificado como texto ASCII.

```bash
cat ./-file07
```

> **Nota:** El número del archivo puede variar entre versiones del laboratorio. Lo importante es la metodología empleada y no el nombre específico del archivo.

## Conceptos aprendidos

* Identificación del tipo de archivo con `file`.
* Uso de comodines (`*`).
* Enumeración eficiente de múltiples archivos.
* Validación del contenido antes de abrir un archivo.

---

# Búsqueda de archivos por propietario y tamaño

## Problema

La contraseña se encontraba en un archivo con las siguientes características:

* Propietario: `bandit7`
* Grupo: `bandit6`
* Tamaño: **33 bytes**.

## Metodología

Se utilizó `find` para localizar archivos que cumplieran simultáneamente con todos los atributos indicados.

### Comando utilizado

```bash
find / -user bandit7 -group bandit6 -size 33c 2>/dev/null
```

### Explicación

* `-user bandit7`: busca archivos cuyo propietario sea `bandit7`.
* `-group bandit6`: filtra por grupo propietario.
* `-size 33c`: busca archivos de exactamente 33 bytes (`c` significa bytes).
* `2>/dev/null`: oculta los mensajes de error de permisos denegados.

## Conceptos aprendidos

* Búsqueda avanzada de archivos con `find`.
* Filtrado por propietario.
* Filtrado por grupo.
* Filtrado por tamaño.
* Redirección de errores estándar (`stderr`) hacia `/dev/null`.

---

# Búsqueda de archivos por tamaño

## Problema

La contraseña se encontraba almacenada en un archivo que cumplía las siguientes características:

* Legible para humanos (*human-readable*).
* Tamaño de **1033 bytes**.
* No ejecutable.

## Metodología

En lugar de recorrer manualmente todos los directorios, se utilizó `find` para localizar únicamente los archivos que cumplían con las características indicadas.

### Comando utilizado

```bash
find . -type f -size 1033c ! -executable
```

### Explicación

* `.`: inicia la búsqueda desde el directorio actual.
* `-type f`: limita la búsqueda únicamente a archivos.
* `-size 1033c`: busca archivos cuyo tamaño sea exactamente 1033 bytes.
* `! -executable`: excluye archivos con permisos de ejecución.

Una vez localizado el archivo, se mostró su contenido mediante:

```bash
cat ./maybehere07/.file2
```

## Conceptos aprendidos

* Búsqueda de archivos mediante atributos específicos.
* Filtrado por tamaño.
* Exclusión de archivos ejecutables.
* Enumeración eficiente utilizando `find`.

---

# Búsqueda de texto dentro de un archivo (grep)

## Problema

La contraseña del siguiente nivel se encontraba en el archivo `data.txt`, junto a la palabra **millionth**.

## Metodología

Se utilizó `grep` para buscar únicamente la línea que contenía la palabra indicada, evitando revisar manualmente todo el archivo.

### Comando utilizado

```bash
grep "millionth" data.txt
```

### Explicación

* `grep`: busca coincidencias dentro de un archivo.
* `"millionth"`: texto que se desea localizar.
* `data.txt`: archivo donde se realiza la búsqueda.

## Conceptos aprendidos

* Búsqueda de texto dentro de archivos.
* Filtrado rápido de información.
* Uso eficiente de `grep` para localizar datos específicos.

---

# Búsqueda de líneas únicas (sort + uniq)

## Problema

La contraseña del siguiente nivel se encontraba en el archivo `data.txt` y correspondía a la **única línea que aparecía una sola vez**.

## Metodología

Primero se ordenaron todas las líneas utilizando `sort`, ya que `uniq` solo identifica líneas repetidas cuando son consecutivas. Posteriormente se utilizó `uniq -u` para mostrar únicamente la línea única.

### Comando utilizado

```bash
sort data.txt | uniq -u
```

### Explicación

* `sort data.txt`: ordena alfabéticamente todas las líneas del archivo.
* `|`: envía la salida del primer comando al siguiente.
* `uniq -u`: muestra únicamente las líneas que aparecen una sola vez.

## Conceptos aprendidos

* Ordenamiento de datos con `sort`.
* Funcionamiento de `uniq`.
* Uso de `uniq -u` para identificar registros únicos.
* Combinación de comandos mediante tuberías (`|`).

---

# Aplicación en Pentesting

Durante una auditoría de seguridad es habitual utilizar herramientas como `find`, `file`, `grep`, `sort` y `uniq` para localizar archivos sensibles, credenciales, binarios SUID, claves SSH, archivos de configuración y otra información de interés. Dominar estas utilidades permite realizar procesos de enumeración de forma más rápida, precisa y eficiente.

---

# Conclusión

Las herramientas `find`, `file`, `grep`, `sort` y `uniq` son fundamentales para la administración de sistemas Linux y el pentesting. Su uso permite localizar archivos por atributos, identificar su tipo, buscar información específica y analizar grandes volúmenes de datos de manera eficiente, reduciendo el tiempo necesario durante la fase de enumeración.
