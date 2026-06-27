# File Discovery

## Introducción

Durante varios niveles de **OverTheWire Bandit** fue necesario localizar archivos utilizando diferentes atributos como tamaño, permisos, propietario, tipo, legibilidad o contenido. Estas técnicas son ampliamente utilizadas durante procesos de enumeración en auditorías de seguridad y administración de sistemas Linux.

---

## Comandos de referencia

| Objetivo | Comando |
|----------|----------|
| Buscar archivos | `find . -type f` |
| Buscar directorios | `find . -type d` |
| Buscar por tamaño | `find . -size 1033c` |
| Buscar por usuario | `find . -user usuario` |
| Buscar por grupo | `find . -group grupo` |
| Buscar por permisos | `find . -perm -4000` |
| Buscar archivos ejecutables | `find . -executable` |
| Excluir ejecutables | `find . ! -executable` |
| Buscar texto dentro de un archivo | `grep "texto" archivo` |
| Buscar una palabra en varios archivos | `grep "texto" *` |
| Extraer cadenas legibles de un archivo binario | `strings archivo` |
| Ordenar líneas de un archivo | `sort archivo` |
| Mostrar líneas únicas | `uniq -u` |
| Contar repeticiones de líneas | `uniq -c` |
| Codificar datos en Base64 | `base64 archivo` |
| Decodificar datos en Base64 | `base64 -d archivo` |
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




# Identificación de cadenas legibles (`strings`)

## Introducción

En este nivel se utilizó por primera vez el comando `strings`, una herramienta que permite extraer las cadenas de texto legibles contenidas en archivos binarios. Es especialmente útil cuando un archivo contiene datos no legibles, pero incluye fragmentos de texto que pueden ser de interés.

---

## Problema

La contraseña del siguiente nivel se encontraba almacenada en el archivo `data.txt`, dentro de una de las pocas cadenas legibles y precedida por varios caracteres `=`.

## Metodología

Como el archivo contenía principalmente datos binarios, se utilizó `strings` para extraer únicamente el texto legible. Posteriormente, se filtró la salida con `grep` para localizar las líneas que contenían varios signos `=`.

### Comando utilizado

```bash
strings data.txt | grep "=="
```

### Explicación

* `strings data.txt`: extrae las cadenas de texto legibles del archivo.
* `|`: envía la salida del primer comando al siguiente.
* `grep "=="`: busca las líneas que contienen varios caracteres `=`.

## Resultado

```text
========== <contraseña_del_siguiente_nivel>
```

> Sustituye `<contraseña_del_siguiente_nivel>` por la contraseña obtenida al ejecutar el comando.

## Conceptos aprendidos

* Uso del comando `strings` para extraer texto de archivos binarios.
* Filtrado de resultados mediante `grep`.
* Combinación de comandos utilizando tuberías (`|`).
* Identificación de información útil dentro de archivos binarios.

## Aplicación en Pentesting

El comando `strings` es ampliamente utilizado durante procesos de análisis forense y pentesting para inspeccionar ejecutables, archivos binarios, volcados de memoria (*memory dumps*) y otros archivos que pueden contener credenciales, rutas, nombres de funciones, mensajes de error o información sensible sin necesidad de desensamblar el archivo.

## Conclusión

El comando `strings` permite extraer rápidamente información legible de archivos binarios. Combinado con herramientas como `grep`, facilita la localización de datos específicos durante tareas de análisis y enumeración en sistemas Linux.


# Decodificación de datos en Base64 (`base64`)

## Introducción

En este nivel se utilizó por primera vez el comando `base64`, una herramienta que permite codificar y decodificar información utilizando el esquema de codificación Base64. Este formato es ampliamente utilizado para representar datos binarios como texto ASCII, facilitando su transmisión y almacenamiento.

---

## Problema

La contraseña del siguiente nivel se encontraba almacenada en el archivo `data.txt`, el cual contenía información codificada en **Base64**.

## Metodología

Se utilizó el comando `base64` con la opción `-d` (*decode*) para decodificar el contenido del archivo y obtener el texto original.

### Comando utilizado

```bash
base64 -d data.txt
```

### Explicación

* `base64`: herramienta para codificar y decodificar datos en Base64.
* `-d`: decodifica el contenido del archivo.
* `data.txt`: archivo que contiene la información codificada.

## Resultado

```text
<contraseña_del_siguiente_nivel>
```

> Sustituye `<contraseña_del_siguiente_nivel>` por la contraseña obtenida al ejecutar el comando.

## Conceptos aprendidos

* Identificación de datos codificados en Base64.
* Uso de `base64 -d` para recuperar el contenido original.
* Diferencia entre **codificación** y **cifrado**.
* Manipulación de datos codificados desde la terminal.

## Aplicación en Pentesting

Base64 es un formato muy común en aplicaciones web, APIs, tokens, archivos de configuración y protocolos de comunicación. Durante una auditoría de seguridad es frecuente encontrar credenciales, cookies, cabeceras HTTP o datos serializados codificados en Base64, por lo que conocer cómo decodificarlos resulta fundamental para su análisis.

## Conclusión

El comando `base64` permite codificar y decodificar información de manera sencilla. Aunque Base64 puede ocultar el contenido a simple vista, **no proporciona seguridad**, ya que cualquier persona puede recuperar el texto original utilizando la opción `-d`.



