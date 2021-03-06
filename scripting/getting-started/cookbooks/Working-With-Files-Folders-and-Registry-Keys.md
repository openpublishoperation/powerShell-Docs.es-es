---
title: Trabajar con archivos, carpetas y claves del Registro
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: e6cf87aa-b5f8-48d5-a75a-7cb7ecb482dc
translationtype: Human Translation
ms.sourcegitcommit: 03ac4b90d299b316194f1fa932e7dbf62d4b1c8e
ms.openlocfilehash: c2d203fee4e1595498c666d4060e7a1060b2aa4d

---

# Trabajar con archivos, carpetas y claves del Registro
En Windows PowerShell se usa el término **Item** para hacer referencia a los elementos contenidos en una unidad de Windows PowerShell. Cuando se trabaja con el proveedor FileSystem de Windows PowerShell, un **Item** puede ser un archivo, una carpeta o la unidad de Windows PowerShell. Enumerar estos elementos y trabajar con ellos son tareas críticas básicas en la mayoría de las configuraciones administrativas, de modo que conviene abordar estas tareas en profundidad.

### Enumeración de archivos, carpetas y claves del Registro (Get\-ChildItem)
Dado que la obtención de una colección de elementos desde una ubicación determinada es una tarea común, el cmdlet **Get**ChildItem\- está diseñado específicamente para devolver todos los elementos que se encuentren dentro de un contenedor, como, por ejemplo, una carpeta.

Si desea que se devuelvan todos los archivos y carpetas contenidos directamente dentro de la carpeta C:\\Windows, escriba:

```
PS> Get-ChildItem -Path C:\Windows
    Directory: Microsoft.Windows PowerShell.Core\FileSystem::C:\Windows
Mode                LastWriteTime     Length Name
----                -------------     ------ ----
-a---        2006-05-16   8:10 AM          0 0.log
-a---        2005-11-29   3:16 PM         97 acc1.txt
-a---        2005-10-23  11:21 PM       3848 actsetup.log
...
```

La lista es similar a lo que se obtendría si se especificara el comando **dir** en **Cmd.exe** o el comando **ls** en un shell de comandos de UNIX.

Mediante el uso de los parámetros del cmdlet **Get\-ChildItem**, se pueden confeccionar listas muy complejas. Pasemos a ver algunos escenarios sobre esto. La sintaxis del cmdlet **Get\-ChildItem** se puede ver escribiendo:

```
PS> Get-Command -Name Get-ChildItem -Syntax
```

Estos parámetros se pueden mezclar y relacionar para obtener resultados muy personalizados.

#### Lista de todos los elementos contenidos (\-Recurse)
Para ver tanto los elementos que hay dentro de una carpeta de Windows como todos los elementos de sus subcarpetas, use el parámetro **Recurse** de **Get\-ChildItem**. La lista muestra todos los elementos dentro de la carpeta de Windows y los elementos de sus subcarpetas. Por ejemplo:

```
PS> Get-ChildItem -Path C:\WINDOWS -Recurse

    Directory: Microsoft.Windows PowerShell.Core\FileSystem::C:\WINDOWS
    Directory: Microsoft.Windows PowerShell.Core\FileSystem::C:\WINDOWS\AppPatch
Mode                LastWriteTime     Length Name
----                -------------     ------ ----
-a---        2004-08-04   8:00 AM    1852416 AcGenral.dll
...
```

#### Filtro de elementos por nombre (\-Name)
Para mostrar solo los nombres de los elementos, use el parámetro **Name** de **Get\-Childitem**:

```
PS> Get-ChildItem -Path C:\WINDOWS -Name
addins
AppPatch
assembly
...
```

#### Lista forzosa de elementos ocultos (\-Force)
Los elementos que normalmente no se ven en el Explorador de archivos o en Cmd.exe no se muestran en la salida de un comando **Get\-ChildItem**. Para mostrar los elementos ocultos, use el parámetro **Force** de **Get\-ChildItem**. Por ejemplo:

```
Get-ChildItem -Path C:\Windows -Force
```

Este parámetro se llama Force porque permite anular de manera forzada el comportamiento normal del comando **Get\-ChildItem**. Force es un parámetro muy usado que fuerza una acción que un cmdlet normalmente no realizaría, si bien no llevará a cabo ninguna acción que ponga en peligro la seguridad del sistema.

#### Buscar nombres de elemento coincidentes con caracteres comodín
**El comando Get\-ChildItem** acepta caracteres comodín en la ruta de acceso de los elementos que se enumeran.

Dado que las coincidencias con caracteres comodín se controla mediante el motor de Windows PowerShell, todos los cmdlets que acepten caracteres comodín usan la misma notación y tienen el mismo comportamiento de búsqueda de coincidencias. La notación de caracteres comodín de Windows PowerShell conlleva las siguientes reglas:

-   El asterisco (\*) coincide con cero o más repeticiones de cualquier carácter.

