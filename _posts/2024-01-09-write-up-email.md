---
title: "Write-Up The Greenholt Phish"
date: 2024-01-09 20:20:00 +0800
categories: [Write-Up, TryHackMe]
tags: [write-up,ctf,tryhackme,phishing]
---

Bueno vamos a ver la resolución de [The Greenholt Phish](https://tryhackme.com/room/phishingemails5fgjlzxc).
Es una maquina **facil** para usuarios de **pago** sobre el  analisis de un email que tiene pinton que es phishing  

## Contexto: 
> A Sales Executive at Greenholt PLC received an email that he didn't expect to receive from a customer. He claims that the customer never uses generic greetings such as "Good day" and didn't expect any amount of money to be transferred to his account. The email also contains an attachment that he never requested. He forwarded the email to the SOC (Security Operations Center) department for further investigation.                                 

En algunas salas de TryHackMe, nos proporcionan un tipo de contexto o historia, en este caso, un Analista de Seguridad. En la situación, un ejecutivo de tu empresa ha enviado un correo electrónico sospechoso al SOC.
Tendrás que determinar si es un correo legítimo o no.

## Preguntas

En este caso, el ejercicio nos proporciona un archivo de texto con el código del correo. Podemos cambiar la extensión a **.eml** y abrirlo con Thunderbird para su análisis. Después, analizaremos la cabeceras.


Las 4 primeras son un regalo pero vamso a verlo con una captura sencilla:

*What date was the email received? (answer format: M/DD/YY)*

*Who is the email from?*

*What is his email address?*

*What email address will receive a reply to this email?*

No creo que la fecha, el remitente, su correo y el campo Reply-To sean muy difíciles de encontrar. De hecho, cualquier usuario podría decírtelo sin problema.

![Respuestas 1-4](/assets/img/write-up/phis1/1-4.png)

Para las siguientes preguntas yo use la herramienta PhishTool pero podeis usar la querais 

Es bastante intuitiva, así que veis revisando una vez tengáis el archivo subido y encontraréis las respuestas fácilmente, seguro.

![PhishTool](/assets/img/write-up/phis1/phishtool.png)


*What is the Originating IP?*

![OriginatingIP](/assets/img/write-up/phis1/originating.png)

*Who is the owner of the Originating IP? (Do not include the "." in your answer.)*

Me he dado cuenta de que se me ha colado la IP, pero bueno, ahí la tenéis de gratis. La gracia es que, aunque sigáis tutoriales como este u otros, hagáis vosotros las cosas para ir recordando y ver los pasos a seguir para llegar a la información.

![OriginatingName](/assets/img/write-up/phis1/organizacion.png)


*What is the SPF record for the Return-Path domain?*


En el caso de SPF y DMARC son mecanismos de autenticación de correo electrónico utilizados para mejorar la seguridad y prevenir el correo fraudulento, aisque etsa en la pestaña *Security*

![SPF](/assets/img/write-up/phis1/SPF.png)

*What is the DMARC record for the Return-Path domain?*

![DMARC](/assets/img/write-up/phis1/DMARC.png)

*What is the name of the attachment?*

![adjunto](/assets/img/write-up/phis1/adjunto.png)

*What is the SHA256 hash of the file attachment?*

En esta pregunta, el hash que me aparecía en PhishTool no era válido, así que tiramos un **sha256sum** en la consola de la VM y lo tenemos:

![hash](/assets/img/write-up/phis1/hash.png)


*What is the attachments file size? (Don't forget to add "KB" to your answer, NUM KB)*

Aquí tampoco nos podemos fiar ni de PhishTool ni del propio sistema de Linux, así que tiramos de VirusTotal haciendo una búsqueda por el hash y lo tenemos:

![size](/assets/img/write-up/phis1/size.png)

*What is the actual file extension of the attachment?*

Aquí lo mismo, lo podemos ver en VirusTotal o con el comando **file** en Linux.

![extension](/assets/img/write-up/phis1/extension.png)

En los próximos Write Up intentaré ser más detallado, pero creo que en este caso es bastante asequible y sencillo, y no necesita mucha explicación. Espero que os haya sido útil, gustado o entretenido.