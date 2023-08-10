# Parte 1: Gestión de Usuarios
## 1. Creación de Usuarios
### Crea tres usuarios llamados `usuario1`, `usuario2` y `usuario3`.

```bash
oescobar@debian-pc:~$ sudo useradd -m usuario1
[sudo] password for oescobar:
oescobar@debian-pc:~$

oescobar@debian-pc:~$ sudo useradd -m usuario2
oescobar@debian-pc:~$

oescobar@debian-pc:~$ sudo useradd -m usuario3
oescobar@debian-pc:~$

oescobar@debian-pc:~$ sudo usermod --shell /bin/bash usuario1
oescobar@debian-pc:~$

oescobar@debian-pc:~$ sudo usermod --shell /bin/bash usuario2
oescobar@debian-pc:~$

oescobar@debian-pc:~$ sudo usermod --shell /bin/bash usuario3
oescobar@debian-pc:~$
```

## 2. Asignación de Contraseñas
### Establece una nueva contraseñas para cada usuario creado.

```bash
oescobar@debian-pc:~$ sudo passwd usuario1
New password:
Retype new password:
passwd: password updated successfully
oescobar@debian-pc:~$

oescobar@debian-pc:~$ sudo passwd usuario2
New password:
Retype new password:
passwd: password updated successfully
oescobar@debian-pc:~$

oescobar@debian-pc:~$ sudo passwd usuario3
New password:
Retype new password:
passwd: password updated successfully
oescobar@debian-pc:~$
```

## 3. Información de Usuarios
### Muestra la información de `usuario1` usando el comando `id`.

```bash
oescobar@debian-pc:~$ id usuario1
uid=1001(usuario1) gid=1001(usuario1) groups=1001(usuario1)
oescobar@debian-pc:~$
```

## 4. Eliminación de Usuarios
### Elimina `usuario3`, pero conserva su directorio principal.

```bash
oescobar@debian-pc:~$ sudo deluser usuario3
info: Removing crontab ...
info: Removing user `usuario3' ...
oescobar@debian-pc:~$
```

# Parte 2: Gestión de Grupos
## 1. Creación de Grupos
### Crea dos grupos llamados `grupo1` y `grupo2`.

```bash
oescobar@debian-pc:~$ sudo groupadd grupo1
oescobar@debian-pc:~$

oescobar@debian-pc:~$ sudo groupadd grupo2
oescobar@debian-pc:~$
```

## 2. Agregar Usuarios a Grupos
### Agrega `usuario1` a `grupo1` y `usuario2` a `grupo2`.

```bash
oescobar@debian-pc:~$ sudo usermod -a -G grupo1 usuario1
oescobar@debian-pc:~$

oescobar@debian-pc:~$ sudo usermod -a -G grupo2 usuario2
oescobar@debian-pc:~$
```

## 3. Verificar Membresía
### Verifica que los usuarios han sido agregados a los grupos utilizando el comando `groups`.

```bash
oescobar@debian-pc:~$ groups usuario1
usuario1 : usuario1 grupo1
oescobar@debian-pc:~$

oescobar@debian-pc:~$ groups usuario2
usuario2 : usuario2 grupo2
oescobar@debian-pc:~$
```

## 4. Eliminar Grupo
### Elimina `grupo2`.

```bash
oescobar@debian-pc:~$ sudo groupdel grupo2
oescobar@debian-pc:~$

oescobar@debian-pc:~$ groups usuario2
usuario2 : usuario2
oescobar@debian-pc:~$
```

# Parte 3: Gestión de Permisos
## 1. Creación de Archivos y Directorios
### Como `usuario1`, crea un archivo llamado `archivo1.txt` en su directorio principal y escribe algo en él.

```bash
oescobar@debian-pc:~$ su - usuario1
Password:
usuario1@debian-pc:~$

usuario1@debian-pc:~$ echo "Hola, soy el usuario1" > archivo1.txt
usuario1@debian-pc:~$ cat archivo1.txt
Hola, soy el usuario1
usuario1@debian-pc:~$
```

### Crea un directorio llamado `directorio1` y dentro de ese directorio, un archivo llamado `archivo2.txt`.

```bash
usuario1@debian-pc:~$ mkdir directorio1
usuario1@debian-pc:~$ cd directorio1/
usuario1@debian-pc:~/directorio1$ touch archivo2.txt
usuario1@debian-pc:~/directorio1$
```

## 2. Verificar Permisos
### Verifica los permisos del archivo y directorio usando el comando `ls -l` y `ls -ld` respectivamente.

```bash
usuario1@debian-pc:~/directorio1$ ls -l
total 0
-rw-r--r-- 1 usuario1 usuario1 0 Aug  9 18:06 archivo2.txt
usuario1@debian-pc:~/directorio1$

