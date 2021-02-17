---
title: Configuración de una identidad común
titleSuffix: Azure Data Science Virtual Machine
description: Obtenga información sobre cómo crear cuentas de usuario comunes que se pueden usar en varias instancias de Data Science Virtual Machine. Puede usar Azure Active Directory o una instancia local de Active Directory para autenticar usuarios en Data Science Virtual Machine.
keywords: aprendizaje profundo, AI, herramientas de ciencia de datos, data science virtual machine, análisis geoespacial, proceso de ciencia de datos en equipo
services: machine-learning
ms.service: data-science-vm
author: vijetajo
ms.author: vijetaj
ms.topic: conceptual
ms.date: 05/08/2018
ms.openlocfilehash: 43ef0f55f0b0f3ff9008f0ddf240ad0ea4f37df6
ms.sourcegitcommit: e972837797dbad9dbaa01df93abd745cb357cde1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2021
ms.locfileid: "100519735"
---
# <a name="set-up-a-common-identity-on-a-data-science-virtual-machine"></a>Configuración de una identidad común en una instancia de Data Science Virtual Machine

En una máquina virtual (VM) de Microsoft Azure, que incluye Data Science Virtual Machine (DSVM), cree cuentas de usuario locales al aprovisionar la máquina virtual. Los usuarios se autentican en la máquina virtual mediante estas credenciales. Si tiene varias máquinas virtuales a las que los usuarios necesitan acceder, la administración de las credenciales puede resultar muy difícil. Una solución excelente es implementar las cuentas de usuario y las tareas de administración comunes a través de un proveedor de identidades basado en estándares. A través de este enfoque, puede usar un único conjunto de credenciales para acceder a varios recursos en Azure, incluidas varias máquinas Data Science Virtual Machine.

Active Directory es un conocido proveedor de identidades y se admite en Azure tanto como servicio en la nube como directorio local. Puede usar Azure Active Directory (Azure AD) o Active Directory local para autenticar a los usuarios en una máquina virtual DSVM independiente o un clúster de DSVM en un conjunto de escalado de máquinas virtuales de Azure. Para ello, debe unir las instancias de DSVM a un dominio de Active Directory.

Si ya tiene una instancia de Active Directory, puede usarla como proveedor de identidad común. Si no tiene Active Directory, puede ejecutar una instancia de Active Directory administrada en Azure mediante [Azure Active Directory Domain Services](../../active-directory-domain-services/index.yml) (Azure AD DS).

En la documentación de [Azure AD](../../active-directory/index.yml) se proporcionan [instrucciones de administración](../../active-directory/hybrid/whatis-hybrid-identity.md), incluidas las de conexión de Azure AD al directorio local, si tiene uno.

En este artículo se describen los pasos necesarios para configurar un servicio de dominio de Active Directory completamente administrado en Azure con Azure AD DS. Después, puede unir su máquina DSVM al dominio de Active Directory administrado. Este enfoque permite a los usuarios tener acceso a un grupo de máquinas DSVM (y otros recursos de Azure) a través de una cuenta de usuario y unas credenciales comunes.

## <a name="set-up-a-fully-managed-active-directory-domain-on-azure"></a>Configuración de un dominio de Active Directory completamente administrado en Azure

Azure AD DS simplifica el proceso de administrar las identidades al proporcionar un servicio completamente administrado en Azure. En este dominio de Active Directory, se administran usuarios y grupos. Para configurar un dominio de Active Directory hospedado en Azure y las cuentas de usuarios de su directorio, siga estos pasos:

