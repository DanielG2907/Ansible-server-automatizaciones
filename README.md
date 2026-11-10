\# Automatización de Servidores con Ansible (IaC)



Este repositorio contiene playbooks y plantillas de Ansible diseñados para automatizar el despliegue, configuración y securización de infraestructura Linux. El enfoque principal es mantener la infraestructura como código, garantizando la idempotencia, reduciendo los tiempos de despliegue y eliminando la configuración manual por SSH.



\## Proyectos Incluidos


### 1. Fail2ban + MSMTP

[Ver proyecto y documentación](./Fail2ban-MSMTP/)

Automatiza la instalación y configuración de Fail2ban en servidores con Nginx u OpenResty. Incluye configuración de filtros y acciones personalizadas y una instalación opcional de MSMTP para el envío de notificaciones.

- Playbook: [Fail2Ban.yml](./Fail2ban-MSMTP/Fail2Ban.yml)
- Documentación específica: [README](./Fail2ban-MSMTP/README.md)

**Nota:** el playbook utiliza rutas y ficheros de configuración externos definidos mediante variables. Hay que revisar y adaptar esas rutas al entorno donde se ejecute.

### 2. Postfix como relay SMTP

[Ver proyecto y documentación](./Postfix/)

Automatiza la instalación y configuración de Postfix como relay SMTP. El playbook contempla las familias Debian y RedHat, genera la configuración mediante Jinja2 y configura la autenticación SASL.

- Playbook: [instalar_postfix.yml](./Postfix/instalar_postfix.yml)
- Inventario de ejemplo: [inventario.ini](./Postfix/inventario.ini)
- Plantilla principal: [main.cf.j2](./Postfix/main.cf.j2)
- Plantilla SASL: [sasl_passwd.j2](./Postfix/sasl_passwd.j2)
- Variables de ejemplo: [secretos.yml](./Postfix/secretos.yml)

**Gestión de credenciales:** `secretos.yml` contiene variables de ejemplo. No se deben almacenar credenciales reales en texto plano ni subirlas al repositorio. Para una ejecución real, cifra el fichero con Ansible Vault (`ansible-vault encrypt Postfix/secretos.yml`) o utiliza un gestor de secretos. Si ya se ha subido una contraseña real, rótala; borrar el fichero en un commit posterior no elimina su contenido del historial.

## Tecnologías

- **Automatización:** Ansible
- **Plantillas:** Jinja2
- **Sistemas:** Debian/Ubuntu y RedHat/Rocky Linux (según el playbook)
- **Servicios:** Fail2ban, MSMTP, Postfix, Nginx y OpenResty
- **Cloud / laboratorio:** Microsoft Azure

## Requisitos

- Ansible instalado en la máquina de control.
- Acceso SSH a los hosts del inventario.
- Privilegios de escalado (`become`) en los servidores de destino.
- Inventario y variables adaptados al entorno.
- Colecciones y paquetes requeridos por cada sistema, cuando corresponda.

## Validación antes de ejecutar

Desde la raíz del repositorio, valida la sintaxis de cada playbook:

```bash
ansible-playbook --syntax-check Fail2ban-MSMTP/Fail2Ban.yml
ansible-playbook --syntax-check Postfix/instalar_postfix.yml