usuario1@debian-pc:~/directorio1$ ls -ld
drwxr-xr-x 2 usuario1 usuario1 4096 Aug  9 18:06 .
usuario1@debian-pc:~/directorio1$
```

## 3. Modificar Permisos usando `chmod` con Modo Numérico
### Cambia los permisos del `archivo1.txt` para que sólo `usuario1` pueda leer y escribir (permisos `rw-`), el grupo pueda leer (permisos `r--`) y nadie más pueda hacer nada.

```bash
usuario1@debian-pc:~/directorio1$ cd ~
usuario1@debian-pc:~$ chmod 640 archivo1.txt
usuario1@debian-pc:~$
```

## 4. Modificar Permisos usando `chmod` con Modo Simbólico
### Agrega permiso de ejecución al propietario del `archivo2.txt`.

```bash
usuario1@debian-pc:~$ cd directorio1/
usuario1@debian-pc:~/directorio1$ chmod u+x archivo2.txt
usuario1@debian-pc:~/directorio1$
```

## 5. Cambiar el Grupo Propietario
### Cambia el grupo propietario de `archivo2.txt` a `grupo1`.

```bash
usuario1@debian-pc:~/directorio1$ chgrp grupo1 archivo2.txt
usuario1@debian-pc:~/directorio1$
```

## 6. Configurar Permisos de Directorio
### Cambia los permisos del `directorio1` para que sólo el propietario pueda entrar (permisos `rwx`), el grupo pueda listar contenidos pero no entrar (permisos `r--`), y otros no puedan hacer nada.

```bash
usuario1@debian-pc:~$ cd ~
usuario1@debian-pc:~$ chmod 740 directorio1/
usuario1@debian-pc:~$
```

## 7. Comprobación de Acceso
### Intenta acceder al `archivo1.txt` y `directorio1/archivo2.txt` como `usuario2`. Nota cómo el permiso de directorio afecta el acceso a los archivos dentro de él.

```bash
usuario1@debian-pc:~$ exit
logout
oescobar@debian-pc:~$ su - usuario2
Password:
usuario2@debian-pc:~$ cd ..
usuario2@debian-pc:/home$ cd usuario1/
usuario2@debian-pc:/home/usuario1$ cat archivo1.txt
cat: archivo1.txt: Permission denied
usuario2@debian-pc:/home/usuario1$ ls -l directorio1/
ls: cannot open directory 'directorio1/': Permission denied
usuario2@debian-pc:/home/usuario1$ cd directorio1/
-bash: cd: directorio1/: Permission denied
usuario2@debian-pc:/home/usuario1$
```

## 8. Verificación Final
### Verifica los permisos y propietario de los archivos y directorio nuevamente con `ls -l` y `ls -ld`.

```bash
usuario2@debian-pc:/home/usuario1$ exit
logout
oescobar@debian-pc:~$ su - usuario1
Password:
usuario1@debian-pc:~$ ls -l
total 8
-rw-r----- 1 usuario1 usuario1   22 Aug  9 18:03 archivo1.txt
drwxr----- 2 usuario1 usuario1 4096 Aug  9 18:06 directorio1
usuario1@debian-pc:~$ ls -ld
drwxr-xr-x 3 usuario1 usuario1 4096 Aug  9 18:17 .
usuario1@debian-pc:~$ cd directorio1/
usuario1@debian-pc:~/directorio1$ ls -l
total 0
-rwxr--r-- 1 usuario1 grupo1 0 Aug  9 18:06 archivo2.txt
usuario1@debian-pc:~/directorio1$
```

# Reflexión
### ¿Por qué es importante gestionar correctamente los usuarios y permisos en un sistema operativo?

Porque de esta forma existe una segmentación de responsabilidades y de seguridad que de otra forma no existiría, se previene que un usuario cualquiera pueda leer o modificar archivos que simplemente no deberían de leer o modificar, e incluso que no puedan ejecutar scripts o programas que no deban de ejecutar y que pueda generar cambios irreversibles o de escucha con funcionalidad de red en el sistema.

### ¿Qué otros comandos o técnicas conocen para gestionar permisos en Linux?

El ya mencionado en clase keycloak con LDAP, y para maquinas en la nube tal vez no sean de gestión de permisos pero de acceso a la red (firewall): iptables (nftables), ipset, ufw, firewalld, XDP con eBPF, grupos de seguridad en AWS + managed prefix lists y los roles de AWS para restringir el acceso al SDK a productos específicos de AWS
