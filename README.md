# Odoo & IDE Visual Studio

## Instalación inicial en maquina local

Instalar en maquina local las librerias de desarrollo:

    sudo apt-get update -y
    sudo apt-get install libsasl2-dev python3-dev libldap2-dev libssl-dev libpq-dev

Instalar el PosgreSQL Client:

    sudo apt install gnupg gnupg2 gnupg1 -y
    sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
    wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | apt-key add -
    sudo apt-get update -y
    sudo apt-get install --no-install-recommends -y postgresql-client-14

Opcional instalar Postgre SQL completo:

    sudo apt-get install postgresql-14 -y

## Clonar un repositorio con Subproyecto

Para clonar un repositorio junto con su submódulo:

    git clone --recurse-submodules --branch 17.0 git@github.com:codlan-labs/odoo_enterprise_vs.git

O se puede clonar y luego descargar los cambios con los siguiente comandos:

    git clone -b 17.0 git@github.com:codlan-labs/odoo_enterprise_vs.git
    cd odoo_enterprise_vs
    git submodule update --init --recursive

Utilizar `--recursive` solo es necesario si algún submódulo tiene submódulos.

## Crear entorno virtual

Crear entorno virtual, se pude crear con python3.10 o python3.11:

    python3.11 -m venv env

Activar el entorno virtual en Linux:

    source env/bin/activate

Activar el entorno virtual en Windows:

    .\env\Script\Activate.ps1

## Instalar dependencias

Ubicado dentro del entorno virtual instalar de forma individual la libreria python-ldap:

    pip install python-ldap==3.4.0

Y luego instalar las otras librerias requeridas:

    pip install -r requirements.txt


## Errores comunes
### OSError: [Errno 24] inotify instance limit reached

    sudo nano /etc/sysctl.conf
    fs.inotify.max_user_instances = 1100000
    sudo sysctl -p

## Manejo de Submódulos

### Agregar un nuevo Subproyecto

    git submodule add -b <branch> <url> <path>
    git commit -m "feat: Se agrego nuevo submódulo <name>"

### Descargar actualizaciones de Superproyectos
De forma predeterminada, el repositorio del submódulo se recupera, pero no se actualiza cuando ejecuta `git pull` en el superproyecto. Se necesita usar `git submodule update`, o agregar `--recurse-submodules` a pull:

    git pull
    git submodule update --init --recursive

o, en un comando (Git >= 2.14)

    git pull --recurse-submodules

`--init` es necesario si el superproyecto agregó nuevos submódulos, y `--recursive` es necesario si algún submódulo tiene submódulos.

Si alguna vez el superproyecto cambia la URL del submódulo, se requiere un comando por separado:

    git submodule sync --recursive
    git submodule update --init --recursive

## Actualizar los submódulos a la última versión en cada rama

Este comando recuperará las últimas versiones de los submódulos según la rama configurada en el archivo `.gitmodules ` y los actualizará en tu repositorio.

    git submodule update --remote

## Si quieres actualizar un submódulo específico

Esto actualizará solo el submódulo con el nombre especificado.

    git submodule update --remote nombre_submodulo

## Si también deseas cambiar a la rama principal de cada submódulo

Estos comandos cambiarán cada submódulo a la rama principal y luego realizarán un pull para actualizarlos.

    git submodule foreach git checkout main
    git submodule foreach git pull origin main

### Cambiar de rama
De forma predeterminada, el árbol de trabajo del submódulo no se actualiza para que coincida con el commit registrado en el superproyecto al cambiar de rama. Necesita usar `git submodule update`, o agregar `--recurse-submodules` a checkout:


    git checkout 17.0
    git submodule update --recursive

o, en un comando (Git >= 2.13)

    git checkout --recurse-submodules <branch>

### Subir cambios al repositorio del Subproyecto

Mientras está en el directorio del submódulo:

    git push

O mientras está en el directorio principal:

    git push --recurse-submodules=on-demand

## Configuraciones útiles para submódulos

Siempre muestre el registro del submódulo cuando vea diferencias:

    git config --global diff.submodule log

Muestre un breve resumen de los cambios del submódulo en su mensaje git status:

    git config --global status.submoduleSummary true

Haga que push sea predeterminado en --recurse-submodules = on-demand:

    git config --global push.recurseSubmodules on-demand

Haga que todos los comandos (excepto clone) estén predeterminados en --recurse-submodules si admiten la bandera (esto funciona para git pull desde Git 2.15):

    git config --global submodule.recurse true

## Extras de Odoo
### Scaffold

Ubicarse en la raiz del proyecto y ejecutar:

Para Linux y MAC el comando:

    python odoo-bin scaffold name_module /addons/

Para Windows el comando:

    python.exe odoo-bin scaffold name_module /addons/

## Fuentes

- [Manejo de dependencias con Submódulos](https://training.github.com/downloads/es_ES/submodule-vs-subtree-cheat-sheet/)
- [Git-submodule](https://git-scm.com/docs/git-submodule)
- [Actualziar los submódulso git de un proyecto](https://mascandobits.es/programacion/actualizar-los-submodulos-git-de-un-proyecto/)
- [Atlassian | Submódulos de Git](https://www.atlassian.com/es/git/tutorials/git-submodule)
- [Actualización de submódulo en Git](https://www.delftstack.com/es/howto/git/submodule-update-in-git/)

## Contribuciones

- [Harrison Chumpitaz](mailto:hchumpitaz92@gmail.com)