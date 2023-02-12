---
title: 'Adquisición y análisis de memoria RAM en Windows'
date: 2023-02-12 22:00:00 +0200
categories: [Tutorial,Forensics]
tags: [Tutorial,Forensics,BlueTeam, Volatility, Windows]
image:
  path: /assets/img/commons/ram-forensics/windowsRam.png
  width: 400
  height: 300
---

La memoria **volátil** o **memoria RAM**, es una parte fundamental de las **investigaciones forenses** de los ordenadores.
Contiene datos volátiles mientras el sistema operativo está en ejecución. La información almacenada en la memoria volátil pierde información de *procesos en ejecución*, *conexiones de red abiertas*, *contraseñas* y demás cuando se apaga el pc. 

**Mimikatz** es una herramienta de seguridad utilizada por hackers que se aprovecha de extraer información de autenticación de la memoria volátil. Al ser una herramienta tan peligrosa la mayoría de sistemas detectan esa amenaza.

Algunos **malwares** utilizan herramientas del propio sistema operativo para saltarse restricciones como scripts en *powershell* o *lolbins* <a href="https://lolbas-project.github.io" target="_blank">https://lolbas-project.github.io</a><br/>
Como todos esos procesos se quedan en memoria, es importante hacer una captura a la memoria para posteriormente hacer la investigación forense. **Sin ella podemos perder información importante.**

Todo ello hace que la adquisición de memoria sea un paso esencial por el que debe empezar toda investigación forense.


<br/>

## Comparativa de herramientas

**3 importantes variables** hay que tener en cuenta a la hora de elegir una herramienta:

RAM Footprint
: Cuánta memoria consume la herramienta al extraer RAM (ya que para obtener esta memoria se necesita memoria). Esto es importante, ya que la *huella de la RAM* equivale a la cantidad de **pruebas potenciales** que posiblemente se sobrescriban como parte inevitable del proceso.

Modo
: Las herramientas de recopilación de RAM funcionan en **Kernel-Mode** o en **User-Mode**. El modo Kernel tiene la capacidad de recopilar más datos de la RAM que son inaccesibles, ya que está diseñado para eludir activamente la protección antidepuración y antidumping. 

Velocidad de extracción
: Rapidez con la que la herramienta copia la memoria RAM a través de una conexión **USB 3.0**. También es importante porque en cada momento que pasa la memoria puede cambiar o puede que el malware la limpie.

![Comparaciones RAM]({{ 'assets/img/commons/ram-forensics/ramDifferences.png' | relative_url }}){: width="650" height="650" .center-image }
_Comparativa de las diferentes herramientas  <a href="https://digitalforensicsurvivalpodcast.com/2016/08/09/dfsp-025-ram-extraction-tools-part-2/" target="_blank">(Fuente)</a>_

Dada la comparativa que vemos en la imagen diríamos que la mejor herramienta es DumpIt porque gana en absolutamente todo. Así que haremos una práctica que en este caso elegiremos **DumpIt**

<br/>

Vamos a descargarnos dumpIt desde la página de Magnet <a href="https://www.magnetforensics.com/resources/magnet-dumpit-for-windows/" target="_blank">aquí</a> (más actualizada) o bien desde <a href="https://github.com/thimbleweed/All-In-USB/blob/master/utilities/DumpIt/DumpIt.exe?raw=true">aquí</a>

![Hash DumpIt]({{ 'assets/img/commons/ram-forensics/hashDumpIt.png' | relative_url }}){: width="650" height="650" .center-image }
_SHA256 del segundo link_

La idea es ejecutar esta o cualquier otra herramienta desde una unidad externa, como un **USB y no instalarla**. Ya que como hemos dicho, queremos dejar la menor huella posible y hacerlo rápido.

También es importante no guardar el archivo de imagen en el sistema, sino en el *USB o en un recurso compartido de red*. Ya que de lo contrario, se sobrescribiría el espacio no asignado dentro del disco duro. La sobrescritura del espacio no asignado puede impedir recuperar el contenido de la memoria borrada que aún pueda estar accesible y ser problema en una adquisición forense del disco.


## Practica DumpIt

Crea una máquina virtual de Windows y ejecuta algunos procesos como powershell.

### Extracción Windows

Una vez que tengamos dumpIt en el **USB** y lo insertemos en un sistema Windows, lo ejecutamos con **doble clic**.

![DumpIt Archivo]({{ 'assets/img/commons/ram-forensics/fileDumpIt.png' | relative_url }}){: width="650" height="650" .center-image }

Este sistema windows tiene *2GB* de RAM que se guardarán en mi USB de *7GB* de almacenamiento. A la hora de hacer la extracción de la imagen, **hay que tener en cuenta la RAM del dispositivo y del USB.** <br/>
El archivo se guardará en la ruta *'E:\DESKTOP-4BLUE0H-20230212-182517.raw'* indicando el **nombre del ordenador** y **fecha actual** con formato **yyyymmdd-hhmmss**

![DumpIt Ejecutado]({{ 'assets/img/commons/ram-forensics/executedDumpIt.png' | relative_url }}){: width="650" height="650" .center-image }

Una vez acabada la extracción aparecerá en nuestro USB. **Comprobamos su hash.**

![Imagen Extraída]({{ 'assets/img/commons/ram-forensics/folderDumpIt.png' | relative_url }}){: width="650" height="650" .center-image }
![Imagen Extraída]({{ 'assets/img/commons/ram-forensics/hashDumpBefore.png' | relative_url }}){: width="650" height="650" .center-image }

<br/>

### Copiado Imagen

Copiamos la imagen en nuestro kali para proceder a analizar. **Comprobamos de nuevo el hash** para asegurarnos de que no ha habido ningún error en el momento de copiado.

![Imagen Extraída]({{ 'assets/img/commons/ram-forensics/pasteDump.png' | relative_url }}){: width="650" height="650" .center-image }
![Imagen Extraída]({{ 'assets/img/commons/ram-forensics/hashDumpAfter.png' | relative_url }}){: width="650" height="650" .center-image }

<br/>
<br/>

### Descarga volatility3

Descargamos volatility3 desde github

```bash
git clone https://github.com/volatilityfoundation/volatility3.git
cd volatility3
python vol.py -h
```
<br/>
Ejecutamos el siguiente plugin, en el que podremos ver que en el momento de la captura está **dumpIt ejecutándose desde el explorer**:
```bash
python vol.py -f tuImagen.raw windows.pstree
``` 
![Windows volatility 1]({{ 'assets/img/commons/ram-forensics/windowsVolatility1.png' | relative_url }}){: width="650" height="650" .center-image }
![Windows volatility 2]({{ 'assets/img/commons/ram-forensics/windowsVolatility2.png' | relative_url }}){: width="650" height="650" .center-image }


<br/>

Apartir de este momento puedes explorar que conexiones, procesos, archivos y demás se encontraban en el sistema en el momento de la captura.

### Cheatsheet de volatility3

<a href="https://book.hacktricks.xyz/generic-methodologies-and-resources/basic-forensic-methodology/memory-dump-analysis/volatility-cheatsheet" target="_blank">https://book.hacktricks.xyz/generic-methodologies-and-resources/basic-forensic-methodology/memory-dump-analysis/volatility-cheatsheet</a>

