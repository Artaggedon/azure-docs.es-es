---
title: Publicación de aplicaciones administradas a través del portal
description: Se explica cómo usar Azure Portal para crear una aplicación administrada de Azure diseñada para los miembros de su organización.
author: tfitzmac
ms.topic: conceptual
ms.date: 11/02/2017
ms.author: tomfitz
ms.openlocfilehash: 05302d92f2304be35a7b88fac6fabfc17b13c63e
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "75649588"
---
# <a name="publish-a-service-catalog-application-through-azure-portal"></a>Publicación de una aplicación de catálogo de servicios mediante Azure Portal

Puede usar Azure Portal para publicar [aplicaciones administradas](overview.md) que están diseñadas para los miembros de su organización. Por ejemplo, un departamento de TI puede publicar aplicaciones administradas que garantizan el cumplimiento de los estándares de la organización. Estas aplicaciones administradas están disponibles a través del catálogo de servicios y no en Azure Marketplace.

## <a name="prerequisites"></a>Prerrequisitos

A la hora de publicar una aplicación administrada, debe especificar una identidad para administrar los recursos. Es recomendable que especifique un grupo de usuarios de Azure Active Directory. Para crear un grupo de usuarios de Azure Active Directory, consulte [Creación de un grupo y adición de miembros en Azure Active Directory](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md). 

El archivo .zip que contiene la definición de la aplicación administrada debe estar disponible mediante un identificador URI. Se recomienda que cargue el archivo .zip en un blob de almacenamiento. 

## <a name="create-managed-application-with-portal"></a>Creación de una aplicación administrada con Azure Portal

1. En la esquina superior izquierda, seleccione **+ Nuevo**.

   ![Nuevo servicio](./media/publish-portal/new.png)

1. Busque **catálogo de servicios**.

1. En los resultados, desplácese hasta que encuentre **Definición de aplicación administrada del catálogo de servicios**. Selecciónelo.

   ![Busque las definiciones de aplicación administrada](./media/publish-portal/select-managed-apps-definition.png)

1. Seleccione **Crear** para iniciar el proceso de creación de la definición de la aplicación administrada.

   ![Creación de la definición de aplicación administrada](./media/publish-portal/create-definition.png)

1. Proporcione valores para nombre, nombre para mostrar, descripción, ubicación, suscripciones y grupo de recursos. Para el identificador URI del archivo de paquete, proporcione la ruta de acceso al archivo ZIP que creó.

   ![Proporcionar valores](./media/publish-portal/fill-application-values.png)

1. Cuando llegue a la sección de Autenticación y Nivel de bloqueo, seleccione **Agregar autorización**.

   ![Agregar autorización](./media/publish-portal/add-authorization.png)

1. Seleccione un grupo de Azure Active Directory para administrar los recursos y seleccione **Aceptar**.

   ![Agregar grupo de autorización](./media/publish-portal/add-auth-group.png)

1. Cuando haya proporcionado todos los valores, seleccione **Crear**.

   ![Creación de aplicación administrada](./media/publish-portal/create-app.png)

## <a name="next-steps"></a>Pasos siguientes

* Para una introducción a las aplicaciones administradas, consulte [Introducción a las aplicaciones administradas](overview.md).
* Para ver ejemplos de aplicaciones administradas, consulte [Sample projects for Azure managed applications](sample-projects.md) (Ejemplos de proyectos para aplicaciones administradas de Azure).
* Para aprender a crear un archivo de definición de interfaz de usuario para una aplicación administrada, consulte [Introducción a CreateUiDefinition](create-uidefinition-overview.md).