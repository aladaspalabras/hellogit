## 4. Gestión de cambios

### 4.1. Ver cambios realizados respecto del remoto

    $ git diff                    # diferencias entre el directorio de trabajo y el remoto
                                  # i.e.fue modificado pero no se agregó al staging
    > + línea añadida (en verde)
      - línea borrada (en rojo)

    $ git diff <file-name>        # diferencias entre archivo llamado <file-name> que se encuentra en el
                                  # directorio del trabajo y el que está en el remoto

    $ git diff --staged           # diferencias entre el área de preparación y el remoto

    $ git diff <remote-repo>/<remote-branch>..HEAD            # diferencias entre el área de confirmación
                                                              # y el remoto
                                                              # (el HEAD apunta al último commit)

    $ git diff <remote-repo>/<remote-branch>..<local-branch>  # diferencias entre el área de confirmación
                                                              # y el remoto
                                                              # (hubo más commits desde el último fetch)

    $ git diff <commit-hash> <file-name>      # diferencias entre archivo llamado <file-name> que se
                                              # encuentra en el directorio de trabajo y el que está
                                              # en el commit indicado con el <commit-hash>

### 4.2. Deshacer cambios en el árbol de trabajo

        $ git checkout -- <nombre-del-archivo>  # deshace cambios realizados y vuelve el archivo a la versión
                                                # en la que se encontraba antes de modificarlo (cuando el
                                                # directorio de trabajo se encontraba actualizado)

### 4.3. Quitar cambios del área de staging

        $ git reset HEAD <nombre-del-archivo>  # quita cambios del área de preparación pero deja el archivo
                                               # modificado en el directorio de trabajo

### 4.4. Quitar cambios del área de confirmación

        $ git revert <SHA>         # revierte el commit con el SHA indicado (pueden escribirse solamente los
                                   # primeros 7 caracteres, que son los que nos devuelve la consola)
                                   # este comando genera un nuevo commit que revierte lo modificado en el
                                   # indicado (i.e. agrega información a la historia de trabajo)
        
        $ git reset --soft HEAD~1  # deshace el último commit hecho pero conserva los archivos modificados en
                                   # el área de stagging
        
        $ git reset --hard HEAD~1  # ídem reset --soft solo que no conserva los archivos modificados 
                                   # es casi borrar el repositorio y volver a clonarlo

### 4.5. Volver un archivo a la versión de algún commit anterior

        $ git log path/to/file               # permite visualizar los commits que han modificado el archivo
                                             # muestra los mensajes
        
        $ git log -p path/to/file            # ídem anterior per muestra además las modificaciones realizadas
        
        $ git checkout <commit hash> path/to/file   # vuelve el archivo a la versión confirmada en el commit
                                                    # indicado