---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines-windows
author: cynthn
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 02/11/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 7a6962a0fa1374edb5a9f43641a0adf398708acf
ms.sourcegitcommit: 9d9221ba4bfdf8d8294cf56e12344ed05be82843
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2021
ms.locfileid: "98570705"
---
## <a name="supported-operating-systems-and-drivers"></a>Sistemas operativos y controladores compatibles

### <a name="nvidia-tesla-cuda-drivers"></a>Controladores NVIDIA Tesla (CUDA)

Los controladores NVIDIA Tesla (CUDA) para máquinas virtuales de las series NC, NCv2, NCv3, NCasT4_v3, ND y NDv2 (opcionales para la serie NV) solo se admiten en las distribuciones de sistemas operativos enumeradas en la tabla siguiente. Los vínculos de descarga de controladores están actualizados en el momento de la publicación. Para ver los controladores más recientes, visite el sitio web de [NVIDIA](https://www.nvidia.com/).

> [!TIP]
> Como alternativa a la instalación manual de controladores de CUDA en una máquina virtual de Windows Server, puede implementar una imagen de [Data Science Virtual Machine](../articles/machine-learning/data-science-virtual-machine/overview.md) de Azure. Las ediciones de DSVM para Windows Server 2016 preinstalan los controladores NVIDIA CUDA, la biblioteca CUDA Deep Neural Network Library y otras herramientas.


| SO | Controlador |
| -------- |------------- |
| Windows Server 2019 | [451.82](http://us.download.nvidia.com/tesla/451.82/451.82-tesla-desktop-winserver-2019-2016-international.exe) (.exe) |
| Windows Server 2016 | [451.82](http://us.download.nvidia.com/tesla/451.82/451.82-tesla-desktop-winserver-2019-2016-international.exe) (.exe) |

### <a name="nvidia-grid-drivers"></a>Controladores de NVIDIA GRID

Microsoft redistribuye los instaladores del controlador NVIDIA GRID para VM de las series NV y NVv3 que se emplean como estaciones de trabajo virtuales o para aplicaciones virtuales. Instale estos controladores GRID en VM de la serie NV de Azure y solo en los sistemas operativos enumerados en la tabla siguiente. Estos controladores incluyen licencias del software GRID Virtual GPU en Azure. No es necesario configurar un servidor de licencias de software vGPU NVIDIA.

Los controladores de GRID redistribuidos por Azure no funcionan en máquinas virtuales que no son de la serie NV, como las máquinas virtuales de las series NCv2, NCv3, ND y NDv2. La única excepción es la serie de VM NCas_T4_V3, donde los controladores de GRID habilitarán las funcionalidades de gráficos similares a las de la serie NV.

La serie NC con las GPU de NVIDIA K80 no admite aplicaciones de gráficos ni cuadrículas.  

Tenga en cuenta que la extensión de NVIDIA siempre instalará el controlador más reciente. Aquí se proporcionan vínculos a la versión anterior para los clientes que tienen dependencias de una versión anterior.

En Windows Server 2019, Windows Server 2016 y Windows 10 (hasta la compilación 2004):
- [GRID 11.3 (452.77)](https://go.microsoft.com/fwlink/?linkid=874181) (.exe)
- [GRID 11.2 (452.57)](https://download.microsoft.com/download/1/b/1/1b15516a-de49-4ba4-8651-6abda4f7fb82/452.57_grid_win10_server2016_server2019_64bit_international.exe) (.exe) 

En Windows Server 2012 R2: 
- [GRID 11.3 (452.77)](https://download.microsoft.com/download/5/4/3/54323644-3c84-4aa1-97ec-35491f94c866/452.77_grid_server2012R2_64bit_azure_swl.exe) (.exe) 
- [GRID 11.0 (451.48)](https://download.microsoft.com/download/f/7/2/f729e28b-57b8-4141-b577-38d2390973ef/451.48_grid_server2012R2_64bit_international.exe) (.exe)

Para obtener la lista completa de los vínculos de todos los controladores de Nvidia GRID anteriores, visite [GitHub](https://github.com/Azure/azhpc-extensions/blob/master/NvidiaGPU/resources.json)
