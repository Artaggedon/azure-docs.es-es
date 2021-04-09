---
title: Transformaciones de sincronización en la nube de Azure AD Connect
description: En este artículo se describe cómo usar transformaciones para modificar las asignaciones de atributos predeterminadas.
author: billmath
ms.author: billmath
manager: davba
ms.date: 12/02/2019
ms.topic: how-to
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 1c81ee0ebace65e50cdab5b5b223d5e5eae4ff9a
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "98612730"
---
# <a name="transformations"></a>Transformaciones

Con una transformación puede cambiar el comportamiento predeterminado del modo de sincronización de un atributo con Azure Active Directory (Azure AD) mediante la sincronización en la nube.

Para realizar esta tarea, debe modificar el esquema y volver a enviarlo a través de una solicitud web.

Para obtener más información sobre los atributos de sincronización en la nube, consulte [Descripción del esquema de Azure AD](concept-attributes.md).


## <a name="retrieve-the-schema"></a>Recuperación del esquema
Para recuperar el esquema, siga los pasos descritos en [Ver el esquema](concept-attributes.md#view-the-schema). 

## <a name="custom-attribute-mapping"></a>Asignación de atributos personalizados
Para agregar una asignación de atributos personalizados, siga estos pasos.

1. Copie el esquema en un editor de texto o de código como [Visual Studio Code](https://code.visualstudio.com/).
1. Busque el objeto que quiere actualizar en el esquema.

   ![Objeto del esquema](media/how-to-transformation/transform-1.png)</br>
1. Busque el código de `ExtensionAttribute3` en el objeto del usuario.

    ```
                            {
                                "defaultValue": null,
                                "exportMissingReferences": false,
                                "flowBehavior": "FlowWhenChanged",
                                "flowType": "Always",
                                "matchingPriority": 0,
                                "targetAttributeName": "ExtensionAttribute3",
                                "source": {
                                    "expression": "Trim([extensionAttribute3])",
                                    "name": "Trim",
                                    "type": "Function",
                                    "parameters": [
                                        {
                                            "key": "source",
                                            "value": {
                                                "expression": "[extensionAttribute3]",
                                                "name": "extensionAttribute3",
                                                "type": "Attribute",
                                                "parameters": []
                                            }
                                        }
                                    ]
                                }
                            },
    ```
1. Edite el código para que el atributo Company se asigne a `ExtensionAttribute3`.

   ```
                                    {
                                        "defaultValue": null,
                                        "exportMissingReferences": false,
                                        "flowBehavior": "FlowWhenChanged",
                                        "flowType": "Always",
                                        "matchingPriority": 0,
                                        "targetAttributeName": "ExtensionAttribute3",
                                        "source": {
                                            "expression": "Trim([company])",
                                            "name": "Trim",
                                            "type": "Function",
                                            "parameters": [
                                                {
                                                    "key": "source",
                                                    "value": {
                                                        "expression": "[company]",
                                                        "name": "company",
                                                        "type": "Attribute",
                                                        "parameters": []
                                                    }
                                                }
                                            ]
                                        }
                                    },
   ```
 1. Vuelva a copiar el esquema en el Probador de Graph, cambie el **tipo de solicitud** a **PUT** y seleccione **Ejecutar consulta**.

    ![Ejecución de la consulta](media/how-to-transformation/transform-2.png)

 1. Ahora, en Azure Portal, vaya a la configuración de la sincronización en la nube y seleccione **Reiniciar aprovisionamiento**.

    ![Reinicio del aprovisionamiento](media/how-to-transformation/transform-3.png)

 1. Después de unos minutos, compruebe que los atributos se están rellenando; para ello, ejecute la siguiente consulta en el Probador de Graph: `https://graph.microsoft.com/beta/users/{Azure AD user UPN}`.
 1. Ahora debería ver el valor.

    ![Se muestra el valor](media/how-to-transformation/transform-4.png)

## <a name="custom-attribute-mapping-with-function"></a>Asignación de atributos personalizados con una función
Si quiere una asignación más avanzada, puede utilizar funciones que le permiten tratar los datos y crear valores para el atributo que se adapten a las necesidades de la organización.

Para realizar esta tarea, siga los pasos anteriores y, a continuación, edite la función que se usa para construir el valor final.

Para más información sobre la sintaxis y ejemplos de expresiones, consulte [Escritura de expresiones para la asignación de atributos en Azure Active Directory](reference-expressions.md).


## <a name="next-steps"></a>Pasos siguientes 

- [¿Qué es el aprovisionamiento?](what-is-provisioning.md)
- [¿Qué es la sincronización en la nube de Azure AD Connect?](what-is-cloud-sync.md)
