# 3. Configuración de un repositorio

## 3.1. Ignorar archivos

Si en nuestra carpeta o directorio tenemos archivo que no queremos que Git rastree cada vez que hacemos un cambio, lo que debemos hacer es agregar un archivo llamado _.gitignore_ a la misma altura que se encuentra el archivo _.git_ y allí listar los paths de archivos que se deseen ignorar, ya sea por nombre o por extensión (ej: *.txt).

Por ejemplo, si queremos que se ignoren las carpetas `data/`, el archivo `config/keys.json` y todos los archivo `.csv` contenidos en la carpeta `result/`, nuestro `.gitignore` debería verse del siguiente modo:

```{txt}
# ignora data/
data/

# ignora config/keys.json
config/keys.json

# ignora los .csv contenidos en results/
results/*.csv
```

El caracter `#` nos permite agregar comentarios que no serán tenidos en cuenta por git. Estos nos permiten dejar indicaciones más claras para la comprensión del archivo.

**¿Y qué pasa si no me di cuenta de hacer esto cuando creé el repositorio?**

Si Git ya estaba haciendo un seguimiento de un archivo y queremos dejar de seguirlo pero no queremos borrar el archivo de nuestro directorio, lo que se debe hacer es ejecutar el siguiente comando:

        $ git rm --cached <path-del-archivo>
        
Esto quitará nuestro archivo del listado de seguimiento sin eliminarlo de nuestra carpeta local. Si no se agrega el flag --cached, Git removerá el archivo de manera --force: lo quitará del listado de seguimiento y lo eliminará de nuestra computadora (similar a hacer ```rm <nombre-del-archivo>``` solo que tampoco lo encontraremos en la papelera).

Una vez ejecutado este comando, git nos mostrará que el archivo que se ha quitado del rastreo fue eliminado, pero también nos mostrará que el mismo archivo se encuentra _unstaged_ (sin seguimiento). Esto ocurre porque el archivo aún existe en nuestro directorio de trabajo y, si bien git recibió la orden de olvidar que debía rastrearlo, todavía no se le indicó que a futuro también se lo quiere ignorar. Para ello, se debe [incluir el path del archivo en el `.gitignore`](#3.1.-Ignorar-archivos) tal como se indicó previamente.

## 3.2. Definir un template para mensajes de confirmación

Cuando escribimos un mensaje de confirmación (`commit`), ya sea utilizando el flag `-m` o no, podemos escribir un mensaje como queramos, e incluir la información que nos parezca pertinente. Sin embargo, cuando estamos contribuyendo en un proyecto con más gente, puede que queramos tener una plantilla que le indique a cada persona la información a incluir.

En este último caso, podemos agregar a nuestro repositorio un archivo (puede estar oculto) con la plantilla deseada y, con el siguiente comando, configurarla para que, cada vez que se ejecute `git commit`, se muestre dicha plantilla.

El comando para la configuración es el siguiente:

        $ git config commit.template <path-a-la-plantilla>

Entonces, por ejemplo, supongamos que tenemos un archivo llamado `.commit-mssg` que contiene la siguiente información:

```{txt}
# [<tag>] <título> (Máx. 72 caracteres)
# |<----   Preferentemente, hasta 50 caracteres   --->|
# Ejemplo:
# [feat] Implementación de mensajes de commit automáticos

# --- FIN DEL COMMIT ---
# Posibles tags 
#    feat     nuevo feature
#    fix      bug fix
#    refactor refactoring code
#    style    formato, errores ortográficos, etc.; sin cambios en el código
#    doc      cambios en la documentación
#    test     Adiciones o cambios en los tests; sin cambios en el código de producción
#    version  salto de versión o nueva release; sin cambios en el código de producción
#    license  Ediciones de la licencia; sin cambios en el código de producción
#    WIP      Work In Progress; para commits intermedios
#    defaults cambios en las opciones por defecto
```

Debemos ejecutar:

        $ git config commit.template .commit-mssg

Y eso ahará que cada vez que ejecutemos `git commit` se muestre tal plantilla.

Aquí es necesario hacer un par de aclaraciones:

- La configuración de la plantilla para los mensajes de confirmación debe realizarse de manera local. Esto significa que cada persona que tenga una copia del repositorio deberá ejecutar el comando anterior para que la plantilla sea utilizada al hacer los commits.
- El nombre del archivo con la plantilla es indistinto, mientras se lo configure correctamente, puede llamarse de cualqier forma.
- Las líneas de la plantilla que tengan `#` no son tenidas en cuenta por git (igual que ocurre cuando escribimos el archiv `.gitignore`).
- Si bien esta configuración asegura que cada vez que se ejecute el comando `git commit` aparezca la plantilla para ser editada en la consola, no prohibe que el usuario utilice el comando `git commit -m`, y en tal caso no mostrará el template. Por otro lado, tampoco prohibe escribir mensajes de confirmación que no respeten las indicaciones de la plantilla (ya sea utlizando el flag `-m` o no). Ésta es solo una sugenrecia o guía para el usuario