---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: c3f913aa3de1b723cd4eae70ff8e578a8abbec12
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "95560864"
---
#### <a name="to-create-a-new-service"></a>Para crear un nuevo servicio
1. Use las credenciales de su cuenta de Microsoft para iniciar sesión en el [Portal de Microsoft Azure Government](https://portal.azure.us/).
2. En el Portal de Azure Government, haga clic en **+** y, una vez en Marketplace, haga clic en **Ver todo**. Busque el dispositivo _físico de StorSimple_. Seleccione y haga clic en **Serie física de dispositivos de StorSimple** y, a continuación, haga clic en **Crear**. O bien, en el portal Government, haga clic en **+** y, una vez en **Almacenamiento**, haga clic en **Serie física de dispositivos de StorSimple**.
3. En la hoja **Administrador de dispositivos de StorSimple**, realice los siguientes pasos:
   
   1. Proporcione un valor único en **Nombre de recurso** para el servicio. Este nombre es un nombre descriptivo que sirve para identificar el servicio. El nombre puede tener entre 2 y 50 caracteres que pueden ser letras, números y guiones. El nombre debe empezar y terminar con una letra o un número.
   2. Elija una **Suscripción** en la lista desplegable. La suscripción está vinculada a la cuenta de facturación. Este campo no está presente si tiene una sola suscripción.
   3. Como **Grupo de recursos**, elija la opción **Usar existente** o **Crear nuevo**. Para más información, consulte [Grupos de recursos en Azure](../articles/azure-resource-manager/management/manage-resource-groups-portal.md).
   4. Proporcione una **Ubicación** para el servicio. La ubicación hace referencia a la región geográfica donde desea implementar el dispositivo. Seleccione **USGov Iowa** o **USGov Virginia**.
   5. Seleccione **Crear una cuenta de almacenamiento nueva** para crear automáticamente una cuenta de almacenamiento con el servicio. Especifique un nombre para esta cuenta de almacenamiento. Si necesitas tener tus datos en una ubicación diferente, desactiva esta casilla.
   6. Active **Anclar al panel** si desea un vínculo rápido a este servicio en el panel.
   7. Haga clic en **Crear** para crear el Administrador de dispositivos de StorSimple. Se tarda unos minutos en crear el servicio. Después de que el servicio se crea correctamente, verá una notificación y se abrirá la hoja del nuevo servicio.