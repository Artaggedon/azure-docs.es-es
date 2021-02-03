---
title: 'Inicio rápido: Biblioteca cliente de Computer Vision para .NET'
description: En este inicio rápido, se muestra una introducción a la biblioteca cliente de Computer Vision para .NET.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: include
ms.date: 12/15/2020
ms.author: pafarley
ms.custom: devx-track-csharp
ms.openlocfilehash: 58ad4acaa60abe56849490df39c91949abec2f5d
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2021
ms.locfileid: "98947979"
---
<a name="HOLTop"></a>

La biblioteca cliente de Computer Vision se usa para:

* Analizar una imagen para ver las etiquetas, la descripción de texto, las caras, el contenido para adultos, etc.
* Lea texto impreso y manuscrito con Read API.

[Documentación de referencia](/dotnet/api/overview/azure/cognitiveservices/client/computervision) | [Código fuente de la biblioteca](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Vision.ComputerVision) | [Paquete (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.ComputerVision/) | [Ejemplos](https://azure.microsoft.com/resources/samples/?service=cognitive-services&term=vision&sort=0)

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción a Azure: [cree una cuenta gratuita](https://azure.microsoft.com/free/cognitive-services/).
* El [IDE de Visual Studio](https://visualstudio.microsoft.com/vs/) o la versión actual de [.NET Core](https://dotnet.microsoft.com/download/dotnet-core).
* Una vez que tenga la suscripción de Azure, <a href="https://portal.azure.com/#create/Microsoft.CognitiveServicesComputerVision"  title="Creación de un recurso de Computer Vision"  target="_blank">cree un recurso de Computer Vision <span class="docon docon-navigate-external x-hidden-focus"></span></a> en Azure Portal para obtener la clave y el punto de conexión. Una vez que se implemente, haga clic en **Ir al recurso**.
    * Necesitará la clave y el punto de conexión del recurso que cree para conectar la aplicación al servicio Computer Vision. En una sección posterior de este mismo inicio rápido pegará la clave y el punto de conexión en el código siguiente.
    * Puede usar el plan de tarifa gratis (`F0`) para probar el servicio y actualizarlo más adelante a un plan de pago para producción.

## <a name="setting-up"></a>Instalación

### <a name="create-a-new-c-application"></a>Creación de una aplicación de C#

#### <a name="visual-studio-ide"></a>[IDE de Visual Studio](#tab/visual-studio)

En Visual Studio, cree una aplicación de .NET Core. 

### <a name="install-the-client-library"></a>Instalación de la biblioteca cliente 

Después de crear un proyecto, instale la biblioteca cliente; para ello, haga clic con el botón derecho en la solución del proyecto en el **Explorador de soluciones** y seleccione **Administrar paquetes NuGet**. En el administrador de paquetes que se abre, seleccione **Examinar** **Incluir versión preliminar** y busque `Microsoft.Azure.CognitiveServices.Vision.ComputerVision`. Seleccione la versión `6.0.0-preview.1` e **Instalar**. 

#### <a name="cli"></a>[CLI](#tab/cli)

En una ventana de consola (por ejemplo, cmd, PowerShell o Bash), use el comando `dotnet new` para crear una nueva aplicación de consola con el nombre `computer-vision-quickstart`. Este comando crea un sencillo proyecto "Hola mundo" de C# con un solo archivo de origen: *Program.cs*.

```console
dotnet new console -n computer-vision-quickstart
```

Cambie el directorio a la carpeta de aplicaciones recién creada. Para compilar la aplicación:

```console
dotnet build
```

La salida de la compilación no debe contener advertencias ni errores. 

```console
...
Build succeeded.
 0 Warning(s)
 0 Error(s)
...
```

### <a name="install-the-client-library"></a>Instalación de la biblioteca cliente

Dentro del directorio de aplicaciones, instale la biblioteca cliente de Computer Vision para .NET con el siguiente comando:

```console
dotnet add package Microsoft.Azure.CognitiveServices.Vision.ComputerVision --version 6.0.0
```

---

> [!TIP]
> ¿Desea ver todo el archivo de código de inicio rápido de una vez? Puede encontrarlo en [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/dotnet/ComputerVision/ComputerVisionQuickstart.cs), que contiene los ejemplos de código de este inicio rápido.

En el directorio del proyecto, abra el archivo *ComputerVisionQuickstart.cs* en el editor o IDE que prefiera. Agregue las siguientes directivas `using`:

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ComputerVision/ComputerVisionQuickstart.cs?name=snippet_using)]

En la clase **Program** de la aplicación, cree variables para el punto de conexión y la clave de Azure del recurso.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ComputerVision/ComputerVisionQuickstart.cs?name=snippet_vars)]

