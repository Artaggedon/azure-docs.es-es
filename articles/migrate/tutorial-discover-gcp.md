---
title: Detección de instancias de máquinas virtuales de GCP con Azure Migrate Server Assessment
description: Obtenga información sobre cómo detectar instancias de máquinas virtuales de GCP con Azure Migrate Server Assessment.
author: vineetvikram
ms.author: vivikram
ms.manager: abhemraj
ms.topic: tutorial
ms.date: 09/14/2020
ms.custom: mvc
ms.openlocfilehash: 181f645540a267d65b15a0345a61752a8a5f78fa
ms.sourcegitcommit: e7152996ee917505c7aba707d214b2b520348302
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/20/2020
ms.locfileid: "97704744"
---
# <a name="tutorial-discover-google-cloud-platform-gcp-instances-with-server-assessment"></a>Tutorial: Detección de instancias de Google Cloud Platform (GCP) con Server Assessment

Como parte del recorrido de la migración a Azure, detectará los servidores para la valoración y la migración.

En este tutorial se muestra cómo detectar instancias de Google Cloud Platform (GCP) con la herramienta Azure Migrate: Server Assessment, con un dispositivo ligero de Azure Migrate. El dispositivo se implementa como una instancia de máquina virtual de GCP para detectar continuamente los metadatos de rendimiento y de la máquina.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Configurar una cuenta de Azure.
> * Preparar las instancias de máquinas virtuales de GCP para la detección.
> * Cree un proyecto de Azure Migrate.
> * Configurar el dispositivo de Azure Migrate.
> * Iniciar la detección continua.

> [!NOTE]
> Los tutoriales muestran la manera más rápida de probar un escenario y utilizar las opciones predeterminadas.  

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/pricing/free-trial/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Antes de empezar este tutorial, compruebe que dispone de estos requisitos previos.

**Requisito** | **Detalles**
--- | ---
**Dispositivo** | Necesita una instancia de máquina virtual de GCP en la que ejecutar el dispositivo Azure Migrate. La máquina debe tener:<br/><br/> - Windows Server 2016 instalado. No se admite la ejecución del dispositivo en una máquina con Windows Server 2019.<br/><br/> - 16 GB de RAM, 8 vCPU, alrededor de 80 GB de almacenamiento en disco y un conmutador virtual externo.<br/><br/> - Una dirección IP estática o dinámica, con acceso a Internet, ya sea directamente o mediante un proxy.
**Instancias de máquinas virtuales de GCP** | Permita las conexiones entrantes en el puerto WinRM 5985 (HTTP), para que el dispositivo pueda extraer los metadatos de configuración y rendimiento.
**Instancias de máquinas virtuales Linux** | Permita las conexiones entrantes en el puerto 22 (TCP).

## <a name="prepare-an-azure-user-account"></a>Preparación de una cuenta de usuario de Azure

Para crear un proyecto de Azure Migrate y registrar el dispositivo Azure Migrate, necesita una cuenta con:
- Permisos de nivel de colaborador o propietario en una suscripción de Azure.
- Permisos para registrar aplicaciones de Azure Active Directory.

Si acaba de crear una cuenta de Azure gratuita, es el propietario de la suscripción. Si no es el propietario, trabaje con él para asignar los permisos, como se indica a continuación:

1. En Azure Portal, busque "suscripciones" y, en **Servicios**, seleccione **Suscripciones**.

    ![Cuadro para buscar la suscripción de Azure](./media/tutorial-discover-gcp/search-subscription.png)

2. En la página **Suscripciones**, seleccione aquella en la que desee crear un proyecto de Azure Migrate. 
3. En la suscripción, seleccione **Access control (IAM)**  > **Comprobar acceso**.
4. En **Comprobar acceso**, busque la cuenta de usuario correspondiente.
5. En **Agregar una asignación de roles**, haga clic en **Agregar**.

    ![Búsqueda de una cuenta de usuario para comprobar el acceso y asignar un rol](./media/tutorial-discover-gcp/azure-account-access.png)

6. En **Agregar asignación de roles**, seleccione el rol Colaborador o Propietario, y seleccione la cuenta (azmigrateuser en nuestro ejemplo). A continuación, haga clic en **Save**(Guardar).

    ![Se abre la página Agregar asignación de roles para asignar un rol a la cuenta.](./media/tutorial-discover-gcp/assign-role.png)

