# 🐧 GUÍA COMPLETA DE ADMINISTRACIÓN DE LINUX: COMANDOS, PIPES Y REDIRECCIONES

---

## 1. Gestión de Paquetes (`aptitude` & `apt`)

### A. Herramienta `aptitude` (Gestor Avanzado)
* `man aptitude` ➡️ Abre el manual detallado de la herramienta.
* `sudo apt-get install aptitude` ➡️ Instala aptitude si no viene por defecto.
* `sudo aptitude search calculador` ➡️ Busca paquetes relacionados con la palabra "calculador".
* `sudo aptitude show gnome-calculator` ➡️ Muestra la información detallada del paquete.
* `sudo aptitude install gnome-calculator` ➡️ Instala el paquete seleccionado.
* `sudo aptitude remove gnome-calculator` ➡️ Elimina el paquete manteniendo sus archivos de configuración.
* `sudo aptitude purge gnome-calculator` ➡️ Elimina el paquete **y limpia** por completo sus configuraciones.
* `sudo aptitude update` ➡️ Actualiza los repositorios (la lista de lo que se puede instalar).
* `sudo aptitude safe-upgrade` ➡️ Actualiza el sistema de forma segura (**no** desinstala paquetes).
* `sudo aptitude full-upgrade` ➡️ Actualiza el sistema completamente (**sí** desinstala o resuelve conflictos si es necesario).

### B. Herramienta `apt` (Sintaxis Estándar)
> **Sintaxis Básica:** `apt <comando> [argumentos]`

* `sudo apt autoremove` ➡️ Borra automáticamente dependencias e historiales huérfanos que ya ningún programa usa.
* `sudo apt update` ➡️ Actualiza el listado de paquetes disponibles en los repositorios.
* `sudo apt upgrade` ➡️ Actualiza los programas del sistema (**no** desinstala paquetes existentes).
* `sudo apt full-upgrade` ➡️ Actualiza todo el sistema de forma agresiva (puede desinstalar paquetes antiguos si es requerido).

---

## 2. Terminal, Rutas y Manipulación de Archivos

### A. Ubicación y Listado
* `pwd` ➡️ Muestra la ubicación actual en la que estamos parados.
* `pwd -P` ➡️ Muestra la **ubicación física real**, resolviendo y omitiendo enlaces simbólicos (accesos directos).
* `cd -` ➡️ Cambia al directorio anterior en el que estuviste trabajando.
* `cd` ➡️ Regresa instantáneamente al directorio raíz de tu usuario (`/home/usuario` o `~`).
* `.` *(Un punto)* ➡️ Representa al directorio actual. (Ej: `./script.sh`).
* `..` *(Dos puntos)* ➡️ Representa al directorio padre (un nivel arriba). (Ej: `cd ..`).
* `ls` ➡️ Lista archivos y carpetas visibles.
* `ls -l` ➡️ Listado largo o extendido (muestra permisos, dueños y tamaños).
* `ls -lh` ➡️ Formato humano (`h`), convierte los tamaños a Kb, Mb o Gb.
* `ls -lht` ➡️ Ordena el listado por tiempo (`t`), mostrando lo más reciente primero.
* `ls -lhtr` ➡️ Invierte el orden (`r`), ideal para ver los archivos modificados al final de la pantalla.
* `ls -R` ➡️ Listado recursivo (muestra carpetas y el contenido de las subcarpetas).

### B. Tamaños y Espacios
* `du -h /tmp/*` ➡️ Muestra el tamaño de cada archivo y carpeta dentro de `/tmp`.
* `du -sh /tmp/` ➡️ Resume (`s`) el tamaño total acumulado de la carpeta `/tmp`.
* `df -h` ➡️ Muestra el espacio libre y ocupado de los discos y particiones montadas.

### C. Historial y Directorios
* `history` ➡️ Despliega el registro de comandos ejecutados anteriormente por el usuario.
* `!!` ➡️ Vuelve a ejecutar de inmediato el último comando introducido.
* `mkdir [nombre]` ➡️ Crea un directorio vacío.
* `mkdir -p /tmp/dirA/dirB` ➡️ Crea estructuras anidadas padres (`-p`). Si `dirA` no existe, lo crea primero en el camino.

