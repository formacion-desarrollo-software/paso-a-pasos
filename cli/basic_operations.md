# Operaciones Básicas

[Paso a Pasos](../README.md) / [Línea de Comandos](readme.md)

---

- Navegar a un directorio en particular:

    ```sh
    cd /ruta/del/directorio
    ```

    El símbolo `/` indica que nos estamos refiriendo a un directorio dentro de otro directorio. El primer `/` indica la raiz del sistema operativo, es decir, el directorio que contiene a todos los demás. Esta forma de indicar una ruta se conoce como ruta absoluta, porque se indica toda la ruta a partir del directorio raiz. También podríamos ejecutar:

    ```sh
    cd ruta/del/directorio
    ```
    
    Pero en este caso `cd`, al no tener `/` al principio buscará el directorio dentro del directorio donde estemos parados.

- Navegar al `home` del usuario:

    ```sh
    cd
    ```

- Navegar al directorio inmediatamente superior al directorio dónde estemos parados:

    ```sh
    cd ..
    ```

    En este caso un `"."` indica el directorio actual. Agregar otro `"."` indica moverse un directorio arriba y así sucesivamente, hasta llegar a la raiz.

- Mostrar la ruta absoluta de dónde me encuentro actualmente:

    ```sh
    pwd
    ```

- Mostrar el contenido del directorio actual:

    ```sh
    ls
    ```

    También podría listar el contenido de un directorio en particular:

    ```sh
    ls ruta/del/directorio
    ```

    Si quisiera listar el contenido de forma vertical y con más información agrego la opción `-l`:

    ```sh
    ls -l
    ```

    Si además quiero listar los archivos y carpetas ocultos:

    ```sh
    ls -la
    ```

- Crear un directorio dentro del directorio actual:

    ```sh
    mkdir nombre-del-directorio
    ```

- Crear un archivo dentro del directorio actual:

    ```sh
    touch nombre-del-archivo
    ```

- Eliminar un archivo:

    ```sh
    rm nombre-del-archivo
    ```

- Eliminar un directorio

    ```sh
    rm -r nombre-del-directorio
    ```

    Aquí la opción `-r` indica que se quiere eliminar el directorio y todo su contenido.

- Cambiar el nombre de un directorio o archivo dentro del directorio actual:

    ```sh
    mv nombre-archivo-o-directorio nuevo-nombre
    ```
