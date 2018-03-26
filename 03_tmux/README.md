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

Puede ajustar las configuraciones de tmux como se muestra a continuación. Algunas personas personalizan tmux con los atajos de vi para no tener que memorizar nuevos comandos. Para que el siguiente cambio tenga efecto debe abandonar la sesión de tmux una vez creado el archivo .tmux.conf e ingresar nuevamente

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
```

Ejecute el comando tmux
```
$ tmux
```

Nota: tmux tiene soporte para múltiples sesiones y múltiples consolas por sesión como se mostrará a continuación

#### Comandos

| Comando   | Usuario | Descripción   |
|------|------|------|
| --- | operativos | Los siguientes comandos se ejecutan como el usuario operativos |
| $ ./count.sh | | Ejecute el script de conteo |
| **ctrl+a** + **c**| | Abra una nueva consola |
| | | Ejecute nuevamente el script de conteo |
| | | Abra una nueva consola |
| | | Ejecute nuevamente el script de conteo |
| **ctrl+a** + **n** | | Va a la consola siguiente |
| **ctrl+a** + **p** | | Va a la consola anterior |
| | | Tenga en cuenta hasta este punto que las consolas creadas pertenecen a una misma sesión |
| **ctrl+a** + **%** | | Dividir la pantalla verticalmente |
| **ctrl+a** + **"** | | Dividir la pantalla horizontalmente |
| **ctrl+a** + **arrow keys** | | Navegar por los paneles, use las flechas de dirección |
| **ctrl+a** + **q** | | Visualizar los numeros de identificación de cada panel |
| **ctrl+a** + **o** | | Navegar por los paneles secuencialmente |
| **ctrl+a** + **x** | | Cerrar uno de los paneles |
| **ctrl+a** + **d** | | Desligarse de la sesión creada, las consolas se continuan ejecutando en background |
| tmux list-sessions | | Lista las sesiones de tmux |
| tmux attach-session -t 0 | | Se enlaza con la sesión 0 |
| | | Desliguese de la sesión |
| $ tmux new-session -s operativos | | Cree una sesión llamada operativos |
| | | Desliguese de la sesión |
| | | Liste las sesiones de tmux |
| | | Ingrese en la sesión con identificador 0 |
| | | Cree dos consolas nuevas |
| **ctrl+a** + **$** | | Renombre la sesión 0 a distribuidos|
| **ctrl+a** + **,** | | Renombre las consolas como workshop1 y workshop2 respectivamente |
| **ctrl+a** + **&** | | Elimine una de las consolas, debe confirmar la eliminación presionando la tecla **y**.  Al eliminar una consola se cierran tambien los paneles abiertos. |
| **ctrl+a** + **s** | | Lista las sesiones dentro de tmux. Permite seleccionar una sesión e ingresar a ella |
| | | Desliguese de la sesión |
| | | Cierre su conexión SSH e ingrese nuevamente |
| tmux kill-session -t operativos | | Elimine la sesión de operativos |
| | | Elimine la sesión de distribuidos |

### Preguntas
* Realice un cuadro comparativo entre la herramienta screen y la herramienta tmux

### Referencias
* https://tmuxcheatsheet.com/
* https://gist.github.com/henrik/1967800
* https://gist.github.com/MohamedAlaa/2961058
* https://gist.github.com/tsl0922/d79fc1f8097dde660b34
