PROYECTO ANSIBLE

1. ENLACE A GITHUB
Repositorio: https://github.com/leonardoguarachi/aso-ansible-grupo11.git
Nota: El proyecto ya fue compartido y se envió la invitación al correo gcussi@yahoo.com.

2. REQUISITOS PREVIOS DEL ENTORNO
Para garantizar la ejecución exitosa del código, la infraestructura virtual debe cumplir con:

Máquina Master:
- Sistema Operativo: openSUSE16.
- Paquetes obligatorios instalados: ansible, git-core.
- Par de llaves SSH generadas localmente (ssh-keygen).
- Archivo ansible.cfg configurado para Pipelining (mejora de tiempo de respuesta).

Máquina Clon (Nodo Administrado / Servidor):
- Sistema Operativo: openSUSE 16.0 (Instalación limpia).
- Paquetes obligatorios instalados: Python 3 (Ansible lo requiere para inyectar módulos).
- Servicio SSH activo.
- Acceso SSH sin contraseña configurado: La llave pública de la Master debe estar copiada en la Clon (ssh-copy-id root@IP_CLON).

3. PASOS PARA EJECUTAR EL DESPLIEGUE
Paso 1: Clonar el repositorio en la Máquina Master e ingresar a la carpeta raíz del proyecto.
Paso 2: Abrir el archivo de inventario llamado "hosts" y actualizar la IP por la dirección actual de la Máquina Clon.
Paso 3: Ejecutar el Playbook maestro con el siguiente comando:
        ansible-playbook deploy.yml

4. RESUMEN DE SERVICIOS AUTOMATIZADOS
- lamp: Instala y configura Apache2, MariaDB, PHP 8 y phpMyAdmin (con permisos de acceso remoto).
- samba_ftp: Despliega vsftpd y Samba, creando usuarios de sistema/red y aplicando directivas "create mask = 0664".
- postgres: Instala PostgreSQL, configura la base de datos para la app, genera certificados y habilita conexiones remotas encriptadas por SSL.
