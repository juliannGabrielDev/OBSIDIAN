---
aliases:
  - "🐘 MySQL"
tags:
  - linux
  - programas
breadcrumb:
  - "[[indice-linux|🐧 Índice Linux]]"
Fecha: 2025-06-17
---
# 🐘 MySQL: Instalación y Configuración en Fedora Linux 🐧
Guía completa para instalar y asegurar un servidor MySQL en Fedora Linux.
## 📥 1. Instalación de MySQL ⚙️
Primero, actualizamos los repositorios de `dnf` y luego instalamos el paquete del servidor de la comunidad de MySQL.
#### Paso 1.1: Añadir el repositorio de MySQL
Necesitas instalar el paquete de repositorio oficial de MySQL para que `dnf` sepa de dónde descargarlo.
1. Ve a la página oficial de descargas de MySQL Yum Repository: [https://dev.mysql.com/downloads/repo/yum/](https://dev.mysql.com/downloads/repo/yum/)
2. Descarga el paquete RPM para la versión más reciente de **Red Hat Enterprise Linux** (generalmente funciona bien con Fedora).
3. Una vez descargado, instálalo desde tu terminal. 
```bash
sudo dnf install ~/Descargas/mysql80-community-release-el9-*.noarch.rpm

sudo dnf install mysql-community-server
```
> [!NOTE] Repositorio Comunitario
> 
> 🗒️ Fedora incluye una pila de servidor de base de datos MySQL o MariaDB. El comando anterior instala el servidor de la comunidad de MySQL.
## 🚀 2. Iniciar y Habilitar el Servicio 🟢
Una vez instalado, es necesario iniciar el servicio de MySQL y habilitarlo para que se ejecute automáticamente al arrancar el sistema.
- **Iniciar el servicio:**
    ```bash
    sudo systemctl start mysqld
    ```
- **Deshabilitar el servicio:**
	```bash
	sudo systemctl stop mysqld
	```
- **Habilitar el servicio en el arranque:**
    ```bash
    sudo systemctl enable mysqld
    ```
    
- **Verificar el estado:**
    ```bash
    sudo systemctl status mysqld
    ```

## 🔐 3. Configuración Segura de MySQL 🛡️
MySQL incluye un script para realizar una configuración inicial de seguridad. Este script te guiará a través de la configuración de la contraseña de root, la eliminación de usuarios anónimos, la deshabilitación del inicio de sesión remoto de root, y la eliminación de la base de datos de prueba.

>[!IMPORTANT] Importante
>A diferencia de MariaDB, la instalación de MySQL de Oracle genera una contraseña temporal para el usuario `root`. Debes encontrarla para poder asegurarlo.
>
>`sudo grep 'temporary password' /var/log/mysqld.log`

Ejecuta el siguiente comando para iniciar el asistente de seguridad:
```bash
sudo mysql_secure_installation
```

> [!WARNING] Contraseña de Root
> 
> ⚠️ Durante este proceso, se te pedirá que crees una contraseña para el usuario root de MySQL. Asegúrate de elegir una contraseña fuerte y segura.

### 📋 Asistente de Configuración

| **Pregunta**                               | **Recomendación 💡** | **Descripción**                                                                                        |
| ------------------------------------------ | -------------------- | ------------------------------------------------------------------------------------------------------ |
| **VALIDATE PASSWORD component?**           | **Sí**               | Instala un componente para validar la fortaleza de las contraseñas.                                    |
| **Change the password for root?**          | **Sí**               | Establece o cambia la contraseña para el superusuario `root` de MySQL.                                 |
| **Remove anonymous users?**                | **Sí**               | Elimina los usuarios anónimos que pueden conectarse sin una cuenta de usuario.                         |
| **Disallow root login remotely?**          | **Sí**               | Impide que el usuario `root` pueda conectarse al servidor de base de datos desde una ubicación remota. |
| **Remove test database and access to it?** | **Sí**               | Elimina la base de datos de prueba `test` y los privilegios asociados a ella.                          |
| **Reload privilege tables now?**           | **Sí**               | Recarga las tablas de privilegios para aplicar todos los cambios realizados.                           |

## 🖥️ 4. Conexión al Servidor MySQL 🧑‍💻
Una vez completada la configuración de seguridad, puedes conectarte a tu servidor de MySQL usando el cliente de línea de comandos.
```bash
mysql -u root -p
```
Se te solicitará la contraseña que estableciste durante el script `mysql_secure_installation`.
> [!SUCCESS] ¡Instalación Completa!
> 
> ✅ ¡Felicidades! Has instalado y configurado exitosamente un servidor MySQL en tu sistema Fedora Linux.

