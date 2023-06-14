---
title: 'Como montar un SIEM y EDR en Elastic Cloud GRATIS'
date: 2023-06-14 17:00:00 +0200
categories: [Tutorial,SIEM]
tags: [Tutorial,SIEM,Elastic,EDR]
image:
  path: /assets/img/commons/elastic-setup/elasticTitle.gif
  width: 600
  height: 300
---

Vamos a aprender a como montar un SIEM con Elastic en Cloud con su EDR activado y además ponerlo a prueba en una máquina Windows virtual.

# ¿Qué es un SIEM?

Un **SIEM** (Security Information and Event Management) es un **programa de monitorización que recopila, analiza y correlaciona logs en tiempo real.**

Tener un SIEM es **necesario** por la capacidad de **analizar logs** fuera de la máquina, **crear alertas** en base a esos logs, integrar nuevas **herramientas de detección y repuesta** y al final correlacionar eventos en un entorno donde la **comodidad** y la **velocidad** de la muestra de los logs es importante.


# Registro Elastic

Lo primero de todo es registrarse en Elastic donde te darán **15 días gratis** sin pedir tarjeta

<a href="https://cloud.elastic.co/registration" target="_blank">https://cloud.elastic.co/registration</a>

![Registro Elastic]({{ 'assets/img/commons/elastic-setup/1.png' | relative_url }}){: width="350" height="350" .center-image }

Una vez registrado te pedirán algunos datos que te los puedes inventar

![Registro Datos Elastic]({{ 'assets/img/commons/elastic-setup/2.png' | relative_url }}){: width="350" height="350" .center-image }

Finalmente, nos pedirá donde crear nuestro deployment. Yo lo dejé **por defecto**

![Registro Deployment Elastic]({{ 'assets/img/commons/elastic-setup/3.png' | relative_url }}){: width="450" height="550" .center-image }


# Añadir agentes

Los **agentes** son programas diseñados para **enviar información de los logs** del host al Stack de Elastic. 
<br/>
<br/>

Para añadir un host y agregarle el agent vamos a instalar la <a href="https://www.microsoft.com/es-es/software-download/windows10" target="_blank">ISO de windows</a> en nuestro **VMware** o **VirtualBox**. 

Después de hacer una correcta instalación de windows en nuestra máquina virtual, seguimos con la instalación del agente.

## Paso Elastic

Nos aparecerá esta interfaz donde haremos clic en **Management - Fleet**

![Fleet Elastic]({{ 'assets/img/commons/elastic-setup/4fleet.png' | relative_url }}){: width="850" height="850" .center-image }

Después, hacemos clic en **Add agent**

![Add Agent Elastic]({{ 'assets/img/commons/elastic-setup/5agent.png' | relative_url }}){: width="850" height="850" .center-image }

Creamos una policy llamada **Windows** 

![Agent Elastic]({{ 'assets/img/commons/elastic-setup/6agent.png' | relative_url }}){: width="850" height="850" .center-image }

Lo dejamos en **recommended**
![Agent Elastic]({{ 'assets/img/commons/elastic-setup/7agent.png' | relative_url }}){: width="850" height="850" .center-image }

Aquí **copiamos el código** para pegarlo en un **powershell con privilegios** en la **máquina Windows**
![Agent Elastic]({{ 'assets/img/commons/elastic-setup/8agent.png' | relative_url }}){: width="850" height="850" .center-image }

<br/>

## Paso Windows

En nuestra máquina virtualizada iniciamos **powershell como administrador**.

![Powershell Windows ]({{ 'assets/img/commons/elastic-setup/9powershell.png' | relative_url }}){: width="550" height="550" .center-image }

Creamos una carpeta nueva y **ejecutamos** el código copiado de Elastic con **botón derecho**

![Powershell Windows ]({{ 'assets/img/commons/elastic-setup/10powershell.png' | relative_url }}){: width="850" height="850" .center-image }

