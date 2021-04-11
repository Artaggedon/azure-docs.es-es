---
title: 'Tutorial: Equilibrio de carga de máquinas virtuales en distintas zonas de disponibilidad: Azure Portal'
titleSuffix: Azure Load Balancer
description: En este tutorial, se explica cómo crear un equilibrador de carga estándar mediante un front-end con redundancia de zona para equilibrar la carga de las máquinas virtuales en distintas zonas de disponibilidad mediante Azure Portal.
services: load-balancer
documentationcenter: na
author: asudbring
manager: twooley
ms.service: load-balancer
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/27/2019
ms.author: allensu
ms.custom: seodec18
ms.openlocfilehash: 54dbc5f4000c9ae10592b04ca55c0fc528f40633
ms.sourcegitcommit: 73fb48074c4c91c3511d5bcdffd6e40854fb46e5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/31/2021
ms.locfileid: "106056086"
---
# <a name="tutorial-load-balance-vms-across-availability-zones-with-a-standard-load-balancer-using-the-azure-portal"></a>Tutorial: Equilibrio de carga de máquinas virtuales en distintas zonas de disponibilidad con Load Balancer Estándar mediante Azure Portal

El equilibrio de carga proporciona un mayor nivel de disponibilidad al distribuir las solicitudes entrantes entre varias máquinas virtuales. En este tutorial, se explican los pasos necesarios para crear una instancia de Standard Load Balancer público que equilibre la carga de las máquinas virtuales entre zonas de disponibilidad. Esto le ayudará a proteger sus aplicaciones y sus datos en el caso improbable de que se produzca una pérdida o un error en la totalidad de un centro de datos. Con la redundancia de zona, aunque se produzcan errores en una o varias zonas disponibilidad, la ruta de los datos puede mantenerse a salvo siempre que una zona de la región permanezca en buen estado. Aprenderá a:

> [!div class="checklist"]
> * Crear un equilibrador de carga estándar
> * Crear grupos de seguridad de red para definir las reglas de tráfico de entrada
> * Crear máquinas virtuales con redundancia de zona entre diferentes zonas y atribuirlas a un equilibrador de carga
> * Crear un sondeo de estado de un equilibrador de carga
> * Crear reglas de tráfico del equilibrador de carga
> * Crear un sitio de IIS básico
> * Ver un equilibrador de carga en acción

Para más información sobre cómo usar las zonas de disponibilidad con Load Balancer Estándar, consulte [Load Balancer Estándar y zonas de disponibilidad](load-balancer-standard-availability-zones.md).

Si lo prefiere, puede realizar los pasos de este artículo con la [CLI de Azure](./quickstart-load-balancer-standard-public-cli.md).

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar. 

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción de Azure

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-standard-load-balancer"></a>Crear un equilibrador de carga estándar

La versión Estándar de Load Balancer solo admite direcciones IP públicas estándar. Cuando se crea una dirección IP pública nueva al crear el equilibrador de carga, se configura automáticamente como una versión de la SKU Estándar y también tiene automáticamente redundancia de zona.

1. En la parte superior izquierda de la pantalla, haga clic en **Crear un recurso** > **Redes** > **Azure Load Balancer**.
2. En la pestaña **Datos básicos** de la página **Crear equilibrador de carga**, escriba o seleccione la siguiente información, acepte los valores predeterminados del resto de la configuración y, luego, seleccione **Revisar y crear**:

    | Configuración                 | Value                                              |
    | ---                     | ---                                                |
    | Subscription               | Seleccione su suscripción.    |    
    | Resource group         | Seleccione **Crear nuevo** y escriba *MyResourceGroupLBAZ* en el cuadro de texto.|
    | Nombre                   | *myLoadBalancer*                                   |
    | Region         | Seleccione **Oeste de Europa**.                                        |
    | Tipo          | Seleccione **Público**.                                        |
    | SKU           | Seleccione **Estándar**.                          |
    | Dirección IP pública | Seleccione **Crear nuevo**. |
    | Nombre de la dirección IP pública              | Escriba *myPublicIP* en el cuadro de texto.   |
    |Zona de disponibilidad| Seleccione **Redundancia de zona**.    |
   

## <a name="create-backend-servers"></a>Creación de servidores back-end