7. En el portal, busque los usuarios y, en **Servicios**, seleccione **Usuarios**.
8. En **Configuración de usuario**, compruebe que los usuarios de Azure AD puedan registrar aplicaciones (establecido en **Sí** de forma predeterminada).

    ![Comprobación en la configuración de usuario de que los usuarios puedan registrar aplicaciones de Active Directory](./media/tutorial-discover-gcp/register-apps.png)


## <a name="prepare-gcp-instances"></a>Preparación de instancias de GCP

Configure una cuenta que el dispositivo pueda usar para acceder a las instancias de máquinas virtuales de GCP.

- Para servidores Windows
    - Configure la cuenta de usuario local y la cuenta de dominio que desee incluir en la detección, en máquinas que no estén unidas a un dominio. Agregue la cuenta de usuario a los siguientes grupos: 
        - Usuarios de Administración remota
        - Usuarios de Monitor de rendimiento
        - Usuarios del registro de rendimiento
- Para servidores Linux:
    - Necesita una cuenta raíz en los servidores Linux que desee detectar. Si no puede proporcionar una cuenta raíz, consulte las instrucciones de la [matriz de compatibilidad](migrate-support-matrix-physical.md#physical-server-requirements) para conocer una alternativa.
    - Azure Migrate usa la autenticación de contraseña al detectar instancias de AWS. Las instancias de AWS no admiten la autenticación de contraseña de forma predeterminada. Antes de poder detectar la instancia, debe habilitar la autenticación de contraseña.
        1. Inicie sesión en cada máquina Linux.
        2. Abra el archivo sshd_config: vi/etc/ssh/sshd_config
        3. En el archivo, busque la línea **PasswordAuthentication** y cambie el valor a **yes** (sí).
        4. Guarde el archivo y ciérrelo. Reinicie el servicio ssh.
    - Si usa un usuario raíz para detectar las máquinas virtuales Linux, asegúrese de que se permite el inicio de sesión raíz en las máquinas virtuales.
        1. Inicie sesión en cada máquina Linux.
        2. Abra el archivo sshd_config: vi/etc/ssh/sshd_config
        3. En el archivo, busque la línea **PermitRootLogin** y cambie el valor a **yes** (sí).
        4. Guarde el archivo y ciérrelo. Reinicie el servicio ssh.

## <a name="set-up-a-project"></a>Configuración de un proyecto

Configure un proyecto nuevo de Azure Migrate.

1. En Azure Portal > **Todos los servicios**, busque **Azure Migrate**.
2. En **Servicios**, seleccione **Azure Migrate**.
3. En **Información general**, seleccione **Crear proyecto**.
5. En **Crear proyecto**, seleccione su suscripción y grupo de recursos de Azure. Cree un grupo de recursos si no tiene ninguno.
6. En **Detalles del proyecto**, especifique el nombre del proyecto y la región geográfica en la que quiere crearlo. Revise las zonas geográficas admitidas para nubes [públicas](migrate-support-matrix.md#supported-geographies-public-cloud) y [nubes gubernamentales](migrate-support-matrix.md#supported-geographies-azure-government).

   ![Cuadros de nombre de proyecto y región](./media/tutorial-discover-gcp/new-project.png)

7. Seleccione **Crear**.
8. Espere unos minutos para que se implemente el proyecto de Azure Migrate.

La herramienta **Azure Migrate: Server Assessment** se agrega de forma predeterminada al nuevo proyecto.

![Página que muestra la herramienta Server Assessment agregada de forma predeterminada](./media/tutorial-discover-gcp/added-tool.png)

## <a name="set-up-the-appliance"></a>Configuración del dispositivo

El dispositivo de Azure Migrate es un dispositivo ligero que Azure Migrate Server Assessment usa para lo siguiente:

- Descubrir servidores locales.
- Enviar metadatos y datos de rendimiento de los servidores detectados a Azure Migrate Server Assessment.

[Más información](migrate-appliance.md) sobre el dispositivo de Azure Migrate.


## <a name="appliance-deployment-steps"></a>Pasos de implementación del dispositivo

Para configurar el dispositivo:
- Proporcione un nombre de dispositivo y genere una clave del proyecto de Azure Migrate en el portal.
- Descargue un archivo comprimido con el script del instalador de Azure Migrate desde Azure Portal.
- Extraiga el contenido del archivo comprimido. Inicie la consola de PowerShell con privilegios administrativos.
- Ejecute el script de PowerShell para iniciar la aplicación web del dispositivo.
- Configure el dispositivo por primera vez y regístrelo en el proyecto de Azure Migrate mediante la clave del proyecto de Azure Migrate.

### <a name="generate-the-azure-migrate-project-key"></a>Generación de la clave del proyecto de Azure Migrate

1. En **Objetivos de migración** > **Servidores** > **Azure Migrate: Server Assessment**, seleccione **Detectar**.
2. En **Detectar máquinas** >  **¿Las máquinas están virtualizadas?** , seleccione **Físico o de otro tipo (AWS, GCP, Xen, etc.)** .
3. En **1: Generar la clave del proyecto de Azure Migrate**, proporcione un nombre para el dispositivo de Azure Migrate que configurará para la detección de los servidores virtuales de GCP. Este nombre debe ser alfanumérico y no puede tener más de 14 caracteres.
4. Haga clic en **Generar clave** para iniciar la creación de los recursos de Azure necesarios. No cierre la página Detectar máquinas durante la creación de recursos.
5. Después de la creación correcta de los recursos de Azure, se genera una **clave del proyecto Azure Migrate**.
6. Copie la clave, ya que la necesitará para completar el registro del dispositivo durante su configuración.

### <a name="download-the-installer-script"></a>Descarga del script del instalador

En **2: Descargar el dispositivo de Azure Migrate**, haga clic en **Descargar**.


### <a name="verify-security"></a>Comprobación de la seguridad

Compruebe que el archivo comprimido es seguro, antes de implementarlo.

1. En la máquina en la que descargó el archivo, abra una ventana de comandos de administrador.
2. Ejecute el siguiente comando para generar el código hash para el archivo ZIP:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Ejemplo de uso para la nube pública: ```C:\>CertUtil -HashFile C:\Users\administrator\Desktop\AzureMigrateInstaller-Server-Public.zip SHA256 ```
    - Ejemplo de uso para la nube gubernamental: ```  C:\>CertUtil -HashFile C:\Users\administrator\Desktop\AzureMigrateInstaller-Server-USGov.zip SHA256 ```
3.  Compruebe las versiones más recientes del dispositivo y los valores hash:
    - Para la nube pública:

        **Escenario** | **Descargar** | **Valor del código hash**
        --- | --- | ---
        Físico (85 MB) | [La versión más reciente](https://go.microsoft.com/fwlink/?linkid=2140334) | ce5e6f0507936def8020eb7b3109173dad60fc51dd39c3bd23099bc9baaabe29

    - Para Azure Government:

        **Escenario** | **Descargar** | **Valor del código hash**
        --- | --- | ---
        Físico (85 MB) | [La versión más reciente](https://go.microsoft.com/fwlink/?linkid=2140338) | ae132ebc574caf231bf41886891040ffa7abbe150c8b50436818b69e58622276
 

### <a name="run-the-azure-migrate-installer-script"></a>Ejecución del script del instalador de Azure Migrate
El script del instalador hace lo siguiente:

- Instala los agentes y una aplicación web para la detección y evaluación de los servidores de GCP.
- Instala los roles de Windows, incluido el servicio de activación de Windows, IIS y PowerShell ISE.
- Descarga e instala un módulo de reescritura de IIS. [Más información](https://www.microsoft.com/download/details.aspx?id=7435).
- Actualiza una clave del registro (HKLM) con detalles de configuración persistentes para Azure Migrate.
- Crea los siguientes archivos en la ruta de acceso:
    - **Archivos de configuración**:%Programdata%\Microsoft Azure\Config
    - **Archivos de registro**:%Programdata%\Microsoft Azure\Logs

Ejecute el script como se indica a continuación:

1. Extraiga el archivo comprimido en la carpeta del servidor que hospedará el dispositivo.  No ejecute el script en una máquina en un dispositivo de Azure Migrate existente.
2. Inicie PowerShell en el servidor anterior con privilegios administrativos (elevados).
3. Cambie el directorio de PowerShell a la carpeta en la que se ha extraído el contenido del archivo comprimido descargado.
4. Ejecute el script denominado **AzureMigrateInstaller.ps1** ejecutando el comando siguiente:

    - Para la nube pública: 
    
        ``` PS C:\Users\administrator\Desktop\AzureMigrateInstaller-Server-Public> .\AzureMigrateInstaller.ps1 ```
    - Para Azure Government: 
    
        ``` PS C:\Users\Administrators\Desktop\AzureMigrateInstaller-Server-USGov>.\AzureMigrateInstaller.ps1 ```

    El script iniciará la aplicación web del dispositivo cuando finalice correctamente.

En caso de que surja algún problema, puede acceder a los registros de script en C:\ProgramData\Microsoft Azure\Logs\AzureMigrateScenarioInstaller_<em>Timestamp</em>.log para solucionarlo.



### <a name="verify-appliance-access-to-azure"></a>Comprobación de que el dispositivo puede acceder a Azure

Asegúrese de que la VM del dispositivo pueda conectarse a las direcciones URL de Azure para las nubes [públicas](migrate-appliance.md#public-cloud-urls) y [gubernamentales](migrate-appliance.md#government-cloud-urls).

### <a name="configure-the-appliance"></a>Configuración del dispositivo

Configure el dispositivo por primera vez.

1. Abra un explorador en cualquier máquina que pueda conectarse al dispositivo y abra la dirección URL de la aplicación web del dispositivo: **https://*nombre o dirección IP del dispositivo*: 44368**.

   Como alternativa, puede abrir la aplicación desde el escritorio, para lo que debe hacer clic en el acceso directo de la aplicación.
2. Acepte los **términos de licencia** y lea la información de terceros.
1. En la aplicación web > **Set up prerequisites** (Configurar los requisitos previos ), realice las siguientes operaciones:
    - **Connectivity** (Conectividad): la aplicación comprueba que el servidor tenga acceso a Internet. Si el servidor usa un proxy:
        - Haga clic en **Configurar el proxy** y especifique la dirección del proxy (con los formatos http://ProxyIPAddress o http://ProxyFQDN) ) y el puerto de escucha.
        - Especifique las credenciales si el proxy requiere autenticación.
        - Solo se admite un proxy HTTP.
        - Si ha agregado detalles del proxy o ha deshabilitado el proxy o la autenticación, haga clic en **Guardar** para desencadenar la comprobación de conectividad.
    - **Time sync** (Sincronización de hora): Se comprueba la hora. Para que la detección del servidor funcione correctamente, la hora del dispositivo debe estar sincronizada con la hora de Internet.
    - **Instalación de actualizaciones**: Azure Migrate Server Assessment comprueba que el dispositivo tiene instaladas las actualizaciones más recientes. Una vez finalizada la comprobación, puede hacer clic en **Ver servicios del dispositivo** para ver el estado y las versiones de los componentes que se ejecutan en el dispositivo.

### <a name="register-the-appliance-with-azure-migrate"></a>Registro del dispositivo en Azure Migrate

1. Pegue la **clave del proyecto de Azure Migrate** copiada desde el portal. Si no tiene la clave, vaya a **Server Assessment > Detectar > Administrar los dispositivos existentes**, seleccione el nombre del dispositivo que proporcionó en el momento de la generación de la clave y copie la clave correspondiente.
1. Necesitará un código de dispositivo para autenticarse con Azure. Al hacer clic en **Iniciar sesión** se abrirá un modal con el código del dispositivo, tal como se muestra a continuación.

    ![Modal que muestra el código del dispositivo](./media/tutorial-discover-vmware/device-code.png)

1. Haga clic en **Copiar código e Iniciar sesión** para copiar el código del dispositivo y abrir un símbolo del sistema de inicio de sesión de Azure en una nueva pestaña del explorador. Si no aparece, asegúrese de que ha deshabilitado el bloqueador de elementos emergentes en el explorador.
1. En la nueva pestaña, pegue el código del dispositivo e inicie sesión con su nombre de usuario y contraseña de Azure.
   
   No se admite el inicio de sesión con un PIN.
3. En caso de que cierre accidentalmente la pestaña de inicio de sesión sin iniciar sesión, deberá actualizar la pestaña explorador del administrador de configuración del dispositivo para volver a habilitar el botón Iniciar sesión.
1. Una vez que haya iniciado sesión correctamente, vuelva a la pestaña anterior con el administrador de configuración del dispositivo.
4. Si la cuenta de usuario de Azure que se usa para el registro tiene los [permisos](#prepare-an-azure-user-account) adecuados en los recursos de Azure creados durante la generación de la clave, se iniciará el registro del dispositivo.
5. Una vez que el dispositivo se ha registrado correctamente, puede ver los detalles de registro haciendo clic en **Ver detalles**.


## <a name="start-continuous-discovery"></a>Inicio de detección continua

Ahora, conecte el dispositivo a los servidores de GCP que se van a detectar e inicie la detección.

1. En **Paso 1: proporcionar credenciales para la detección de servidores físicos o virtuales de Windows y Linux**, haga clic en **Agregar credenciales**.
1. En el caso de Windows Server, seleccione el tipo de origen como **Windows Server**, especifique un nombre descriptivo para las credenciales, agregue el nombre de usuario y la contraseña. Haga clic en **Guardar**.
1. Si usa la autenticación basada en contraseña para el servidor Linux, seleccione el tipo de origen como **Servidor Linux (basado en contraseña)** , especifique un nombre descriptivo para las credenciales y agregue el nombre de usuario y la contraseña. Haga clic en **Guardar**.
1. Si usa la autenticación basada en clave SSH para el servidor Linux, puede seleccionar el tipo de origen como **Servidor Linux (basado en clave SSH)** , especifique un nombre descriptivo para las credenciales, agregue el nombre de usuario. busque el archivo de clave privada SSH y selecciónela. Haga clic en **Guardar**.

    - Azure Migrate admite la clave privada SSH generada por el comando ssh-keygen mediante los algoritmos RSA, DSA, ECDSA y ed25519.
    - Actualmente, Azure Migrate no admite la clave SSH basada en frase de contraseña. Use una clave SSH sin frase de contraseña.
    - Actualmente, Azure Migrate no admite el archivo de clave privada SSH generado por PuTTy.
    - Azure Migrate admite el formato OpenSSH del archivo de clave privada SSH, como se muestra a continuación:
    
    ![Formato compatible con clave privada SSH](./media/tutorial-discover-physical/key-format.png)


2. Si quiere agregar varias credenciales a la vez, haga clic en **Agregar más** para guardar y agregar más credenciales. 
3. En **Paso 2: Proporcionar los detalles del servidor virtual o físico**, haga clic en **Agregar origen de detección** para especificar la **dirección IP o el FQDN** del servidor y el nombre descriptivo de las credenciales para conectarse al servidor.
4. Puede **Agregar un solo elemento** cada vez o **Agregar varios elementos** de una sola vez. También hay una opción para proporcionar los detalles del servidor a través de **Importar CSV**.

    - Si elige **Agregar un solo elemento**, puede elegir el tipo de sistema operativo, especificar el nombre descriptivo de las credenciales, agregar la **dirección IP o el FQDN** del servidor y hacer clic en **Guardar**.
    - Si elige **Agregar varios elementos**, puede agregar varios registros a la vez mediante la especificación de la **dirección IP o el FQDN** del servidor con el nombre descriptivo de las credenciales en el cuadro de texto. **Compruebe** los registros agregados y haga clic en **Guardar**.
    - Si elige **importar CSV** _(opción seleccionada de manera predeterminada)_ , puede descargar un archivo de plantilla CSV, rellenar el archivo con la **dirección IP o el FQDN** del servidor y el nombre descriptivo de las credenciales. A continuación, importe el archivo en el dispositivo, **compruebe** los registros del archivo y haga clic en **Guardar**.

5. Al hacer clic en Guardar, el dispositivo intentará validar la conexión a los servidores agregados y mostrar el **estado de validación** en la tabla en cada servidor.
    - Si se produce un error de validación para un servidor, haga clic en **Error en la validación** en la columna Estado de la tabla para revisar el error. Corrija el problema y vuelva a validar.
    - Para quitar un servidor, haga clic en **Eliminar**.
6. Puede **volver a validar** la conectividad a los servidores en cualquier momento antes de iniciar la detección.
7. Haga clic en **Iniciar detección** para dar comienzo a la detección de los servidores validados correctamente. Una vez que la detección se ha iniciado correctamente, puede comprobar el estado de detección en cada servidor de la tabla.


De esta forma comienza la detección. Los metadatos de los servidores detectados tardan alrededor de 2 minutos por servidor en aparecer en Azure Portal.

## <a name="verify-servers-in-the-portal"></a>Comprobación de los servidores en el portal

Una vez finalizada la detección, puede verificar que los servidores aparezcan en el portal.

1. Abra el panel de Azure Migrate.
2. En la página **Azure Migrate - Servidores** > **Azure Migrate: Server Assessment**, haga clic en el icono que muestra el número de **servidores detectados**.

## <a name="next-steps"></a>Pasos siguientes

- [Evaluación de los servidores de GCP](tutorial-assess-gcp.md) para la migración a máquinas virtuales de Azure.
- [Revise los datos](migrate-appliance.md#collected-data---physical) que el dispositivo recopila durante la detección.