1. En Azure Portal, agregue el usuario a Active Directory: 

   1. Inicie sesión en el [Centro de administración de Azure Active Directory](https://aad.portal.azure.com) con una cuenta que tenga el rol de administrador global en el directorio.
    
   1. Seleccione **Azure Active Directory** y, después, **Usuarios y grupos**.
    
   1. En **Usuarios y grupos**, seleccione **Todos los usuarios** y **Nuevo usuario**.
   
        Se abre el panel **Usuario**:
      
        ![Panel "Usuario"](./media/add-user.png)
    
   1. Especifique los detalles del usuario, como el **nombre** y el **nombre de usuario**. La parte del nombre de dominio del nombre de usuario debe ser el nombre de dominio del nombre de dominio predeterminado inicial "[nombre de dominio].onmicrosoft.com" o un [nombre de dominio personalizado](../../active-directory/fundamentals/add-custom-domain.md) comprobado y no federado, como "contoso.com".
    
   1. Copie o anote la contraseña de usuario generada para poder proporcionársela al usuario cuando finalice este proceso.
    
   1. También puede abrir y rellenar la información de **Perfil**, **Grupos** o **Rol del directorio** del usuario. 
    
   1. En **Usuario**, seleccione **Crear**.
    
   1. Distribuya de manera segura la contraseña generada al nuevo usuario para que pueda iniciar sesión.

1. Cree una instancia de Azure AD DS. Siga las instrucciones de [Habilitación de Azure Active Directory Domain Services mediante Azure Portal](../../active-directory-domain-services/tutorial-create-instance.md) (la sección "Creación de una instancia y configuración de las opciones básicas"). Es importante actualizar las contraseñas de usuario existentes en Active Directory para que la contraseña de Azure AD DS esté sincronizada. También es importante agregar DNS a Azure AD DS, tal y como se describe en "Complete los campos de la ventana Datos básicos de Azure Portal para crear una instancia de Azure AD DS" en esa sección.

1. Cree una subred de DSVM independiente en la red virtual creada en la sección "Creación y configuración de la red virtual" del paso anterior.
1. Cree una o varias instancias de DSVM en la subred de DSVM.
1. Siga las [instrucciones](../../active-directory-domain-services/join-ubuntu-linux-vm.md) para agregar DSVM a Active Directory. 
1. Monte un recurso compartido de Azure Files para hospedar su directorio particular o de cuaderno de forma que el área de trabajo se pueda montar en cualquier máquina. (Si necesita permisos de nivel de archivo estrictos, necesitará que Network File System [NFS] se ejecute en una o varias máquinas virtuales).

   1. [Cree un recurso compartido de Azure Files](../../storage/files/storage-how-to-create-file-share.md).
    
   2.  Monte este recurso compartido en la instancia de DSVM de Linux. Al seleccionar **Conectar** para el recurso compartido de Azure Files en la cuenta de almacenamiento en Azure Portal, se muestra el comando para la ejecución en el shell de Bash en la máquina virtual DSVM de Linux. El comando tiene este aspecto:
   
   ```
   sudo mount -t cifs //[STORAGEACCT].file.core.windows.net/workspace [Your mount point] -o vers=3.0,username=[STORAGEACCT],password=[Access Key or SAS],dir_mode=0777,file_mode=0777,sec=ntlmssp
   ```
1. Supongamos que ha montado el recurso compartido de Azure Files en /data/workspace, por ejemplo. Ahora cree directorios para cada uno de los usuarios en el recurso compartido: /data/workspace/user1, /data/workspace/user2 y así sucesivamente. Cree un directorio `notebooks` en el área de trabajo de cada usuario. 
1. Cree vínculos simbólicos para `notebooks` en `$HOME/userx/notebooks/remote`.   

Ahora tendrá los usuarios en la instancia de Active Directory hospedada en Azure. Con las credenciales de Active Directory, los usuarios pueden iniciar sesión en cualquier máquina virtual DSVM (SSH o JupyterHub) unida a Azure AD DS. Dado que el área de trabajo del usuario se encuentra en un recurso compartido de Azure Files, los usuarios tendrán acceso a sus cuadernos y demás trabajo desde cualquier DSVM al usar Jupyterhub.

Para el escalado automático, puede usar un conjunto de escalado de máquinas virtuales para crear un grupo de máquinas virtuales, todas ellas unidas al dominio de esta forma y con el disco compartido montado. Los usuarios pueden iniciar sesión en cualquier máquina disponible en el conjunto de escalado de máquinas virtuales y tener acceso al disco compartido donde se guardan sus cuadernos. 

## <a name="next-steps"></a>Pasos siguientes

* [Almacenamiento seguro de las credenciales para acceder a recursos en la nube](dsvm-secure-access-keys.md)