### D. Eliminación de Archivos y Carpetas
* `rm [archivo]` ➡️ Elimina solo archivos individuales.
* `rmdir [carpeta]` ➡️ Elimina únicamente directorios vacíos.
* `rm -r [carpeta]` ➡️ Borra de forma recursiva (`-r`) carpetas con todo su contenido dentro.
* `rm -ri [carpeta]` ➡️ Modo interactivo (`-i`), pregunta confirmación archivo por archivo antes de borrar.
* `rm -rf [carpeta]` ➡️ Fuerza absoluta (`-f`). Borra recursivamente sin preguntar nada. **¡Usar con extremo cuidado!**

### E. Mover, Copiar y Tiempos
* `mv -i archivo.txt /destino/` ➡️ Mueve e interrumpe (`-i`) si ya existe un archivo homónimo en el destino.
* `mv archivo*.txt /destino/` ➡️ Mueve todos los archivos que inicien con la cadena `archivo` y terminen en `.txt`.
* `mv *.txt /destino/` ➡️ Mueve absolutamente todos los archivos con extensión `.txt`.
* `mv anterior.txt nuevo.txt` ➡️ Renombra un archivo si se apunta dentro del mismo directorio.
* `mv -u /origen/* /destino/` ➡️ Actualiza (`-u`). Solo mueve si el archivo es nuevo o si es una versión más reciente que la del destino.
* `mv -uv /origen/* /destino/` ➡️ Agrega verbose (`-v`) para listar en pantalla qué se está moviendo y qué se saltó.
* `cp archivo.txt /destino/nuevo_nombre.txt` ➡️ Copia renombrando el archivo final en el destino.
* `cp -r dir1/ backup/` ➡️ Copia carpetas de forma recursiva (`-r`).
* `cp -i archivo.txt /destino/` ➡️ Pregunta antes de sobrescribir un archivo existente en el destino.
* `cp fileA fileB fileC /destino/` ➡️ Copia múltiples archivos en un solo comando hacia la ruta final.
* `cp -a /origen/ /destino/` ➡️ Copia de archivo de archivo (`-a`). Mantiene intactos permisos, dueños, enlaces y marcas de tiempo originales.
* `touch archivo.txt` ➡️ Crea un archivo vacío instantáneo.
* `touch -a archivo.txt` ➡️ Modifica exclusivamente la fecha y hora del último acceso.
* `touch -t 202603150830 archivo.txt` ➡️ Configura una fecha personalizada fija de forma explícita (`YYYYMMDDHHMM`).
* `ls -l archivo.txt` ➡️ Muestra la última fecha de **modificación** (escritura).
* `ls -lu archivo.txt` ➡️ Muestra la última fecha de **acceso** (lectura con cat, less, etc.).

---

## 3. Encadenamiento de Comandos (`Pipes` | )

* `ls -l /tmp/ | more` ➡️ Paginación simple de salida línea por línea.
* `ls -l /tmp/ | less` ➡️ Paginador interactivo avanzado (permite subir y bajar con las flechas).
* `ls -l /tmp/ | awk '{print $5"\t"$9}'` ➡️ Filtra la salida del listado aislando la columna 5 (Tamaño) y columna 9 (Nombre).
* `ls -l /tmp/ | awk '{print $5"\t"$9}' | grep systemd` ➡️ Aplica un segundo filtro para mostrar solo líneas que contengan "systemd".
* `ip a | grep inet | awk '{print $2}' | cut -d'/' -f1` ➡️ Cadena avanzada: extrae tarjetas de red, extrae segmentos de IP y corta (`-f1`) usando el delimitador diagonal (`-d'/'`) para aislar tu IP limpia.

---

## 4. Redirecciones: Entrada, Salida y Errores (FD)

