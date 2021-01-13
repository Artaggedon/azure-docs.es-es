---
title: Identidades administradas en Azure HDInsight
description: Proporciona información general de la implementación de las identidades administradas en Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 04/15/2020
ms.openlocfilehash: 87bc2338ecc48f1115a406c276ef221cb185a4c5
ms.sourcegitcommit: aacbf77e4e40266e497b6073679642d97d110cda
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/12/2021
ms.locfileid: "98118632"
---
# <a name="managed-identities-in-azure-hdinsight"></a>Identidades administradas en Azure HDInsight

Una identidad administrada es una identidad registrada en Azure Active Directory (Azure AD) cuyas credenciales son administradas por Azure. Con identidades administradas, no es necesario registrar entidades de servicio en Azure AD. Tampoco mantener credenciales como certificados.

Se usan identidades administradas en Azure HDInsight para acceder a servicios de dominio de Azure AD o a archivos de Azure Data Lake Storage Gen2 cuando sea necesario.

Hay dos tipos de identidades administradas: asignadas por el sistema y asignadas por el usuario. Azure HDInsight solo admite identidades administradas asignadas por el usuario; no admite identidades administradas asignadas por el sistema. Una identidad administrada asignada por el usuario se crea como un recurso independiente de Azure que, a continuación, se puede asignar a una o varias instancias de servicio de Azure. En cambio, una identidad administrada asignada por el sistema se crea en Azure AD y, a continuación, se habilita directamente en una instancia en particular de un servicio de Azure de forma automática. La vida útil de esa identidad administrada asignada por el sistema se asocia entonces a la vida de la instancia de servicio en la que se ha habilitado.

## <a name="hdinsight-managed-identity-implementation"></a>Implementación de la identidad administrada de HDInsight

En Azure HDInsight, las identidades administradas solo se pueden usar en el servicio HDInsight para los componentes internos. Actualmente, no hay ningún método admitido para generar tokens de acceso con las identidades administradas instaladas en los nodos de clúster de HDInsight para acceder a los servicios externos. En algunos servicios de Azure, como máquinas virtuales de proceso, las identidades administradas se implementan con un punto de conexión que puede usar para adquirir tokens de acceso. Actualmente, este punto de conexión no está disponible en los nodos de HDInsight.

Si necesita arrancar las aplicaciones para evitar colocar secretos y contraseñas en los trabajos de análisis (por ejemplo, trabajos de SCALA), puede distribuir sus propios certificados a los nodos de clúster mediante acciones de script y, luego, usar ese certificado para adquirir un token de acceso (por ejemplo, para acceder a Azure Key Vault).

## <a name="create-a-managed-identity"></a>Creación de una entidad administrada

Las identidades administradas se pueden crear con cualquiera de los métodos siguientes:

* [Azure Portal](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md)
* [Azure PowerShell](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-powershell.md)
* [Azure Resource Manager](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-arm.md)
* [CLI de Azure](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-cli.md)

Los pasos restantes para configurar la identidad administrada dependen del escenario donde se vaya a usar.

## <a name="managed-identity-scenarios-in-azure-hdinsight"></a>Escenarios de identidades administradas en Azure HDInsight

Las identidades administradas se usan en Azure HDInsight en varios escenarios. Consulte en los documentos relacionados las instrucciones detalladas para la instalación y la configuración:

* [Azure Data Lake Storage Gen2](hdinsight-hadoop-use-data-lake-storage-gen2-portal.md#create-a-user-assigned-managed-identity)
* [Paquete de seguridad de la empresa](domain-joined/apache-domain-joined-configure-using-azure-adds.md#create-and-authorize-a-managed-identity)
* [Cifrado de disco mediante claves administradas por el cliente](disk-encryption.md)

HDInsight renovará automáticamente los certificados para las identidades administradas que use en estos escenarios. Aun así, hay una limitación cuando se usan varias identidades administradas diferentes para clústeres de larga duración. Es posible que la renovación de certificados no funcione según lo esperado para todas las identidades administradas. Debido a esta limitación, si planea usar clústeres de larga duración (por ejemplo, más de 60 días), se recomienda que use la misma identidad administrada para todos los escenarios anteriores. 

Si ya ha creado un clúster de larga duración con varias identidades administradas diferentes y se encuentra con uno de estos problemas:
 * En los clústeres de ESP, empiezan a producirse errores en los servicios de clúster, o bien empiezan a producirse errores de autenticación en el escalado vertical y otras operaciones.
 * En los clústeres de ESP, al cambiar el certificado LDAPS de AAD-DS, el certificado LDAPS no se actualiza automáticamente y, por tanto, empiezan a producirse errores en el escalado vertical y la sincronización de LDAP.
 * Empiezan a producirse errores en el acceso de MSI a ADLS Gen2.
 * Las claves de cifrado no se pueden girar en el escenario CMK.

Después, debe asignar los roles y permisos necesarios para los escenarios anteriores a todas las identidades administradas que se usan en el clúster. Por ejemplo, si ha usado identidades administradas diferentes para los clústeres ADLS Gen2 y ESP, ambos deben tener asignados los roles "Propietario de datos de Storage Blob" y "Colaborador de HDInsight Domain Services" para evitar encontrarse con estos problemas.

## <a name="faq"></a>Preguntas más frecuentes

### <a name="what-happens-if-i-delete-the-managed-identity-after-the-cluster-creation"></a>¿Qué ocurre si elimino la identidad administrada después de la creación del clúster?

El clúster tendrá problemas cuando se necesite la identidad administrada. Actualmente no hay ninguna manera de actualizar o cambiar la identidad administrada una vez creado el clúster. Por lo tanto, nuestra recomendación es asegurarse de que la identidad administrada no se elimina durante el tiempo de ejecución del clúster. También puede volver a crear el clúster y asignar una nueva identidad administrada.

## <a name="next-steps"></a>Pasos siguientes

* [¿Qué son las identidades administradas de los recursos de Azure?](../active-directory/managed-identities-azure-resources/overview.md)
