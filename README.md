###
Pasos instalar Jenkins en Ubuntu EC2

* sudo apt update
* sudo apt install openjdk-8-jdk
* wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
* sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
* sudo apt update
* sudo apt install jenkins
* sudo systemctl start jenkins
* sudo systemctl enable jenkins
* sudo systemctl status jenkins
* abrir puerto 8080 en security group de la instancia HTTP
* ec2-mi-ip.compute-1.amazonaws.com:8080
* sudo cat /var/lib/jenkins/secrets/initialAdminPassword