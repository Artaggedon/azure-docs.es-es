---
author: v-demjoh
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 05/15/2020
ms.author: v-demjoh
ms.openlocfilehash: d94b83dd658193069f24202b978d32389eb82ac1
ms.sourcegitcommit: 1f1d29378424057338b246af1975643c2875e64d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2021
ms.locfileid: "99579822"
---
## <a name="download-and-install"></a>Descargar e instalar

#### <a name="windows-install"></a>[Instalación de Windows](#tab/windowsinstall)

Siga estos pasos para instalar la CLI de Voz en Windows:

1. En Windows, necesita [Microsoft Visual C++ Redistributable para Visual Studio 2019](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads) para su plataforma. Durante la primera instalación es posible que deba reiniciar.
1. Instale [.NET Core 3.1](/dotnet/core/install/linux).
2. Para instalar la CLI de Voz para que use NuGet, escriba este comando:

    `dotnet tool install --global Microsoft.CognitiveServices.Speech.CLI --version 1.15.0`

Escriba `spx` para ver la ayuda de la CLI de Voz.

> [!NOTE]
> Como alternativa a NuGet, puede descargar y extraer [archivo zip](https://aka.ms/speech/spx-zips.zip) de la CLI de Voz, buscar y extraer la plataforma del directorio `spx-zips`, y agregar la ruta de acceso `spx` a la variable **PATH** del sistema.


### <a name="font-limitations"></a>Limitaciones de fuentes

En Windows, la CLI de Voz solo puede mostrar las fuentes disponibles para el símbolo del sistema en el equipo local.
El [terminal de Windows](https://www.microsoft.com/en-us/p/windows-terminal/9n0dx20hk701) admite todas las fuentes que genera de forma interactiva la CLI de Voz.

Si se genera la salida a un archivo, un editor de texto como el Bloc de notas o un explorador Web como Microsoft Edge también pueden mostrar todas las fuentes.

#### <a name="linux-install"></a>[Instalación de Linux](#tab/linuxinstall)

Siga estos pasos para instalar la CLI de Voz en Linux en una CPU x64:

1. Instale [.NET Core 3.1](/dotnet/core/install/linux).
2. Para instalar la CLI de Voz para que use NuGet, escriba este comando:

    `dotnet tool install --global Microsoft.CognitiveServices.Speech.CLI --version 1.15.0`

Escriba `spx` para ver la ayuda de la CLI de Voz.

> [!NOTE]
> Como alternativa a NuGet, puede descargar los binarios en [archivo zip](https://aka.ms/speech/spx-zips.zip), extraer `spx-netcore-30-linux-x64` a un directorio `~/spx` nuevo, escribir `sudo chmod +r+x spx` en el binario y agregar la ruta de acceso `~/spx` a la variable PATH del sistema.


#### <a name="docker-install-windows-linux-macos"></a>[Instalación de Docker (Windows, Linux, macOS)](#tab/dockerinstall)

Siga estos pasos para instalar la CLI de Voz en un contenedor de Docker:

1. <a href="https://www.docker.com/get-started" target="_blank">Instale Docker Desktop<span class="docon docon-navigate-external x-hidden-focus"></span></a> para su plataforma si aún no está instalado.
2. En un nuevo símbolo del sistema o terminal, escriba este comando: 
   ```console   
   docker pull msftspeech/spx
   ```
3. Escriba este comando. Debería ver la información de ayuda de la CLI de Voz: 
   ```console 
   docker run -it --rm msftspeech/spx help
   ```

### <a name="mount-a-directory-in-the-container"></a>Montaje de un directorio en el contenedor

La herramienta CLI de Voz guarda los valores de configuración como archivos y los carga al ejecutar cualquier comando (excepto los de ayuda).
Al usar la CLI de Voz dentro de un contenedor de Docker, debe montar un directorio local desde el contenedor, de modo que la herramienta pueda almacenar o buscar los valores de configuración y, por tanto, leer o escribir los archivos necesarios para el comando, como los de audio de la voz.

En Windows, escriba este comando para crear un directorio local que la CLI de Voz pueda usar desde dentro del contenedor:

`mkdir c:\spx-data`

En Linux o Mac, escriba este comando en un terminal para crear un directorio y ver su ruta de acceso absoluta:

```bash
mkdir ~/spx-data
cd ~/spx-data
pwd
```

Usará la ruta de acceso absoluta al llamar a la CLI de Voz.

### <a name="run-speech-cli-in-the-container"></a>Ejecución de la CLI de Voz en el contenedor

En esta documentación se muestra el comando `spx` de la CLI de Voz que se usa en instalaciones que no son de Docker.
Al llamar al comando `spx` en un contenedor de Docker, debe montar un directorio en el contenedor para el sistema de archivos donde la CLI de Voz pueda almacenar y buscar los valores de configuración, y leer y escribir los archivos.

En Windows, los comandos se iniciarán de la siguiente manera:

```console
docker run -it -v c:\spx-data:/data --rm msftspeech/spx
```

En Linux o macOS, los comandos tendrán un aspecto similar al del ejemplo siguiente. Reemplace `ABSOLUTE_PATH` por la ruta de acceso absoluta del directorio montado. El comando `pwd` de la sección anterior devolvió esta ruta de acceso. 

Si ejecuta este comando antes de establecer la clave y la región, recibirá un error indicándole que las establezca:
```console   
sudo docker run -it -v ABSOLUTE_PATH:/data --rm msftspeech/spx
```

Para usar el comando `spx` instalado en un contenedor, escriba siempre el comando completo mostrado anteriormente, seguido de los parámetros de la solicitud.
Por ejemplo, en Windows, este comando establece la clave:

```console
docker run -it -v c:\spx-data:/data --rm msftspeech/spx config @key --set SUBSCRIPTION-KEY
```

Para una interacción extendida con la herramienta de línea de comandos, puede iniciar un contenedor con un shell de Bash interactivo mediante la adición de un parámetro EntryPoint.
En Windows, escriba este comando para iniciar un contenedor que muestre una interfaz de línea de comandos interactiva, donde puede escribir varios comandos `spx`:
```console
docker run -it --entrypoint=/bin/bash -v c:\spx-data:/data --rm msftspeech/spx
```

> [!WARNING]
> No puede usar el micrófono del equipo al ejecutar la CLI de Voz dentro de un contenedor de Docker. Sin embargo, puede leer y guardar archivos de audio en el directorio montado local. 

<!-- Need to troubleshoot issues with docker pull image

### Optional: Create a command line shortcut

If you're running the the Speech CLI from a Docker container on Linux or macOS you can create a shortcut. 

Follow these instructions to create a shortcut:
1. Open `.bash_profile` with your favorite text editor. For example:
   ```shell
   nano ~/.bash_profile
   ```
2. Next, add this function to your `.bash_profile`. Make sure you update this function with the correct path to your mounted directory:
   ```shell   
   spx(){
       sudo docker run -it -v ABSOLUTE_PATH:/data --rm msftspeech/spx
   }
   ```
3. Source your profile:
   ```shell
   source ~/.bash_profile
   ```
4. Now instead of running `sudo docker run -it -v ABSOLUTE_PATH:/data --rm msftspeech/spx`, you can just type `spx` followed by arguments. For example: 
   ```shell
   // Get some help
   spx help recognize

   // Recognize speech from an audio file 
   spx recognize --file /mounted/directory/file.wav
   ```

> [!WARNING]
> If you change the mounted directory that Docker is referencing, you need to update the function in `.bash_profile`.
--->
***

## <a name="create-subscription-config"></a>Creación de la configuración de la suscripción

Si desea empezar a usar la CLI de Voz, debe especificar la clave de la suscripción al servicio de voz y la información de la región. Para obtener estas credenciales, siga los pasos descritos en [Prueba gratuita del servicio Voz](../overview.md#try-the-speech-service-for-free).
Una vez que tenga la clave de suscripción y el identificador de región (p. ej., `eastus`, `westus`), ejecute los comandos siguientes.

```console
spx config @key --set SUBSCRIPTION-KEY
spx config @region --set REGION
```

La autenticación de la suscripción se almacena ahora para futuras solicitudes de SPX. Si tiene que quitar cualquiera de estos valores almacenados, ejecute `spx config @region --clear` o `spx config @key --clear`.
