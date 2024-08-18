¿Que son **Git** y **Github**?
[link](https://www.aluracursos.com/blog/git-y-github-que-son-y-primeros-pasos).

Video para [iniacirse](https://www.youtube.com/watch?v=-LmFK6skG7s) en git.
## Controla y comparte tu código

Curso [Git y Github](https://app.aluracursos.com/course/git-github-control-version).
### Que es git
Control de versiones, trabajo en equipo. En servidor o local.

Sistemas de control de versiones:

- CVS
- SVN
- Mercurial
- Git

Un sistema de control de versiones registra cada cambio realizado en un historial  que se puede revisar y restaurar, en cualquier punto de este.

Permite organizar el trabajo en equipo, manteniendo los cambios de los archivos en un servidor dedicado.

Apuntes [Git](https://gitea.kickto.net/devfzn/basicos_git/wiki/).

Iniciando un [repositorio](https://www.aluracursos.com/blog/iniciando-repositorio-con-git)

- `gitinit` inicia repositorio.
- `git status` estado del repositorio.
- `git add <file>` agrega el archivo al sistema de control de versiones.
- `git add .` agrega todos los archivos al sistema de control de versiones.
- `git commit` crea un punto en el historial de cambios, `-m` para el mensaje.
- `git config`, `--local`, `--global`, `--help`.

Iniciar un repositorio

```sh
git init
git config --local user.name <git-username>
git config --local user.email <git-user-email>
git config --local core.sshcommand '-i ~/.ssh/<key> -F /dev/null'
```

> ..."Al ejecutar el comando git status, recibimos información que puede no ser
tan clara, especialmente cuando nos encontramos con términos como HEAD, working
tree, index, etc.
>  
>Solo para aclarar un poco, ya que entenderemos mejor cómo funciona Git durante
el curso, aquí hay algunas definiciones interesantes:
>
> - **HEAD**: Estado actual de nuestro código, es decir, donde nos colocó Git
> - **Working tree**: Lugar donde los archivos realmente están siendo almacenados
> - **index**: Lugar donde Git almacena lo que será commiteado, es decir,
la ubicación entre el working tree y el repositorio de Git en sí.
>
> Además de eso, los posibles estados de nuestros archivos son explicados con
detalle en este
[link](https://git-scm.com/book/es/v2/Fundamentos-de-Git-Guardando-cambios-en-el-Repositorio)"...

[Git log](https://gitea.kickto.net/devfzn/basicos_git/wiki/Comandos-Basicos-Git#user-content-log)

- `git log`

    ```sh
    commit 7eb3ca2b356ab72fcb6d3cdd378f6e4ea86f7fcc (HEAD -> master, origin/master)
    Author: 
    Date:   Thu Apr 13 21:31:07 2023 -0400

        HTML5 y CSS3 pt. 4: Avanzando en CSS 06
        
        Diseño responsivo - fin del curso.

    commit bb393c6e7349af7da65806aa1743545734228227
    Author:
    Date:   Thu Apr 13 18:59:51 2023 -0400

        HTML5 y CSS3 pt. 4: Avanzando en CSS 05
        
        Uso de: opacity, box-shadow, text-shadow.
        Selección de colores RGB y RGBA.

    commit 17d1a78e5f9f8012826b0bc68785c410018601c4
    Author: 
    Date:   Thu Apr 13 15:52:25 2023 -0400

        HTML5 y CSS3 pt. 4: Avanzando en CSS 04
        
        Selectores avanzados css.
        Cálculos con css y medidas proporcionales.

    commit e28f9798705280a2a2e37a9f5556edd1815b31d5
    Author: 
    Date:   Thu Apr 13 14:22:10 2023 -0400

        HTML5 y CSS3 pt. 4: Avanzando en CSS 03
        
        Mejora de semantica con nuevos divisores 'div', uso de mas pseudo-clases css,
        background degaradado, uso de mas pseudo-elementos.

    commit edfb89ab04aa43227cafb07f796eeb6cec53c53c
    Author:
    Date:   Thu Apr 13 13:05:29 2023 -0400

        HTML5 y CSS3 pt. 4: Avanzando en CSS 02
        
        Uso de fuente externa google font; map & video embedded.

    commit ee5b469838f175e2f16e100a381861b800bad4c3
    Author: 
    Date:   Wed Apr 12 22:33:32 2023 -0400

        HTML5 y CSS3 pt. 4: Avanzando en CSS 01
        
        Ajustes de página para usar estilo 'style.css'.
        Medidas proporcionales CSS con 'em'.
        propiedad 'float' y 'clear'.
    ```

- git-[log](https://devhints.io/git-log) config.
- archivo `.gitignore`: los archivos/directorios especificados en este archivo son excluidos del control de veriones.
- `git remote`, repositorios remotos
    - `git remote add <origin>`: Configura repositorio remoto como origen.
    - `git remote remove <origin>`: Elimina el vinculo con el repositorio remoto.
    - `git remote -v`: Muestra El/los repositorios remotos.

- `git clone`, clonar repos
    - `git clone <repo>`: Clona repositorio remoto en el directorio actual.
    - `git clone <repo>` proyecto: Clona repositorio remoto en directorio `proyecto`.
    - `git clone <repo>` .: Clona repositorio remoto en el directorio actual sin
    crear otro subdirectorio para el proyecto.

- `git push`: Envia los cambios al repostorio remoto.
    - `git push <origin> <master>`: Envia los cambios de la rama `master`
    repostorio remoto `origin`.

- `git fetch <origin>`: Actualiza la información local respecto a la remota,
informa los cambios, pero no atualiza el working tree.
- `git push <origin> <master>`: Actualiza la copia local según los datos del servidor.
- `git pull <origin> <master>`: Actualiza la copia remota según los datos locales.

#### Servicios de repositorios online

- [GitLab](https://gitlab.com), Disponible online y self-host.
- [Gitea](https://gitea.io), Solo Self-host.
- [GitHub](https://github.com), Solo online.
- [Git](https://git-scm.com/docs/gitweb) Web, solo Self-host.

> - GitHub new Token [Restriction](https://www.aluracursos.com/blog/exigendia-autenticacion-por-token).
> - Acceso [ssh](https://www.aluracursos.com/blog/ssh-acceso-remoto-a-servidores) a maquinas.

- Ramas, `git branch`
    - `git branch`: muestra el nombre de rama actual.
    - `git branch -a`: muestra las ramas existentes.
    - `git branch <rama>`: crea la ramas <rama>.
- `git checkout <rama>`: cambia a la rama <rama>.
- `git checkout -b <rama>`: crea y cambia a la rama <rama>.
> Las branches ("ramas") se utilizan para desarrollar funcionalidades aisladas entre sí. La branch master es la branch "predeterminada" cuando creas un repositorio.
> Nota: [Licencias](https://www.aluracursos.com/blog/open-source-una-breve-introduccion)
de software, FreeSofware y Open Source.

- `git merge <rama>`: Fusiona rama actual con `rama`. ej.
`git checkout master; git merge rama`.
- `git rebase <rama>`: Sobreesbribe la rama actual sobre `rama`. ej.
`git checkout master; git rebase rama`.
- Editar manualmente cuando falla el `merge`. Git marca la zona del conflicto.

> Siempre actualizar repositorio local antes de hacer push.

- `git restore <archivo>`: restaurar el archivo al ultimo estado guardado.
- `git restore --staged <archivo>`: saca el archivo de los cambios a incluir en
el próximo commit (staging area).
- `git reverse <commit-id>`: crea un nuevo commit que deshace el anterior.
- `git stash`: Guarda el trabajo en progreso, no crea commit.
- `git stash list`: Lista los trabajos guardados.
- `git stash apply <n>`: Aplica los cambios del trabajo guardado nro. `n`
(n queda en la lista de stach).
- `git stash pop`: Aplica los cambios del último trabajo guardado, y lo elimina
de la lista.
- `git checkout <commit_id>`: Revisar commits anteriores.
- `git diff`: Muestra las modificaciones que no han sido agregadas para el
proximo commit (staging area).
- `git diff <commit_id>`: Muestra las diferencias entre el commit actual y `commit_id`.

#### Tag

Punto en el historial, al cual se le asigna una etiqueta

Mas info en repo
[basicos_git](https://gitea.kickto.net/devfzn/basicos_git/wiki/Administracion-de-Proyecto#tags).

```sh
git tag -a v0.3 -m "Release 0.3"
    git push origin "Release 0.3"
```

Ejemplos de [tags](https://gitea.kickto.net/SyDeVoS/Caldera-ino/tags).
