# Bandit Level 1 → Level 2

## Objetivo

Encontrar la contraseña del siguiente nivel almacenada en un archivo cuyo nombre es `-`.

## Metodología

Se realizó una enumeración del directorio personal y se identificó un archivo llamado `-`.

Inicialmente se intentó leer el archivo utilizando `cat -`; sin embargo, este comando interpreta `-` como la entrada estándar (stdin), por lo que no mostró el contenido del archivo.

Para evitar esta interpretación, se especificó la ruta relativa del archivo (`./-`), permitiendo acceder correctamente a su contenido.

## Comandos utilizados

```bash
dir
cat -
cat ./-
```

## Explicación

En Linux, muchos comandos interpretan `-` como un argumento especial. En el caso de `cat`, representa la entrada estándar (stdin).

Al utilizar `./-`, se indica explícitamente que `-` es el nombre de un archivo ubicado en el directorio actual, evitando que `cat` lo interprete como una opción especial.

## Resultado

Se obtuvo la contraseña necesaria para autenticarse como `bandit2`.

> **Nota:** La contraseña se omite siguiendo las reglas de OverTheWire.

## Conceptos aprendidos

* Uso de rutas relativas (`./`).
* Diferencia entre un argumento especial y un nombre de archivo.
* Funcionamiento de la entrada estándar (stdin) en Linux.

## Conclusión

Este nivel demuestra que algunos caracteres tienen un significado especial para los comandos de Linux. Cuando un archivo posee un nombre que puede interpretarse como una opción o argumento especial, es necesario utilizar su ruta (`./`) para acceder a él correctamente.
