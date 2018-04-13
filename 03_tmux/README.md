### Introducción a servicios en Linux
Universidad ICESI  
Curso: Sistemas Operativos  
Docente: Daniel Barragán C.  
Tema: Introducción al uso de tmux  
Correo: daniel.barragan at correo.icesi.edu.co   

### Objetivos
* Comprender la utilidad de la aplicación tmux
* Comparar herramientas de Linux que cumplen el mismo proposito y seleccionar la más adecuada para una tarea

### Introducción
Una de las tareas que se presenta con mayor frecuencia en el uso de sistemas operativos tiene que ver con la ejecución de procesos en background. En esta guía se presenta un breve tutorial del uso del aplicativo tmux.

### Desarrollo

#### Prerrequisitos
Crear un usuario de nombre operativos y password operativos. Si el usuario ya existe no deberá crearlo

```
# adduser operativos
# passwd operativos
# su operativos
```

#### Script de pruebas
Crear el siguiente script por medio del editor vim con el nombre count.sh

```
$ cd ~/
$ vim count.sh
```
```
#!/bin/bash
i=0;
while true
do
   echo "$i"
   let i=i+1;
   sleep 1;
done
```

Digite el siguiente comando para asignar permisos de ejecución
```
$ chmod +x count.sh
```

#### Aplicativo tmux
Ejecute el siguiente comando como el usuario root para realizar la instalación del programa tmux:
```
# sudo yum install tmux -y
```
Active la sesión del usuario operativos
```
# su operativos
```

#### Comandos

| Comando   | Usuario | Descripción   |
|------|------|------|
| --- | operativos | Los siguientes comandos se ejecutan como el usuario operativos |
| $ tmux | | Ejecuta el aplicativo tmux |
| $ ./count.sh | | Ejecute el script de conteo |
| **ctrl+b** + **c**| | Abra una nueva consola |
| | | Ejecute nuevamente el script de conteo |
| | | Abra una nueva consola |
| | | Ejecute nuevamente el script de conteo |
| **ctrl+b** + **n** | | Va a la consola siguiente |
| **ctrl+b** + **p** | | Va a la consola anterior |
| | | Tenga en cuenta hasta este punto que las consolas creadas pertenecen a una misma sesión |
| **ctrl+b** + **%** | | Dividir la pantalla verticalmente |
| **ctrl+b** + **"** | | Dividir la pantalla horizontalmente |
| **ctrl+b** + **arrow keys** | | Navegar por los paneles, use las flechas de dirección |
| **ctrl+b** + **q** | | Visualizar los numeros de identificación de cada panel |
| **ctrl+b** + **o** | | Navegar por los paneles secuencialmente |
| **ctrl+b** + **z** | | Expandir panel / Contraer panel |
| **ctrl+b** + **t** | | Muestra un reloj, presione q para salir |
| **ctrl+b** + **x** | | Cerrar uno de los paneles |
| **ctrl+b** + **d** | | Desligarse de la sesión creada, las consolas se continuan ejecutando en background |
| tmux list-sessions | | Lista las sesiones de tmux |
| tmux attach-session -t 0 | | Se enlaza con la sesión 0, puede emplear este comando tambien para compartir sesiones de forma remota |
| | | Desliguese de la sesión |
| $ tmux new-session -s operativos | | Cree una sesión llamada operativos |
| | | Desliguese de la sesión |
| | | Liste las sesiones de tmux |
| | | Ingrese en la sesión con identificador 0 |
| | | Cree dos consolas nuevas |
| **ctrl+b** + **$** | | Renombre la sesión 0 a distribuidos|
| **ctrl+b** + **,** | | Renombre las consolas como workshop1 y workshop2 respectivamente |
| **ctrl+b** + **&** | | Elimine una de las consolas, debe confirmar la eliminación presionando la tecla **y**.  Al eliminar una consola se cierran tambien los paneles abiertos. |
| **ctrl+b** + **s** | | Lista las sesiones dentro de tmux. Permite seleccionar una sesión e ingresar a ella |
| **ctrl+b** + **?** | | Despliega los atajos de tmux, presione q para salir |
| | | Desliguese de la sesión |
| | | Cierre su conexión SSH e ingrese nuevamente |
| tmux kill-session -t operativos | | Elimine la sesión de operativos |
| | | Elimine la sesión de distribuidos |

Al escribir tmux seguido de una letra, se visualizarán los posibles argumentos de tmux que comienzan por dicha letra
```
$ tmux l
ambiguous command: l, could be: last-pane, last-window, link-window, list-buffers, list-clients, list-commands, list-keys, list-panes, list-sessions, list-windows, load-buffer, lock-client, lock-server, lock-session
```

Nota: Hemos visto con las instrucciones anteriores que tmux tiene soporte para múltiples sesiones y múltiples consolas por sesión.  

#### Configuración personalizada

Puede ajustar configuraciones como la edición de la salida de comandos con vi ó la combinación de teclas para ejecutar una acción a través de un archivo de configuración llamado .tmux.conf. Algunas personas personalizan tmux con los atajos de vi para no tener que memorizar nuevos comandos. 

```
$ vi ~/.tmux.conf
# use C-a, since it's on the home row and easier to hit than C-b
set-option -g prefix C-a
unbind-key C-a
bind-key C-a send-prefix
set -g base-index 1

# Easy config reload
bind-key R source-file ~/.tmux.conf \; display-message "tmux.conf reloaded."

# vi is good
setw -g mode-keys vi

# Setup 'v' to begin selection as in Vim
bind-key -Tcopy-mode-vi v send -X begin-selection
```

Para que la configuración tenga efecto debe salir de tmux e ingresar nuevamente. La nueva configuración le permitirá hacer lo siguiente:

- Cambiar la combinación de teclas **ctrl+a** por **ctrl+b** 
- Aplicar las configuraciones realizadas en el archivo .tmux.conf sin reiniciar tmux
- Seleccionar y copiar texto de la salida estandar

#### Configuración personalizada - Modo Copia (vi)

| Comando   | Usuario | Descripción   |
|------|------|------|
| $ ls /bin | | Liste los archivos del directorio /bin |
| **ctrl+a** + **[** | | Entre en modo vi |
| **space key** | | Seleccione el nombre de algun archivo de la salida del listado |
| **enter** | | Copie la selección |
| **q** | | Salga del modo vi |
| **ctrl+a** + **]** | | Pegue la selección |

### Preguntas
* Realice un cuadro comparativo entre la herramienta screen y la herramienta tmux
* Investigue como pegar la selección del modo copia en algún editor de texto como vi o nano, tenga en cuenta que esto requiere cambios en el .tmux.conf para enviar el texto copiado al portapapeles

### Referencias
* https://tmuxcheatsheet.com/
* https://github.com/tmux/tmux/blob/master/CHANGES#L235
* https://gist.github.com/henrik/1967800
* https://gist.github.com/MohamedAlaa/2961058
* https://gist.github.com/tsl0922/d79fc1f8097dde660b34
* https://superuser.com/questions/196060/selecting-text-in-tmux-copy-mode
* https://www.howtoforge.com/sharing-terminal-sessions-with-tmux-and-screen
