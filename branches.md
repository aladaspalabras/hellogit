# 5. Ramas

Cuando hablamos de ramificaciones, nos referimos a que podemos tomar una rama de desarrollo (supongamos, llamada master) y, a partir de ella, generar otra igual en la que seguiremos trabajando, pero cuyos cambios en principio no afectarás a la rama de origen.

Esto es sumamente útil cuando estamos compartiendo nuestro repositorio con otras personas y no queremos que sus cambios interfieran en nuestro trabajo, pero es posible que luego queramos integrar todo, los archivos en nuestra rama con los de las ramas de los demás.

Una rama Git es simplemente un apuntador móvil que señala cada una de las confirmaciones que vamos haciendo en nuestro repositorio, lo que significa que señala cada foto que de nuestros archivos que vamos tomando.

Cuando creamos un repositorio, la primera rama qe se crea por defecto suele ser `master` o `main` y esta es la que se considera la _rama base_, pero esto es configurable y se puede indicar cualquier otra rama en su lugar.

Que una rama sea la _rama base_ implica, entre otras cosas, que cuando alguien clone el repo, la rama en la que se encontrará ni bien se genera la copia local será esta rama.

Con cada confirmación que realicemos, y mientras no nos cambiemos de rama, el apuntador de la rama base (o de la rama en la que nos enontemos) irá avanzando en la historia que registra git de un archivo.

![alt text](./pictures/master.png)

![alt text](./pictures/advance-testing2.png)

## 5.1. Creación de ramas
        
        $ git checkout -b <new-branch>          # crea una nueva rama idéntica a la rama en la que nos
                                                # encontramos y nos mueve hacia ella
    
Esto creará un nuevo apuntador llamado \<new-branch> que estará basado en la rama desde la cual fue creado. Esto significa que, hasta que hagamos alguna modificación en esta nueva rama o en aquella desde la que partimos, ambas serán iguales. 
Además, al mismo tiempo que crea la rama, el comando nos moverá a ese apuntador. Cualquier cambio que hagamos será seguido por ese apuntador y no por aquel donde nos encontrábamos previamente.

![alt text](./pictures/two-branches.png)

Si, en cambio, solo queremos crear una nueva rama pero no movernos hacia ella, debemos ejecutar el siguiente comando:

        $ git branch <new-branch>               # crea una nueva rama idéntica a la rama en la que nos
                                                # encontramos

## 5.2. Cambio entre ramas

        $ git checkout <branch-name>      # mueve mi apuntador a la rama indicada
        
Para poder ejecutar este comando es necesario tener el árbol de trabajo o working directory de la rama en la que me encuentro limpio. De lo contrario, Git nos pedirá que subamos nuestros cambios al remoto o los descartemos. 

También es posible utilizar el comando ```stash```, que nos permite guardar temporalmente estos cambios sin confirmarlos ni descartarlos.

## 5.3. Fusión de ramas

### 5.3.1. Merge

Si deseamos fusionar los cambios de una rama en otra, debemos movernos a aquella en la que queremos importar los cambios utilizando el comando ```checkout``` y llevar los cambios de la rama deseada con el comando ```merge```:

        $ git checkout <one-branch>                             # nos mueve a la rama que recibirá los
                                                                # cambios

        $ git fetch <remote> <other-branch>:<other-branch>      # se asegura de que la rama cuyos
                                                                # cambios queremos introducir se 
                                                                # encuentre sincronizada con el
                                                                # remoto (origin)
        
        $ git merge <other-branch>                              # importa los cambios de la rama 
                                                                # <other-branch> en aquella en la que
                                                                # nos encontramos

### 5.3.2. Merge Request (MR) o Pull Request (PR)

Cuando hacemos un merge, una rama A (en la que estamos posicionados) se trae los cambios de otra rama B.

En un merge request (en GitLab) o pull request (en GitHub) es la rama B la que le pide a la rama A que incorpore sus cambios.

Esta acción debe hacerse desde la interfaz de la página del servidor donde se encuentre el remoto.

**Aclaración:** Tanto el merge como el MR o PR pueden tener conflictos si la rama que se intenta fusionar no tiene (al momento de hacer la fusión) todos los cambios que tiene la rama a la que se quiere fusionar (i.e. debe tener en su historial el último commit de la rama a la cual se quieren fusionar los cambios). En caso de existir conflictos, se los deberá resolver como se detalla en la sección [6](./merge.md)