En esta sección, creará una red virtual y máquinas virtuales en diferentes zonas de la región. Después, instalará IIS en las máquinas virtuales para que le ayude a probar el equilibrador de carga con redundancia de zona. Por lo tanto, si se produce un error en una zona, se produce un error en el sondeo de estado de la máquina virtual en la misma zona y continúa el tráfico servido por las máquinas virtuales de las demás zonas.

## <a name="virtual-network-and-parameters"></a>Red virtual y parámetros

En los pasos de esta sección, tendrá que reemplazar los siguientes parámetros por la siguiente información:

| Parámetro                   | Value                |
|-----------------------------|----------------------|
| **\<resource-group-name>**  | myResourceGroupLBAZ (seleccione el grupo de recursos existente) |
| **\<virtual-network-name>** | myVNet          |
| **\<region-name>**          | Oeste de Europa      |
| **\<IPv4-address-space>**   | 10.0.0.0/16          |
| **\<subnet-name>**          | myBackendSubnet        |
| **\<subnet-address-range>** | 10.0.0.0/24          |

[!INCLUDE [virtual-networks-create-new](../../includes/virtual-networks-create-new.md)]

## <a name="create-a-network-security-group"></a>Crear un grupo de seguridad de red

Cree un grupo de seguridad de red para definir las conexiones entrantes a la red virtual.

1. En la parte superior izquierda de la pantalla, haga clic en **Crear un recurso**, en el cuadro de búsqueda, escriba *Grupo de seguridad de red* y, en la página del grupo de seguridad de red, haga clic en **Crear**.
2. En la página Crear grupo de seguridad de red, escriba estos valores:
    - *myNetworkSecurityGroup*: como nombre del grupo de seguridad de red.
    - *myResourceGroupLBAZ*: como nombre del grupo de recursos existente.
   
![Captura de pantalla que muestra el panel Crear grupo de seguridad de red.](./media/load-balancer-standard-public-availability-zones-portal/create-nsg.png)

### <a name="create-network-security-group-rules"></a>Creación de reglas de grupo de seguridad de red

En esta sección, va a crear reglas de grupo de seguridad de red para permitir conexiones entrantes que usen HTTP y RDP mediante Azure Portal.

1. En Azure Portal, haga clic en **Todos los recursos** en el menú de la izquierda y, a continuación, busque y haga clic en **myNetworkSecurityGroup**, que se encuentra en el grupo de recursos **myResourceGroupLBAZ**.
2. En **Configuración**, haga clic en **Reglas de seguridad de entrada** y, después, en **Agregar**.
3. Especifique estos valores para la regla de seguridad de entrada llamada *myHTTPRule* para permitir que las conexiones HTTP entrantes usen el puerto 80:
    - *Etiqueta de servicio*: en **Origen**.
    - *Internet*: en **Etiqueta de servicio de origen**
    - *80*: en **Intervalos de puerto de destino**
    - *TCP*: en **Protocolo**
    - *Permitir*: en **Acción**
    - *100* en **Prioridad**
    - *myHTTPRule*: en el nombre de la regla del equilibrador de carga
    - *Allow HTTP* (Permitir HTTP): en la descripción de la regla del equilibrador de carga
4. Haga clic en **OK**.
 
   ![Captura de pantalla que muestra el panel Agregar regla de seguridad de entrada.](./media/load-balancer-standard-public-availability-zones-portal/8-load-balancer-nsg-rules.png)
5. Repita los pasos 2 a 4 para crear otra regla llamada *myRDPRule* que permita una conexión RDP entrante con el puerto 3389 con los valores siguientes:
    - *Etiqueta de servicio*: en **Origen**.
    - *Internet*: en **Etiqueta de servicio de origen**
    - *3389*: en **Intervalos de puerto de destino**
    - *TCP*: en **Protocolo**
    - *Permitir*: en **Acción**
    - *200* en **Prioridad**
    - *myRDPRule* como nombre
    - *Permitir RDP*: como descripción

### <a name="create-virtual-machines"></a>Creación de máquinas virtuales

Cree máquinas virtuales en distintas zonas (zona 1, zona 2 y zona 3) de la región que puedan actuar como servidores back-end en el equilibrador de carga.

