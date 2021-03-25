---
title: Cifrado doble de infraestructura para Azure Database for PostgreSQL
description: Más información sobre el uso del cifrado doble de infraestructura para agregar una segunda capa de cifrado con claves administradas por el servicio.
author: mksuni
ms.author: sumuth
ms.service: postgresql
ms.topic: conceptual
ms.date: 6/30/2020
ms.openlocfilehash: 83635b732318a4ada76d1d71c1ce419cae8b35e9
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "98630153"
---
# <a name="azure-database-for-postgresql-infrastructure-double-encryption"></a>Cifrado doble de infraestructura de Azure Database for PostgreSQL

Azure Database for PostgreSQL usa el [cifrado de datos en reposo](concepts-security.md#at-rest) del almacenamiento mediante claves administradas de Microsoft. Los datos, incluidas las copias de seguridad, se cifran en el disco y este cifrado está siempre activo y no se puede deshabilitar. El cifrado usa el módulo criptográfico validado FIPS 140-2 y un cifrado AES de 256 bits para el cifrado del almacenamiento de Azure.

El cifrado doble de infraestructura agrega una segunda capa de cifrado con claves administradas por el servicio. Usa el módulo criptográfico validado FIPS 140-2, pero con un algoritmo de cifrado diferente. Esto proporciona una capa adicional de protección para los datos en reposo. La clave que se usa en el cifrado doble de infraestructura también la administra el servicio Azure Database for PostgreSQL. El cifrado doble de infraestructura no está habilitado de forma predeterminada ya que el nivel adicional de cifrado puede afectar al rendimiento.

> [!NOTE]
> Esta característica solo se admite en los planes de tarifa "De uso general" y "Optimizado para memoria" de Azure Database for PostgreSQL.

El cifrado del nivel de infraestructura tiene la ventaja de que se implementa en la capa más cercana al dispositivo de almacenamiento o a los cables de red. Azure Database for PostgreSQL implementa las dos capas de cifrado mediante claves administradas por el servicio. Aunque técnicamente siga en la capa de servicio, está muy cerca del hardware que almacena los datos en reposo. También puede habilitar opcionalmente el cifrado de datos en reposo mediante el uso de una [clave administrada por el cliente](concepts-data-encryption-postgresql.md) para el servidor PostgreSQL aprovisionado.  

La implementación en los niveles de infraestructura también admite una diversidad de claves. La infraestructura debe tener en cuenta los diferentes clústeres de máquinas y redes. Por ello, se usan diferentes claves para minimizar el radio de impacto de los ataques a la infraestructura y de diversos errores de hardware y de red. 

> [!NOTE]
> El uso del cifrado doble de infraestructura afectará al rendimiento en el servidor de Azure Database for PostgreSQL debido al proceso de cifrado adicional.

## <a name="benefits"></a>Ventajas

El cifrado doble de infraestructura de Azure Database for PostgreSQL proporciona las siguientes ventajas:

1. **Diversidad de implementación de cifrado adicional**: La intención planeada de cambiar al cifrado basado en hardware diversificará aún más las implementaciones proporcionando una implementación basada en hardware además de otra basada en software.
2. **Errores de implementación**: dos capas de cifrado en el nivel de infraestructura protegen frente a los errores en el almacenamiento en caché o la administración de memoria en niveles superiores que exponen datos de texto no cifrado. Además, las dos capas también protegen frente a los errores de implementación del cifrado en general.

La combinación de estas proporciona una protección sólida frente a las amenazas comunes y los puntos débiles utilizados para atacar la criptografía.

## <a name="supported-scenarios-with-infrastructure-double-encryption"></a>Escenarios admitidos en el cifrado doble de infraestructura

Las funcionalidades de cifrado que proporciona Azure Database for PostgreSQL se pueden usar juntas. A continuación se muestra un resumen de los distintos escenarios que puede usar:

|  ##   | Cifrado predeterminado | Cifrado doble de infraestructura | Cifrado de datos con claves administradas por el cliente  |
|:------|:------------------:|:--------------------------------:|:--------------------------------------------:|
| 1     | *Sí*              | *No*                             | *No*                                         |
| 2     | *Sí*              | *Sí*                            | *No*                                         |
| 3     | *Sí*              | *No*                             | *Sí*                                        |
| 4     | *Sí*              | *Sí*                            | *Sí*                                        |
|       |                    |                                  |                                              |

> [!Important]
> - Los escenarios 2 y 4 tendrán impacto en el rendimiento en el servidor de Azure Database for PostgreSQL debido a la capa adicional de cifrado de infraestructura.
> - La configuración del cifrado doble de infraestructura para Azure Database for PostgreSQL solo se permite durante la creación del servidor. Una vez que se ha aprovisionado el servidor, no se puede cambiar el cifrado del almacenamiento. No obstante, podrá habilitar el cifrado de datos mediante claves administradas por el cliente para el servidor creado con o sin cifrado doble de infraestructura.

## <a name="limitations"></a>Limitaciones

Para Azure Database for PostgreSQL, la compatibilidad con el cifrado doble de infraestructura mediante una clave administrada por el servicio tiene las siguientes limitaciones:

* La compatibilidad con esta funcionalidad se limita a los planes de tarifa **De uso general** y **Optimizados para memoria**.
* Esta característica solo se admite en regiones y servidores que admiten almacenamiento de hasta 16 TB. Para ver la lista de regiones de Azure que admiten almacenamiento de hasta 16 TB, consulte la [documentación de almacenamiento](concepts-pricing-tiers.md#storage).

    > [!NOTE]
    > - Todos los **nuevos** servidores PostgreSQL creados en las regiones mencionadas anteriormente también admiten el cifrado de datos con claves administradas por el cliente. En este caso, los servidores creados mediante la restauración a un momento dado (PITR) o las réplicas de lectura no se califican como "nuevos".
    > - Para validar si el servidor aprovisionado admite hasta 16 TB, puede ir a la hoja del plan de tarifa en el portal y comprobar si el control deslizante del almacenamiento se puede mover hasta 16 TB. Si solo puede mover el control deslizante hasta 4 TB, es posible que el servidor no admita el cifrado con claves administradas por el cliente. No obstante, los datos están cifrados en todo momento mediante claves administradas por el servicio. Póngase en contacto con AskAzureDBforPostgreSQL@service.microsoft.com si tiene alguna pregunta.

## <a name="next-steps"></a>Pasos siguientes

Aprenda a [configurar el cifrado doble de infraestructura para Azure Database for PostgreSQL](howto-double-encryption.md).
