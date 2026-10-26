Documentación de Despliegue de Infraestructura mediante Ansible

Este documento detalla el procedimiento técnico para automatizar la instalación y configuración de servicios LAMP, FTP, SFTP, Samba y PostgreSQL en openSUSE.


1. Parámetros de Configuración
* Usuario de sistema y servicios: leo
* Contraseña global: mundolibre
* Base de datos PostgreSQL: aso_db

2. Preparación del Nodo Destino (Servidor)
Ejecutar con privilegios de administrador para preparar la recepción de comandos automatizados.
Instalar dependencias y ajustar servicios base:
zypper install python3
systemctl disable --now firewalld
systemctl enable --now sshd

Habilitar autenticación remota en /etc/ssh/sshd_config:
PermitRootLogin yes
PasswordAuthentication yes

Aplicar configuración SSH:
systemctl restart sshd


3. Preparación del Nodo Controlador (Master)
Ejecutar en la máquina encargada de la orquestación.
Instalar paquetes requeridos y generar clave SSH:
zypper install ansible git-core
ssh-keygen -t rsa -b 4096

Transferir la clave SSH al nodo destino:
ssh-copy-id root@<IP_NODO_DESTINO>

Obtener el código de infraestructura y definir el inventario:
git clone https://github.com/leonardoguarachi/aso-ansible-grupo11.git
cd aso-ansible-grupo11

*Nota: Editar el archivo hosts e insertar únicamente la dirección IP del nodo destino.*

4. Ejecución del Despliegue
Iniciar la orquestación de servicios:
ansible-playbook deploy.yml

5. Validación de Servicios (Desde el Nodo Controlador)
Crear archivos temporales para las pruebas de transferencia:
echo "Reporte" > documento.txt
touch diagrama.png

Conectividad de Ansible:
ansible all -m ping


Pila LAMP:
* HTTP: Acceder a http://<IP_NODO_DESTINO>
* phpMyAdmin: Acceder a http://<IP_NODO_DESTINO>/phpmyadmin

PostgreSQL (Conexión SSL Remota):
psql -h <IP_NODO_DESTINO> -U leo -d aso_db

FTP (Transferencia estándar):
ftp <IP_NODO_DESTINO>
*Autenticar y ejecutar put documento.txt.*

SFTP (Transferencia segura SSH):
sftp root@<IP_NODO_DESTINO>
*Autenticar y ejecutar put diagrama.png.*

Samba (Recurso compartido):
smbclient //<IP_NODO_DESTINO>/Recursos -U leo
*Autenticar y ejecutar put documento.txt.*

6. Auditoría Interna (Desde el Nodo Destino)
Validar el estado de los puertos de red:
ss -tulpn | grep -E "21|22|80|139|445|3306|5432"
Validar el estado de los demonios del sistema:

systemctl is-active apache2 mariadb postgresql vsftpd smb nmb sshd
