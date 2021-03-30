---
title: 'Tutorial: Invitación en bloque a usuarios de colaboración B2B: Azure AD'
description: En este tutorial, obtendrá información sobre cómo usar PowerShell y un archivo CSV para enviar invitaciones de forma masiva a usuarios externos de colaboración de Azure AD B2B.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: tutorial
ms.date: 03/17/2021
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.collection: M365-identity-device-management
ms.openlocfilehash: 01deae46c442fc95c6aead0f11de929f47163c3c
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "104586564"
---
# <a name="tutorial-bulk-invite-azure-ad-b2b-collaboration-users"></a>Tutorial: Invitación en masa a usuarios de colaboración de Azure AD B2B

Si usas la colaboración de Azure Active Directory (Azure AD) B2B para trabajar con asociados externos, puede invitar a varios usuarios a su organización al mismo tiempo. En este tutorial aprenderá a usar Azure Portal para enviar invitaciones de forma masiva a usuarios externos. En particular, haga lo siguiente:

> [!div class="checklist"]
> * Use **Invitar usuarios en bloque** para preparar un archivo de valores separados por comas (.csv) con la información del usuario y las preferencias de invitación.
> * Cargue el archivo .csv en Azure AD.
> * Verifique si los usuarios se agregaron al directorio.

Antes de comenzar, si no tiene una cuenta de Azure Active Directory, [cree una gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="understand-the-csv-template"></a>Nociones sobre la plantilla CSV

Descargue y rellene la plantilla CSV de carga masiva para ayudarle a invitar correctamente a los usuarios invitados de Azure AD de forma masiva. La plantilla CSV que descargue podría parecerse a este ejemplo:

![Hoja de cálculo para la carga y notas que explican el propósito y los valores de cada fila y columna](media/tutorial-bulk-invite/understand-template.png)

### <a name="csv-template-structure"></a>Estructura de la plantilla CSV

Las filas de una plantilla CSV descargada son las siguientes:

- **Número de versión**: la primera fila, que contiene el número de versión, debe estar incluida en el archivo CSV de carga.
- **Encabezados de columna**: el formato de los encabezados de columna es &lt;*Nombre del elemento*&gt; [nombreDePropiedad] &lt;*Required (Obligatorio) o en blanco*&gt;. Por ejemplo, `Email address to invite [inviteeEmail] Required`. Algunas versiones anteriores de la plantilla podrían tener ligeras variaciones.
- **Fila de ejemplos**: en la plantilla se incluye una fila de ejemplos de valores para cada columna. Debe quitar la fila de ejemplos y reemplazarla por sus propias entradas.

### <a name="additional-guidance"></a>Instrucciones adicionales

- Las dos primeras filas de la plantilla de carga no se deben eliminar ni modificar, o no se podrá procesar la carga.
- Las columnas necesarias se enumeran en primer lugar.
- No se recomienda agregar nuevas columnas a la plantilla. Cualquier columna adicional que agregue se omitirá y no se procesará.
- Se recomienda que descargue la versión más reciente de la plantilla CSV tan a menudo como sea posible.

## <a name="prerequisites"></a>Prerrequisitos

Necesita dos o más cuentas de correo electrónico de prueba a las que poder enviar las invitaciones. Las cuentas deben estar fuera de su organización. Puede usar cualquier tipo de cuenta, incluidas las cuentas sociales como las direcciones de gmail.com o outlook.com.

## <a name="invite-guest-users-in-bulk"></a>Invitación en bloque a usuarios

1. Inicie sesión en Azure Portal con una cuenta que sea la del administrador global de la organización.
2. En el panel de navegación, seleccione **Azure Active Directory**.
3. En **Administrar**, seleccione **Todos los usuarios**.
4. Seleccione **Operaciones masivas** > **Invitar en bloque**.

    ![Botón Invitar en bloque](media/tutorial-bulk-invite/bulk-invite-button.png)

4. En la página **Invitar usuarios en bloque**, seleccione **Descargar** para obtener una plantilla .csv válida con las propiedades de la invitación.

     ![Descarga del archivo CSV](media/tutorial-bulk-invite/download-button.png)

1. Abra la plantilla .csv y agregue una línea por cada usuario invitado. Los valores obligatorios son:

   * **Dirección de correo electrónico para enviar la invitación**: el usuario que recibirá una invitación.

   * **URL de redireccionamiento**: la dirección URL a la que se reenviará al usuario invitado después de que acepte la invitación. Si desea reenviar al usuario a la página Aplicaciones, debe cambiar este valor a https://myapps.microsoft.com o https://myapplications.microsoft.com.

    ![Ejemplo de un archivo csv con usuarios invitados especificados](media/tutorial-bulk-invite/bulk-invite-csv.png)

   > [!NOTE]
   > No use comas en **Mensaje de invitación personalizado**, porque impedirán que el mensaje se analice correctamente.

6. Guarde el archivo.
7. En la página **Invitar usuarios en bloque**, en **Cargue el archivo csv**, vaya al archivo. Al seleccionarlo, comienza su validación. 
8. Cuando finalice la validación del contenido del archivo, aparecerá el mensaje **Archivo cargado correctamente**. Si hay errores, debe corregirlos para poder enviar el trabajo.
9. Cuando el archivo supere la validación, seleccione **Enviar** para iniciar la operación masiva de Azure que agrega las invitaciones. 
10. Para ver el estado del trabajo, seleccione **Haga clic aquí para ver el estado de cada operación**. O bien, puede seleccionar **Resultados de la operación masiva** en la sección **Actividad**. Para más información sobre cada elemento de línea dentro de la operación en bloque, seleccione los valores de las columnas **Número de elementos correctos**, **Número de errores** o **Total de solicitudes**. Si se produjeron errores, se mostrarán sus motivos.

    ![Ejemplo de resultados de la operación masiva](media/tutorial-bulk-invite/bulk-operation-results.png)

11. Cuando el trabajo finalice, verá una notificación indicando que la operación masiva se realizó correctamente.

## <a name="verify-guest-users-in-the-directory"></a>Verificación de usuarios invitados en el directorio

Compruebe en Azure Portal o mediante PowerShell que los usuarios invitados que ha agregado existen en el directorio.

### <a name="view-guest-users-in-the-azure-portal"></a>Visualización de los usuarios invitados en Azure Portal

1. Inicie sesión en Azure Portal con una cuenta que sea la del administrador de usuarios de la organización.
2. En el panel de navegación, seleccione **Azure Active Directory**.
3. En **Administrar**, seleccione **Usuarios**.
4. En **Mostrar**, seleccione **Solo usuarios invitados** y compruebe que los usuarios que ha agregado aparecen en la lista.

### <a name="view-guest-users-with-powershell"></a>Visualización de los usuarios invitados con PowerShell

Ejecute el siguiente comando:

```powershell
 Get-AzureADUser -Filter "UserType eq 'Guest'"
```

Debería ver los usuarios invitados en la lista, con un nombre principal de usuario (UPN) con el formato *direccióndecorreoelectrónico*#EXT#\@*dominio*. Por ejemplo, *lstokes_fabrikam.com#EXT#\@contoso.onmicrosoft.com*, donde contoso.onmicrosoft.com es la organización desde la que se enviaron las invitaciones.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no necesite las cuentas de usuario de prueba del directorio de Azure Portal, puede eliminarlas desde la página Usuarios; para ello, active la casilla situada junto al usuario invitado y, a continuación, seleccione **Eliminar**. 

También puede ejecutar el siguiente comando de PowerShell para eliminar una cuenta de usuario:

```powershell
 Remove-AzureADUser -ObjectId "<UPN>"
```

Por ejemplo: `Remove-AzureADUser -ObjectId "lstokes_fabrikam.com#EXT#@contoso.onmicrosoft.com"`

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha enviado invitaciones en masa a usuarios invitados fuera de la organización. A continuación, obtenga información sobre cómo funciona el proceso de canje de la invitación.

> [!div class="nextstepaction"]
> [Obtenga información sobre el proceso de canje de la invitación de colaboración de Azure AD B2B](redemption-experience.md)
