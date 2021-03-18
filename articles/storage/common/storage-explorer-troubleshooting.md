---
title: Guía de solución de problemas del Explorador de Azure Storage | Microsoft Docs
description: Introducción a las técnicas de depuración para el Explorador de Azure Storage
services: storage
author: Deland-Han
manager: dcscontentpm
ms.service: storage
ms.topic: troubleshooting
ms.date: 07/28/2020
ms.author: delhan
ms.openlocfilehash: 15df9b38abe35fe3eefad2fa160e1c1f16fe7aa7
ms.sourcegitcommit: 5bbc00673bd5b86b1ab2b7a31a4b4b066087e8ed
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/07/2021
ms.locfileid: "102439466"
---
# <a name="azure-storage-explorer-troubleshooting-guide"></a>Guía de solución de problemas del Explorador de Azure Storage

Explorador de Microsoft Azure Storage es una aplicación independiente que facilita el trabajo con los datos de Azure Storage en Windows, macOS y Linux. La aplicación puede conectarse a las cuentas de Storage hospedadas en Azure, nubes nacionales y Azure Stack.

En esta guía se resumen las soluciones de los problemas comunes que se encuentran con frecuencia en Explorador de Storage.

## <a name="azure-rbac-permissions-issues"></a>Problemas de permisos de RBAC de Azure

El control de acceso basado en rol ([RBAC](../../role-based-access-control/overview.md)) de Azure permite realizar una administración muy pormenorizada del acceso a los recursos de Azure mediante la combinación de conjuntos de permisos para formar _roles_. Aquí encontrará algunas estrategias para que RBAC de Azure funcione de forma óptima en el Explorador de Storage.

### <a name="how-do-i-access-my-resources-in-storage-explorer"></a>¿Cómo se accede a los recursos en Explorador de Storage?

Si tiene problemas para acceder a los recursos de almacenamiento mediante RBAC de Azure, es posible que se deba a que no se le han asignado los roles adecuados. En las siguientes secciones se describen los permisos que el Explorador de Storage requiere actualmente para acceder a los recursos de almacenamiento. Si no está seguro de tener los roles o permisos adecuados, póngase en contacto con el administrador de la cuenta de Azure.

#### <a name="read-listget-storage-accounts-permissions-issue"></a>"Problema de permisos de lectura para enumerar u obtener cuentas de almacenamiento"

Debe tener permiso para enumerar las cuentas de almacenamiento. Para obtener este permiso, se le debe haber asignado el rol _Lector_.

#### <a name="list-storage-account-keys"></a>Enumerar claves de cuentas de almacenamiento

El Explorador de Storage también puede usar las claves de cuenta para autenticar las solicitudes. Puede acceder a las claves de las cuentas con roles más eficaces, como el rol _Colaborador_.

