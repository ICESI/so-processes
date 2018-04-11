### Introducción a servicios en Linux
Universidad ICESI  
Curso: Sistemas Operativos  
Docente: Daniel Barragán C.  
Tema: Introducción al uso de screen
Correo: daniel.barragan at correo.icesi.edu.co   

### Objetivos
* Comprender la utilidad de la aplicación screen

### Introducción
Una de las tareas que se presenta con mayor frecuencia en el uso de sistemas operativos tiene que ver con la ejecución de procesos en background. En esta guía se presenta un breve tutorial del uso del aplicativo screen.

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

#### Aplicativo screen
Ejecute el siguiente comando como el usuario root para realizar la instalación del programa screen:
```
# sudo yum install screen -y
```
Active la sesión del usuario operativos
```
# su operativos
```
Ejecute el comando screen para crear una sesion llamada icesi_sesion
```
$ screen -S icesi_sesion
```
En la consola que se abre digite el comando para ejecutar el script
```
$ ./count.sh
```
Presione **ctrl+a** y luego la tecla **c** para abrir una nueva consola. Digite nuevamente el comando para ejecutar el script  
```
$ ./count.sh
```

**Nota:** Podrá ir de una consola a otra presionando **ctrl+a** y luego la tecla **n** ó la tecla **p**

Todas las consolas que crea con **ctrl+a** + **c** pertenecen a una misma sesión. screen tiene soporte para múltiples sesiones y múltiples consolas por sesión.

Presione **ctrl+a** y luego la tecla **d** para desligarse de la sesión creada  

Presione el siguiente comando para crear una nueva sesión llamada icesi_new_sesion
```
$ screen -S icesi_new_sesion
```

Presione **ctrl+a** y luego la tecla **d** para desligarse de la sesión creada 

Presione el siguiente comando para listar las sesiones creadas
```
$ screen -r
```

Presione el siguiente comando para ingresar a una de las sesiones creadas
```
$ screen -r PID (identificador de la sesión)
```

Presione **ctrl+a** y luego la tecla **d** para desligarse de la sesión

**Nota**: Recuerde que puede ingresar a alguna de las sesiones escribiendo el commando **screen -r** seguido del identificador de 4 dígitos

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

Para la realización de esta actividad se requieren dos maquinas virtuales de centos conectadas por una red privada o publica. Ambas máquinas virtuales deben tener screen instalado. Una de las máquinas virtuales debe tener dos usuarios creados: el usuario operativos_server y el usuario operativos_client, a esta máquina la llamaremos **máquina A**  

En la máquina A desde el usuario operativos_server ejecutar

```
screen -S terminal_remota
```
Dentro de la sesión de screen presionar **ctrl+a** y luego escribir los comandos

```
:multiuser on  
:acladd operativos_client  
:aclchg operativos_client -w "#"  
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

Recuerde restaurar los los permisos de la máquina A cuando haya terminado por motivos de seguridad

```
# chmod u-s /usr/bin/screen
# chmod 775 /var/run/screen
```

### Preguntas
* Investigue como aumentar el buffer de screen
* Instale la herramienta byobu y practique algunos comandos

### Referencias
https://www.linux.com/learn/using-screen-remote-interaction  
http://stackoverflow.com/questions/9366707/is-running-gnu-screen-suid-root-the-only-way-to-make-multiuser-mode-work  
http://unix.stackexchange.com/questions/160523/how-can-i-start-a-screen-session-as-non-root-user  
http://byobu.co/
