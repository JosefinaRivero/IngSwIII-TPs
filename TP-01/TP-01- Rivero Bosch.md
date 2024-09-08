INGENIERIA DE SOFTWARE III- RIVERO BOSCH JOSEFINA

### TP - 01: GIT BASICO

## Desarrollo:

# 1- Instalar Git

Los pasos y referencias asumen el uso del sistema operativo Windows, en caso otros SO seguir recomendaciones espec�ficas.
- Bajar e instalar el cliente git. Por ejemplo, https://git-for-windows.github.io/
- Bajar e instalar un cliente visual. Por ejemplo, TortoiseGit para Windows o SourceTree para Windows/MAC:
- https://tortoisegit.org/
- https://www.sourcetreeapp.com/
- Lista completa: https://git-scm.com/downloads/guis/

# 2- Crear un repositorio local y agregar archivos

- Crear un repositorio local en un nuevo directorio.
- Agregar un archivo Readme.md, agregar algunas l�neas con texto a dicho
archivo.
![Descripci�n de la imagen](imagen1.jpg)

- Crear un commit y proveer un mensaje descriptivo.
![Descripci�n de la imagen](imagen2.jpg)
![Descripci�n de la imagen](imagen3.jpg)




# 3- Configuraci�n del Editor Predeterminado

- Instalar Notepad ++ para Windows o TextMate para Mac OS, colocarle un alias y configurarlo como editor predeterminado

# 4- Creaci�n de Repos 01 -> Crearlo en GitHub, clonarlo localmente y subir cambios

- Crear una cuenta en https://github.com
- Crear un nuevo repositorio en dicha p�gina con el Readme.md por defecto
- Clonar el repo remoto en un nuevo directorio local
- Editar archivo Readme.md agregando algunas lineas de texto
- Editar (o crear si no existe) el archivo .gitignore agregando los archivos *.bak
- Crear un commit y porveer un mensaje descriptivo
- Intentar un push al repo remoto
![Descripci�n de la imagen](imagen4.jpg)

- En caso de ser necesario configurar las claves SSH requeridas y reintentar el push.

![Descripci�n de la imagen](imagen6.jpg)

![Descripci�n de la imagen](imagen7.jpg)
![Descripci�n de la imagen](imagen8.jpg)
![Descripci�n de la imagen](imagen9.jpg)
![Descripci�n de la imagen](imagen10.jpg)





# 5- Creaci�n de Repos 02-> Crearlo localmente y subirlo a GitHub

- Crear un repo local
- Agregar archivo Readme.md con algunas lineas de texto
- Crear repo remoto en GitHub
![Descripci�n de la imagen](imagen11.jpg)

- Asociar repo local con remoto
![Descripci�n de la imagen](imagen12.jpg)

- Crear archivo .gitignore
- Crear un commit y proveer un mensaje descriptivo
- Subir cambios.


# 6- Ramas

- Crear una nueva rama
- Cambiarse a esa rama
- Hacer un cambio en el archivo Readme.md y hacer commit
- Revisar la diferencia entre ramas
![Descripci�n de la imagen](imagen13.jpg)


# 7- Merges

- Hacer un merge FF
- Borrar la rama creada
- Ver el log de commits
![Descripci�n de la imagen](imagen14.jpg)

- Repetir el ejercicio 6 para poder hacer un merge con No-FF

![Descripci�n de la imagen](imagen15.jpg)
![Descripci�n de la imagen](imagen16.jpg)


# 8- Resoluci�n de Conflictos

- Instalar alguna herramienta de comparaci�n. Idealmente una 3-Way:
  -  P4Merge https://www.perforce.com/downloads/helix-visual-client-p4v:
  - Se puede omitir registraci�n. Instalar solo opci�n Merge and DiffTool.
- ByondCompare trial version https://www.scootersoftware.com/download.php
  - Configurar Tortoise/SourceTree para soportar esta herramienta.
  - https://www.scootersoftware.com/support.php?zz=kb_vcs
  - https://medium.com/@robinvanderknaap/using-p4merge-with-tortoisegit-8
7c1714eb5e2
- Crear una nueva rama conflictBranch
- Realizar una modificaci�n en la linea 1 del Readme.md desde main y commitear
- En la conflictBranch modificar la misma l�nea del Readme.md y commitear
![Descripci�n de la imagen](imagen17.jpg)

- Ver las diferencias con git difftool main conflictBranch
![Descripci�n de la imagen](imagen18.jpg)

- Cambiarse a la rama main e intentar mergear con la rama conflictBranch
- Resolver el conflicto con git mergetool
![Descripci�n de la imagen](imagen19.jpg)
![Descripci�n de la imagen](imagen20.jpg)
![Descripci�n de la imagen](imagen21.jpg)



- Agregar .orig al .gitignore
- Hacer commit y push
![Descripci�n de la imagen](imagen22.jpg)

# 9- Familiarizarse con el concepto de Pull Request

- Explicar que es un pull request.
- Crear un branch local y agregar cambios a dicho branch.
- Subir el cambio a dicho branch y crear un pull request.
![Descripci�n de la imagen](imagen23.jpg)

- Completar el proceso de revisi�n en github y mergear el PR al Branch master.

![Descripci�n de la imagen](imagen24.jpg)
![Descripci�n de la imagen](imagen25.jpg)
![Descripci�n de la imagen](imagen26.jpg)



# 10- Algunos ejercicios online

- Entrar a la p�gina https://learngitbranching.js.org/
- Completar los ejercicios Introduction Sequence
- Opcional - Completar el resto de los ejercicios para ser un experto en Git!!!

# 11- Crear Repositorio de la materia

- Crear un repositorio para la materia en github. Por ejemplo ing-software-3
- Subir archivo(s) .md con los resultados e im�genes de este trabajo pr�ctico. Puede ser en una subcarpeta trabajo-practico-01

# Referencias
- https://try.github.io/
- https://github.github.com/training-kit/downloads/es_ES/github-git-cheat-sheet.pdf
- https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet