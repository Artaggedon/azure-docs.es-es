---
title: Información general del instalador del paquete de herramientas de desarrollo de Azure Percept
description: Obtenga información sobre el uso del instalador del paquete de herramientas de desarrollo para acelerar el desarrollo avanzado con Azure Percept.
author: mimcco
ms.author: mimcco
ms.service: azure-percept
ms.topic: how-to
ms.date: 02/18/2021
ms.custom: template-how-to
ms.openlocfilehash: d684311ef959ac13f3be8bac7ffbbb06a741962a
ms.sourcegitcommit: 4b7a53cca4197db8166874831b9f93f716e38e30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "102097710"
---
# <a name="dev-tools-pack-installer-overview"></a>Información general del instalador del paquete de herramientas de desarrollo

El instalador del paquete de herramientas de desarrollo es una solución única que instala y configura todas las herramientas necesarias para desarrollar una solución de inteligencia perimetral. Si ya ha instalado alguno de los paquetes de software que se enumeran a continuación, el instalador del paquete de herramientas de desarrollo volverá a instalar esos paquetes para que las herramientas sean coherentes con las versiones de software del instalador.

## <a name="mandatory-tools-installed"></a>Herramientas obligatorias instaladas

* [Visual Studio Code](https://code.visualstudio.com/)
* [Python 3.6 (Windows) o 3.5 (Linux)](https://www.python.org/)
* [Docker 19.03](https://www.docker.com/)
* [PIP3](https://pip.pypa.io/en/stable/user_guide/)
* [TensorFlow 1.13](https://www.tensorflow.org/)
* [SDK de Azure Machine Learning 1.1](https://docs.microsoft.com/python/api/overview/azure/ml/?view=azure-ml-py)

## <a name="optional-tools-available-for-installation"></a>Herramientas opcionales disponibles para la instalación

* [SDK 5 de NVIDIA DeepStream](https://developer.nvidia.com/deepstream-sdk) (kit de herramientas para desarrollar soluciones para aceleradores de NVIDIA)
* [Intel OpenVino Toolkit 2020.2](https://docs.openvinotoolkit.org/) (kit de herramientas para desarrollar soluciones para aceleradores de Intel)
* [Lobe.ai](https://lobe.ai/)  
* [Streamlit](https://www.streamlit.io/)
* [Pytorch 1.4.0 (Windows) o 1.2.0 (Linux)](https://pytorch.org/)
* [Miniconda3](https://docs.conda.io/en/latest/miniconda.html)
* [Chainer 5.2](https://chainer.org/)
* [Caffe](https://caffe.berkeleyvision.org/)
* [CUDA Toolkit 10.0.130](https://developer.nvidia.com/cuda-toolkit)
* [Microsoft Cognitive Toolkit 2.5.1](https://www.microsoft.com/research/product/cognitive-toolkit/?lang=fr_ca)

## <a name="known-issues"></a>Problemas conocidos

- Se puede producir un error en la instalación opcional de Caffe si Docker no se ejecuta correctamente en el sistema. Si desea instalar Caffe, asegúrese de que Docker está instalado y en ejecución antes de intentar la instalación de Caffe mediante el instalador del paquete de herramientas de desarrollo. 

- La instalación opcional de CUDA no se completará correctamente en sistemas incompatibles. Antes de intentar instalar [CUDA Toolkit 10.0.130](https://developer.nvidia.com/cuda-toolkit) mediante el instalador del paquete de herramientas de desarrollo, compruebe la compatibilidad del sistema.

## <a name="minimum-requirements"></a>Requisitos mínimos

* Requisitos mínimos de Docker:

    * Windows:
        * https://docs.docker.com/docker-for-windows/install/#system-requirements

        - Windows 10 de 64 bits: Pro, Enterprise o Education (compilación 16299 o posterior).

             Para Windows 10 Home, consulte Instalación de Docker Desktop en Windows Home.
           - Las características Hyper-V y Contenedores de Windows deben estar habilitadas.
           - Se necesitan los siguientes requisitos previos de hardware para ejecutar correctamente el cliente Hyper-V en Windows 10:

              - Procesador de 64 bits con [traducción de direcciones de segundo nivel (SLAT)](https://en.wikipedia.org/wiki/Second_Level_Address_Translation)
              - 4 GB de RAM del sistema
              - La compatibilidad con la virtualización de hardware a nivel de BIOS debe estar habilitada en la configuración de BIOS. Para más información, consulte Virtualización.

        > [!NOTE]
        > Docker es compatible con Docker Desktop en Windows según el ciclo de vida de soporte técnico de Microsoft para el sistema operativo Windows 10. Para más información, consulte [Hoja de datos del ciclo de vida de Windows](https://support.microsoft.com/help/13853/windows-lifecycle-fact-sheet).

    * Mac:
        * https://docs.docker.com/docker-for-mac/install/#system-requirements
       
        El Mac debe cumplir los requisitos siguientes para la correcta instalación de Docker Desktop:
         
         - **El hardware del Mac debe ser un modelo de 2010 o más reciente con un procesador Intel**, con compatibilidad de hardware de Intel con la virtualización de la unidad de administración de memoria (MMU), incluidas las tablas de páginas extendidas (EPT) y el modo sin restricciones. Puede comprobar si su máquina tiene esta compatibilidad ejecutando el siguiente comando en un terminal: ```sysctl kern.hv_support```

        Si el equipo Mac es compatible con el marco Hypervisor, el comando muestra ```kern.hv_support: 1```.

         - **El Mac debe tener macOS 10.14 o posterior**. Es decir, Mojave, Catalina o Big Sur. Se recomienda actualizar a la versión más reciente de macOS.

        Si experimenta algún problema después de actualizar el macOS a la versión 10.15, debe instalar la versión más reciente de Docker Desktop para que sea compatible con esta versión de macOS.

        - Al menos 4 GB de RAM.
        - VirtualBox anterior a la versión 4.3.30 no debe estar instalado, ya que no es compatible con Docker Desktop.

        > [!NOTE]
        > Docker es compatible con Docker Desktop en las versiones más recientes de macOS. Es decir, la versión actual de macOS y las dos versiones anteriores. A medida que haya nuevas versiones principales de macOS disponibles con carácter general, Docker dejará de admitir la versión más antigua y admitirá la versión más reciente de macOS (además de las dos versiones anteriores). Docker Desktop admite actualmente macOS Mojave, macOS Catalina y macOS Big Sur.
        > 
        - El instalador no es compatible con Apple M1.

## <a name="instructions"></a>Instructions

1. Descargue el instalador del paquete de herramientas de desarrollo para [Windows](https://go.microsoft.com/fwlink/?linkid=2132187), [Linux](https://go.microsoft.com/fwlink/?linkid=2132186) y [Mac](https://go.microsoft.com/fwlink/?linkid=2132296).

1. Según la plataforma, habrá algunas diferencias al iniciar el instalador.

    1. Para Windows:
    
        1. Haga clic en **Dev-Tools-Pack-Installer** para abrir el asistente para la instalación.
        
    1. Para Mac:
    
        1. Después de la descarga, mueva el archivo Dev-Tools-Pack-Installer. app a la carpeta Aplicaciones.
        
        1. Haga clic en **Dev-Tools-Pack-Installer.app** para abrir el asistente para la instalación.
        
        1. Si aparece el cuadro de diálogo de seguridad "Desarrollador no identificado":
        
            1. Vaya a Preferencias del Sistema > Seguridad y privacidad > General y haga clic en el botón "Abrir igualmente" junto a "Dev-Tools-Pack-Installer.app".
        
            1. Haga clic en el icono del electrón del Dock de nuevo.
        
            1. Haga clic en el botón "Abrir" del cuadro de diálogo de seguridad.
    
    1. Para Linux:
    
        1. Cuando el explorador se lo solicite, haga clic en "Guardar" para completar la descarga del instalador.
        
        1. Agregue permisos de ejecución al método 1 del archivo **.appimage** (línea de comandos):
            
            1. Abrir el terminal de Linux.
            
            1. Escriba lo siguiente en el terminal para ir a la carpeta Descargas:
            
                1. cd ~/Downloads/
                
            1. Escriba lo siguiente en el terminal para que el archivo Appimage sea ejecutable:
            
                1. chmod +x **Dev-Tools-Pack-Installer.AppImage**
                
            1. Escriba lo siguiente en el terminal para ejecutar el instalador:
            
                1. ./Dev-Tools-Pack-Installer.AppImage
        
        1. Agregue permisos de ejecución al método 2 del archivo **.appimage** (UI):
        
            1. Haga clic con el botón derecho en el archivo .appimage y seleccione las propiedades.
            
            1. Abra la pestaña Permisos.
            
            1. Active la casilla "Allow executing file as a program" (Permitir ejecutar el archivo como un programa).
            
            1. Cierre las propiedades y abra el archivo .appimage.

1. En la página **Install Dev Tools Pack Installer** (Instalar Instalador del paquete de herramientas de desarrollo), haga clic en **View license** (Ver licencia) para ver los contratos de licencia de cada paquete de software incluido en el instalador. Si acepta los términos de los contratos de licencia, active la casilla y haga clic en **Siguiente**.

    :::image type="content" source="./media/dev-tools-installer/dev-tools-license-agreements.png" alt-text="Pantalla del contrato de licencia en el instalador.":::

1. Haga clic en **Declaración de privacidad** para revisar la declaración de privacidad de Microsoft. Si acepta los términos de la declaración de privacidad y desea enviar datos de diagnóstico a Microsoft, seleccione **Sí** y haga clic en **Siguiente**. En caso contrario, seleccione **No** y haga clic en **Siguiente**.

    :::image type="content" source="./media/dev-tools-installer/dev-tools-privacy-statement.png" alt-text="Pantalla del acuerdo de declaración de privacidad en el instalador.":::

1. En la página **Configurar componentes**, seleccione las herramientas opcionales que desea instalar (las herramientas obligatorias se instalarán de forma predeterminada).

    1. Si está trabajando con el módulo de sistema Azure Percept Audio, que forma parte de Azure Percept DK, asegúrese de instalar Intel OpenVino Toolkit y Miniconda3.

    1. Haga clic en **Instalar** para continuar con la instalación.

    :::image type="content" source="./media/dev-tools-installer/dev-tools-configure-components.png" alt-text="Pantalla del instalador que muestra los paquetes de software disponibles.":::

1. Después de la correcta instalación de todos los componentes seleccionados, el asistente pasa a la página **Completing the Setup Wizard** (Finalización del asistente para la configuración). Haga clic en **Finalizar** para salir del asistente.

    :::image type="content" source="./media/dev-tools-installer/dev-tools-finish.png" alt-text="Pantalla de finalización del instalador.":::

## <a name="docker-status-check"></a>Comprobación del estado de Docker

Si el instalador le informa de que Docker Desktop está en buen estado, consulte los pasos siguientes:

   1. Windows:
   
      1. Expandir iconos ocultos de la bandeja del sistema:
      
         1. Expandir iconos ocultos de la bandeja del sistema si están ocultos:

            :::image type="content" source="./media/dev-tools-installer/system-tray.png" alt-text="Bandeja del sistema.":::
         
         1. Comprobar que el icono de Docker Desktop muestra "Docker Desktop se está ejecutando":

            :::image type="content" source="./media/dev-tools-installer/docker-status-running.png" alt-text="Estado de Docker.":::
         
         1. Si no ve el icono anterior en la bandeja del sistema, inicie Docker Desktop desde el menú Inicio.
         
         1. Si Docker le pide que reinicie, puede cerrar el instalador y volver a iniciarlo después de que se haya completado el reinicio y de que Docker se encuentre en estado de ejecución. Cualquier aplicación de terceros instalada correctamente debería detectarse y no se reinstalará automáticamente.

## <a name="next-steps"></a>Pasos siguientes

Consulte el [repositorio de desarrollo avanzado de Azure Percept](https://github.com/microsoft/azure-percept-advanced-development) para comenzar con el desarrollo avanzado de Azure Percept DK.