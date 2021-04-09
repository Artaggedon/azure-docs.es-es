---
title: Evaluación de grandes cantidades de servidores físicos para la migración a Azure con Azure Migrate | Microsoft Docs
description: Describe cómo evaluar grandes cantidades de servidores físicos para su migración a Azure mediante el servicio Azure Migrate.
author: rashi-ms
ms.author: rajosh
ms.manager: abhemraj
ms.topic: how-to
ms.date: 01/19/2020
ms.openlocfilehash: 232475c50ab56fe6fb7a39a3497a8de3947fe851
ms.sourcegitcommit: f611b3f57027a21f7b229edf8a5b4f4c75f76331
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2021
ms.locfileid: "104780317"
---
# <a name="assess-large-numbers-of-physical-servers-for-migration-to-azure"></a>Evaluación de grandes cantidades de servidores físicos para la migración a Azure

En este artículo se describe cómo evaluar grandes cantidades de servidores físicos locales para su migración a Azure mediante Azure Migrate y la herramienta de detección y evaluación.

[Azure Migrate](migrate-services-overview.md) proporciona un centro de herramientas que le ayuda a detecta las aplicaciones, la infraestructura y las cargas de trabajo, a evaluarlas y a migrarlas a Microsoft Azure. Este centro incluye herramientas de Azure Migrate y ofertas de fabricantes de software independientes (ISV) de terceros. 


En este artículo aprenderá a:
> [!div class="checklist"]
> * Planificar la evaluación a escala.
> * Configure los permisos de Azure y prepare los servidores físicos para la evaluación.
> * Crear un proyecto de Azure Migrate y crear una evaluación.
> * Revisar la evaluación mientras planea la migración.


> [!NOTE]
> Si quiere probar una prueba de concepto para evaluar un par de servidores antes de realizar la evaluación a escala, siga nuestra [serie de tutoriales](./tutorial-discover-physical.md).

## <a name="plan-for-assessment"></a>Planeación de la evaluación

Al planear la evaluación de un gran número de servidores físicos, hay un par de cosas que hay que tener en cuenta:

- **Planeación de proyectos de Azure Migrate**: Averigüe cómo implementar proyectos de Azure Migrate. Por ejemplo, si los centros de datos están en zonas geográficas diferentes, o si necesita almacenar metadatos relacionados con la detección, la evaluación o la migración en una zona geográfica diferente, es posible que necesite varios proyectos.
- **Planear dispositivos**: Azure Migrate usa un dispositivo de Azure Migrate local, implementado en un servidor de Windows, para detectar continuamente servidores para la evaluación y migración. El dispositivo supervisa los cambios de entorno, como la incorporación de servidores, discos o adaptadores de red. También envía metadatos y datos de rendimiento acerca de ellos a Azure. Debe averiguar el número de dispositivos que se van a implementar.


## <a name="planning-limits"></a>Límites de planeación
 
Use los límites resumidos en esta tabla para la planeación.

**Planeamiento** | **Límites**
--- | --- 
**Proyectos de Azure Migrate** | Evalúe un máximo de 35 000 en un proyecto.
**Dispositivo con Azure Migrate** | Un dispositivo puede detectar hasta 1000 servidores.<br/> Un dispositivo solo se puede asociar con un único proyecto de Azure Migrate.<br/> Se puede asociar cualquier número de dispositivos a un solo proyecto de Azure Migrate. <br/><br/> 
**Grupo** | Puede agregar hasta 35 000 servidores en un solo grupo.
**Evaluación de Azure Migrate** | Puede evaluar hasta 35 000 máquinas virtuales en una única evaluación.


## <a name="other-planning-considerations"></a>Otras consideraciones de planeación

- Para iniciar la detección desde el dispositivo, tiene que seleccionar cada servidor físico. 

## <a name="prepare-for-assessment"></a>Preparación para la evaluación

Preparación de Azure y de los servidores físicos para la herramienta de detección y evaluación:  

1. Compruebe [los requisitos y las limitaciones de compatibilidad de los servidores físicos](migrate-support-matrix-physical.md).
2. Configure los permisos de la cuenta de Azure para interactuar con Azure Migrate.
3. Prepare los servidores físicos.

Siga las instrucciones de [este tutorial](./tutorial-discover-physical.md) para definir estas configuraciones.

## <a name="create-a-project"></a>Crear un proyecto

Según los requisitos de planeación, haga lo siguiente:

1. Cree un proyecto de Azure Migrate.
2. Agregue a los proyectos el Azure Migrate y la herramienta de detección y evaluación.

[Más información](./create-manage-projects.md)

## <a name="create-and-review-an-assessment"></a>Creación y revisión de una evaluación

1. Cree evaluaciones para los servidores físicos.
1. Revise las evaluaciones en preparación para la planeación de la migración.

[Más información](tutorial-assess-physical.md) sobre la creación y revisión de evaluaciones.
    

## <a name="next-steps"></a>Pasos siguientes

En este artículo:
 
> [!div class="checklist"] 
> * Ha planeado el escalado de evaluaciones de Azure Migrate para servidores físicos.
> * Ha preparado Azure y los servidores físicos para la evaluación.
> * Ha creado un proyecto de Azure Migrate y ha ejecutado las evaluaciones.
> * Revisó las evaluaciones como preparación para la migración.

Ahora, [obtenga información](concepts-assessment-calculation.md) sobre cómo se calculan las evaluaciones y cómo [modificarlas](how-to-modify-assessment.md).