Una vez finalice, en Elastic podremos ver que se ha completado correctamente.

![Fleet Completado Windows ]({{ 'assets/img/commons/elastic-setup/11fleet.png' | relative_url }}){: width="850" height="850" .center-image }

![Fleet Completado Windows ]({{ 'assets/img/commons/elastic-setup/12fleet.png' | relative_url }}){: width="850" height="850" .center-image }


# Instalación de Sysmon 

**Sysmon** es una utilidad que **monitorea Windows** de una forma la cual te provee de logs con información detallada sobre procesos, conexiones de red y más. Es uno de los mejores logs para investigar.

Vamos a descargar **Sysmon** desde <a href="https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon" target="_blank">aquí</a> y la **configuración** necesaria desde <a href="https://github.com/SwiftOnSecurity/sysmon-config/blob/master/sysmonconfig-export.xml" target="_blank">aquí</a>

![Sysmon Windows ]({{ 'assets/img/commons/elastic-setup/13sysmon.png' | relative_url }}){: width="750" height="750" .center-image }

<br/>
Una vez descargados, iniciamos **powershell como administrador** y ejecutamos el siguiente código

```powershell
.\Sysmon64.exe -accepteula -i .\sysmonconfig-export.xml
```
![Sysmon Windows]({{ 'assets/img/commons/elastic-setup/14sysmon.png' | relative_url }}){: width="650" height="860" .center-image }

<br/>

Para ver que el servicio está corriendo correctamente, ejecutamos el siguiente código

```powershell
Get-Service | findstr 'Sysmon'
```

![Sysmon Windows]({{ 'assets/img/commons/elastic-setup/15sysmon.png' | relative_url }}){: width="550" height="550" .center-image }

Con esto hemos instalado Sysmon y nos hace falta poder enviarlo a Elastic


# Instalación de Integraciones

Para **enviar todos los logs** del agent de Windows, **hay que instalar** una integración a la política existente.

<br/>
Vamos a **Management - Fleet - Agent Policy** y hacemos clic en la política que creamos de **Windows**

![Policy Windows]({{ 'assets/img/commons/elastic-setup/16policy.png' | relative_url }}){: width="750" height="750" .center-image }

Clicamos en **Add integration**

![Policy Windows]({{ 'assets/img/commons/elastic-setup/17policy.png' | relative_url }}){: width="750" height="750" .center-image }

Buscamos **Windows**, **hacemos clic** y lo **añadimos**

![Integration Windows]({{ 'assets/img/commons/elastic-setup/18integration.png' | relative_url }}){: width="750" height="750" .center-image }

![Integration Windows]({{ 'assets/img/commons/elastic-setup/19integration.png' | relative_url }}){: width="750" height="750" .center-image }


Dejamos todo como está y guardamos

![Integration Windows]({{ 'assets/img/commons/elastic-setup/20integration.png' | relative_url }}){: width="750" height="750" .center-image }

![Integration Windows]({{ 'assets/img/commons/elastic-setup/21integration.png' | relative_url }}){: width="750" height="750" .center-image }

Ahora podremos recopilar todos los logs de Sysmon de forma correcta y nos faltaría el EDR.

# Instalación del EDR

Un **EDR** (Endpoint Detection and Response) es una **solución de seguridad** diseñada para **detectar**, **investigar y responder** ante posibles **amenazas** de los endpoints conectados. Las técnicas de detección de un EDR son avanzadas y además nos provee de alertas a nuestro SIEM.

Para instalarlo hay que ir a **integraciones** y buscar **Elastic Defend**

![EDR Windows]({{ 'assets/img/commons/elastic-setup/22edr.png' | relative_url }}){: width="550" height="550" .center-image }

Añadimos **Elastic Defend**

![EDR Windows]({{ 'assets/img/commons/elastic-setup/23edr.png' | relative_url }}){: width="750" height="750" .center-image }

Lo dejamos todo por defecto

