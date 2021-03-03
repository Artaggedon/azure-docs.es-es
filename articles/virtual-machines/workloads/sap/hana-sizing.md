---
title: Ajuste de tamaño de SAP HANA en Azure (instancias grandes) | Microsoft Docs
description: Ajuste de tamaño de SAP HANA en Azure (instancias grandes).
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
ms.service: virtual-machines-sap
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/04/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d49191abbba9c189672be4cd8bad4346e9689bf6
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2021
ms.locfileid: "101675429"
---
# <a name="sizing"></a>Ajuste de tamaño

El ajuste de tamaño para HANA (instancias grandes) es igual que para HANA en general. Para los sistemas existentes e implementados que desea trasladar de otro RDBMS a HANA, SAP proporciona una serie de informes que se ejecutan en los sistemas SAP existentes. Si la base de datos se traslada a HANA, estos informes comprueban los datos y calculan los requisitos de memoria para la instancia de HANA. Consulte las siguientes notas de SAP para más información sobre cómo ejecutar estos informes y cómo obtener las revisiones o versiones más recientes:

- [SAP Note #1793345 - Sizing for SAP Suite on HANA](https://launchpad.support.sap.com/#/notes/1793345) (Nota de SAP 1793345: Ajuste de tamaño para SAP Suite en HANA)
- [SAP Note #1872170 - Suite on HANA and S/4 HANA sizing report](https://launchpad.support.sap.com/#/notes/1872170) (Nota de SAP 1872170: Informe de ajuste de tamaño de Suite en HANA y S/4 HANA)
- [Nota de SAP 2121330 - FAQ: SAP BW on HANA Sizing Report](https://launchpad.support.sap.com/#/notes/2121330) (Preguntas más frecuentes: informe de ajuste de tamaño de SAP BW en HANA)
- [SAP Note #1736976 - Sizing Report for BW on HANA](https://launchpad.support.sap.com/#/notes/1736976) (Nota de SAP 1736976: Informe de ajuste de tamaño para BW en HANA)
- [SAP Note #2296290 - New Sizing Report for BW on HANA](https://launchpad.support.sap.com/#/notes/2296290) (Nota de SAP 2296290: Nuevo informe de ajuste de tamaño para BW en HANA)

Para las implementaciones en green field, está disponible SAP Quick Sizer para calcular los requisitos de memoria de la implementación de software de SAP sobre HANA.

Los requisitos de memoria de HANA aumentan a medida que aumenta el volumen de datos. Tenga en cuenta el consumo de memoria actual, que le ayudará a predecir lo que va a ocurrir en el futuro. En función de los requisitos de memoria, puede asignar la demanda a una de las SKU de HANA (instancias grandes).

**Pasos siguientes**
- Consulte [Requisitos de incorporación](hana-onboarding-requirements.md).