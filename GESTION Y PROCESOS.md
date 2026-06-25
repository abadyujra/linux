# 🐧 ADMINISTRACIÓN DE LINUX: GESTIÓN Y PROCESOS

---

## Comandos de Monitoreo

### El Comando `ps` (Process Status)
*   `ps` ➡️ Lista solo los procesos del usuario actual que tienen una terminal disponible.
*   `ps x` ➡️ Muestra todos los procesos del usuario actual (tengan o no una terminal asociada).
*   `ps a` ➡️ Muestra los procesos de todos los usuarios, independiente de quién los ejecute.
*   `ps f` ➡️ Muestra la jerarquía de procesos (árbol genealógico de quién levantó a quién).
*   `tty` ➡️ Muestra el nombre exacto de la terminal asociada a la sesión actual.

###  Monitores en Tiempo Real
*   `top` ➡️ Monitor nativo del sistema con la lista dinámica de procesos.
*   `htop` ➡️ Monitor interactivo y gráfico.
*   `sudo htop` ➡️ Permite cambiar prioridades (Nice) de modo gráfico usando las flechas.

---

## Estados de un Proceso (Columna STAT)

| Estado | Significado Técnico | Explicación Simple |
| :---: | :--- | :--- |
| **D** | Espera ininterrumpible | Esperando respuesta de Disco o Red (I/O). |
| **R** | Corriendo (Running) | Usando la CPU en este milisegundo. |
| **S** | Espera interrumpible | Durmiendo, esperando un evento para despertar. |
| **T** | Detenido (Stopped) | Pausado (ej. con `Ctrl + Z`). |
| **Z** | Zombie | Proceso muerto cuyo padre no ha liberado su espacio. |
| **I** | Idle | Hilo del núcleo (Kernel) en estado ocioso. |
| **t** | Traced | Detenido temporalmente por un debugger (depurador). |

### Atributos Adicionales (Letras Secundarias)
*   `l` ➡️ Proceso multi-hilo (Multi-threaded).
*   `s` ➡️ Líder de sesión (Genera nuevos procesos hijos).
*   `+` ➡️ Proceso que corre en **primer plano** (Foreground).
*   `N` ➡️ Baja prioridad (Nice positivo).
*   `<` ➡️ Alta prioridad de ejecución (Nice negativo).
*   `L` ➡️ Páginas de memoria bloqueadas en el núcleo.

---

##  Prioridades y Control de CPU (Nice)

### Rango del Kernel
*   **[0 - 99]** ➡️ PRIORIDADES REAL-TIME (Reservado para Kernel/Hardware).
*   **[100 - 139]** ➡️ PRIORIDADES DE NIVEL DE USUARIO (Tus programas).
    *   *Nota:* htop a veces lo mapea visualmente en un rango resumido de **0 a 39**.

### Comandos de Modificación
*   `sudo nice -n -10 [comando]` ➡️ Inicia un comando nuevo con alta prioridad (valores negativos requieren `sudo`).
*   `sudo renice -n +5 -p [PID]` ➡️ Cambia la prioridad de un proceso que **ya está corriendo** pasándole su número de ID (PID).

---

## Señales del Sistema (Kill)

*   `kill -l` ➡️ Lista todas las señales disponibles que el sistema puede enviar.

### Señales Críticas de Uso Diario
*   `kill -2 [PID]` ➡️ Equivalente a enviar un `Ctrl + C` desde la terminal (Interrupción limpia).
*   `kill -15 [PID]` (o `kill -TERM`) ➡️ Termina el proceso de forma ordenada, permitiéndole guardar datos.
*   `kill -9 [PID]` (o `kill -KILL`) ➡️ Mata el proceso de forma inmediata y forzada. El proceso no puede ignorarla.
*   `killall [Nombre]` ➡️ Mata todos los procesos que se llamen igual (Ej: `killall yes`).

## procesos en 1ro y 2do plano
#  CONTROL DE TRABAJOS EN SEGUNDO PLANO (JOB CONTROL)

---

## Comandos y Atajos Esenciales

* `sleep 100` ➡️ Ejecuta el comando en **primer plano** (Foreground). Bloquea la terminal actual hasta que termine o lo canceles con `Ctrl + C`.
* `sleep 100 &` ➡️ El símbolo **`&`** al final lanza el proceso directamente al **segundo plano** (Background). La terminal se libera inmediatamente para que puedas seguir escribiendo otros comandos.
* `jobs` ➡️ Lista todos los trabajos que están corriendo o suspendidos en segundo plano **únicamente en la sesión de la terminal actual**.
* `fg` (Foreground) ➡️ Trae de vuelta al primer plano el último trabajo que tenías en segundo plano (o puedes especificar cuál usando `fg %1`).
*  `bg` ➡️ reactiva la tarea en el segundo plano
* `Ctrl + Z` ➡️ **Pausa/Detiene** (estado `T` - Stopped) el proceso que se está ejecutando en primer plano en ese momento y lo manda al segundo plano.
* `bg` (Background) ➡️ Reactiva un proceso que fue pausado con `Ctrl + Z`, haciendo que continúe su ejecución pero dentro del segundo plano.

---

## Administración y Reactivación mediante Señales

Cuando listas tus trabajos con el comando `jobs`, verás un número pequeño entre corchetes (Ej: `[1]`). Ese es el **Job ID**, que es diferente al **PID** general del sistema.



### El comando `ps` combinado
* `ps fax | grep sleep` ➡️ Busca en todo el árbol de procesos del sistema (`ps fax`) cualquier línea que contenga la palabra "sleep" para averiguar su número de `PID` real, su estado y su dueño.

### Reactivación con `kill`
Si pausaste un proceso con `Ctrl + Z`, se queda congelado en memoria. Para ordenarle que despierte y siga trabajando sin traerlo al primer plano, usas la señal de continuación:

* `kill -18 [PID]` (o `kill -CONT [PID]`) ➡️ Envía la señal `SIGCONT` al identificador del proceso para que reanude su marcha en segundo plano.


> 💡 **Tip de Sintaxis:** Si quieres usar el comando `kill` pero apuntando al número que te da `jobs` en lugar del PID, usa el símbolo de porcentaje: `kill -CONT %1`.

### CONTROL DE TRABAJOS: SINTAXIS Y SIGNOS

* `fg %1` ➡️ Trae el Trabajo `[1]` al **primer plano** (bloquea la terminal).
* `bg %2` ➡️ Despierta/Activa el Trabajo `[2]` en **segundo plano** (sigue corriendo atrás).

⚠️ **Signos `+` y `-` (Ojo: ¡Todos están en segundo plano!):**
* **El más (`+`)** ➡️ Último proceso usado. Es el "por defecto": si escribes `fg` a secas, levantará este.
* **El menos (`-`)** ➡️ Penúltimo proceso usado. Es el segundo en la lista de espera.

---

### 💻 EJEMPLOS REALES EN CONSOLA

#### Caso 1: Estás corriendo procesos en segundo plano
Si ejecutas `jobs`, la terminal te mostrará esto:

abad@UBUNTU:~$ jobs
[1]-  Running                 sleep 2000 &
[2]+  Stopped                 sleep 300 ```

## Crea un proceso sombi
*   `vi zombie.c` crea proceso sombie

    #include <stdlib.h>
    #include <unistd.h>
    int main(int argc, const char *argv[]){
        if  (!fork()) //proceso hijo
         exit(0);
         
         // proceso padre
         pause();
         return();
    }
*   `gcc zombie.c -o zombie` compila
*   `./zombie` ejecuta
*   `nohup`➡️ Mantiene un comando corriendo de fondo aunque cierres la terminal


