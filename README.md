# Simulación de un ejercicio de Red Team en máquinas virtualizadas

> Trabajo Final de Grado — Ingeniería Informática  
> Autor: Jack Nesperos Oba  
> Curso: 2025/2026

Entorno CTF (Capture The Flag) desplegado sobre VirtualBox con dos máquinas virtuales preconfiguradas. Incluye cuatro retos de dificultad progresiva que cubren vectores de ataque reales documentados en el NIST y la OWASP. 

---

## Índice

1. [Requisitos](#1-requisitos)
2. [Importación de las máquinas](#2-importación-de-las-máquinas)
3. [Configuración de la red](#3-configuración-de-la-red)
4. [Credenciales](#4-credenciales)
5. [Uso del ctf_checker](#5-uso-del-ctf_checker)
6. [Herramientas recomendadas](#6-herramientas-recomendadas)

---

## 1. Requisitos

Para ejecutar el entorno se necesita [VirtualBox](https://www.virtualbox.org/) 7.0 o superior, un mínimo de 8 GB de RAM disponibles (4 GB por máquina) y unos 40 GB de espacio en disco libre. El sistema operativo del host puede ser Windows, macOS o Linux.

---

## 2. Importación de las máquinas

Las dos máquinas se encuentran en formato `.ova` descargables en el siguiente enlace de Internet Archive debido a su tamaño [Maquinas](https://archive.org/download/maquinas-ctf). Impórtalas en el siguiente orden desde VirtualBox: `Archivo` → `Importar servicio virtualizado...`, selecciona el archivo correspondiente, revisa los parámetros y haz clic en **Importar**. Repite el proceso primero con `Ubuntu Victima.ova` y después con `Kali VM.ova` si no dispones de una máquina para realizar los ataques, en el caso contrario puedes usar las herramientas que más te convengan.

---

## 3. Configuración de la red

Las dos máquinas deben estar en la misma NAT Network para poder comunicarse. Si no tienes ninguna creada, ve a `Archivo` → `Herramientas` → `Red`, selecciona la pestaña **Redes NAT**, haz clic en **Crear** y asigna el nombre `ctf-network` con el rango `10.0.2.0/24`.

Una vez creada la red, entra en la configuración de cada máquina (`Configuración` → `Red` → `Adaptador 1`), en conectar selecciona **Red NAT** y elige el nombre `ctf-network`. Aplica los cambios en ambas máquinas.

Para verificar que se ven entre ellas, arranca las dos máquinas y ejecuta desde Kali:

```bash
nmap -sn 10.0.2.0/24
```

La máquina víctima aparecerá en la lista. Toma nota de la IP, que usarás como `IP_VICTIMA` durante todo el CTF.

> ⚠️ Este entorno está diseñado para uso en red local aislada. No expongas las máquinas a Internet.

---

## 4. Credenciales

La máquina víctima tiene un usuario `ctfuser` con contraseña `ctfuser`, que es el que se utiliza para ejecutar el verificador de flags. La máquina Kali usa el usuario `kali` con contraseña `kali`.

---

## 5. Uso del ctf_checker

El verificador de flags está disponible en la máquina víctima. Conéctate por SSH en una terminal y ejecútalo:

```bash
ssh ctfuser@IP_VICTIMA
./ctf_game.sh
```

El script muestra el enunciado de cada reto y verifica la flag introducida. Hasta que una flag no es correcta no se desbloquea el enunciado del siguiente reto. El progreso se guarda entre sesiones, por lo que puedes cerrar la terminal y retomarlo más tarde. Si quieres volver a empezar desde el principio, ejecuta:

```bash
./ctf_game.sh --reset
```

---

## 6. Herramientas recomendadas

Todas las herramientas necesarias están preinstaladas en Kali Linux. Se utilizan `nmap` para el reconocimiento, `msfconsole` para la explotación, `gobuster` para la enumeración de directorios web, `hydra` para el ataque de fuerza bruta SSH y `john` para el cracking de hashes.

La wordlist `rockyou.txt` se encuentra en `/usr/share/wordlists/rockyou.txt`. Si aparece comprimida, descomprímela antes de usarla:

```bash
sudo gunzip /usr/share/wordlists/rockyou.txt.gz
```

---

## 7. Writeup

Si te quedas atascado o quieres revisar el proceso de resolución completo una vez hayas terminado, el writeup está disponible en [`Guia/Guia_CTF.md`](Guia/Guia_CTF.md). Incluye la justificación técnica de cada ataque y las referencias a las vulnerabilidades explotadas.

---

## Licencia

Este proyecto se ha desarrollado con fines educativos en el marco de un TFG universitario.
