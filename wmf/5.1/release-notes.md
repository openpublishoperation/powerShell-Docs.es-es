---
title: "Notas de la versión 5.1 de WMF (versión preliminar)"
ms.date: 2016-07-27
keywords: PowerShell, DSC, WMF
description: 
ms.topic: article
author: keithb
manager: dongill
ms.prod: powershell
ms.technology: WMF
translationtype: Human Translation
ms.sourcegitcommit: 5eb9eae6257cdb57f4f778b5dddf5aa7ef9d10bb
ms.openlocfilehash: 12f2c084ab92134b733ee037c3d9fbd512af2e4c

---

# Notas de la versión preliminar de Windows Management Framework (WMF) 5.1 #

La versión preliminar de WMF 5.1 incluye los componentes de PowerShell, WMI, WinRM e inventario y licencias de software (SIL) que se lanzan con Windows Server 2016. WMF 5.1 puede instalarse en Windows 7, Windows 8.1, Windows Server 2008 R2, 2012 y 2012 R2 y proporciona una serie de mejoras con respecto a WMF 5.0 RTM, incluidos:

- Nuevos cmdlets: usuarios locales y grupos; Get-ComputerInfo
- En las mejoras de PowerShellGet se incluyen la aplicación de módulos firmados y la instalación de módulos JEA
- PackageManagement ha agregado compatibilidad con contenedores, configuración de CBS, configuración basada en EXE y paquetes de CAB
- Mejoras de depuración para las clases DSC y PowerShell
- Mejoras de seguridad, incluido el cumplimiento de módulos firmados por catálogos procedentes del servidor de extracción y al usar cmdlets de PowerShellGet
- Respuestas a varios problemas y varias solicitudes de usuarios

**Notas importantes:**

- **La versión preliminar de WMF 5.1 requiere Windows Management Framework 4.6**. La instalación se realizará correctamente, pero se producirá un error en las características clave si no está instalado .NET 4.6. Las instrucciones están disponibles en el tema [Instalación y configuración de WMF 5.1 (versión preliminar)](https://msdn.microsoft.com/en-us/powershell/wmf/5.1/install-configure). 
- **La versión preliminar de WMF 5.1 no admite implementaciones** de producción en este momento. Está diseñado para proporcionar información anticipada sobre qué hay en la versión y darle la oportunidad de enviar comentarios al equipo de PowerShell.
- La versión preliminar de WMF 5.1 puede instalarse directamente sobre WMF 5.0.
- Es un problema conocido que, actualmente, se necesita WMF 4.0 para instalar la versión preliminar de WMF 5.1 en Windows 7 y Windows Server 2008. Se espera retirar este requisito antes de la versión final.
- Instalar las versiones futuras de WMF 5.1, incluida la versión RTM, requerirá desinstalar la versión preliminar de WMF 5.1.



<!--HONumber=Jul16_HO5-->