> [!IMPORTANT]
> Vaya a Azure Portal. Si el recurso de Computer Vision que ha creado en la sección **Requisitos previos** se ha implementado correctamente, haga clic en el botón **Ir al recurso** en **Pasos siguientes**. Puede encontrar su clave y punto de conexión en la página de **clave y punto de conexión** del recurso, en **Administración de recursos**. 
>
> Recuerde quitar la clave del código cuando haya terminado y no hacerla nunca pública. En el caso de producción, considere la posibilidad de usar alguna forma segura de almacenar las credenciales, y acceder a ellas. Para más información, consulte el artículo sobre la [seguridad](../../../cognitive-services-security.md) de Cognitive Services.

En el método `Main` de la aplicación, agregue llamadas para los métodos que se usan en este inicio rápido. Las creará más adelante.


[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ComputerVision/ComputerVisionQuickstart.cs?name=snippet_client)]

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ComputerVision/ComputerVisionQuickstart.cs?name=snippet_analyzeinmain)]

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ComputerVision/ComputerVisionQuickstart.cs?name=snippet_extracttextinmain)]

> [!div class="nextstepaction"]
> [He configurado el cliente](?success=set-up-client#object-model) [He tenido un problema](https://www.research.net/r/7QYZKHL?issue=set-up-client)

## <a name="object-model"></a>Modelo de objetos

Las siguientes clases e interfaces controlan algunas de las características principales del SDK de .NET para Computer Vision.

|Nombre|Descripción|
|---|---|
| [ComputerVisionClient](/dotnet/api/microsoft.azure.cognitiveservices.vision.computervision.computervisionclient) | Esta clase es necesaria para todas las funcionalidades de Computer Vision. Cree una instancia de ella con la información de suscripción y úsela para realizar la mayoría de las operaciones con imágenes.|
|[ComputerVisionClientExtensions](/dotnet/api/microsoft.azure.cognitiveservices.vision.computervision.computervisionclientextensions)| Esta clase contiene métodos adicionales para **ComputerVisionClient**.|
|[VisualFeatureTypes](/dotnet/api/microsoft.azure.cognitiveservices.vision.computervision.models.visualfeaturetypes)| Esta enumeración define los diferentes tipos de análisis de imágenes que se pueden realizar en una operación de análisis estándar. Debe especificar un conjunto de valores de VisualFeatureTypes en función de sus necesidades. |

## <a name="code-examples"></a>Ejemplos de código

En estos fragmentos de código se muestra cómo realizar las siguientes tareas con la biblioteca cliente de Computer Vision para .NET:

* [Autenticar el cliente](#authenticate-the-client)
* [Analizar una imagen](#analyze-an-image)
* [Leer texto manuscrito e impreso](#read-printed-and-handwritten-text)

## <a name="authenticate-the-client"></a>Autenticar el cliente

> [!NOTE]
> En este inicio rápido se da por supuesto que ha [creado variables de entorno](../../../cognitive-services-apis-create-account.md#configure-an-environment-variable-for-authentication) para la clave de Computer Vision y el punto de conexión, denominadas `COMPUTER_VISION_SUBSCRIPTION_KEY` y `COMPUTER_VISION_ENDPOINT` respectivamente.

En un nuevo método, cree una instancia de un cliente con la clave y el punto de conexión. Cree un objeto **[ApiKeyServiceClientCredentials](/dotnet/api/microsoft.azure.cognitiveservices.vision.computervision.apikeyserviceclientcredentials)** con la clave y úselo con el punto de conexión para crear un objeto **[ComputerVisionClient](/dotnet/api/microsoft.azure.cognitiveservices.vision.computervision.computervisionclient)** .

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ComputerVision/ComputerVisionQuickstart.cs?name=snippet_auth)]

> [!div class="nextstepaction"]
> [He autenticado el cliente](?success=authenticate-client#analyze-an-image) [He tenido un problema](https://www.research.net/r/7QYZKHL?issue=authenticate-client)

## <a name="analyze-an-image"></a>Análisis de una imagen

El código siguiente define un método, `AnalyzeImageUrl`, que utiliza el objeto de cliente para analizar una imagen remota e imprimir los resultados. El método devuelve una descripción de texto, categorización, lista de etiquetas, caras detectadas, marcas de contenido para adultos, colores principales y tipo de imagen.

> [!TIP]
> También puede analizar una imagen local. Consulte los métodos [ComputerVisionClient](/dotnet/api/microsoft.azure.cognitiveservices.vision.computervision.computervisionclient), como **AnalyzeImageInStreamAsync**. O bien, consulte el código de ejemplo en [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/dotnet/ComputerVision/ComputerVisionQuickstart.cs) para escenarios relacionados con imágenes locales.

### <a name="set-up-test-image"></a>Configuración de una imagen de prueba

En la clase **Program**, guarde una referencia a la dirección URL de la imagen que desea analizar.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ComputerVision/ComputerVisionQuickstart.cs?name=snippet_analyze_url)]

### <a name="specify-visual-features"></a>Especificación de características visuales

Defina el nuevo método para el análisis de imágenes. Agregue el código siguiente, que especifica qué características visuales desea extraer en el análisis. Vea la enumeración **[VisualFeatureTypes](/dotnet/api/microsoft.azure.cognitiveservices.vision.computervision.models.visualfeaturetypes)** para obtener una lista completa.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ComputerVision/ComputerVisionQuickstart.cs?name=snippet_visualfeatures)]

