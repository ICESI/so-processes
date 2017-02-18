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

En algunas versiones de CentOS7 se requiere hacer lo siguiente en caso de que la conexión por ssh muestre el error: **Permission denied (publickey,gssapi-keyex,gssapi-with-mic).**

```
# vi /etc/ssh/sshd_config
PasswordAuthentication yes
sudo systemctl restart sshd
```
Para habilitar el uso multiusuario de screen debera cambiar los siguiente permisos

```
# chmod u+s /usr/bin/screen
# chmod 755 /var/run/screen
```

Para la realización de esta actividad se requieren dos maquinas virtuales de centos conectadas por una red privada o publica. Ambas máquinas virtuales deben tener screen instalado. Una de las máquinas virtuales debe tener dos usuarios creados: el usuario operativos_server y el usuario operativos_client

En la máquina A desde el usuario operativos_server ejecutar

```
screen -S terminal_remota
```
Dentro de la sesión de screen presionar **ctrl+a** y luego escribir los comandos

```
:multiuser on  
:acladd operativos_client  
:aclchg operativos -w "#"  
```

Desde la máquina B no importa en que usuario este activa su sesión, hacer una conexión ssh a la máquina A

```
$ ssh operativos_client@ip_maquina_a
```
Para acceder a la sesión activa debe ejecutar a continuación el comando

```
$ screen -x operativos_server/terminal_remota
```
Si llega a observar el error **/dev/pts/2 operation not permitted** ejecute el comando
```
$ script /dev/null
```

Recuerde restauralos los permisos de la máquina A cuando haya terminado por motivos de seguridad

```
# chmod u-s /usr/bin/screen
# chmod 775 /var/run/screen
```

#### Systemd y servicios en el arranque

Para convertir el script count.sh en un servicio digite los comandos que se muestran a continuación
```
# cd /etc/systemd/system
# vim count.service
[root@localhost system]# cat count.service 
[Unit]
Description = making network connection up
[Service]
ExecStart = /home/operativos/count.sh 
[Install]
WantedBy = default.target
# systemctl enable count.service
```

Los siguientes comandos permiten gestionar el servicio count.service. Verifique que una vez activado el servicio se puede observar el proceso count.sh en ejecución.
```
# systemctl start count.service
# ls multi-user.target.wants/
# ps -auxwww | grep count.sh
# systemctl stop count.service
# ls multi-user.target.wants/
# ps -auxwww | grep count.sh
```

Al realizar cambios en el archivo count.service debera recargar el servicio de systemctl
```
# systemctl daemon-reload
```

### Preguntas
* ¿Que información proporciona cada una de las columnas del comando ps-auxwww?
* ¿Porque es conveniente crear usuarios con perfiles o permisos de acuerdo con cada aplicación en un sistema Linux?
* ¿Cuál es la utilidad de los niveles de ejecución en Linux?
* (Opcional) Realice un script que inicie una máquina virtual en el arranque del sistema operativo, adicione comentarios explicando las líneas del script. Muestre evidencias del funcionamiento del script.

### Referencias
http://www.ee.surrey.ac.uk/Teaching/Unix/  
https://www.linux.com/learn/using-screen-remote-interaction  
http://stackoverflow.com/questions/9366707/is-running-gnu-screen-suid-root-the-only-way-to-make-multiuser-mode-work  
http://unix.stackexchange.com/questions/160523/how-can-i-start-a-screen-session-as-non-root-user  
https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/System_Administrators_Guide/sect-Managing_Services_with_systemd-Unit_Files.html

