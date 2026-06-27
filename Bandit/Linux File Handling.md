


# Linux File Handling

## Introducción

Durante los primeros niveles de **OverTheWire Bandit** se presentaron diferentes escenarios relacionados con el manejo de archivos en Linux. Estos retos permiten comprender cómo el shell interpreta nombres de archivos especiales y cómo acceder correctamente a ellos.

---

# Archivos cuyo nombre comienza con "-"

## Problema

El archivo tenía como nombre únicamente:

```text
-
```

Al ejecutar:

```bash
cat -
```

el comando no mostraba el contenido del archivo.

## Explicación

En Linux, el carácter `-` tiene un significado especial para muchos comandos. En el caso de `cat`, representa la **entrada estándar (stdin)**, por lo que el programa espera que el usuario introduzca datos desde el teclado.

## Solución

Se especificó la ruta relativa del archivo:

```bash
cat ./-
```

Al utilizar `./`, el shell interpreta `-` como el nombre de un archivo ubicado en el directorio actual y no como un argumento especial.

## Conceptos aprendidos

* Entrada estándar (stdin).
* Uso de rutas relativas (`./`).
* Interpretación de argumentos especiales.

---

# Archivos con espacios en el nombre

## Problema

El archivo tenía el siguiente nombre:

```text
--spaces in this filename--
```

Al escribir el nombre directamente, el shell separaba cada palabra debido a los espacios.

## Explicación

En Bash, los espacios se utilizan para separar argumentos. Cuando un nombre de archivo contiene espacios, el shell interpreta cada palabra como un argumento diferente.

## Soluciones

Escapar cada espacio con `\`:

```bash
cat ./--spaces\ in\ this\ filename--
```

O utilizar comillas:

```bash
cat "./--spaces in this filename--"
```

Ambas opciones permiten que el shell interprete el nombre completo como un único archivo.

## Conceptos aprendidos

* Escape de caracteres con `\`.
* Uso de comillas para nombres de archivos.
* Interpretación de argumentos por parte del shell.

---

# Conclusiones




---

# Archivos ocultos

## Problema

Dentro del directorio `inhere` no se observaban archivos al ejecutar:

```bash
dir
```

Sin embargo, el reto indicaba que la contraseña se encontraba almacenada en ese directorio.

## Explicación

En Linux, los archivos cuyo nombre comienza con `.` se consideran **archivos ocultos** y no se muestran con un listado convencional (`ls` o `dir`).

Para visualizar este tipo de archivos es necesario utilizar la opción `-a`.

## Solución

Se listó el contenido del directorio incluyendo los archivos ocultos:

```bash
ls -lah
```

La salida mostró el archivo:

```text
...Hiding-From-You
```

Posteriormente se leyó su contenido:

```bash
cat ./...Hiding-From-You
```

## Conceptos aprendidos

* Archivos ocultos en Linux.
* Uso de `ls -a` para mostrar archivos ocultos.
* Uso de `-h` para tamaños legibles por humanos.
* Uso de rutas relativas (`./`).

## Conclusión

Los archivos ocultos son utilizados frecuentemente por aplicaciones y usuarios para almacenar configuraciones o información que no debe mostrarse en un listado estándar. Durante actividades de enumeración en pentesting es recomendable utilizar opciones como `ls -la` para evitar pasar por alto información relevante.

Estos niveles muestran que el shell no siempre interpreta los nombres de los archivos literalmente. Comprender cómo Bash procesa caracteres especiales, espacios y rutas relativas es fundamental para trabajar correctamente en sistemas Linux y durante actividades de administración de sistemas o pentesting.



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

