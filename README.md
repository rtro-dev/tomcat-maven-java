# Despliegue de una aplicación Java usando Tomcat y Maven

<br>

## Contenidos
- [Configuración inicial de la máquina](#configuración-inicial-de-la-máquina)  
        - [](#)  

<br>

## Configuración inicial de la máquina

Generar el Vagrantfile:  
`vagrant init debian/bullseye64`

Modificación del Vagrantfile:
```ruby
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.define "practica" do |p|
    p.vm.box = "debian/bullseye64"
    p.vm.hostname = "practica"
    p.vm.network "forwarded_port", guest: 8080, host: 8080
    p.vm.network "private_network", ip: "192.168.10.10"
  end # practica
end # cofig
```

Levantamiento de la máquina:  
`vagrant up`

Conectar a la máquina por ssh:  
`vagrant ssh`

## OpenJDK

Actualización del repositorio:  
`sudo apt update`

Instalación de OpenJDK:  
`sudo apt install -y openjdk-11-jdk`

## Tomcat

Instalación:  
`sudo apt install -y tomcat9`

Creación de un grupo de usuarios para tomcat9:  
`sudo groupadd tomcat9`

Creación del usuario para el servicio:  
`sudo useradd -s /bin/false -g tomcat9 -d /etc/tomcat9 tomcat9`  
Se impide que el usuario inicie sesión en el sistema por seguridad *(-s /bin/false)*, se especifica el grupo *(-g tomcat9)*, define el directorio de inicio *(-d /etc/tomcat9)* y se le asigna un nombre *(tomcat9)*.

Arrancar el servicio:  
`sudo systemctl start tomcat9`

Comprobación del servicio:  
`sudo systemctl status tomcat9`  
Devuelve:
```bash
tomcat9.service - Apache Tomcat 9 Web Application Server
     Loaded: loaded (/lib/systemd/system/tomcat9.service; enabled; vendor preset: enabled)
     Active: active (running) since Mon 2025-01-20 11:30:54 UTC; 1min 35s ago
       Docs: https://tomcat.apache.org/tomcat-9.0-doc/index.html
    Process: 5832 ExecStartPre=/usr/libexec/tomcat9/tomcat-update-policy.sh (code=exited, status=0/SUCCESS)
   Main PID: 5836 (java)
      Tasks: 29 (limit: 510)
     Memory: 75.7M
        CPU: 12.143s
     CGroup: /system.slice/tomcat9.service
             └─5836 /usr/lib/jvm/java-11-openjdk-amd64/bin/java -Djava.util.logging.config.file=/var/lib/tomcat9/conf/logging.properties -Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager -Djava.awt.headless=t>

Jan 20 11:30:56 practica tomcat9[5836]: OpenSSL successfully initialized [OpenSSL 1.1.1w  11 Sep 2023]
Jan 20 11:30:58 practica tomcat9[5836]: Initializing ProtocolHandler ["http-nio-8080"]
Jan 20 11:30:58 practica tomcat9[5836]: Server initialization in [2718] milliseconds
```