Inserte cualquiera de los siguientes bloques de código en el método **AnalyzeImageUrl** para implementar sus características. No olvide agregar un corchete de cierre al final.

```csharp
}
```

### <a name="analyze"></a>Analizar

El método **AnalyzeImageAsync** devuelve un objeto **ImageAnalysis** que contiene toda la información extraída.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ComputerVision/ComputerVisionQuickstart.cs?name=snippet_analyze_call)]

En las secciones siguientes se muestra cómo analizar esta información en detalle.

### <a name="get-image-description"></a>Obtención de la descripción de la imagen

El código siguiente obtiene la lista de títulos generados para la imagen. Consulte [Descripción de imágenes](../../concept-describing-images.md) para más detalles.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ComputerVision/ComputerVisionQuickstart.cs?name=snippet_describe)]

### <a name="get-image-category"></a>Obtención de la categoría de imagen

El código siguiente obtiene la categoría detectada de la imagen. Consulte [Categorización de imágenes](../../concept-categorizing-images.md) para más detalles.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ComputerVision/ComputerVisionQuickstart.cs?name=snippet_categorize)]

### <a name="get-image-tags"></a>Obtención de etiquetas de imagen

El código siguiente obtiene el conjunto de las etiquetas detectadas en la imagen. Consulte [Etiquetas de contenido](../../concept-tagging-images.md) para más detalles.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ComputerVision/ComputerVisionQuickstart.cs?name=snippet_tags)]

### <a name="detect-objects"></a>Detección de objetos

El código siguiente detecta objetos comunes en la imagen y los imprime en la consola. Consulte [Detección de objetos](../../concept-object-detection.md) para más información.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ComputerVision/ComputerVisionQuickstart.cs?name=snippet_objects)]

### <a name="detect-brands"></a>Detección de marcas

El código siguiente detecta marcas corporativas y logotipos en la imagen y los imprime en la consola. Consulte [Detección de marcas](../../concept-brand-detection.md) para más información.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ComputerVision/ComputerVisionQuickstart.cs?name=snippet_brands)]

### <a name="detect-faces"></a>Detección de caras

El código siguiente devuelve las caras detectadas en la imagen con sus coordenadas de rectángulo y selecciona los atributos de cara. Consulte [Detección de caras](../../concept-detecting-faces.md) para más información.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ComputerVision/ComputerVisionQuickstart.cs?name=snippet_faces)]

### <a name="detect-adult-racy-or-gory-content"></a>Detección de contenido para adultos, explícito o sangriento

El siguiente código imprime la presencia detectada de contenido para adultos en la imagen. Para más información, consulte [Contenido para adultos, subido de tono y sangriento](../../concept-detecting-adult-content.md).

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ComputerVision/ComputerVisionQuickstart.cs?name=snippet_adult)]

### <a name="get-image-color-scheme"></a>Obtención de la combinación de colores de imagen