-   El signo de interrogación de cierre (?) coincide exactamente con un carácter.

-   Los caracteres de corchete de apertura (\[) y corchete de cierre (]) rodean un juego de caracteres que debe coincidir.

A continuación presentamos algunos ejemplos de cómo funciona la especificación de caracteres comodín.

Escriba el siguiente comando para encontrar todos los archivos en el directorio de Windows con el sufijo **.log** y exactamente cinco caracteres en el nombre base:

```
PS> Get-ChildItem -Path C:\Windows\?????.log
    Directory: Microsoft.Windows PowerShell.Core\FileSystem::C:\Windows
Mode                LastWriteTime     Length Name
----                -------------     ------ ----
...
-a---        2006-05-11   6:31 PM     204276 ocgen.log
-a---        2006-05-11   6:31 PM      22365 ocmsn.log
...
-a---        2005-11-11   4:55 AM         64 setup.log
-a---        2005-12-15   2:24 PM      17719 VxSDM.log
...
```

Escriba lo siguiente para encontrar todos los archivos que comienzan por la letra **x** en el directorio de Windows:

```
Get-ChildItem -Path C:\Windows\x*
```

Escriba lo siguiente para encontrar todos los archivos cuyos nombres comiencen por **x** o **z**:

```
Get-ChildItem -Path C:\Windows\[xz]*
```

#### Exclusión de elementos (\-Exclude)
Puede excluir elementos concretos mediante el parámetro **Exclude** de Get\-ChildItem. Esto le permite realizar filtrados complejos en una sola instrucción.

Por ejemplo, imaginemos que estamos buscando el archivo DLL del Servicio de hora de Windows en la carpeta System32, y todo lo que recuerda del nombre del archivo DLL es que comienza por "W" y contiene "32".

Una expresión como **w\&#42;32\&#42;.dll** buscará todos los archivos DLL que cumplan las condiciones, pero también pueden devolver los archivos DLL de compatibilidad con Windows 95 y con Windows de 16 bits que incluyan "95" o "16" en sus nombres. Puede omitir todos los archivos que contengan cualquiera de estos números en sus nombres, para lo que debe usar el parámetro **Exclude** con el patrón **\&#42;\[9516]\&&#42;**:

<pre>PS> Get-ChildItem -Path C:\WINDOWS\System32\w*32*.dll -Exclude *[9516]* Directory: Microsoft.PowerShell.Core\FileSystem::C:\WINDOWS\System32 Mode                LastWriteTime     Length Name ----                -------------     ------ ---- -a---        2004-08-04   8:00 AM     174592 w32time.dll -a---        2004-08-04   8:00 AM      22016 w32topl.dll -a---        2004-08-04   8:00 AM     101888 win32spl.dll -a---        2004-08-04   8:00 AM     172032 wldap32.dll -a---        2004-08-04   8:00 AM     264192 wow32.dll -a---        2004-08-04   8:00 AM      82944 ws2_32.dll -a---        2004-08-04   8:00 AM      42496 wsnmp32.dll -a---        2004-08-04   8:00 AM      22528 wsock32.dll -a---        2004-08-04   8:00 AM      18432 wtsapi32.dll</pre>

#### Mezcla de parámetros de Get\-ChildItem
Se pueden usar varios de los parámetros del cmdlet **Get\-ChildItem** en el mismo comando. Antes de mezclar parámetros, asegúrese de que comprende el concepto de búsqueda de coincidencias con caracteres comodín. Por ejemplo, el siguiente comando no devuelve ningún resultado:

```
PS> Get-ChildItem -Path C:\Windows\*.dll -Recurse -Exclude [a-y]*.dll
```

No hay ningún resultado, a pesar de que hay dos archivos DLL que comienzan por la letra "z" en la carpeta de Windows.

No se devolvieron resultados porque hemos especificado el carácter comodín como parte de la ruta de acceso. Aunque el comando era recursivo, el cmdlet **Get\-ChildItem** restringió los elementos a aquellos que están en la carpeta de Windows y cuyos nombres terminan en ".dll".

Para especificar una búsqueda recursiva de archivos cuyos nombres coincidan con un patrón especial, use el parámetro **\-Include**.

```
PS> Get-ChildItem -Path C:\Windows -Include *.dll -Recurse -Exclude [a-y]*.dll

    Directory: Microsoft.Windows PowerShell.Core\FileSystem::C:\Windows\System32\Setup

Mode                LastWriteTime     Length Name
----                -------------     ------ ----
-a---        2004-08-04   8:00 AM       8261 zoneoc.dll

    Directory: Microsoft.Windows PowerShell.Core\FileSystem::C:\Windows\System32

Mode                LastWriteTime     Length Name
----                -------------     ------ ----
-a---        2004-08-04   8:00 AM     337920 zipfldr.dll
```




<!--HONumber=Jun16_HO4-->


