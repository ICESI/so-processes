### Introducción a servicios en Linux
Universidad ICESI  
Curso: Sistemas Operativos  
Docente: Daniel Barragán C.  
Tema: Introducción a Procesos en Linux  
Correo: daniel.barragan at correo.icesi.edu.co   

### Objetivos
* Comprender la utilidad de algunos comandos en Linux.
* Implementar scripts que agrupen comandos y parámetros de entrada para la realización de tareas. 

### Introducción
Una de las tareas que se presenta con mayor frecuencia en la configuración de sistemas operativos tiene que ver con la detención y reanudación de procesos. Un proceso puede ser instalado por medio del gestor de paquetes o pueden ser creados por el desarrollador. En esta guía se presentan algunos comandos útiles relacionados con los procesos.

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
#### Ejecutando un proceso
Digite los siguientes comandos para asignar permisos de ejecución 

```
$ chmod +x count.sh
```

#### Cancelando la ejecución de un proceso
Ejecutar el script

```
$ ./count.sh
```

Cancelar la ejecución del script presione la combinación de teclas **ctrl+c**

#### Deteniendo y reanudando un proceso
Ejecutar el script. Detener la ejecución del script presione la combinación de teclas **ctrl+z**.
Digite el siguiente comando para reanudar la ejecución del script

```
$ fg
```

#### Reanundando una lista de procesos detenidos
Ejecute el script dos veces y para cada ejecución presione **ctrl+z**. Ejecute el siguiente 
comando para observar el listado de procesos detenidos

```
$ jobs
```
Podrá continuar la ejecución de alguno de los procesos por medio del comando **fg** y el identificador de la salida del comando jobs

#### Proceso en background
Ejecutar el script como se indica a continuación

```
$ ./count.sh &
```
Al presionar la combinación de teclas **ctrl+c** el proceso no se detiene debido a que no se ejecuta en el primer plano. Para traer el proceso al primer plano y cancelarlo digite el comando **fg** seguido de la combinación de teclas **ctrl+c** 

#### Obtener el identificador de un proceso
Ejecutar el script y detenerlo con la combinación de teclas **ctrl+c**. Ejecute el siguiente comando para observar el listado de los procesos

```
$ ps -auxwww | grep script.sh
```
Podra observar en la salida del comando la segunda columna que corresponde al identificador del proceso

#### Deteniendo un proceso por medio de su identificador
A partir del identificador de un proceso es posible cancelar su ejecución. Suponiendo que el identificador de un proceso es el numéro 123 para cancelar la ejecución del proceso puede ejecutar el siguiente comando

```
$ kill -9 123
```

#### Aplicativo Screen
Ejecute el siguiente comando como el usuario root para realizar la instalación del programa screen: 
```
# sudo yum install screen -y 
```
Active la sesión del usuario operativos
```
# su operativos
```
Ejecute el comando screen 
```
$ screen
```
En la consola que se abre digite el comando para ejecutar el script 
```
$ ./count.sh 
```
Presione **ctrl+a** y luego la tecla **c** para abrir una nueva consola. Digite nuevamente el comando para ejecutar el script  
```
$ ./count.sh 
```
Presione **ctrl+a** y luego la tecla **d** para desligarse de las consolas creadas  

Presione el siguiente comando para retomar el control de las consolas
```
$ screen -r 
```
Presione **ctrl+a** y luego la tecla **d** para desligarse nuevamente de las consolas creadas  

Presione el siguiente comando para abrir una nueva sesión de screen
```
$ screen -S nueva_sesion
```
Presione **ctrl+a** y luego la tecla **d** para desligarse nuevamente de las consolas creadas  

Presione el siguiente comando para observar las sesiones activas
```
$ screen -r 
```

#### Aplicativo Screen y Pantalla remota

#### Systemd y servicios en el arranque


### Preguntas
* ¿Que información proporciona cada una de las columnas del comando ps-auxwww?
* ¿Cuál es la utilidad de los niveles de ejecución en Linux?, Realice un script que inicie una máquina virtual en el arranque del sistema operativo, adicione comentarios explicando las líneas del script. Muestre evidencias del funcionamiento del script.
* Investigue acerca de la utilidad del comando visudo. Presente ejemplos y capturas de pantalla. 
* ¿Porque es conveniente crear usuarios con perfiles o permisos de acuerdo con cada aplicación en un sistema Linux?

### Referencias
http://www.ee.surrey.ac.uk/Teaching/Unix/
