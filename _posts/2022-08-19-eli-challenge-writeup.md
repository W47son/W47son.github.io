---
title: Eli Forensics Challenge | CyberDefenders
date: 2022-08-21 18:00:00 +0200
categories: [CTF,Forensics]
tags: [CyberDefenders,CTF,Forensics,Chromebook,Takeout]
image:
  path: /assets/img/commons/eli/Eli.png
  width: 600
  height: 400
---
<a href="https://cyberdefenders.org/blueteam-ctf-challenges/98">https://cyberdefenders.org/blueteam-ctf-challenges/98</a>
<br/>
<a target="_blank" href="https://cyberdefenders.org/blueteam-ctf-challenges/progress/W4tson/98/"><img src="https://img.shields.io/badge/eli%20chromebook%20forensic-completed-brightgreen" alt="Completed Badge"/></a>

## Detalles del reto:

Un entusiasta del lacrosse a la caza de un delicioso sándwich de pollo.
Puntos: 255

Obtenemos un zip que al descomprimirlo nos dan 2 comprimidos:

* *'2021 CTF - Chromebook.tgz'  Imagen del chromebook*
* *'2021 CTF - Takeout.zip'     Datos extraídos de la cuenta de google* <a href="https://takeout.google.com/">https://takeout.google.com/</a>

\*  **¡Atención!** Hay dos formas de hacer la sala. Puedes utilizar un **software** (cleapp,magnet axiom,etc.) para que te liste toda la información, lo puedes hacer **manualmente** o de ambas formas *

## Recursos: <a href="https://github.com/markmckinnon/cLeapp">https://github.com/markmckinnon/cLeapp</a>


## <ins>**Preguntas**</ins><br/>
### 1-The folder to store all your data in - How many files are in Eli's downloads directory?
Si vamos al directorio *./2021 CTF - Chromebook/decrypted/mount/user/Downloads* vemos que nos salen `6` archivos.

![Foto Downloads]({{ 'assets/img/commons/eli/1.png' | relative_url }}){: .center-image }
<br/>
(Con la aplicación CLEAPP solo nos muestra 2 descargas ya que solo 2 fueron realmente descargadas de una pagina web)

### 2-Smile for the camera - What is the MD5 hash of the user's profile photo?

La foto de perfil del usuario se encuentra en *./2021 CTF - Chromebook/decrypted/mount/user/Accounts/Avatar Images*

El hash de la imagen es `5ddd4fe0041839deb0a4b0252002127b`

![Foto Hash]({{ 'assets/img/commons/eli/2.png' | relative_url }}){: .center-image }

### 3-Road Trip! - What city was Eli's destination in?

Volviendo a la carpeta de Downloads tenemos una screenshot de un viaje en Google Maps.

![Foto Maps]({{ 'assets/img/commons/eli/3.png' | relative_url }}){: .center-image }

Podemos ver que se dirige a Chick-fil-A a la ciudad de `Plattsburgh` en el estado de Nueva York.

### 4-Promise Me - How many promises does Wickr make?

En la misma carpeta hay un pdf de Wickr-Customer-Security-Promises y en la página 3 nos cuentan las `9` promesas que hacen.

![Foto Pdf]({{ 'assets/img/commons/eli/4.png' | relative_url }}){: .center-image }

### 5-Key-ty Cat - What are the last five characters of the key for the Tabby Cat extension?

Buscamos el nombre de la extensión de Chrome en google para encontrar el identificador.

![Foto Extension]({{ 'assets/img/commons/eli/5.png' | relative_url }}){: .center-image }

Una vez con el identificador podemos ir a la carpeta *./2021 CTF - Chromebook/decrypted/mount/user/Extensions/mefhakmgclhhfbdadeojlkbllmecialg*, hacemos un cat al archivo *manifest.json* y obtenemos la key

```bash 
❯ cat manifest.json          
{
   "author": "Leslie Zacharkow",
   "chrome_url_overrides": {
      "newtab": "public/index.html"
   },
   "description": "A new friend in every tab.",
   "icons": {
      "128": "icon128.png"
   },
   "key": "MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAjA9ElNuqHGIuDyrztUxn0nV1CLRgjSHEKw1zfhzE/6OdWd3teRV9wq4EukIX8UKf96fhS2WsLVnTuRmF7mQuqeAIxOkSvFm4Set3YRQ5994NA8Y9un1ZGhPtjFnpoLo1pEA5PPly5/BX2y3Ci2Rb0C3wp8NdeXud4r6BKEzhPdWmkwIS+YbTWhOBR1IbP/sF6+l8EdG/Ks881p8LO0J3qepmhNDA/LjVWzJq5ARmIYeT3TnvR/qRGTY9UXfUvg3gcUWV2LBBpeil8t6Np0PVYvG44zOBhKUnYSVE6gAcXENLTJJABgRgKT1K1kDYl1N8LJZf/S/9Ix1P+wItoEinTwIDAQAB",
   "manifest_version": 2,
   "name": "Tabby Cat",
   "offline_enabled": true,
   "permissions": [ "storage" ],
   "update_url": "https://clients2.google.com/service/update2/crx",
   "version": "2.0.0"
}
```
Los últimos 5 caracteres son: `DAQAB`

