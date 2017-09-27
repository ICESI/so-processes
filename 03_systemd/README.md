### Introducción a servicios en Linux
Universidad ICESI  
Curso: Sistemas Operativos  
Docente: Daniel Barragán C.  
Tema: Introducción a systemd
Correo: daniel.barragan at correo.icesi.edu.co   

### Objetivos
* Comprender la utilidad de la aplicación systemd

### Introducción
Una de las tareas que se presenta con mayor frecuencia en el uso de sistemas operativos tiene que ver con gestión de procesos. En esta guía se presenta un breve tutorial del uso de la herramienta systemd

### Desarrollo

#### Prerrequisitos
Crear un usuario de nombre operativos y password operativos. Si el usuario ya existe no deberá crearlo

```
# adduser operativos
# passwd operativos
# su operativos
```

Instalar el paquete numactl que provee la herramienta para stress de memoria ram llamada memhog
```
# yum install numactl -y
```

Instalar el paquete httpd
```
# yum install httpd -y
```

#### Systemd
Fue escrito por Lennart Poettering con el principal objetivo de unificar las configuraciones básicas de Linux y los comportamientos de servicios en todas las distribuciones. **systemd** es un paquete de software completo, el cual, además del demonio init systemd, incluye a los daemons journald, logind y networkd, junto a otros componentes de bajo nivel. **init systemd** reemplaza el sistema de inicio (init) heredado de los sistemas operativos estilo UNIX System V y Berkeley Software Distribution (BSD)

A continuación se presentan algunos comandos de uso común con systemd

| Comandos | Descripción |
|------|------|
| systemctl list-units --type service --all | Listar información de todos los servicios |
| systemctl list-units --type service | Listar información solo de servicios activos y en ejecución |
| ls /etc/systemd/system | Listar subcomponentes del servicio systemd |
| ls /run/systemd/system | Listar servicios que se inician asincronicamente |
| ls /usr/lib/systemd/system | Listar los unit files de los distintos servicios |
| systemctl status httpd  | Consultar el estado del servicio httpd |
| systemctl enable httpd | Habilitar el servicios httpd en el arranque del sistema operativo |
| systemctl start httpd | Iniciar el servicio httpd |
| systemctl list-units --type service --all | Listar todos los runlevels o targets |
| systemctl list-units --type target | Listar solo los runlevels o targets activos |
| systemctl get-default | Obtener el target por defecto para el sistema operativo |
| systemctl set-default multi-user.target | Cambiar el target por defecto del sistema operativo |

A continuación se presentan comandos de ejemplo de systemd para la interacción con CGroups

| Comandos | Descripción |
|------|------|
| systemd-cgls | Para obtener la jerarquía completa de los CGroups |
| systemctl kill httpd | Para detener todos los procesos asociados con httpd |
| systemd-cgtop | Obtener la lista de control ordenada por recurso |
| mkdir /etc/systemd/system/sshd.service.d | Para visualizar información del servicio sshd u otro servicion con cgtop |
| vi  /etc/systemd/system/sshd.service.d/accounting.conf | |
| systemctl daemon-reload | Después de un cambio en las configuraciones de systemd se debe reiniciar el demonio |
| systemctl restart sshd | Se reinicia el servicio sshd |

_accounting.conf_
```
[Service]
MemoryAccounting=true
```

#### Systemd Resource Controllers
A través de systemd varios recursos pueden ser restringidos:

| Resource controller | Description |
|------|------|
| CPUShares | Por defecto tiene un valor de 1024 |
| MemoryLimit | Se expresa en unidades de Megabytes o Gigabytes |
| BlockIOWeight | Se expresa con un valor entre 10 y 1000 |
| CPUQuota | Restringe la cpu a un porcentaje específico |

A continuación se presentan algunos comandos de ejemplo para la restricción de recursos por medio de CGroups y systemd

| Comandos | Descripción |
|------|------|
| **Ejemplo 1** | |
| systemctl set-property httpd CPUShares=500 | Poner limites a un servicio |
| systemctl daemon-reload | |
| ls /etc/systemd/system/httpd.service.d | |
| systemctl show -p CPUShares httpd | |
| systemctl show httpd | grep CPUShares | |
| **Ejemplo 2** | |
| systemctl set-property system.slice CPUShares=7168 | Allocate 70% of CPU to the system processes |
| systemctl set-property user.slice CPUShares=2048 | Allocate 20% of CPU to the user processes |
| systemctl set-property machine.slice CPUShares=1024 | Allocate 10% of CPU to the virtual machines |
| **Ejemplo 3** | |
| systemctl set-property user-1000.slice CPUQuota=20% | Restrict the user with the uid 1000 to use less than 20% of cpu |
| systemctl set-property user-1000.slice MemoryLimit=1024M | Reduce the memory available for the same user to 1GB |
| systemctl set-property mariadb.service BlockIOWriteBandwidth="/dev/vdb 2M" | Limit the mariadb service to write below 2MB/s onto the /dev/vdb partition |
| **Ejemplo 4** | |
| systemd-run --unit=toptest --slice=operativos.slice top -b | |
| systemctl status operativos.slice | |
| systemd-cgls | |
| systemctl stop toptest.service |  |
| systemctl status toptest.service | |
| systemctl start toptest.service | Al toptest ser un proceso transient, al detenerlo desaparece del sistema |
| systemctl start operativos.slice | |
| systemctl status operativos.slice | Aún así el proceso no se puede traer de vuelta |
| **Ejemplo 5** | |
| cp memhogtest.sh /home/operativos
| cp memhogtest.service /usr/lib/systemd/system | |
| systemctl daemon-reload | |
| systemctl status memhogtest.service | |
| systemctl enable memhogtest.service | |
| systemctl start memhogtest.service | |
| systemctl status memhogtest.service | |
| systemctl stop memhogtest.service | |
| systemctl status memhogtest.service | |
| systemctl set-property --runtime memhogtest.service MemoryLimit=1200M | |
| systemctl daemon-reload | |
| systemctl start memhogtest.service | |
| systemctl status memhogtest.service | Esperar un momento y consultar el estado |

_memhogtest.sh_
```
#!/bin/bash
while true; do memhog 2g; sleep 2; done
```

_memhogtest.service_
```
[Unit]
Description=Operativos Memhog test
After=network.target remote-fs.target nss-lookup.target

[Service]
Type=notify
ExecStart=/home/operativos/memhogtest.sh -DBACKGROUND
ExecStop=/bin/kill -WINCH ${MAINPID}
KillSignal=SIGTERM
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```

#### Systemd y servicios en el arranque

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

Para convertir el script count.sh en un servicio digite los comandos que se muestran a continuación
```
# cd /etc/systemd/system
# vim count.service
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
* Investigue acerca de la herramienta para crear contenedores virtuales llamada rocket, ¿Cuál es su relación con systemd?

### Referencias
https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/System_Administrators_Guide/sect-Managing_Services_with_systemd-Unit_Files.html