1. En la parte superior izquierda de la pantalla, haga clic en **Crear un recurso** > **Proceso** > **Windows Server 2016 Datacenter** y especifique estos valores para la máquina virtual:
    - *myVM1*: como el nombre de la máquina virtual.        
    - *azureuser*: como nombre del usuario administrador.    
    - *myResourceGroupLBAZ*: como **grupo de recursos**. Seleccione **Usar existente** y, después, seleccione *myResourceGroupLBAZ*.
2. Haga clic en **OK**.
3. Seleccione **DS1_V2** como tamaño de la máquina virtual y haga clic en **Seleccionar**.
4. Especifique estos valores para la configuración de la máquina virtual:
    - *zona 1*: para la zona en la que va a situar la máquina virtual.
    -  *myVNet*: asegúrese de que se selecciona como red virtual.
    - *myBackendSubnet*: asegúrese de que se selecciona como subred.
    - *myNetworkSecurityGroup*: como nombre del grupo de seguridad de red (firewall).
5. Haga clic en **Deshabilitado** para deshabilitar los diagnósticos de arranque.
6. Haga clic en **Aceptar**, revise la configuración en la página de resumen y haga clic en **Crear**.
7. Cree una segunda máquina virtual llamada *VM2* en la Zona 2 y una tercera máquina virtual en la Zona 3, con *myVnet* como la red virtual, *myBackendSubnet* como la subred y **myNetworkSecurityGroup* como el grupo de seguridad de red mediante los pasos del 1 al 6.

### <a name="install-iis-on-vms"></a>Instalación de IIS en las máquinas virtuales

1. Haga clic en **Todos los recursos** en el menú de la izquierda y, después, en la lista de recursos haga clic en **myVM1**, que se encuentra en el grupo de recursos *myResourceGroupLBAZ*.
2. En la página **Información general**, haga clic en **Conectar** a RDP en la máquina virtual.
3. Inicie sesión en la máquina virtual con el nombre de usuario *azureuser*.
4. En el escritorio del servidor, vaya a **Herramientas administrativas de Windows**>**Windows PowerShell**.
5. En la ventana de PowerShell, ejecute los comandos siguientes para instalar el servidor IIS, eliminar el archivo iisstart.htm predeterminado y agregar uno nuevo que muestre el nombre de la máquina virtual:
   ```azurepowershell-interactive
    
    # install IIS server role
    Install-WindowsFeature -name Web-Server -IncludeManagementTools
    
    # remove default htm file
     remove-item  C:\inetpub\wwwroot\iisstart.htm
    
    # Add a new htm file that displays server name
     Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from" + $env:computername)
   ```
6. Cierre la sesión de RDP con *myVM1*.
7. Repita los pasos 1 a 6 para instalar IIS y el archivo iisstart.htm actualizado en *myVM2* y *myVM3*.

## <a name="create-load-balancer-resources"></a>Creación de recursos del equilibrador de carga

En esta sección, va a configurar el equilibrador de carga para un grupo de direcciones de back-end y un sondeo de mantenimiento, y a especificar el equilibrador de carga y las reglas NAT.


### <a name="create-a-backend-address-pool"></a>Creación de un grupo de direcciones de back-end

Para distribuir el tráfico a las máquinas virtuales, un grupo de direcciones de back-end contiene las direcciones IP de las tarjetas de interfaz de red (NIC) virtual conectadas al equilibrador de carga. Cree el grupo de direcciones de back-end *myBackendPool* para incluir *VM1*, *VM2* y *VM3*.

1. Haga clic en **Todos los recursos** en el menú de la izquierda y, después, haga clic en **myLoadBalancer** en la lista de recursos.
2. En **Configuración**, haga clic en **Grupos de back-end** y luego en **Agregar**.
3. En la página **Agregar grupo back-end**, realice lo siguiente:
    - En el nombre, escriba *myBackEndPool* como nombre del grupo de servidores back-end.
    - Para **Red virtual**, en el menú desplegable, haga clic en **myVNet**
    - Para **Máquina virtual**, en el menú desplegable, haga clic en **myVM1**.
    - Para **Dirección IP**, en el menú desplegable, haga clic en la dirección IP de myVM1.