> [!NOTE]
> Las claves de acceso otorgan permisos ilimitados a cualquier persona que disponga de ellas. Por tanto, no se recomienda dar estas claves a los usuarios de la cuenta. Si necesita revocar las claves de acceso, puede regenerarlas desde [Azure Portal](https://portal.azure.com/).

#### <a name="data-roles"></a>Roles de datos

Se le debe haber asignado al menos un rol que conceda acceso para leer datos de los recursos. Por ejemplo, si desea enumerar o descargar blobs, necesitará tener, como mínimo, el rol _Lector de datos de Storage Blob_.

### <a name="why-do-i-need-a-management-layer-role-to-see-my-resources-in-storage-explorer"></a>¿Por qué se necesita un rol de la capa de administración para ver los recursos en el Explorador de Storage?

Azure Storage tiene dos capas de acceso: _administración_ y _datos_. Se accede a las suscripciones y cuentas de almacenamiento mediante la capa de administración. Se accede a los contenedores, blobs y otros recursos de datos mediante la capa de datos. Por ejemplo, si desea obtener una lista de las cuentas de almacenamiento de Azure, debe enviar una solicitud al punto de conexión de administración. Si desea obtener una lista de los contenedores de blobs de una cuenta, envíe una solicitud al punto de conexión de servicio adecuado.

Los roles de Azure pueden concederle los permisos necesario s para obtener acceso a la capa de datos o de administración. Por ejemplo, el rol Lector otorga acceso de solo lectura a los recursos de la capa de administración.

Estrictamente hablando, el rol Lector no proporciona permisos para la capa de datos y no es necesario para acceder a esa capa.

El Explorador de Storage facilita el acceso a los recursos mediante la recopilación de la información necesaria para conectarse a los recursos de Azure. Por ejemplo, para mostrar los contenedores de blobs, el Explorador de Storage envía una solicitud de "lista de contenedores" al punto de conexión del servicio de blobs. Para obtener ese punto de conexión, el Explorador de Storage busca la lista de suscripciones y cuentas de almacenamiento a las que tiene acceso. Para encontrar sus suscripciones y cuentas de almacenamiento, el Explorador de Storage también necesita acceso a la capa de administración.

Si no tiene ningún rol que conceda permisos para la capa de administración, el Explorador de Storage no puede obtener la información que necesita para conectarse a la capa de datos.

### <a name="what-if-i-cant-get-the-management-layer-permissions-i-need-from-my-administrator"></a>¿Qué sucede si no puedo obtener los permisos de la capa de administración que necesito de mi administrador?

Si quiere acceder a los contenedores de blobs, a los contenedores o directorios de ADLS Gen2, o a las colas, puede asociarlos a esos recursos con las credenciales de Azure.

1. Abra el cuadro de diálogo Conectar.
1. Seleccione el tipo de recurso al que desea conectarse.
1. Seleccione **Iniciar sesión con Azure Active Directory (Azure AD)** . Seleccione **Next** (Siguiente).
1. Seleccione la cuenta de usuario y el inquilino asociados al recurso al que los está adjuntando. Seleccione **Next** (Siguiente).
1. Escriba la dirección URL del recurso y un nombre para mostrar único para la conexión. Seleccione **Siguiente** y luego **Conectar**.

Actualmente no tenemos una solución relacionada con RBAC de Azure para otros tipos de recursos. Como solución alternativa, puede solicitar una dirección URL de SAS y asociarla al recurso siguiendo estos pasos:

1. Abra el cuadro de diálogo Conectar.
1. Seleccione el tipo de recurso al que desea conectarse.
1. Seleccione **Firma de acceso compartido (SAS) de cuenta**. Seleccione **Next** (Siguiente).
1. Escriba la dirección URL de SAS que ha recibido y un nombre para mostrar único para la conexión. Seleccione **Siguiente** y luego **Conectar**.
 
Para más información sobre cómo asociar recursos, consulte cómo [conectarse a un recurso individual](../../vs-azure-tools-storage-manage-with-storage-explorer.md?tabs=linux#attach-to-an-individual-resource).

### <a name="recommended-azure-built-in-roles"></a>Roles integrados de Azure recomendados

Hay varios roles integrados de Azure que pueden proporcionar los permisos necesarios para usar el Explorador de Storage. Algunos de estos roles son:
- [Propietario](../../role-based-access-control/built-in-roles.md#owner): administra todo, incluido el acceso a los recursos.
- [Colaborador](../../role-based-access-control/built-in-roles.md#contributor): administra todo, excepto el acceso a los recursos.
- [Lector](../../role-based-access-control/built-in-roles.md#reader): lee y enumera los recursos.
- [Colaborador de la cuenta de almacenamiento](../../role-based-access-control/built-in-roles.md#storage-account-contributor): administración completa de cuentas de almacenamiento.
- [Propietario de datos de Storage Blob](../../role-based-access-control/built-in-roles.md#storage-blob-data-owner): acceso completo a contenedores de blobs y datos de Azure Storage.
- [Colaborador de datos de Storage Blob](../../role-based-access-control/built-in-roles.md#storage-blob-data-contributor): Lee, escribe y elimina blobs y contenedores de Azure Storage.
- [Lector de datos de Storage Blob](../../role-based-access-control/built-in-roles.md#storage-blob-data-reader): Lee y enumera blobs y contenedores de Azure Storage.

> [!NOTE]
> Los roles de propietario, colaborador y colaborador de la cuenta de almacenamiento conceden acceso a la clave de cuenta.

## <a name="error-self-signed-certificate-in-certificate-chain-and-similar-errors"></a>Error: certificado autofirmado en la cadena de certificados (y errores similares)

Los errores de certificado habitualmente se producen en una de las situaciones siguientes:

- La aplicación se conecta a través de un _proxy transparente_. Esto significa que un servidor (por ejemplo, el servidor de la empresa) intercepta el tráfico HTTPS, lo descifra y, después, lo cifra mediante un certificado autofirmado.
- Ejecuta una aplicación que inserta un certificado TLS/SSL autofirmado en los mensajes HTTPS que recibe. Entre los ejemplos de aplicaciones que insertan certificados se incluyen los antivirus y el software de inspección del tráfico de red.

Cuando el Explorador de Storage ve un certificado autofirmado o que no es de confianza, ya no sabe si se ha modificado el mensaje HTTPS recibido. Si tiene una copia del certificado autofirmado, puede indicar a Explorador de Storage que confíe en él. Para ello, debe seguir estos pasos:

1. Obtenga una copia X.509 codificado base 64 (.cer) del certificado.
2. Vaya a **Editar** > **Certificados SSL** > **Importar certificados** y, luego, utilice el selector de archivos para encontrar, seleccionar y abrir el archivo .cer.

Este problema también puede aparecer si hay varios certificados (raíz e intermedio). Para solucionar este error, deben agregarse ambos certificados.

Si no está seguro de cuál es la procedencia del certificado, siga estos pasos para encontrarlo:

1. Instale OpenSSL.
    * [Windows](https://slproweb.com/products/Win32OpenSSL.html): cualquiera de las versiones ligeras debería ser suficiente.
    * Mac y Linux: debe estar incluido en el sistema operativo.
2. Ejecute OpenSSL.
    * Windows: abra el directorio de instalación, seleccione **/bin/** y, luego, haga doble clic en **openssl.exe**.
    * Mac y Linux: Ejecute `openssl` desde un terminal.
3. Ejecute `s_client -showcerts -connect microsoft.com:443`.
4. Busque certificados autofirmados. Si no está seguro de qué certificados están autofirmados, tome nota de todos aquellos en los que el asunto `("s:")` y el emisor `("i:")` sean el mismo.
5. Cuando encuentre certificados autofirmados, en cada uno de ellos, copie y pegue todo el contenido desde `-----BEGIN CERTIFICATE-----` a `-----END CERTIFICATE-----` (inclusive) a un archivo .cer nuevo.
6. Abra el Explorador de Storage y vaya a **Editar** > **Certificados SSL** > **Importar certificados**. Luego, use el selector de archivos para encontrar, seleccionar y abrir los archivos .cer que creó.

Si no encuentra ningún certificado autofirmado con estos pasos, póngase en contacto con nosotros mediante la herramienta de comentarios. También puede abrir el Explorador de Storage desde la línea de comandos mediante la marca `--ignore-certificate-errors`. Cuando se abre con esta marca, el Explorador de Storage pasa por alto los errores de certificado.

## <a name="sign-in-issues"></a>Problemas de inicio de sesión

### <a name="blank-sign-in-dialog-box"></a>Cuadro de diálogo de inicio de sesión en blanco

Los cuadros de diálogo de inicio de sesión en blanco a menudo aparecen cuando Servicios de federación de Active Directory (AD FS) solicita al Explorador de Storage que realice una redirección, lo que no es compatible con Electron. Para solucionar este problema, puede intentar usar un flujo de código de dispositivo para el inicio de sesión. Para hacerlo, siga estos pasos:

1. En la barra de herramientas vertical izquierda, abra **Configuración**. En el panel Configuración, vaya a **Aplicación** > **Iniciar sesión**. Habilite **Use device code flow sign-in** (Usar inicio de sesión de flujo de código del dispositivo).
2. Abra el cuadro de diálogo **Connect** (Conectar) (mediante el icono de interruptor de la barra vertical izquierda o seleccionando **Add Account** (Agregar cuenta) en el panel de la cuenta).
3. Elija el entorno en el que desea iniciar sesión.
4. Seleccione **Iniciar sesión**.
5. Siga las instrucciones del siguiente panel.

Si no puede iniciar sesión en la cuenta que desea utilizar porque el explorador predeterminado ya está registrado en otra cuenta, realice una de estas acciones:

- Copie manualmente el vínculo y el código en una sesión privada del explorador.
- Copie manualmente el vínculo y el código en otro navegador distinto.

### <a name="reauthentication-loop-or-upn-change"></a>Bucle de reautenticación o cambio UPN

Si se encuentra en un bucle de reautenticación o ha cambiado el UPN de una de sus cuentas, siga estos pasos:

1. Quite todas las cuentas y cierre el Explorador de Storage.
2. Elimine la carpeta .IdentityService de la máquina. En Windows, la carpeta se encuentra en `C:\users\<username>\AppData\Local`. En Mac y Linux, puede encontrar la carpeta en la raíz de su directorio de usuario.
3. En Mac o Linux, también deberá eliminar la entrada Microsoft.Developer.IdentityService del almacén de claves de su sistema operativo. En Mac, el almacén de claves es la aplicación *Gnome Keychain*. En Linux, la aplicación habitualmente se denomina _Keyring_ pero el nombre puede ser diferente en función de su distribución.

### <a name="conditional-access"></a>Acceso condicional

A causa de una limitación en la biblioteca Azure AD que usa el Explorador de Storage, el acceso condicional no se admite si el Explorador de Storage se usa en Windows 10, Linux o macOS.

## <a name="mac-keychain-errors"></a>Errores de cadena de claves de Mac

En ocasiones, el llavero de macOS puede entrar en un estado que ocasiona problemas en la biblioteca de autenticación del Explorador de Storage. Para sacar el llavero de ese estado, siga estos pasos:

1. Cierre el Explorador de Storage.
2. Abra el llavero (presione Comando + barra espaciadora, escriba **llavero** y presione Entrar).
3. Seleccione el llavero de "inicio de sesión".
4. Seleccione el icono de candado para cerrar el llavero (el candado aparecerá cerrado cuando se complete el proceso. Puede tardar unos segundos, en función de las aplicaciones que haya abiertas).

    ![Icono del candado](./media/storage-explorer-troubleshooting/unlockingkeychain.png)

5. Abra el Explorador de Storage.
6. Aparece un mensaje similar a este: "Service hub wants to access the Keychain" (El Centro de servicios desea acceder al llavero). Escriba la contraseña de la cuenta de administrador de Mac y seleccione **Permitir siempre** (o **Permitir** si **Permitir siempre** no está disponible).
7. Intente iniciar sesión.

### <a name="general-sign-in-troubleshooting-steps"></a>Pasos generales para solucionar problemas en inicio de sesión

* Si se encuentra en macOS y la ventana de inicio de sesión no aparece en el cuadro de diálogo **Waiting for authentication** (Esperando autenticación), intente [estos pasos](#mac-keychain-errors).
* Reinicie el Explorador de Storage.
* Si la ventana de autenticación está en blanco, espere al menos un minuto antes de cerrar el cuadro de diálogo de autenticación.
* Asegúrese de que el proxy y el certificado están configurados correctamente tanto para la máquina como para el Explorador de Storage.
* Si utiliza Windows y tiene acceso a Visual Studio 2019 en el mismo equipo y a las credenciales de inicio de sesión, pruebe a iniciar sesión en Visual Studio 2019. Después de un correcto inicio de sesión en Visual Studio 2019, puede abrir el Explorador de Storage y ver la cuenta en el panel de la cuenta.

Si ninguno de estos métodos funciona, [abra una incidencia en GitHub](https://github.com/Microsoft/AzureStorageExplorer/issues).

### <a name="missing-subscriptions-and-broken-tenants"></a>Suscripciones que faltan e inquilinos rotos

Si no puede recuperar las suscripciones después de iniciar sesión correctamente, pruebe los siguientes métodos de solución de problemas:

* Compruebe que la cuenta tiene acceso a las que espera. Para comprobar que puede acceder, inicie sesión en el portal para el entorno de Azure que intenta usar.
* Asegúrese de que ha iniciado sesión en el entorno correcto de Azure (Azure, Azure China 21Vianet, Azure Alemania, Azure US Government o entorno personalizado).
* Si está detrás de un servidor proxy, asegúrese de que ha configurado correctamente el proxy del Explorador de Storage.
* Pruebe a eliminar la cuenta y volver a agregarla.
* Si hay un vínculo "Más información", compruebe qué mensajes de error se notifican a los inquilinos en los que se producen errores. Si no está seguro de cómo responder a los mensajes de error, no dude en [abrir una incidencia en GitHub](https://github.com/Microsoft/AzureStorageExplorer/issues).

## <a name="cant-remove-an-attached-account-or-storage-resource"></a>No se puede quitar una cuenta adjunta ni un recurso de almacenamiento

Si no puede hacerlo mediante la interfaz de usuario, para eliminar manualmente todos los recursos adjuntos debe eliminar las siguientes carpetas:

* Windows: `%AppData%/StorageExplorer`
* macOS: `/Users/<your_name>/Library/Application Support/StorageExplorer`
* Linux: `~/.config/StorageExplorer`

> [!NOTE]
> Cierre el Explorador de Storage antes de eliminar estas carpetas.

> [!NOTE]
> Si alguna vez ha importado un certificado SSL, haga una copia de seguridad del contenido del directorio `certs`. Después, puede utilizar la copia de seguridad para volver a importar los certificados SSL.

## <a name="proxy-issues"></a>Problemas de proxy

Explorador de Storage admite la conexión a recursos de Azure Storage a través de un servidor proxy. Si experimenta algún problema al conectarse a Azure a través de un proxy, estas son algunas sugerencias.

> [!NOTE]
> Explorador de Storage solo admite la autenticación básica con servidores proxy. No se admiten otros métodos de autenticación, como NTLM.

> [!NOTE]
> El Explorador de Storage no es compatible con los archivos de configuración automática del proxy para configurar el proxy.

### <a name="verify-storage-explorer-proxy-settings"></a>Comprobación de la configuración del proxy del Explorador de Storage

El valor de **Aplicación → Proxy → Configuración de proxy** determina de qué origen el Explorador de Storage obtiene la configuración del proxy.

Si selecciona "Use environment variables" (Usar variables de entorno), asegúrese de establecer las variables de entorno `HTTPS_PROXY` o `HTTP_PROXY` (las variables de entorno distinguen mayúsculas de minúsculas, por lo que debe asegurarse de establecer las variables correctas). Si estas variables están sin definir o no son válidas, el Explorador de Storage no utilizará un proxy. Reinicie el Explorador de Storage después de modificar las variables de entorno.

Si selecciona "Use app proxy settings" (Usar la configuración de proxy de aplicación), asegúrese de que la configuración del proxy de la aplicación sea correcta.

### <a name="steps-for-diagnosing-issues"></a>Pasos para el diagnóstico de problemas

Si todavía tiene problemas, pruebe estos métodos de solución de problemas:

1. Si puede conectarse a Internet sin usar el proxy, compruebe que el Explorador de Storage funciona sin la configuración del proxy habilitada. Si el Explorador de Storage se conecta correctamente, puede haber un problema con el servidor proxy. Junto con el administrador identifique los problemas.
2. Compruebe que las restantes aplicaciones que utilizan el servidor proxy funcionan como cabría esperar.
3. Compruebe que puede conectarse al portal del entorno de Azure que intenta usar.
4. Compruebe que puede recibir las respuestas de los puntos de conexión de servicio. Especifique una de las direcciones URL de punto de conexión en el explorador. Si puede conectarse, debe recibir una respuesta `InvalidQueryParameterValue` o XML similar.
5. Compruebe si otro usuario que usa el Explorador de Storage con el mismo servidor proxy puede conectarse. Si puede, es posible que deba ponerse en contacto con el administrador del servidor proxy.

### <a name="tools-for-diagnosing-issues"></a>Herramientas para el diagnóstico de problemas

Una herramienta de red, como Fiddler, puede ayudarle a diagnosticar problemas.

1. Configure la herramienta de red como un servidor proxy que se ejecute en el host local. Si tiene que seguir trabajando detrás de un proxy real, es posible que tenga que configurar la herramienta de red para conectarse a través del proxy.
2. Compruebe el número de puerto utilizado por la herramienta de red.
3. Configure los valores del proxy del Explorador de Storage para usar el host local y el número de puerto de la herramienta de red (por ejemplo, "localhost: 8888").
 
Cuando lo haya hecho correctamente, la herramienta de red registrará las solicitudes de red realizadas por el Explorador de Storage para puntos de conexión de servicio y administración.
 
Si la herramienta de red no parece estar registrando el tráfico del Explorador de Storage, intente probar la herramienta con otra aplicación. Por ejemplo, escriba la dirección URL del punto de conexión de uno de los recursos de almacenamiento (como `https://contoso.blob.core.windows.net/`) en un explorador web y recibirá una respuesta similar a la siguiente:

  ![Código de ejemplo](./media/storage-explorer-troubleshooting/4022502_en_2.png)

  La respuesta sugiere que el recurso existe, aunque no pueda acceder a él.

Si la herramienta de red solo muestra el tráfico de otras aplicaciones, es posible que necesite ajustar la configuración del proxy en el Explorador de Storage. De lo contrario, deberá ajustar la configuración de la herramienta.

### <a name="contact-proxy-server-admin"></a>Póngase en contacto con el administrador del servidor proxy

Si la configuración del proxy es correcta, es posible que deba ponerse en contacto con el administrador del servidor proxy para:

* Asegurarse de que el proxy no bloquee el tráfico a los puntos de conexión de administración de Azure o de los recursos.
* Compruebe el protocolo de autenticación utilizado por el servidor proxy. El Explorador de Storage solo admite protocolos de autenticación básicos. El Explorador de Storage no es compatible con los servidores proxy NTLM.

## <a name="unable-to-retrieve-children-error-message"></a>Mensaje de error "No se pueden recuperar los elementos secundarios"

Si está conectado a Azure a través de un servidor proxy, compruebe que la configuración de proxy sea correcta.

Si el propietario de una suscripción o una cuenta le concedió acceso a un recurso, compruebe que disponga de permisos de lectura o lista para dicho recurso.

## <a name="connection-string-doesnt-have-complete-configuration-settings"></a>La cadena de conexión no tiene una configuración completa

Si recibe este mensaje de error, es posible que no tenga los permisos necesarios para obtener las claves para la cuenta de Storage. Para confirmar si este es el motivo, vaya al portal y busque la cuenta de Storage. Para ello, haga clic con el botón derecho en el nodo de la cuenta de Storage y seleccione **Abrir en el portal**. Luego, vaya a la hoja **Claves de acceso**. Si no tiene permisos para ver las claves, verá el mensaje "You don't have access" (No tiene acceso). Para solucionar este problema, puede obtener la clave de cuenta de otra persona y asociarla mediante el nombre y la clave, o bien puede pedir una SAS para la cuenta de Storage y usarla para asociar la cuenta de Storage.

Si ve las claves de la cuenta, abra una incidencia en GitHub para que podamos ayudarle a resolver el problema.

## <a name="error-occurred-while-adding-new-connection-typeerror-cannot-read-property-version-of-undefined"></a>Error al agregar una nueva conexión: TypeError: no se puede leer la propiedad "version" de la conexión no definida

Si recibe este mensaje de error al intentar agregar una conexión personalizada, es posible que los datos de la conexión almacenados en el administrador de credenciales local estén dañados. Para solucionar este problema, pruebe a eliminar las conexiones locales dañadas y, después, vuelva a agregarlas:

1. Inicie el Explorador de Storage. En el menú, vaya a **Help** (Ayuda) > **Toggle Developer Tools** (Activar y desactivar herramientas de desarrollo).
2. En la ventana abierta, en la pestaña **Application** (Aplicación), vaya a **Local Storage** (Almacenamiento local) (lado izquierdo) > **file://** .
3. En función del tipo de conexión con el que tenga un problema, busque su clave y copie su valor en un editor de texto. El valor es una matriz de los nombres de conexión personalizados parecida a la siguiente:
    * Cuentas de almacenamiento
        * `StorageExplorer_CustomConnections_Accounts_v1`
    * Contenedores de blobs
        * `StorageExplorer_CustomConnections_Blobs_v1`
        * `StorageExplorer_CustomConnections_Blobs_v2`
    * Recursos compartidos de archivos
        * `StorageExplorer_CustomConnections_Files_v1`
    * Colas
        * `StorageExplorer_CustomConnections_Queues_v1`
    * Tablas
        * `StorageExplorer_CustomConnections_Tables_v1`
4. Después de guardar los nombres de conexión actuales, en Herramientas de desarrollo seleccione `[]`.

Si desea conservar las conexiones que no estén dañadas, puede usar los siguientes pasos para buscar las conexiones dañadas. Si no le importa perder todas las conexiones existentes, puede omitir estos pasos y seguir las instrucciones específicas de la plataforma para borrar los datos de la conexión.

1. En un editor de texto, vuelva a agregar los nombres de conexión a Herramientas de desarrollo y compruebe si la conexión sigue funcionando.
2. Si una conexión funciona correctamente, no está dañada y se puede dejar ahí sin ningún riesgo. Si una conexión no funciona, quite su valor de Herramientas de desarrollo y guárdelo para poder volver a agregarlo más adelante.
3. Repita el procedimiento hasta que haya examinado todas las conexiones.

Después de recorrer todas las conexiones, en busca de todos los nombres de conexión que no se han agregado de nuevo, debe borrar los datos dañados (si los hay) y volver a agregarlos mediante los pasos estándar del Explorador de Storage:

# <a name="windows"></a>[Windows](#tab/Windows)

1. En el menú **Inicio**, busque **Administrador de credenciales** y ábralo.
2. Vaya a **Credenciales de Windows**.
3. En **Credenciales genéricas** , busque las entradas que tengan la clave `<connection_type_key>/<corrupted_connection_name>` (por ejemplo, `StorageExplorer_CustomConnections_Accounts_v1/account1`).
4. Elimine estas entradas y vuelva a agregar las conexiones.

# <a name="macos"></a>[macOS](#tab/macOS)

1. Abra Spotlight (comando + barra espaciadora) y busque **Keychain access** (Acceso a llavero).
2. Busque las entradas que tengan la clave `<connection_type_key>/<corrupted_connection_name>` (por ejemplo, `StorageExplorer_CustomConnections_Accounts_v1/account1`).
3. Elimine estas entradas y vuelva a agregar las conexiones.

# <a name="linux"></a>[Linux](#tab/Linux)

La administración de las credenciales locales varía en función de la distribución de Linux. Si la distribución de Linux no proporciona una herramienta de GUI integrada para la administración de credenciales locales, puede instalar una herramienta de terceros para administrar las credenciales locales. Por ejemplo, puede usar [Seahorse](https://wiki.gnome.org/Apps/Seahorse/), una herramienta de GUI de código abierto para administrar credenciales locales de Linux.

1. Abra la herramienta de administración de credenciales locales y busque las credenciales que ha guardado.
2. Busque las entradas que tengan la clave `<connection_type_key>/<corrupted_connection_name>` (por ejemplo, `StorageExplorer_CustomConnections_Accounts_v1/account1`).
3. Elimine estas entradas y vuelva a agregar las conexiones.
---

Si el error no desaparece después de realizar estos pasos o si desea compartir lo que crea que ha dañado las conexiones, [abra una incidencia](https://github.com/microsoft/AzureStorageExplorer/issues) en nuestra página de GitHub.

## <a name="issues-with-sas-url"></a>Problemas con la URL de SAS

Si se va conectar a un servicio mediante una dirección URL de SAS y se produce este error:

* Compruebe que la dirección URL proporciona los permisos necesarios para leer o enumerar los recursos.
* Compruebe que la dirección URL no haya expirado.
* Si la dirección URL de SAS se basa en una directiva de acceso, compruebe que la directiva de acceso no haya sido revocada.

Si por error realizó la asociación mediante una dirección URL de SAS no válida y ahora no puede desasociarla, siga estos pasos:

1. Al ejecutar el Explorador de Storage, presione F12 para abrir la ventana Herramientas de desarrollo.
2. En la pestaña **Aplicación**, seleccione **Almacenamiento local** > **file://** en el árbol de la izquierda.
3. Busque la clave asociada al tipo de servicio del URI de SAS problemática. Por ejemplo, si el identificador URI de SAS incorrecto es para un contenedor de blobs, busque la clave llamada `StorageExplorer_AddStorageServiceSAS_v1_blob`.
4. El valor de la clave debe ser una matriz JSON. Busque el objeto asociado con el identificador URI incorrecto y elimínelo.
5. Presione Ctrl + R para volver a cargar el Explorador de Storage.

## <a name="linux-dependencies"></a>Dependencias de Linux

### <a name="snap"></a>Ajustar

Tanto el Explorador de Storage 1.10.0 como las versiones posteriores en forma de complemento están disponibles en Snap Store. El complemento de Explorador de Storage instala automáticamente todas sus dependencias y se actualiza cuando hay una nueva versión del complemento disponible. La instalación del complemento de Explorador de Storage es el método recomendado de instalación.

El Explorador de Storage requiere el uso de un administrador de contraseñas, que puede que se necesite para establecer una conexión manualmente para que el Explorador de Storage funcione correctamente. Puede conectar el Explorador de Storage al administrador de contraseñas del sistema mediante la ejecución del siguiente comando:

```bash
snap connect storage-explorer:password-manager-service :password-manager-service
```

### <a name="targz-file"></a>Archivo .tar.gz

También puede descargar la aplicación en forma de archivo .tar.gz, pero tendrá que instalar las dependencias manualmente.

El Explorador de Storage tal y como se proporciona en la descarga .tar.gz solo es compatible con las siguientes versiones de Ubuntu. Es posible que el Explorador de Storage funcione en otras distribuciones de Linux, pero no se admiten oficialmente.

- Ubuntu 20.04 x64
- Ubuntu 18.04 x64
- Ubuntu 16.04 x64

El Explorador de Storage requiere que .NET Core esté instalado en el sistema. Se recomienda .NET Core 2,1, pero el Explorador de Storage funcionará también con 2.2.

> [!NOTE]
> La versión 1.7.0 del Explorador de Storage y anteriores requieren .NET Core 2.0. Si tiene una versión más reciente de .NET Core instalada, tendrá que [aplicar un parche al Explorador de Storage](#patching-storage-explorer-for-newer-versions-of-net-core). Si ejecuta el Explorador de Storage 1.8.0, o cualquier versión posterior, por lo menos necesita .NET Core 2.1.

# <a name="ubuntu-2004"></a>[Ubuntu 20.04](#tab/2004)

1. Descargue el archivo .tar.gz del Explorador de Storage.
2. Instale el [entorno de ejecución de .NET Core](/dotnet/core/install/linux):
   ```bash
   wget https://packages.microsoft.com/config/ubuntu/20.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb; \
     sudo dpkg -i packages-microsoft-prod.deb; \
     sudo apt-get update; \
     sudo apt-get install -y apt-transport-https && \
     sudo apt-get update && \
     sudo apt-get install -y dotnet-runtime-2.1
   ```

# <a name="ubuntu-1804"></a>[Ubuntu 18.04](#tab/1804)

1. Descargue el archivo .tar.gz del Explorador de Storage.
2. Instale el [entorno de ejecución de .NET Core](/dotnet/core/install/linux):
   ```bash
   wget https://packages.microsoft.com/config/ubuntu/18.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb; \
     sudo dpkg -i packages-microsoft-prod.deb; \
     sudo apt-get update; \
     sudo apt-get install -y apt-transport-https && \
     sudo apt-get update && \
     sudo apt-get install -y dotnet-runtime-2.1
   ```

# <a name="ubuntu-1604"></a>[Ubuntu 16.04](#tab/1604)

1. Descargue el archivo .tar.gz del Explorador de Storage.
2. Instale el [entorno de ejecución de .NET Core](/dotnet/core/install/linux):
   ```bash
   wget https://packages.microsoft.com/config/ubuntu/16.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb; \
     sudo dpkg -i packages-microsoft-prod.deb; \
     sudo apt-get update; \
     sudo apt-get install -y apt-transport-https && \
     sudo apt-get update && \
     sudo apt-get install -y dotnet-runtime-2.1
   ```
---

Muchas de las bibliotecas que necesita el Explorador de Storage viene preinstaladas con las instalaciones estándar de Canonical de Ubuntu. Es posible que falten algunas de estas bibliotecas en los entornos personalizados. Si tiene problemas para iniciar el Explorador de Storage, se recomienda que se asegure de que los siguientes paquetes están instalados en el sistema:

- iproute2
- libasound2
- libatm1
- libgconf2-4
- libnspr4
- libnss3
- libpulse0
- libsecret-1-0
- libx11-xcb1
- libxss1
- libxtables11
- libxtst6
- xdg-utils

### <a name="patching-storage-explorer-for-newer-versions-of-net-core"></a>Revisión del Explorador de Storage para las versiones más recientes de .NET Core

En el caso del Explorador de Storage 1.7.0, u otras versiones anteriores, es posible que tenga que aplicar un parche a la versión de .NET Core que usa el Explorador de Storage:

1. Descargue la versión 1.5.43 de StreamJsonRpc [de NuGet](https://www.nuget.org/packages/StreamJsonRpc/1.5.43). Busque el vínculo "Descargar paquete" en el lado derecho de la página.
2. Después de descargar el paquete, cambie la extensión del archivo de `.nupkg` a `.zip`.
3. Descomprima el paquete.
4. Abra la carpeta `streamjsonrpc.1.5.43/lib/netstandard1.1/`.
5. Copie `StreamJsonRpc.dll` en las siguientes ubicaciones de la carpeta del Explorador de Storage:
   * `StorageExplorer/resources/app/ServiceHub/Services/Microsoft.Developer.IdentityService/`
   * `StorageExplorer/resources/app/ServiceHub/Hosts/ServiceHub.Host.Core.CLR.x64/`

## <a name="open-in-explorer-from-the-azure-portal-doesnt-work"></a>"Abrir en el Explorador", de Azure Portal, no funciona

Si el botón **Abrir en el Explorador** de Azure Portal no funciona, asegúrese de que usa un explorador compatible. Se ha probado la compatibilidad de los siguientes exploradores:
* Microsoft Edge
* Mozilla Firefox
* Google Chrome
* Microsoft Internet Explorer

## <a name="next-steps"></a>Pasos siguientes

Si ninguna de estas soluciones funciona, [abra una incidencia en GitHub](https://github.com/Microsoft/AzureStorageExplorer/issues). Para ello, seleccione el botón **Report issue to GitHub** (Informar de problema a GitHub), que se encuentra en la esquina inferior izquierda.

![Comentarios](./media/storage-explorer-troubleshooting/feedback-button.PNG)