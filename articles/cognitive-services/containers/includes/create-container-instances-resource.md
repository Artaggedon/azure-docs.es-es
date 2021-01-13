---
title: Compatibilidad con los contenedores
titleSuffix: Azure Cognitive Services
description: Aprenda cómo crear un recurso de instancia de contenedor de Azure.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: 24f6052c436b73d0075371fa74160d21826e2209
ms.sourcegitcommit: aeba98c7b85ad435b631d40cbe1f9419727d5884
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/04/2021
ms.locfileid: "97865958"
---
## <a name="create-an-azure-container-instance-resource-using-the-azure-portal"></a>Creación de un recurso de instancia de contenedor de Azure con Azure Portal

1. Vaya a la página [Crear](https://ms.portal.azure.com/#create/Microsoft.ContainerInstances) de las instancias de contenedor.

2. En la pestaña **Datos básicos**, escriba la siguientes información:

    |Configuración|Value|
    |--|--|
    |Suscripción|Seleccione su suscripción.|
    |Resource group|Seleccione el grupo de recursos disponible o cree uno nuevo, como `cognitive-services`.|
    |Nombre del contenedor|Escriba un nombre como `cognitive-container-instance`. Este nombre debe estar en minúsculas.|
    |Location|Seleccione una región para la implementación.|
    |Tipo de imagen|Si la imagen de contenedor está almacenada en un registro de contenedor que no requiere credenciales, elija `Public`. Si el acceso a la imagen de contenedor requiere credenciales, elija `Private`. Consulte los [repositorios de contenedores y las imágenes](../container-image-tags.md) para información sobre si la imagen de contenedor es `Public` o `Private` ("versión preliminar pública"). |
    |Nombre de la imagen|Escriba la ubicación del contenedor de Cognitive Services. La ubicación es lo que se usa como argumento para el comando `docker pull`. Consulte las [imágenes y los repositorios de contenedor](../container-image-tags.md) para ver los nombres de imagen disponibles y su repositorio correspondiente.<br><br>El nombre de la imagen debe estar completo y especificar tres partes. La primera, el registro de contenedor, después el repositorio y, por último, el nombre de la imagen: `<container-registry>/<repository>/<image-name>`.<br><br>Este es un ejemplo, `mcr.microsoft.com/azure-cognitive-services/keyphrase` representaría la imagen de extracción de frases clave en el registro de contenedor de Microsoft del repositorio de Azure Cognitive Services. Otro ejemplo, `containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text` representaría la imagen de conversión de voz en texto en el repositorio de Microsoft del registro de contenedor de la versión preliminar del contenedor. |
    |Tipo de SO|`Linux`|
    |Size|Seleccione el tamaño de las recomendaciones sugeridas para su contenedor específico de Cognitive Services:<br>2 núcleos de CPU<br>4 GB

3. En la pestaña **Redes**, escriba los siguientes detalles:

    |Configuración|Value|
    |--|--|
    |Puertos|Establezca el puerto TCP en `5000`. Expone el contenedor en el puerto 5000.|

4. En la pestaña **Avanzado**, escriba las **Variables de entorno** necesarias para la configuración de la facturación del recurso de la instancia de contenedor de Azure:

    | Clave | Value |
    |--|--|
    |`ApiKey`|Se copia de la página **Keys y endpoint** (Claves y punto de conexión) del recurso. Es una cadena de 32 caracteres alfanuméricos sin espacios ni guiones, `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`.|
    |`Billing`| La dirección URL del punto de conexión se copia de la página **Keys and endpoint** (Claves y punto de conexión) del recurso.|
    |`Eula`|`accept`|

5. Haga clic en **Revisar y crear**.
6. Después de realizar correctamente la validación, haga clic en **Crear** para finalizar el proceso de creación.
7. Cuando el recurso se implemente con éxito, estará listo.
