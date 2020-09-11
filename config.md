# 3. Configuración de un repositorio

## 3.1. Ignorar archivos

Si en nuestra carpeta o directorio tenemos archivo que no queremos que Git rastree cada vez que hacemos un cambio, lo que debemos hacer es agregar un archivo llamado _.gitignore_ a la misma altura que se encuentra el archivo _.git_ y allí listar los archivos que se deseen ignorar, ya sea por nombre o por extensión (ej: *.txt).

**¿Y qué pasa si no me di cuenta de hacer esto cuando cree el repositorio?**

Si Git ya estaba haciendo un seguimiento de un archivo y queremos dejar de seguirlo pero no queremos borrar el archivo de nuestro directorio, lo que se debe hacer es ejecutar el siguiente comando:

        $ git rm --cached <nombre-del-archivo>
        
Esto quitará nuestro archivo del listado de seguimiento sin eliminarlo de nuestra carpeta local. Si no se agrega el flag --cached, Git removerá el archivo de manera --force y lo quitará del listado de seguimiento y lo eliminará de nuestra computadora (similar a hacer ```rm <nombre-del-archivo>``` solo que tampoco lo encontraremos en la papelera).

## 3.2. Definir un template para mensajes de confirmación

TODO