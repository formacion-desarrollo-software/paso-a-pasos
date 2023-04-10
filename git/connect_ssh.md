# Conexión SSH con Github

[Paso a Pasos](../README.md) / [Git](readme.md)

___

Este tutorial muestra los pasos para configurar una llave SSH y conectarla con nuestro servicio del almacenamiento de repositorios, en este caso Github. Esta información se basa en la documentación oficial de [GitHub](https://docs.github.com/es/authentication/connecting-to-github-with-ssh/about-ssh)

## Comprobar las claves SSH existentes

1. Abrir una terminal.

2. Ejecutar el siguiente comando para verificar la existecia de claves SSH:
    ```bash
    ls -l ~/.ssh
    ```

    El comando `ls` sirve para listar el contenido de un directorio. La opción `-l` lista el contenido de forma vertical con alguna información asociada a cada archivo o directorio. La ruta `~/.ssh` indica que nos estamos refiriendo al `home` del usuario (el símbolo `~` indica el `home` del usuario) y dentro de este el directorio `.ssh`.

3. Comprobar que en la lista que aparece haya archivos que en su nombe contengan los siguiente caracteres:

    - rsa
    - ecdsa
    - ed25519

    Además deben terminar en `.pub`. Los anteriores caracteres son los nombres de los algoritmos usados para generar la clave SSH y son los que son admitidos por GitHub. El `.pub` indica que es la clave publica.

    Si ya existe una clave y decide usarla, debe asegurarse que en caso de que haya sido generada con contraseña se acuerde de esta. En caso contrario debera generar una nueva.

## Generar una nueva clave SSH

1. Abrir una terminal.

2. Ejecutar el siguiente comando:

    ```bash
    ssh-keygen -t ed25519 -C "your_email@example.com"
    ```

    `ssh-keygen` es el comando para general la clave SSH. La opción `-t` se usa para indicar el tipo de clave que se va a crear o el algoritmo que se va a usar, en ese caso `ed25519`. La opción `-C` se usa para introducir un comentario, en este caso se va a usar el email, solo para identificar quién creo la clave.

3. Dar enter al aparecer el siguiente mensaje:

    ```bash
    Enter file in which to save the key (/Users/my_user/.ssh/id_ed25519):
    ```

    La anterior opción se usa para indicar el nombre del archivo donde va a quedar guardada la clave SSH. Si lo desea puede cambiarle el nombre, para lo cual tendría que indicar la ruta completa del archivo, que para este caso podría ser: `/Users/my_user/.ssh/id_ed25519_mia`

4. Aparecerá el siguiente mensaje:

    ```bash
    Enter passphrase (empty for no passphrase):
    ```

    Para este caso no vamos a introducir una contraseña. Como la intensión es tener una configuración que no nos vuelva a pedir contraseña cada vez que queramos hacer una operación contra Github, no tiene mucho sentido agregar una contraseña a la clave ssh. La seguridad va a recaer en el acceso al computador en el que trabajemos. Quien tenga acceso a él podrá hacer uso de la clave ssh. Entonces, en este paso solo damos Enter.

5. Vuelva a dar Enter al aparecer el siguiente mensaje:

    ```bash
    Enter same passphrase again:
    ```

6. Si todo fue correcto debería ver un mensaje como:

    ```bash
    Your identification has been saved in /Users/my_user/.ssh/id_ed25519
    Your public key has been saved in /Users/my_user/.ssh/id_ed25519.pub
    The key fingerprint is:
    SHA256:wCrLxOkZJD0/hRVggY2nzG/dUHSgeq5V3kOVmXFU2ZY your_email@example.com
    The key's randomart image is:
    +--[ED25519 256]--+
    |   ++oooo.. ..o.=|
    | .o.o+ ...   * Eo|
    |.o+o. =.    =  . |
    | +++ +..   .     |
    |  *.=..oS .      |
    | + =o+.o.o       |
    |  =.  o . o      |
    |     o     .     |
    |    .            |
    +----[SHA256]-----+
    ```

## Agregar la clave SSH al ssh-agent

1. Abrir una terminal y ejecutar el siguiente comando para iniciar el agente SSH:

    ```bash
    eval "$(ssh-agent -s)"
    ```

2. Abrir el archivo de configuracion de SSH. Este paso solo es necesario para usuario Mac:

    ```bash
    code ~/.ssh/config
    ```

    El comando `code` indica que vamos a abrir el archivo con Visual Studio Code. La ruta `~/.ssh/config` indica que nos referimos al archivo `config` que esta dentro del directorio `.ssh`, que a su vez esta dentro del `home` del usuario.

3. Agrega al archivo las siguientes líneas. Este paso solo es necesario para usuario Mac:

    ```txt
    Host github.com
      AddKeysToAgent yes
      UseKeychain yes
      IdentityFile ~/.ssh/id_ed25519
    ```

    Asegúrese que `~/.ssh/id_ed25519` coincida que el nombre de archivo que creó para la clave SSH. Guarde los cambios y cierre el archivo.

4. Agregue la llave privada al agente SSH:

    - Mac:

        ```bash
        ssh-add --apple-use-keychain ~/.ssh/id_ed25519
        ```

    - Windows, Linux:

        ```bash
        ssh-add ~/.ssh/id_ed25519
        ```

## Agregar una clave SSH a tu cuenta de GitHub

1. En una terminal ejecutar el siguiente comando para la llave pública:

    ```bash
    code ~/.ssh/id_id_ed25519.pub
    ```

    Asegúrese que `~/.ssh/id_ed25519.pub` coincida que el nombre de archivo que creó para la clave SSH.

2. Copiar el contenido que aparece al abrir el archivo. Puede cerrarlo después.

3. En su cuenta de GitHub, en la parte superior derecha, dar clic en la imagen de perfil y luego en *Settings*:

    ![GitHub Settings](src/github_settings.png)

4. En la sección *Access* de la barra del lado izquierdo, dar clic en *SSH and GPG keys*.

5. Click en el botón *New SSH key*.

6. En el formulario que aparece, ingrese un título que sea descriptivo de la llave que está usando.

7. En *Key type* dejar seleccionado *Authentication key*.

8. En el campo *Key* pegar la llave pública copiada en el paso 2.

9. Click en el botón *Add SSH key*.


## Probar la conexión SSH

1. En una terminal ejecutar el siguiente comando:

    ```bash
    ssh -T git@github.com
    ```

    Es posible que aparezca una advertencia como la siguiente:

    ```bash
    > The authenticity of host 'github.com (IP ADDRESS)' can't be established.
    > RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
    > Are you sure you want to continue connecting (yes/no)?
    ```

    Simplemente escriba yes y dele Enter.

2. Si todo sale bien, se mostrará un mensaje como el siguiente:

    ```bash
    > Hi USERNAME! You've successfully authenticated, but GitHub does not
    > provide shell access.
    ```
