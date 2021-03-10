# 6. Resolución de conflictos

Supongamos que estamos trabajando en el mismo repositorio con otras personas y estamos compartiendo nuestra rama de trabajo con alguien más, por lo que dos personas están pusheando a la misma rama.

Supongamos, además, que la otra persona se sincronizó su rama en el mismo momento que nosotros y, por ende, tenemos las mismas versiones de los archivos.

En ese contexto, ambos empezamos a trabajar y a introducir cambios en el repositorio. Sin embargo, la otra persona pushea sus modificaciones antes que nosotros y entonces, cuando queremos subir al remoto las nuestras, la consola nos dice que éste contiene cambios que no tenemos en nuestro repo local (claro, los que acaba de subir la otra persona). 

¿Qué hacer entonces? Hacemos un `pull` y nos descargamos los nuevos cambios.

Si las modificaciones que estaban en el remoto no afectaban a los mismos archivos que nosotros modificamos, no tendremos problemas. Simplemente nos saldrá un mensaje editable (que no es otra cosa que un mensaje de confirmación) indicando que se realiza un Merge y podremos hacer el `push`.

Si, en cambio, la otra persona modificó un archivo también editado por nosotros, habrá conflictos y tendremos que solucionarlos. A continuación se explica cómo.

## 6.1. ¿Cómo saber si alguien más hizo cambios?

Los siguientes comandos nos dirán si hubo cambios en el remoto que no tenemos en el repositorio local

        $ git fetch <remote-name> <branch-name>   # nos trae los cambios de la rama indicada del remoto
                                                  # para que podamos verlos, pero no los descarga a
                                                  # nuestro directorio de trabajo
        
        $ git fetch --all                         # ídem anterior pero con todas las ramas del remoto de
                                                  # todos los remotos (usarlo con cuidado)

Esto nos permite ver si hay cambios que puedan generar conflictos con los nuestros (porque modifican el mismo archivo, por ejemplo).
Si no hay cambios o si vemos que no resultan conflictivos, lo que debemos hacer es ejecutar los comandos `add` y `commit` de modo que nuestros cambios se guarden en el área de preparación. Una vez hecho esto, nos descargamos los cambios del remoto utilizando el comando `pull` y, finalmente, pusheamos nuestros cambios (solo pullear si hay cambios, si no los hay, luego de commitear, pushear directamente).

## 6.2. Conflictos en un merge

Si no tomamos la precaución de controlar previamente que no hubiese conflictos o si la tomamos pero pensamos que era una buena idea resolverlos luego, tenemos que empezar a reflexionar sobre las decisiones que tomamos. Además, no vamos a llegar muy lejos porque, si hay conflictos, cuando queramos hacer el push de nuestros cambios, Git no nos va a dejar y nos va a pedir que los resolvamos.

Por ejemplo, si el archivo con conflictos es _README_ y, ante la advertencia de Git, ejecutamos el comando ```git status```, veremos:

```
  $ git status

  On branch master
  You have unmerged paths.
    (fix conflicts and run "git commit")

  Unmerged paths:
    (use "git add <file>..." to mark resolution)

      both modified:      README

  no changes added to commit (use "git add" and/or "git commit -a")
```
        
Todo aquello que sea conflictivo y no se haya podido resolver, se marca como "sin fusionar" (unmerged). Git añade a los archivos conflictivos unos marcadores especiales de resolución de conflictos que nos guiarán cuando los abramos manualmente y los editemos para corregirlos. 

Si abrimos los archivos en un editor de texto plano convencional, veremos algo como lo siguiente:

```
<<<<<<< HEAD (Current Changes)
Mi repositorio en GitHub
=======
Mi primer repositorio en GitHub
>>>>>>> master (Incomming Changes)
```

Aquí se nos marca que la versión en HEAD (nuetra versión del directorio de trabajo) contiene lo indicado en la parte superior del bloque y la versión entrante contiene el resto, lo indicado en la parte inferior del bloque. 

