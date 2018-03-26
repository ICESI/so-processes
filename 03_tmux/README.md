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
Ejecute el comando tmux
```
$ tmux
```

En la consola que se abre digite el comando para ejecutar el script
```
$ ./count.sh
```
Presione **ctrl+b** y luego la tecla **c** para abrir una nueva consola. Digite nuevamente el comando para ejecutar el script  
```
$ ./count.sh
```

**Nota:** Podrá ir de una consola a otra presionando **ctrl+b** y luego la tecla **n** ó la tecla **p**.

Todas las consolas que crea con **ctrl+b** + **c** pertenecen a una misma sesión. tmux tiene soporte para múltiples sesiones y múltiples consolas por sesión.

Para crear nuevos paneles en una consola presione **ctrl+b** y luego la tecla **%** ó la tecla **"**.

Para navegar por los paneles presione **ctrl+b** y las teclas de dirección.

Ṕara visualizar los números de los paneles presione **ctrl+b** y la tecla **q**

Para intercambiar el orden de los paneles presione **ctrl+b** y la tecla **o**

Para cerrar un panel presione **ctrl+b** y la tecla **x**.

Presione **ctrl+b** y luego la tecla **d** para desligarse de la sesión creada, las consolas permanecerán ejecutandose en background.

Presione el siguiente comando para ingresar de nueva a la sesión
```
$ tmux list-sessions
0: 1 windows (created Sun Mar 25 22:26:57 2018) [162x47]
$ tmux attach-session -t 0
```
Presione **ctrl+b** y luego la tecla **d** para desligarse nuevamente de la sesión

Presione el siguiente comando para abrir una nueva sesión de screen
```
$ tmux new-session -s nueva_sesion
```
Presione **ctrl+a** y luego la tecla **d** para desligarse de la sesión

Presione el siguiente comando para observar las sesiones activas
```
$ tmux list-sessions
0: 1 windows (created Sun Mar 25 22:26:57 2018) [162x47]
nueva_sesion: 1 windows (created Sun Mar 25 22:40:21 2018) [162x47]
```

Ingrese a la sesión 0 y cree dos consolas nuevas

Renombre la sesión 0 presionando **ctrl+b** y luego la tecla **$**.

Renombre la primera consola de la sesión 0 presionando **ctrl+b** y luego la tecla **,**.

Elimíne la primera consola de la sesión 0 presionando **ctrl+b** y luego la tecla **&** . Confirme la eliminación presionando **y** . Al eliminar una consola se cierran tambien los paneles abiertos.

Liste las sesiones presionando **ctrl+b** y la tecla **s**. Seleccione la sesión llamada nueva sesión y elimínela.

### Preguntas
* Realice un cuadro comparativo entre la herramienta screen y la herramienta tmux

### Referencias
* https://tmuxcheatsheet.com/
* https://gist.github.com/henrik/1967800
