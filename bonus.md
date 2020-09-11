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


### 4.5. Volver un archivo a la versión de de algún commit anterior

git reset <commit hash> <filename>
    
    You may need to use the --hard option if you have local modifications.