El código siguiente imprime los atributos de color detectados en la imagen, como los colores dominantes y el color de énfasis. Consulte [Combinaciones de colores](../../concept-detecting-color-schemes.md) para más detalles.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ComputerVision/ComputerVisionQuickstart.cs?name=snippet_color)]

### <a name="get-domain-specific-content"></a>Obtención de contenido específico del dominio

Computer Vision puede usar modelos especializados para realizar análisis adicionales en las imágenes. Consulte [Contenido específico del dominio](../../concept-detecting-domain-content.md) para más detalles. 

En el código siguiente se analizan los datos sobre las celebridades detectadas en la imagen.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ComputerVision/ComputerVisionQuickstart.cs?name=snippet_celebs)]

En el código siguiente se analizan los datos sobre los paisajes detectados en la imagen.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ComputerVision/ComputerVisionQuickstart.cs?name=snippet_landmarks)]

### <a name="get-the-image-type"></a>Obtención del tipo de imagen

El código siguiente imprime información sobre el tipo de imagen (si es una imagen prediseñada o dibujo lineal).

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ComputerVision/ComputerVisionQuickstart.cs?name=snippet_type)]

> [!div class="nextstepaction"]
> [He analizado una imagen](?success=analyze-image#read-printed-and-handwritten-text) [He tenido un problema](https://www.research.net/r/7QYZKHL?issue=analyze-image)

## <a name="read-printed-and-handwritten-text"></a>Lectura de texto manuscrito e impreso

Computer Vision puede leer texto visible de una imagen y convertirlo en un flujo de caracteres. Para obtener más información sobre el reconocimiento de texto, consulte la documentación conceptual sobre el [reconocimiento óptico de caracteres (OCR)](../../concept-recognizing-text.md#read-api). En el código de esta sección se usa la versión más reciente de la [versión 3.0 del SDK Computer Vision para lectura](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.ComputerVision/6.0.0-preview.1) y se define un método, `BatchReadFileUrl`, que usa el objeto de cliente para detectar y extraer texto en la imagen.

> [!TIP]
> También puede extraer texto de una imagen local. Consulte los métodos [ComputerVisionClient](/dotnet/api/microsoft.azure.cognitiveservices.vision.computervision.computervisionclient), como **ReadInStreamAsync**. O bien, consulte el código de ejemplo en [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/dotnet/ComputerVision/ComputerVisionQuickstart.cs) para escenarios relacionados con imágenes locales.

### <a name="set-up-test-image"></a>Configuración de una imagen de prueba

En la clase **Program**, guarde una referencia de la dirección URL de la imagen de la que quiere extraer texto. Este fragmento de código incluye imágenes de ejemplo para texto impreso y manuscrito.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ComputerVision/ComputerVisionQuickstart.cs?name=snippet_readtext_url)]

### <a name="call-the-read-api"></a>Llamada a la API Read

Agregue el método siguiente, que llama al método **ReadAsync** para la imagen especificada. Esto devuelve un identificador de operación e inicia un proceso asincrónico para leer el contenido de la imagen. A continuación, obtenga el identificador de operación devuelto por la llamada a **ReadAsync** y úselo para sondear los resultados de la operación en el servicio. Por último, imprima el texto extraído en la consola.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ComputerVision/ComputerVisionQuickstart.cs?name=snippet_read_url)]

## <a name="run-the-application"></a>Ejecución de la aplicación

#### <a name="visual-studio-ide"></a>[IDE de Visual Studio](#tab/visual-studio)

Ejecute la aplicación haciendo clic en el botón **Depurar** en la parte superior de la ventana del IDE.

#### <a name="cli"></a>[CLI](#tab/cli)

Ejecute la aplicación desde el directorio de la aplicación con el comando `dotnet run`.

```dotnet
dotnet run
```

---

## <a name="clean-up-resources"></a>Limpieza de recursos

Si quiere limpiar y eliminar una suscripción a Cognitive Services, puede eliminar el recurso o grupo de recursos. Al eliminar el grupo de recursos, también se elimina cualquier otro recurso que esté asociado a él.

* [Portal](../../../cognitive-services-apis-create-account.md#clean-up-resources)
* [CLI de Azure](../../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
>[Referencia de la API Computer Vision (.NET)](/dotnet/api/overview/azure/cognitiveservices/client/computervision)

* [¿Qué es Computer Vision?](../../overview.md)
* El código fuente de este ejemplo está disponible en [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/dotnet/ComputerVision/ComputerVisionQuickstart.cs).