### 6-Time to jam out - How many songs does Eli have downloaded?

`2` canciones en el path *./2021 CTF - Chromebook/decrypted/mount/user/MyFiles/Music*

![Foto Musica]({{ 'assets/img/commons/eli/6.png' | relative_url }}){: .center-image }

### 7-Autofill, roll out - Which word was Autofilled the most?

Para hacernos el trabajo mas fácil a partir de aquí vamos a ayudarnos de CLEAPP.

Con el reporte de CLEAPP vemos que el campo que mas se autocompletó fue: `email`
![Foto CLEAPP]({{ 'assets/img/commons/eli/7.png' | relative_url }}){: .center-image }

Esta misma información podríamos haberla sacado del archivo 'Web Data', que es una base de datos sqlite con sqlitebrowser 

### 8-Dress for success - What is this bird's image's logical size in bytes?

Aquí no entendía a que se refería con el pájaro y recordé que había una descarga de tux.

```bash
❯ du -b tux.png               
46791   tux.png
```

### 9-It's about the journey, not the destination - How many miles would the trip have been if Eli had taken a long way?

En la screenshot de google de descargas tenemos la respuesta: `81.2 millas`

![Foto Recorrido]({{ 'assets/img/commons/eli/9.png' | relative_url }}){: .center-image }

### 10-Repeat customer - What was Eli's top visited site?

Si nos dirigimos a *Chromebook Top Sites*, podemos ver que `"https://protonmail.com/"` tiene el rango más alto siendo el número 0
![Foto TopSites]({{ 'assets/img/commons/eli/10.png' | relative_url }}){: .center-image }

### 11-Vroom Vroom, What is the name of the car-related theme?

Esta pregunta no la entendí del todo. Hasta que busqué la palabra "theme" y encontré un archivo en *"./Extensions/dkkklbgbfaeockpgbkleblklmcjdbnbj/1_0/images/theme_frame.png* que mostraba un Lamborghini Rojo
![Portada Unicode]({{ 'assets/img/commons/eli/11.jpg' | relative_url }}){: .center-image }

Si leemos el manifest.json de la extensión nos da la respuesta: `Lamborghini Cherry`

### 12-You got mail - How many emails were received from notification@service.tiktok.com?

Aquí empezamos con la carpeta de **Takeout**.<br/>
Vamos a *./Takeout/Mail* y tenemos todos los emails en un mbox. Para leerlos fácilmente, los he importado al Feed en la aplicación de **ThunderBird** y en ajustes he cambiado el destino del almacenamiento de mensajes a la carpeta Mail. La respuesta son `6` emails

![Foto ThunderBird]({{ 'assets/img/commons/eli/12.png' | relative_url }}){: .center-image }


### 13-Hungry for directions - Where did the user request directions to on Mar 4, 2021, at 4:15:18 AM EDT

(Abriendo la carpeta de Takeout en el navegador se nos hace más fácil movernos por los archivos).<br/>
Encontramos esa dirección pedida en *./My Activity/Maps/MyActivity.html*<br/>
 `Chick-fil-A`<br/>
![Foto Direccion]({{ 'assets/img/commons/eli/13.png' | relative_url }}){: .center-image }

### 14-Who defines essential? - What was searched on Mar 4, 2021, at 4:09:35 AM EDT

Lo podemos encontrar en *./My Activity/Search/MyActivity.html*<br/>
`is travelling to get chicken essential travel`<br/>
![Foto Busqueda]({{ 'assets/img/commons/eli/14.png' | relative_url }}){: .center-image }

### 15-I got three subscribers, and counting - How many YouYube channels is the user subscribed to?

En el json que encontramos en *./YouTube and YouTube Music/subscriptions/subscriptions.json* obtenemos `0`
```json
[]
```

### 16-Time flies when you're watching YT - What date was the first YouTube video the user watched uploaded?

Los videos vistos se encuentran en *./YouTube and YouTube Music/history/watch-history.html* y el primer video fue visto el Feb 3, 2021 y era de Lacrosse pero lo que realmente pregunta es cuando fue subido ese video visto. El video es <a href="https://www.youtube.com/watch?v=R0UhA3fwz_g">https://www.youtube.com/watch?v=R0UhA3fwz_g</a> y se subió el `27-01-2021`

### 17-How much? - What is the price of the belt?

Aquí se entiende que el usuario ha pagado o buscado un cinturón y así es, lo ha buscado. En *./My Activity/Chrome/MyActivity.html*
<br/>
![Foto Busqueda]({{ 'assets/img/commons/eli/17.1.png' | relative_url }}){: .center-image }
<br/>
Hacemos clic en la página web y nos aparece como Agotado y no tenemos precio alguno.
<br/>
![Foto SinStock]({{ 'assets/img/commons/eli/17.2.png' | relative_url }}){: .center-image }
<br/>
Sin embargo, podemos ver en el html el precio original que aparecía antes que es `98.5`
<br/>
![Foto Precio]({{ 'assets/img/commons/eli/17.3.png' | relative_url }}){: .center-image }
<br/>

¡Gracias por haber leído mi primer writeup!


