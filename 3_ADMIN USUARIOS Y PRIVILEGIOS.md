# Administración de Usuarios y Procesos en Linux

##  Comandos de Superusuario

* `su` ➡️ Permite cambiar a la cuenta de `root` (u otro usuario si se especifica).
* `sudo` ➡️ Permite ejecutar un comando individual con privilegios de `root` sin cambiar de sesión.

---

##  Archivos de Configuración de Usuarios y Grupos

| Archivo | Descripción |
| :--- | :--- |
| `/etc/passwd` | Contiene la información básica de las cuentas de usuario. |
| `/etc/shadow` | Almacena la información segura de las cuentas de usuario (contraseñas cifradas y expiración). |
| `/etc/group` | Contiene la información de los grupos del sistema. |
| `/etc/gshadow` | Almacena la información segura y contraseñas de los grupos. |

---

##  Estructura del Archivo 

![Texto alternativo](imagenes/1.png)
![Texto alternativo](imagenes/2.png)
![Texto alternativo](imagenes/3.png)
![Texto alternativo](imagenes/4.png)
![Texto alternativo](imagenes/5.png)


# AGREGAR USUARIOS NUEVOS 

    1)Crear la entrada en /etc/passwd
    2)Crear la entrada en /etc/shadow
    3)Crear la entrada (si correponde) en /etc/group
    4)Crear la entrada (si correponde) en /etc/gshadow
    5)Crear el directorio personal (fuente de /etx/skel)
    6)Cambiar los permisos al directorio personal
    7)Cambiar la contrasena cifrada
    
# CREACIÓN Y MODIFICACIÓN DE USUARIOS Y GRUPOS EN LINUX
======================================================

##  Creación de Usuarios: Alto Nivel vs. Bajo Nivel
-----------------------------------------------------

En Linux existen dos comandos para crear usuarios. La diferencia clave es qué tan automático es el proceso:

* **Alto Nivel (`adduser`):** Es un script interactivo más amigable. Te pregunta la contraseña, el nombre completo y **crea automáticamente** el directorio `/home` y copia los archivos de configuración por defecto.
* **Bajo Nivel (`useradd`):** Es el comando binario nativo del sistema. Es directo y "silencioso". Si no le pasas los parámetros manualmente, creará al usuario **sin contraseña, sin carpeta personal (/home) y sin una Shell interactiva válida**.

---

##  Comandos Prácticos de Creación (Paso a Paso)
--------------------------------------------------

### Paso 1: Crear un Grupo
Permite registrar un nuevo grupo en el sistema (`/etc/group`).
* **Sintaxis:** `sudo groupadd [nombre_grupo]`
* **Ejemplo:** `sudo groupadd juancito`

### Paso 2: Crear un Usuario (Bajo Nivel con Parámetros)
Cuando usas `useradd`, debes especificar manualmente los detalles con opciones (banderas):
* **Sintaxis:** `sudo useradd -m -u [UID] -g [GrupoPrincipal] -G [GruposSecundarios] -s [Shell] -d [RutaHome] -c "[Descripción]" [NombreUsuario]`
* **Ejemplo:** ```bash
    sudo useradd -m -u 1100 -g 1004 -G audio,video -s /bin/bash -d /home/juancito -c "cuenta del usuario juancito" juancito
    ```

> 📌 **Acordeón de Banderas (`useradd`):**
> * `-m` ➡️ Crea automáticamente el directorio personal (`/home/usuario`).
> * `-u` ➡️ Asigna un ID de usuario personalizado (`UID`).
> * `-g` ➡️ Define el **Grupo Principal** (debe existir previamente, usando su ID o nombre).
> * `-G` ➡️ Define **Grupos Secundarios** adicionales (separados por comas y sin espacios).
> * `-s` ➡️ Define la Shell por defecto (Ej: `/bin/bash` para que sea interactiva).
> * `-d` ➡️ Define una ruta personalizada para el directorio Home.
> * `-c` ➡️ Añade un comentario o descripción (campo GECOS).

### Paso 3: Setear o Cambiar la Contraseña
Un usuario recién creado con `useradd` está bloqueado hasta que le asignes una clave.
* **Sintaxis:** `sudo passwd [nombreUsuario]`
* **Ejemplo:** `sudo passwd juancito`

