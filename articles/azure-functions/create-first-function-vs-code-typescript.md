---
title: 'Creación de una función de TypeScript con Visual Studio Code: Azure Functions'
description: Obtenga información sobre cómo crear una función de TypeScript y, a continuación, publicar el proyecto de Node.js local en el hospedaje sin servidor de Azure Functions con la extensión de Azure Functions en Visual Studio Code.
ms.topic: quickstart
ms.date: 11/04/2020
ms.openlocfilehash: 4470741c8c7f2f6c9832ea73aa9c6207764bc903
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "99493545"
---
# <a name="quickstart-create-a-function-in-azure-with-typescript-using-visual-studio-code"></a>Inicio rápido: Creación de una función en Azure con TypeScript mediante Visual Studio Code

[!INCLUDE [functions-language-selector-quickstart-vs-code](../../includes/functions-language-selector-quickstart-vs-code.md)]

En este artículo se usa Visual Studio Code para crear una función de TypeScript que responda a solicitudes HTTP. Después de probar el código localmente, se implementa en el entorno sin servidor de Azure Functions.

Este inicio rápido supone un pequeño costo en su cuenta de Azure. 

También hay una [versión basada en la CLI](create-first-function-cli-typescript.md) de este artículo.

## <a name="configure-your-environment"></a>Configurar su entorno

Antes de empezar, asegúrese de que cumple los siguientes requisitos:

+ Una cuenta de Azure con una suscripción activa. [Cree una cuenta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

+ Node.js [, las versiones Active LTS](https://nodejs.org/) y Maintenance LTS (se recomienda la 10.14.1). Use el comando `node --version` para comprobar la versión.  

+ [Visual Studio Code](https://code.visualstudio.com/) en una de las [plataformas admitidas](https://code.visualstudio.com/docs/supporting/requirements#_platforms).

+ La [extensión de Azure Functions](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) para Visual Studio Code.

## <a name="create-your-local-project"></a><a name="create-an-azure-functions-project"></a>Creación del proyecto local

En esta sección se usa Visual Studio Code para crear un proyecto local en Azure Functions en TypeScript. Más adelante en este artículo, publicará el código de función en Azure. 

1. Seleccione el icono de Azure en la barra de actividades y después en el área **Azure: Functions**, seleccione el icono **Crear un proyecto**.

    ![Elija Crear un proyecto.](media/functions-create-first-function-vs-code/create-new-project.png)

1. Elija una ubicación de directorio para el área de trabajo del proyecto y elija **Seleccionar**.

    > [!NOTE]
    > Estos pasos se han diseñado para completarse fuera de un área de trabajo. Por tanto, no seleccione una carpeta de proyecto que forme parte de un área de trabajo.

1. Escriba la siguiente información cuando se le indique:

    + **Seleccione un lenguaje para el proyecto de funciones**: Elija `TypeScript`.

    + **Seleccione una plantilla para la primera función del proyecto**: Elija `HTTP trigger`.

    + **Especifique un nombre de función**: Escriba `HttpExample`.

    + **Nivel de autorización**: Elija `Anonymous`, que permite que cualquier llame al punto de conexión de la función. Para obtener información sobre el nivel de autorización, consulte [Claves de autorización](functions-bindings-http-webhook-trigger.md#authorization-keys).

    + **Seleccione cómo desea que se abra el proyecto**: Elija `Add to workspace`.

1. Con esta información, Visual Studio Code genera un proyecto de Azure Functions con un desencadenador HTTP. Los archivos del proyecto locales se pueden ver en Explorer. Para obtener más información sobre los archivos que se crean, consulte [Archivos del proyecto generados](functions-develop-vs-code.md#generated-project-files). 

[!INCLUDE [functions-run-function-test-local-vs-code](../../includes/functions-run-function-test-local-vs-code.md)]

Después de comprobar que la función se ejecuta correctamente en el equipo local, es el momento de usar Visual Studio Code para publicar el proyecto directamente en Azure.

[!INCLUDE [functions-sign-in-vs-code](../../includes/functions-sign-in-vs-code.md)]

## <a name="publish-the-project-to-azure"></a>Publicar el proyecto en Azure

En esta sección, va a crear una aplicación de funciones y los recursos relacionados en su suscripción de Azure y, después, va a implementar el código.

> [!IMPORTANT]
> La publicación en una aplicación de función existente sobrescribe el contenido de esa aplicación en Azure. 

1. Seleccione el icono de Azure en la barra de actividades y después en el área **Azure: Functions**, seleccione el botón de **implementación en la aplicación de funciones**.

    ![Publicación del proyecto en Azure](../../includes/media/functions-publish-project-vscode/function-app-publish-project.png)

1. Escriba la siguiente información cuando se le indique:

    + **Seleccione la carpeta**: elija una carpeta en el área de trabajo o busque una que contenga la aplicación de funciones. No verá esta opción si ya tiene una aplicación de funciones válida abierta.

    + **Seleccione la suscripción**: elija la suscripción que desee usar. No se mostrará esta opción si solo tiene una suscripción.

    + **Seleccione la aplicación de funciones en Azure**: Elija `+ Create new Function App`. (No elija la opción `Advanced`, que no se trata en este artículo).

    + **Escriba un nombre único global para la aplicación de funciones**: Escriba un nombre que sea válido en una ruta de acceso de la dirección URL. El nombre que escriba se valida para asegurarse de que es único en Azure Functions. 

    + **Seleccione un entorno de ejecución**: Elija la versión de Node.js en la que se ha estado ejecutando localmente. Ejecute el comando `node --version` para comprobar la versión.

    + **Seleccione una ubicación para los nuevos recursos**:  Para mejorar el rendimiento, elija una [región](https://azure.microsoft.com/regions/) cerca de usted.

    La extensión muestra el estado de los recursos individuales a medida que se crean en Azure en el área de notificación.

    :::image type="content" source="../../includes/media/functions-publish-project-vscode/resource-notification.png" alt-text="Notificación de creación de recursos de Azure":::

1. Cuando se complete, se crearán los siguientes recursos de Azure en la suscripción con nombres que se basan en el nombre de la aplicación de funciones:

    [!INCLUDE [functions-vs-code-created-resources](../../includes/functions-vs-code-created-resources.md)]

    Una vez que se haya creado la aplicación de función se mostrará una notificación y se aplicará el paquete de implementación. 

    [!INCLUDE [functions-vs-code-create-tip](../../includes/functions-vs-code-create-tip.md)]

4. Seleccione **View Output** (Ver salida) en esta notificación para ver la creación y los resultados de la implementación, incluidos los recursos de Azure que ha creado. Si se pierde la notificación, seleccione el icono de campana en la esquina inferior derecha para verlo de nuevo.

    ![Creación de la notificación completa](./media/functions-create-first-function-vs-code/function-create-notifications.png)

[!INCLUDE [functions-vs-code-run-remote](../../includes/functions-vs-code-run-remote.md)]

[!INCLUDE [functions-cleanup-resources-vs-code.md](../../includes/functions-cleanup-resources-vs-code.md)]

## <a name="next-steps"></a>Pasos siguientes

Ha usado [Visual Studio Code](functions-develop-vs-code.md?tabs=javascript) para crear una aplicación de función con una función simple desencadenada por HTTP. En el siguiente artículo, va a expandir esa función mediante la conexión a Azure Storage. Para más información sobre cómo conectarse a otros servicios de Azure, consulte [Incorporación de enlaces a una función existente de Azure Functions](add-bindings-existing-function.md?tabs=typescript).   

> [!div class="nextstepaction"]
> [Conexión a una cola de Azure Storage](functions-add-output-binding-storage-queue-vs-code.md?pivots=programming-language-typescript)

[Azure Functions Core Tools]: functions-run-local.md
[Azure Functions extension for Visual Studio Code]: https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions
