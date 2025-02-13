# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.define "practica" do |p|
    p.vm.box = "debian/bullseye64"
    p.vm.hostname = "practica"
    p.vm.network "forwarded_port", guest: 8080, host: 8080
    p.vm.network "private_network", ip: "192.168.10.10"
    p.vm.provision "shell", name: "openjdk",inline: <<-SHELL
      apt update
      apt install -y openjdk-11-jdk
    SHELL
    p.vm.provision "shell", name: "tomcat", inline: <<-SHELL
      apt install -y tomcat9
      groupadd tomcat9
      useradd -s /bin/false -g tomcat9 -d /etc/tomcat9 tomcat9
      systemctl start tomcat9
      cp -v /vagrant/tomcat-users.xml /etc/tomcat9/
      apt install -y tomcat9-admin
      cp -v /vagrant/context.xml /usr/share/tomcat9-admin/host-manager/META-INF/
      systemctl restart tomcat9
    SHELL
    p.vm.provision "shell", name: "maven", inline: <<-SHELL
      apt-get update && sudo apt-get -y install maven
      cp -v /vagrant/tomcat-users.xml /etc/tomcat9/
      cp -v /vagrant/settings.xml /etc/maven/
      cd /home/vagrant
      mvn archetype:generate -DgroupId=org.zaidinvergeles \
                        -DartifactId=test-app \
                        -DarchetypeArtifactId=maven-archetype-webapp \
                        -DinteractiveMode=false
      rm /home/vagrant/test-app/pom.xml
      cp -v /vagrant/pom1.xml /home/vagrant/test-app/
      mv /home/vagrant/test-app/pom1.xml /home/vagrant/test-app/pom.xml
      cd /home/vagrant/test-app
      mvn tomcat7:deploy
    SHELL
    p.vm.provision "shell", name: "appgit", inline: <<-SHELL
      apt install -y git
      cd /home/vagrant
      git clone https://github.com/cameronmcnz/rock-paper-scissors.git
      cd rock-paper-scissors
      git checkout patch-1
      rm pom.xml
      cp -v /vagrant/pom2.xml /home/vagrant/rock-paper-scissors/
      mv /home/vagrant/rock-paper-scissors/pom2.xml /home/vagrant/rock-paper-scissors/pom.xml
      mvn tomcat7:deploy
    SHELL
  end # practica
end # cofig