4. Haga clic en **Agregar nuevo recurso de back-end** para agregar cada máquina virtual (*myVM2* y *myVM3*) al grupo de back-end del equilibrador de carga.
5. Haga clic en **Agregar**.

    ![Incorporación al grupo de direcciones de back-end](./media/load-balancer-standard-public-availability-zones-portal/add-backend-pool.png)

3. Compruebe que la configuración del grupo de back-end del equilibrador de carga muestra las tres máquinas virtuales: **myVM1**, **myVM2** y **myVM3**.

### <a name="create-a-health-probe"></a>Creación de un sondeo de estado

Para permitir que el equilibrador de carga supervise el estado de la aplicación, utilice un sondeo de estado. El sondeo de estado agrega o quita de forma dinámica las máquinas virtuales de la rotación del equilibrador de carga en base a su respuesta a las comprobaciones de estado. Cree un sondeo de estado, *myHealthProbe*, para supervisar el estado de las máquinas virtuales.

1. Haga clic en **Todos los recursos** en el menú de la izquierda y, después, haga clic en **myLoadBalancer** en la lista de recursos.
2. En **Configuración**, haga clic en **Sondeos de estado** y luego en **Agregar**.
3. Use estos valores para crear el sondeo de estado:
    - *myHealthProbe*: como nombre del sondeo de estado.
    - **HTTP**: en tipo de protocolo.
    - *80*: en número de puerto.
    - *15*: como número de **Intervalo**, en segundos, entre los intentos de sondeo.
    - *2*: como número de **Umbral incorrecto** o errores de sondeo consecutivos que deben producirse para que una máquina virtual se considere que no funciona de manera incorrecta.
4. Haga clic en **OK**.

   ![Incorporación de un sondeo](./media/load-balancer-standard-public-availability-zones-portal/4-load-balancer-probes.png)

### <a name="create-a-load-balancer-rule"></a>Creación de una regla de equilibrador de carga

Las reglas de equilibrador de carga se utilizan para definir cómo se distribuye el tráfico a las máquinas virtuales. Se define la configuración de IP front-end para el tráfico entrante y el grupo IP de back-end para recibir el tráfico, junto con el puerto de origen y destino requeridos. Cree una regla de Load Balancer *myLoadBalancerRuleWeb* para escuchar el puerto 80 en el front-end *FrontendLoadBalancer* y enviar tráfico de red con equilibrio de carga al conjunto de direcciones back-end, *myBackEndPool*, que también usan el puerto 80. 

1. Haga clic en **Todos los recursos** en el menú de la izquierda y, después, haga clic en **myLoadBalancer** en la lista de recursos.
2. En **Configuración**, haga clic en **Reglas de equilibrio de carga** y luego en **Agregar**.
3. Use estos valores para configurar la regla de equilibrio de carga:
    - *myHTTPRule*: como nombre de la regla de equilibrio de carga.
    - **TCP**: en tipo de protocolo.
    - *80*: en número de puerto.
    - *80*: como puerto de back-end.
    - *myBackendPool*: como nombre del grupo back-end.
    - *myHealthProbe*: como nombre del sondeo de estado.
4. Haga clic en **OK**.
    
    
    ![Incorporación de una regla de equilibrio de carga](./media/load-balancer-standard-public-availability-zones-portal/load-balancing-rule.png)

## <a name="test-the-load-balancer"></a>Prueba del equilibrador de carga
1. Busque la dirección IP pública de Load Balancer en la pantalla **Información general**. Haga clic en **Todos los recursos** y, después, en **myPublicIP**.

2. Copie la dirección IP pública y péguela en la barra de direcciones del explorador. La página predeterminada del servidor web de IIS se muestra en el explorador.

      ![Servidor web de IIS](./media/tutorial-load-balancer-standard-zonal-portal/load-balancer-test.png)

Para ver cómo el equilibrador de carga reparte el tráfico entre las máquinas virtuales distribuidas por la zona, puede realizar una actualización forzada del explorador web.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando no los necesite, elimine el grupo de recursos, el equilibrador de carga y todos los recursos relacionados. Para ello, seleccione el grupo de recursos que contiene el equilibrador de carga y **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre el equilibrio de carga en las máquinas virtuales de una zona de disponibilidad específica.
> [!div class="nextstepaction"]
> [Equilibrio de carga en las máquinas virtuales de una zona de disponibilidad](tutorial-load-balancer-standard-public-zonal-portal.md)