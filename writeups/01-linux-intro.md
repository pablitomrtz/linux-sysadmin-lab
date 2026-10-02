<div align="center">

#  Linux Introduction

*Una guía fundamental sobre el núcleo, la historia y la arquitectura del ecosistema GNU/Linux.*

</div>

---

##  Tabla de Contenidos

- [Introducción](#-introducción)
- [Historia de GNU y Linux](#-historia-de-gnu-y-linux)
- [Open Source y Software Libre](#-open-source-y-software-libre)
- [Arquitectura de un Sistema Linux](#-arquitectura-de-un-sistema-linux)
- [Distribuciones Linux](#-distribuciones-linux)
- [Gestión de Paquetes](#-gestión-de-paquetes)
- [Referencias](#-referencias)

---

##  Introducción

Cuando hablamos de **Linux**, normalmente hacemos referencia al ecosistema **GNU/Linux**, una combinación entre el núcleo Linux (Kernel) y las herramientas desarrolladas por el Proyecto GNU.

Actualmente, GNU/Linux es el motor de gran parte de la tecnología global. Se utiliza en:
*  **Servidores** e infraestructuras Cloud.
*  **Dispositivos embebidos** y como base de Android.
*  **Supercomputadoras** a nivel mundial.

---

##  Historia de GNU y Linux

###  El Proyecto GNU
Iniciado en **1983** por **Richard Stallman**, su objetivo era desarrollar un sistema operativo completamente libre inspirado en UNIX.

Durante años, la comunidad GNU desarrolló componentes que hoy son pilares fundamentales:
- `Bash` (Bourne Again Shell)
- `GCC` (GNU Compiler Collection)
- `Coreutils` y librerías del sistema
- Herramientas de administración y desarrollo

> **Nota histórica:** A pesar de tener casi todas las herramientas listas, el proyecto GNU aún carecía de un núcleo (kernel) funcional ampliamente adoptado.

### 🐧 Nacimiento de Linux
En **1991**, **Linus Torvalds** (estudiante de Ciencias de la Computación en la Universidad de Helsinki) comenzó el desarrollo de un núcleo propio inspirado en **MINIX** (un sistema educativo creado por Andrew Tanenbaum). 

Las limitaciones de licenciamiento y las capacidades reducidas de MINIX motivaron a Torvalds a programar su propia solución.
* **Dato curioso:** Inicialmente el proyecto iba a llamarse **Freax**, pero posteriormente adoptó el nombre mundialmente conocido: **Linux**.

La combinación del **Kernel Linux** y las **herramientas GNU** permitió, finalmente, construir un sistema operativo completo y funcional.

---

##  Open Source y Software Libre

Aunque suelen usarse como sinónimos, sus fundamentos filosóficos son distintos:

| Concepto | Filosofía Principal | Libertades / Permisos Clave |
| :--- | :--- | :--- |
| **Open Source**<br>*(Código Abierto)* | Promueve un modelo de desarrollo **pragmático** y colaborativo. | Permite a cualquier persona estudiar, modificar, mejorar y redistribuir el código fuente para fomentar la innovación. |
| **Software Libre**<br>*(Free Software)* | Impulsado por Richard Stallman, tiene un enfoque **ético** centrado en el usuario. | **1.** Ejecutar para cualquier propósito.<br>**2.** Estudiar cómo funciona.<br>**3.** Modificarlo.<br>**4.** Redistribuir copias y mejoras. |

---

##  Arquitectura de un Sistema Linux

Una forma sencilla de comprender Linux es visualizarlo como una serie de capas de abstracción. *(GitHub renderizará este gráfico automáticamente)*:

```mermaid
graph TD
    A[<b>User Space</b><br>Aplicaciones, Bash, Firefox, SSH] -->|System Calls| B
    B[<b>Kernel</b><br>Gestión de CPU, RAM, Drivers, Filesystem] -->|Controlador| C
    C[<b>Hardware</b><br>Placa Base, CPU, Discos, Tarjetas de Red]