---

##  Verificación de Identidad y Acceso
----------------------------------------

* **Cambiar de usuario:** Permite ingresar a la sesión del usuario para comprobar si puede acceder correctamente.
    ```bash
    su [nombreUsuario]
    ```
* **¿Quién soy?:** Muestra el nombre del usuario activo en la terminal actual.
    ```bash
    `whoami`
    ```
* **Listar IDs numéricos:** Muestra las carpetas en `/home` visualizando los IDs numéricos (`UID` y `GID`) de los dueños en lugar de sus nombres.
    ```bash
    ls -ln /home/
    ```

---

##  Modificación de Usuarios (`usermod`)
-------------------------------------------

El comando `usermod` permite editar los valores de una cuenta que ya existe.

* **Modificar Shell y Descripción:**
    ```bash
    sudo usermod -s /bin/sh -c "info adicional del usuario" juancito
    ```
* **Reemplazar Grupos Secundarios:** Asigna al usuario **únicamente** a los grupos listados (elimina los secundarios anteriores que tuviera).
    ```bash
    sudo usermod -G audio,video pedrito
    ```
* **Verificar Grupos de un Usuario:** Filtra el archivo `/etc/group` para auditar a qué grupos pertenece.
    ```bash
    grep pedrito /etc/group
    ```
* **Añadir a un nuevo grupo sin borrar los anteriores:** Si usas `-G` a secas, borras los otros grupos del usuario. Para **añadir** manteniendo los anteriores, debes usar obligatoriamente la bandera `-a` (append).
    * **Sintaxis:** `sudo usermod -a -G [grupos] [usuario]`
    * **Ejemplo:** `sudo usermod -a -G audio,video,sshusers pedrito`

---

##  Administración de Grupos con `gpasswd`
--------------------------------------------

El comando `gpasswd` es una alternativa más directa y limpia para añadir o remover usuarios de un grupo específico sin tocar el resto de sus configuraciones.

* **Añadir un usuario a un grupo (`-a`):**
    * **Sintaxis:** `sudo gpasswd -a [nombreUsuario] [nombreGrupo]`
    * **Ejemplo:** `sudo gpasswd -a juancito sshusers`
* **Eliminar un usuario de un grupo (`-d`):**
    * **Sintaxis:** `sudo gpasswd -d [nombreUsuario] [nombreGrupo]`
    * **Ejemplo:** `sudo gpasswd -d juancito sshusers`
    
===========================================
# /etc/shadow

juan:$6$902vABSb$8AShXXX:16969:0:99999:7:::
 1           2             3   4   5    6 789

1: UserName
2: Password
3: Ultimo cambio de password
4: Edad mínima de la password
5: Edad máxima de la password
6: Periodo de advertencia
7: Periodo de inactividad
8: Fecha de expiración
9: Reservado para uso futuro

* passwd -n | --mindays dias
* passwd -x | --maxdays dias
* passwd -w | --warndays dias
* passwd -i | --inactive dias
* usermod -e | --expiredate yyyy-mm-dd
* passwd -l | -u usuario
* passwd -S usuario

# Administración y Modificación de Grupos en Linux

* Crear un nuevo grupo
`sudo groupadd` nuevos_usuarios
* Verificar si el grupo existe en el archivo /etc/group
`grep nuevos /etc/group`
* Añadir al usuario "pedrito" al grupo secundario "nuevos_usuarios"
`sudo gpasswd -a pedrito nuevos_usuarios`
* Mostrar el UID, GID y todos los grupos del usuario "pedrito"
`id pedrito`
* Volver a verificar los miembros del grupo (Corrección de: greep -> grep)
`grep nuevos /etc/group`
* Cambiar el grupo primario del usuario "pedrito" al grupo "nuevos_usuarios"
`sudo usermod pedrito -g nuevos_usuarios`
* Eliminar el grupo "nuevos_usuarios" del sistema
`sudo groupdel nuevos_usuarios`

  <Notas Rápidas:>
    `id [usuario]` ➡️ Sirve para revisar al instante si se aplicaron bien los cambios de grupo.
    `usermod -g` ➡️ Cambia el grupo primario (el principal del usuario)
    `gpasswd -a` ➡️ Añade al usuario a un grupo secundario (manteniendo los que ya tenía). 