> En Linux existen 3 descriptores de archivos estándar (File Descriptors):
> * **0** ➡️ `STDIN` (Entrada estándar / Teclado)
> * **1** ➡️ `STDOUT` (Salida estándar / Pantalla)
> * **2** ➡️ `STDERR` (Salida de errores / Pantalla)

* `ps fax` ➡️ Muestra un mapa de árbol de absolutamente todos los procesos vivos del sistema.
* `/proc/` ➡️ Directorio virtual del sistema que contiene los descriptores de archivos vivos en tiempo real (Ej: `/proc/6045/fd/`).
* `ip a > /tmp/red.txt` ➡️ Redirecciona la salida estándar (`>`). Sobrescribe el archivo con la información de red.
* `ls /ruta_falsa 2> /tmp/errores.txt` ➡️ Captura solo los errores (`2>`) y los almacena en el archivo especificado.
* `ls /home 1> /tmp/salida.txt 2> /tmp/errores.txt` ➡️ Envía lo que salga bien a `salida` y lo que falle al archivo `errores`.
* `ls /home/ 1> /tmp/salida.txt 2>&1` ➡️ Fusiona los canales. Envía tanto los aciertos como los errores al mismo archivo (`salida.txt`).

### Redirección Acumulada y Entrada Avanzada
* `echo "----------" >> /tmp/salida.txt` ➡️ Acumula (`>>`). Agrega el texto al final del archivo sin borrar lo anterior.
* `cat << FIN > /tmp/archivo.txt` ➡️ Redirección de entrada (`<<`). Permite escribir un texto multilínea en la terminal y guarda el archivo cuando escribes la palabra clave `FIN`.

---

## 5. Inspección de Archivos de Texto y Búsquedas

### A. Visualización Rápida
* `head -n5 listado.txt` ➡️ Muestra únicamente las primeras 5 líneas superiores del archivo.
* `tail -n5 listado.txt` ➡️ Muestra las últimas 5 líneas finales del archivo.
* `tail -f /var/log/syslog` ➡️ Modo interactivo en vivo (`-f`). Muestra logs en tiempo real a medida que el sistema los añade.
* `wc listado.txt` ➡️ Cuenta e indica estadísticas (líneas, palabras y bytes del archivo).

### B. Ubicación de Comandos y Archivos
* `which ls` ➡️ Localiza el archivo ejecutable binario exacto que corre al escribir `ls`.
* `whereis ls` ➡️ Localiza el binario, el código fuente y las páginas de manual (`man`) asociadas al comando.
* `find . -name 'arc*'` ➡️ Busca en la carpeta actual archivos cuyo nombre empiece con "arc".
* `find /tmp/ -name '*.tsv'` ➡️ Busca archivos con extensión tsv en la carpeta temporal.
* `find /tmp/ -type d` ➡️ Filtra búsquedas para encontrar únicamente directorios (`-type d`).
* `find /tmp/ -type f -size +2048k` ➡️ Busca archivos regulares (`-type f`) mayores a 2 Megabytes (`+2048k`).
* `find /tmp/ -iname 'arc*' -exec echo "Encontrado: {}" \;` ➡️ Busca sin discriminar mayúsculas (`-iname`) y ejecuta un comando directo por cada coincidencia hallada.

### C. Filtrado Avanzado (`grep`)
* `grep root /etc/passwd` ➡️ Busca y extrae las líneas completas donde aparezca la palabra "root".
* `grep -c -i -v 'root' listado.txt` ➡️ Modificadores potentes: Cuenta las líneas (`-c`), ignora mayúsculas (`-i`), e invierte la selección (`-v`) buscando todo lo que **no** contenga la palabra.

---

## 6. Introducción Práctica a `awk`

* `cat /etc/passwd | awk -F':' '{print $1}'` ➡️ Usa como delimitador el carácter de dos puntos (`-F':'`) y extrae la columna 1 (los nombres de usuarios del sistema).
* `echo "hola mundo, que tal" | awk '{print $NF}'` ➡️ La variable especial `$NF` extrae automáticamente el último elemento de la cadena entregada (en este caso imprimirá la palabra "tal")..
