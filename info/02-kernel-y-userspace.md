 <div align="center">

# Linux: hardware, kernel y el user space

</div>

---


Al pensar en un sistema operativo Linux por primera vez, su complejidad puede resultar abrumadora. Sin embargo, pensarlo de forma general ayuda a comprenderlo mejor. La mejor forma de desglosarlo es dividiéndolo en **tres capas principales de abstracción**:

1. **Hardware:** La capa de nivel más bajo. Los componentes físicos que ejecutan instrucciones y procesan datos.
2. **Kernel:** El núcleo del software que se carga en memoria durante el funcionamiento del sistema. Actúa como mediador entre los programas y el hardware, administrando los recursos del sistema.
3. **User Space:** El ambiente donde predominan los procesos de usuario y los programas en ejecución, los cuales utilizan los servicios proporcionados por el kernel.

---

## Modos de Ejecución: Kernel Mode y User Mode

Tanto el kernel como los procesos de usuario son, en esencia, programas. Lo que los diferencia radicalmente es **el modo en el que corren en el sistema**.

* **Kernel Mode:** El kernel se ejecuta con privilegios elevados, lo que le permite realizar operaciones protegidas y administrar los recursos del sistema. El espacio de direcciones virtuales reservado para el kernel se conoce como *Kernel Space*.
* **User Mode:** Los procesos y programas cotidianos (como un navegador web) corren en este modo, donde existen restricciones sobre las operaciones privilegiadas y el acceso a la memoria.

> ** ¿Por qué existe esta división?**  
> Esta distinción arquitectónica permite que, si un programa del *User Space* falla o colapsa, el error generalmente quede contenido y no tenga consecuencias catastróficas para el resto del sistema operativo.
>
> *(Nota: Aunque existe esta barrera, un usuario operando con permisos elevados o de `root` aún puede ejecutar acciones destructivas a nivel global. Sin embargo, sus procesos normalmente continúan ejecutándose en modo usuario).*

---

## Hardware

Es la base sobre la que opera todo el sistema.

* **Memoria Principal (RAM):** Es un componente fundamental para el software. Actúa como un almacén de información representada mediante `0`s y `1`s (bits). Los procesos y el kernel utilizan memoria durante su ejecución, aunque no necesariamente todos sus datos deben permanecer cargados en la RAM al mismo tiempo.
* **CPU (Procesador):** Es el componente encargado de ejecutar instrucciones. Para ello, obtiene instrucciones, realiza operaciones utilizando sus registros internos y trabaja con datos que pueden encontrarse en memoria. Los resultados pueden almacenarse en registros, escribirse en memoria o enviarse a otros componentes.

## El Kernel

El kernel realiza numerosas tareas relacionadas con la administración de los recursos del sistema. Entre ellas destacan tres grandes responsabilidades:

#### Gestión de Procesos y *Context Switch*

El kernel es el encargado de crear y finalizar procesos, administrar sus estados y planificar su ejecución. Cada núcleo lógico de CPU puede ejecutar una secuencia de instrucciones a la vez, por lo que el kernel distribuye el tiempo de procesamiento entre las tareas que están listas para ejecutarse.

El ciclo de ejecución y cambio de contexto ocurre de la siguiente manera:

1. Un proceso se ejecuta durante un intervalo de tiempo asignado de CPU, conocido como **Time Slice**.
2. Cuando termina ese intervalo, puede producirse una interrupción que permite al kernel recuperar el control. Sin embargo, un cambio de tarea también puede ocurrir cuando un proceso se bloquea, cede la CPU o sucede otro evento que requiere planificación.
3. Se produce un **Context Switch** (Cambio de Contexto): el kernel coordina el cambio de una tarea a otra. En los casos en que una tarea de usuario es interrumpida para ejecutar código del kernel, el procesador pasa a modo kernel.
4. El kernel guarda el contexto necesario para poder reanudar la tarea posteriormente, incluyendo información como los registros de la CPU y el estado de ejecución. No necesita copiar toda la memoria del proceso para realizar un cambio de contexto.
5. El kernel selecciona otra tarea que esté lista para ejecutarse, prepara el contexto necesario y transfiere la ejecución a ella. Si la tarea seleccionada es de usuario, la CPU vuelve a modo usuario para continuar ejecutándola.

#### Gestión de Memoria y el MMU

El kernel también debe gestionar la memoria que utilizan los procesos. Para esto, trabaja con un componente de hardware llamado **MMU** (*Memory Management Unit*).

* El kernel administra las **Page Tables** (tablas de páginas).
* Estas tablas relacionan direcciones de memoria virtual con direcciones de memoria física y establecen permisos de acceso. La memoria virtual permite que cada proceso disponga de su propio espacio de direcciones, independiente del utilizado por otros procesos.
* **Objetivo:** Aislar los espacios de memoria de los procesos y controlar sus accesos. La MMU utiliza las tablas de páginas para traducir direcciones virtuales y comprobar los permisos correspondientes.

#### Llamadas al Sistema (Syscalls)

Las aplicaciones en el *User Space* no pueden ejecutar directamente operaciones privilegiadas ni acceder libremente al hardware. Cuando necesitan realizar operaciones como abrir, leer o escribir un archivo, solicitan los servicios correspondientes al kernel mediante una **Syscall** (Llamada al Sistema).

Ejemplos fundamentales relacionados con la creación y ejecución de procesos:

* `fork()`: Crea un proceso hijo a partir del proceso que realiza la llamada. El hijo comienza con un contexto muy similar al del padre, pero ambos pasan a ser procesos independientes. Linux utiliza mecanismos como *copy-on-write* para evitar copiar inmediatamente todas las páginas de memoria.
* `exec()`: Reemplaza la imagen del programa que ejecuta el proceso actual por la de otro programa. Si la operación tiene éxito, el proceso continúa ejecutando el nuevo programa, sin que se cree necesariamente un proceso adicional.

### User Space

Es la capa de más alto nivel, el terreno de juego de los usuarios. Aquí se ejecutan las interfaces gráficas, las terminales, los servidores web y otras aplicaciones que utilizan los servicios del sistema operativo. Los programas pueden realizar muchas tareas por sí mismos y recurren al kernel cuando necesitan acceder a recursos o realizar operaciones administradas por este.

## Referencias 

Ward, B. (2021). How Linux Works: What Every Superuser Should Know (3.ª ed.). No Starch Press.

How Linux Works — No Starch Press
