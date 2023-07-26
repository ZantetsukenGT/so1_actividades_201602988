# Tipos de Kernel y sus diferencias
## Monolithic kernels
En un kernel monolítico, todos los servicios del sistema operativo se ejecutan en el hilo principal del kernel, y también residen en el mismo espacio de memoria.

Un kernel monolítico contiene todas las funciones principales y los drivers de todos los dispositivos, un kernel monolítico es un solo programa que contiene todo el código necesario para realizar todas las tareas relacionadas al hardware y al kernel.

## Microkernels
A diferencia de un kernel monolítico, en un microkernel, toda la funcionalidad del sistema se mueve a una serie de `servidores` que se pueden comunicar a través de un kernel mínimo, estos tipos de kernel buscan implementar lo más que se pueda en `user space`, e implementar lo menos que se pueda en `kernel space`, y estar lo más modularizado que se pueda de forma que un microkernel está diseñado para que implemente lo mínimo que un dispositivo o plataforma necesite para poder operar.

Los microkernel son mucho más fáciles de implementar por su naturaleza modular, sin embargo, por esta misma abstracción, las `system calls` y `cambios de contexto` tienen mucho más `overhead` que en un kernel monolítico por lo que el rendimiento puede llegar a ser menor.

Otra de las diferencias entre kernel monolítico y microkernel es el tema de la seguridad conforme aumenta el tamaño del codebase y de la cantidad de dispositivos a los que se les añade soporte de drivers, en un kernel monolítico todo este tema tiene memoria compartida en kernel space, por lo que hay más probabilidades de que existan vulnerabilidades, pero al mismo tiempo al compartir el mismo espacio de memoria en kernel space, un kernel monolítico es muchísimo más eficiente que un microkernel que si bien su diseño se encarga de aislar las vulnerabilidades, ese mismo diseño requiere un sistema de mensajería para poder comunicarse, y es mucho más lento (IPC: inter-process communication).

[Tanenbaum–Torvalds debate](https://en.wikipedia.org/wiki/Tanenbaum%E2%80%93Torvalds_debate)

## Hybrid (or modular) kernels
Estos son similares a los microkernels, pero trasladan algo de la funcionalidad crítica a `kernel space` para aumentar el performance, aprovechando las ventajas de tanto el kernel monolítico que es eficiente y el microkernel que es más seguro y fácil de expandir e implementar, sin embargo no goza de las ventajas al 100%, ya que no es igual de eficiente que el kernel monolítico y no goza del mismo grado de estabilidad y resistencia a los errores y vulnerabilidades que los microkernel, se encuentra en un punto intermedio.

## Nanokernels
Estos delegan virtualmente todos los servicios y funcionalidad esencial a `user space`, tienen un requerimiento de memoria en `kernel space` mucho menor que los microkernel.

## Exokernels
Los exokernel se diferencian de todos los otros tipos de kernel al limitarse a proteger y de multiplexar el hardware en su más bajo nivel, no provee abstracciones, más bien le dejan esta tarea al desarrollador para que él mismo determine la forma más eficiente de utilizar el hardware disponible para cada programa en específico.

En otras palabras, al desarrollador se le provee de una serie de librerías para que él mismo desarrolle su programa, pero que al mismo tiempo implemente él mismo el resto de la funcionalidad del kernel desde cero, por lo que, en un entorno industrial orientado a la alta productividad, este tipo de kernel no es viable y no ha ganado tracción.

Aunque una de las posibles ventajas es la posibilidad de tener un control completo en la aplicación sobre lo que debe suceder en tiempo real y lo que no.

## Unikernel
El unikernel está construido sobre el concepto de un Exokernel, un unikernel es un programa que se compila con el código del sistema operativo `estáticamente enlazado (statically linked)`, y el resultado de esta compilación es nuestro programa pero que no requiere de un sistema operativo para correr y puede ser utilizado directamente como un sistema operativo en un hypervisor.

## Multikernels
Un multikernel trata a los diferentes núcleos de un CPU como una red de núcleos independientes, en donde no existe la memoria compartida, sino que existe un `proceso de intercomunicación` (IPC) por mensajes.

# User vs Kernel Mode
La razón de ser de estos dos modos es el de segregar el manejo de la memoria virtual para proveer protección a la memoria y el hardware de ataques maliciosos y bugs de memoria en el software.

## Kernel space
El kernel space está reservado para ejecutar únicamente operaciones privilegiadas del sistema operativo, extensiones del kernel o kernel modules, y los drivers del hardware.

## User space
El user space es un área de memoria que el código de nuestras aplicaciones puede manipular, en user space cada proceso que se ejecuta tiene su propio espacio virtual de memoria y a menos que se le permita, no puede acceder a la memoria de otros procesos.

En otras palabras, user space es la base de la separación de privilegios y es la base de la protección de memoria en los sistemas operativos más populares de hoy en día.

Con los suficientes permisos, un proceso de user space le puede solicitar al kernel que mapee como suyo el espacio de memoria de otro proceso, dando vida a los debuggers, también puede solicitar `compartir el espacio de memoria (shared memory)` con otros procesos, dando vida a las librerías dinámicas (.dll, .so)

## Implementación
Una manera común de implementar el user mode y kernel mode es utilizar los `anillos de protección del sistema operativo (protection rings)`, y estos anillos a su vez se implementan con los `CPU modes`. Los programas en `kernel space` se ejecutan en `kernel mode` o `supervisor mode` y a su vez las aplicaciones normales en `user space` se ejecutan en `user mode`.

Algunos sistemas operativos tienen un solo espacio virtual de memoria para todos los procesos en `user mode` y un espacio de memoria dedicado para `kernel mode` y otros tienen un espacio de memoria diferente para cada proceso en `user mode`.

Otros sistemas operativos toman la decisión de utilizar un solo espacio de memoria para ambos `kernel mode` y `user mode` y confiar en la semántica de los lenguajes de programación para evitar el acceso arbitrario a la memoria, efectivamente obligando a las aplicaciones a no poder acceder a regiones de memoria que no les pertenecen.
