---
title: Cifrado de datos en reposo del servicio Personalizer
titleSuffix: Azure Cognitive Services
description: Microsoft ofrece claves de cifrado administradas por Microsoft y también le permite administrar las suscripciones de Cognitive Services con sus propias claves, llamadas claves administradas por el cliente (CMK). En este artículo se trata el cifrado de datos en reposo de Personalizer y cómo habilitar y administrar las CMK.
author: erindormier
manager: venkyv
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 08/28/2020
ms.author: egeaney
ms.openlocfilehash: 1a27199930587c1a096dd99462ebd0c9d65054ee
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "100524066"
---
# <a name="personalizer-service-encryption-of-data-at-rest"></a>Cifrado de datos en reposo del servicio Personalizer

El servicio Personalizer cifra automáticamente los datos al guardarlos en la nube. El cifrado del servicio Personalizer protege los datos y le ayuda a satisfacer los requisitos de cumplimiento y de seguridad de la organización.

[!INCLUDE [cognitive-services-about-encryption](../includes/cognitive-services-about-encryption.md)]

> [!IMPORTANT]
> Las claves administradas por el cliente solo están disponibles en el plan de tarifa E0. Para solicitar la capacidad de usar claves administradas por el cliente, rellene y envíe el [formulario de solicitud de claves administradas por el cliente del servicio Personalizer](https://aka.ms/cogsvc-cmk). Tardará de tres a cinco días hábiles aproximadamente en recibir una respuesta sobre el estado de la solicitud. En función de la demanda, es posible que se coloque en una cola y se apruebe a medida que haya espacio disponible. Una vez recibida la aprobación para usar CMK con el servicio Personalizer, deberá crear un nuevo recurso de Personalizer y seleccionar el plan de tarifa E0. Después de crear el recurso de Personalizer con el plan de tarifa E0, puede usar Azure Key Vault para configurar la identidad administrada.

[!INCLUDE [cognitive-services-cmk](../includes/configure-customer-managed-keys.md)]

## <a name="next-steps"></a>Pasos siguientes

* [Formulario de solicitud de claves administradas por el cliente del servicio Personalizer](https://aka.ms/cogsvc-cmk)
* [Más información sobre Azure Key Vault](../../key-vault/general/overview.md)