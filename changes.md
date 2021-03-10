# 4. Gestión de cambios

## 4.1. Ver cambios realizados respecto del remoto

        $ git diff                      # diferencias entre el área de trabajo y directorio de git
                                        # i.e. archivos que fueron modificados pero no se agregaron
                                        # al staging (el directorio de git tendrá los archivos en
                                        # la versión en la que se encontraban en el remoto la última
                                        # vez que se sincronizó el repo local)
        > + línea añadida (en verde)
          - línea borrada (en rojo)

        $ git diff <file-path>          # ídem diff pero solo muestra las diferencias para el archivo
                                        # indicado en <file-path>

        $ git diff --staged             # diferencias entre el staging y el directorio de git (.git)
                                        # (el staging o área de preparación tendrá los archivos
                                        # modificados agregados con add)

        $ git diff <remote-repo>/<remote-branch>..<local-branch>        # diferencias entre el área de
                                                                        # confirmación y el remoto     

        $ git diff <commit-hash> <file-path>    # diferencias entre archivo indicado en <file-path>
                                                # que se encuentra en el directorio de trabajo y el
                                                # que está en el commit indicado con el <commit-hash>

## 4.2. Deshacer cambios en el árbol de trabajo

        $ git checkout -- <file-path>           # deshace cambios realizados en <file-path> y vuelve el
                                                # archivo a la versión en la que se encontraba antes de
                                                # modificarlo (en su última actualización con el remoto)

        $ git checkout -- .                     # ídem anterior pero deshace los cambios en todos los
                                                # archivos que se encuentren donde se está posicionado
                                                # y debajo de ella

## 4.3. Quitar cambios del área de preparación

        $ git reset HEAD <file-path>    # quita los cambios en el área de preparación del archivo
                                        # <file-path> pero lo deja modificado en el directorio de
                                        # trabajo

        $ git reset HEAD                # ídem anterior pero quita los cambios de todos los archivos
                                        # en el área de preparación

## 4.4. Quitar cambios del área de confirmación

        $ git revert <SHA>              # revierte el commit con el SHA indicado (pueden escribirse
                                        # solamente los primeros 7 caracteres, que son los que nos
                                        # devuelve la consola) este comando genera un nuevo commit que
                                        # revierte lo modificado en el indicado (i.e. agrega información
                                        # a la historia de trabajo)
        
        $ git reset --soft HEAD~1       # deshace el último commit hecho pero conserva los archivos
                                        # modificados en el área de staging
                                        # (este comando modifica la historia recopilada en git)
        
        $ git reset --hard HEAD~1       # ídem reset --soft solo que no conserva los archivos 
                                        # modificados

## 4.5. Visualizar qué commits modificaron un archivo

        $ git log <file-path>                   # permite visualizar los commits que han modificado el
                                                # archivo indicado en <file-path>
                                                # también muestra los mensajes de confirmación
        
        $ git log -p <file-path>                # ídem anterior pero muestra además las modificaciones
                                                # realizadas

        $ git log                               # permite visualizar todos los commits y sus mensajes de
                                                # confirmación

        $ git log --oneline                     # ídem anterior pero muestra los commits en una sola
                                                # línea

        # git log --author=<author-name>        # muestra los commits realizador por <author-name>

## 4.6. Volver un archivo a la versión de algún commit anterior
        
        $ git checkout <commit-hash> -- <file-path>     # vuelve el archivo <file-path> a la versión
                                                        # confirmada en el commit indicado en
                                                        # <commit-hash>

## 4.7. Traer un archivo de otra rama

        $ git checkout <branch-name> -- <file-path>     # trae el archivo <file-path> desde la rama
                                                        # <branch-name> (sin importar si el archivo ya
                                                        # se encontraba en la rama a la cual se lo 
                                                        # quiere traer o no; si ya se encontraba allí,
                                                        # se pisa la versión anterior con la que se trae
                                                        # de la rama indicada)