De todos modos, para una visualización más clara, lo mejor suele ser usar un editor de código del tipo de  [Visual Studio Code](https://code.visualstudio.com/)(VSC), donde veremos algo como lo siguiente:

![alt text](./pictures/visualconflicts.png)

Si queremos aceptar los cambios entrantes, simplemente debemos hacer click en el **Accept Incoming Change**.
Si, en cambio, queremos dejar nuestros cambios, debemos elegir **Accept Current Change**.
O podemos aceptar ambo, para ello usamos **Accept Both Changes**.

Esto debemos hacerlo para cada cambio. Pero, si ya sabemos que queremos aceptar todos los cambios entrantes o todos los cambios actuales (los que están en nuestro directorio de trabajo) y estamos usando VSC, podemos utilizar la paleta de comandos. Para ello, apretamos `Ctrl+Shift+P` y escribimos _Accept All_. Esto hará que se nos muestren las opciones para resolver todos los conflictos de fusión de la misma forma (aceptando todos los cambios entrantes, todos los actuales o ambos cambios).

Si no estaos usando algún editor de código, deberíamos considerar seriamente hacerlo. Mientras tanto, para resolver nuestros conflictos, deberemos abrir un editor de texto plano (Text Editor, Notepad o el que sea de nuestra preferencia), arremangarnos y buscar cada ocurrencia de conflicto (podemos buscar _HEAD_). Cada vez que encontremos una, tenemos que borrar las líneas que indican _<<<<<<< HEAD (Current Changes)_, _>>>>>>> \<branch> (Incomming Changes)_, _=======_ y las de la porción de código que queremos desestimar. Solo debe quedar el cambio que queremos (el current o el incomming). En el ejemplo anterior, deberíamos dejar o bien _Mi repositorio en GitHub_, o bien _Mi primer repositorio en GitHub_

Una vez hecho esto guardamos el archivo y agregamos el archivo al stagging (con el ya conocido comando `add`). Y si ejecutamos el comando ```git status```, veremos:

```
  $ git status

  On branch master
  All conflicts fixed but you are still merging.
    (use "git commit" to conclude merge)

  Changes to be committed:

      modified:   README
```
            
Si todo ha salido correctamente y vemos que todos los archivos conflictivos están marcados como preparados, podemos utilizar el comando ```commit``` para terminar de confirmar la fusión y luego pushear.

## 6.3. Conflictos en una pull request

Cuando realizamos una PR, puede suceder que nos surjan conflictos. En el caso de que los mismos se deban a que estamos compartiendo la rama desde la que hacemos la PR con otra persona, la resolución se debe realizar del mismo modo que se indicó en el apartado [6.2](#62-conflictos-en-un-merge).

Sin embargo, también puede ocurrir que la interfaz de nuestro remoto nos indique que el conflicto se da con la rama en la que queremos introducir nuestros cambios (recordemos que una pull request es una solicitud a una rama para que incorpore los cambios de nuestra rama). En este caso, lo que sucede es que la rama que desea introducir cambios y aquella en la cual se los introduce divergen en algún punto de su historia. ¿Qué significa esto? Que la rama que recibirá los cambios tiene algún archivo con modificaciones que la rama que desea introducir cambios no tiene. 

Para evitar esto, lo mejor es siempre armar nuestra rama de trabajo partiendo de aquella en la que luego querremos introducir nuestros arreglos o mejoras. Por ejemplo, si la rama base de nuestro repo es _master_ y sabemos que nuestros cambios deben fusionarse con ella, partimos de ahí para generar nuestra rama de trabajo. Pongámosle a esta, por nombre, _fix/bug123_. 

Ahora bien, una vez que tenemos nuestro trabajo listo, hacemos la PR a master. Si master no tuvo cambios en medio de que armamos nuestra rama, trabajamos e hicimos la PR, probablemente no tengamos mayores iconvenientes. Pero si, en cambio, alguien más hizo una PR a master y esa PR se mergeó, nuestra rama estará intentando meterse en master sin tener la misma historia, puesto que no tendremos los últimos cambios incorporados. Para solucionar esto, debemos traer los cambios de master a nuestra rama y así alinear las historias.

En este caso, lo que debemos hacer es lo siguiente. En nuestra copia local ejecutamos:

    $ git checkout <fix-branch>                           # nos posicionamos en la rama que tiene
                                                          # nuestro trabajo
    
    $ git fetch <remote> <target-branch>:<target-branch>  # nos aseguramos de que la rama a la que
                                                          # hacemos la PR está sincronizada

    $ git merge <target-branch>                           # fusionamos los cambios de nuestra rama
                                                          # target (a la que le hacemos la PR) en
                                                          # nuestra rama de trabajo

Siguiendo nuestro ejemplo anterior:

    $ git checkout fix/bug123
    
    $ git fetch origin master:master

    $ git merge master

Una vez hecho esto, pueden suceder dos cosas:
- En el mundo más feliz, el merge con la rama target se hace sin conflictos y solo nos restará hacer un `push` a nuestra rama de trabajo. Luego de hacerlo, veremos que la PR realizada se actualiza y las advertencias de conflictos en el remoto deberían desaparecer.
- En un mundo menos feliz, Git podría indicarnos que hay conflictos porque en la rama target había modificaciones que alteraban los mismos archivos que nosotros cambiamos. En este caso, debemos resolverlos del mismo modo que se indicó en la sección [6.2](#62-conflictos-en-un-merge). Una vez resuletos, realizar los ya clásicos `add`, `commit` y `push`. Igual que en el mundo feliz, la PR se actualizará (debido a que la rama se actualizó) y deberíamos dejar de ver las advertencias de conflictos.