![EDR Windows]({{ 'assets/img/commons/elastic-setup/24edr.png' | relative_url }}){: width="950" height="950" .center-image }

Guardamos y desplegamos los cambios

![EDR Windows]({{ 'assets/img/commons/elastic-setup/25edr.png' | relative_url }}){: width="650" height="650" .center-image }

<br/>

## Verificación de la instalación 

Buscamos en **Security - Manage - Endpoints** que todo se haya producido correctamente

![EDR Windows]({{ 'assets/img/commons/elastic-setup/26edr.png' | relative_url }}){: width="750" height="750" .center-image }

Una vez que nos aparezca nuestro host podremos: <br/>
**Aislar la máquina**, **ejecutar comandos** con **"Respond"** para poder hacer de respuesta a incidentes (aunque no es la mejor opción) y **ver todos los detalles del host**.

![EDR Windows]({{ 'assets/img/commons/elastic-setup/27edr.png' | relative_url }}){: width="750" height="750" .center-image }

<br/>

## Añadir Reglas

Vamos a **añadir todas las reglas** que nos proporciona **Elastic Defend**
<br/>
En **Alerts** hacemos clic en **Manage Rules**

![Rules Windows]({{ 'assets/img/commons/elastic-setup/28rules.png' | relative_url }}){: width="750" height="750" .center-image }

Las **seleccionamos todas** y las **habilitamos**
![Rules Windows]({{ 'assets/img/commons/elastic-setup/29rules.png' | relative_url }}){: width="750" height="750" .center-image }



# Uso del EDR

## Detección Doble extensión 

Para ponerlo a prueba vamos a **copiar** el **ejecutable de la calculadora** y vamos a añadir de nombre **calc.jpg** para que haga de **doble extensión** y lo ejecutamos.

Al iniciar la calculadora podemos ver que ha **bloqueado la ejecución** por tener doble extensión ya que es una técnica de **Masquerading** utilizada para **engañar al usuario**.
![Calc Windows]({{ 'assets/img/commons/elastic-setup/30calc.png' | relative_url }}){: width="750" height="750" .center-image }

Vamos al **explorador de archivos** y en **vista** marcamos **Extensiones de nombre de archivo**

![Calc Windows]({{ 'assets/img/commons/elastic-setup/31calc.png' | relative_url }}){: width="750" height="750" .center-image }

Ahora podremos ver que realmente tenía **doble extensión** y estábamos intentando **enmascarar la extensión** real que era **.exe** 

![Calc Windows]({{ 'assets/img/commons/elastic-setup/32calc.png' | relative_url }}){: width="150" height="150" .center-image }


Esta incidencia ha aparecido en **Alertas** de **Security** (se ejecutó dos veces)

![Alerta Windows]({{ 'assets/img/commons/elastic-setup/33alert.png' | relative_url }}){: width="750" height="750" .center-image }

También si buscamos en Kibana **calc.jpg** nos aparece (Kibana se encuentra en **Analytics - Discover**) 

![Alerta Windows]({{ 'assets/img/commons/elastic-setup/34kibana.png' | relative_url }}){: width="750" height="750" .center-image }

## Detección RTLO

Para probar otra **técnica mas sofisticada** hemos probado **RTLO** con esta herramienta <a href="https://github.com/BlackShell256/RTLO">https://github.com/BlackShell256/RTLO</a>

Esta técnica haría **bypass al antivirus** pero sería **bloqueado por el EDR**.

![Alerta Windows]({{ 'assets/img/commons/elastic-setup/35rtlo.png' | relative_url }}){: width="750" height="750" .center-image }

![Alerta Windows]({{ 'assets/img/commons/elastic-setup/36rtlo.png' | relative_url }}){: width="750" height="750" .center-image }




Para más información de este tipo de técnicas de **Masquerading** visita: <a href="https://attack.mitre.org/techniques/T1036/" target="_blank">https://attack.mitre.org/techniques/T1036/</a>

