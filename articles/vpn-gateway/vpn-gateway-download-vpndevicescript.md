---
title: 'Transfira scripts de configuração de dispositivo VPN para ligações S2S VPN: Azure Resource Manager | Microsoft Docs'
description: Este artigo explica como transferir scripts de configuração de dispositivo VPN para ligações S2S VPN com Gateways de VPN do Azure com o Azure Resource Manager.
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: ''
tags: azure-resource-manager
ms.assetid: 238cd9b3-f1ce-4341-b18e-7390935604fa
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/29/2018
ms.author: yushwang
ms.openlocfilehash: 254f5012bfbf827aebc20d90405636dcb204193c
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/03/2018
---
# <a name="download-vpn-device-configuration-scripts-for-s2s-vpn-connections"></a>Transfira scripts de configuração de dispositivo VPN para ligações S2S VPN

Este artigo explica como transferir scripts de configuração de dispositivo VPN para ligações S2S VPN com Gateways de VPN do Azure com o Azure Resource Manager. O diagrama seguinte mostra o fluxo de trabalho de alto nível.

![download-script](./media/vpn-gateway-download-vpndevicescript/downloaddevicescript.png)

Os seguintes dispositivos de ter scripts disponíveis:

[!INCLUDE [scripts](../../includes/vpn-gateway-device-configuration-scripts.md)]

## <a name="about"></a>Sobre scripts de configuração de dispositivo VPN

Uma ligação de VPN em vários locais é composta por um gateway de VPN do Azure, um dispositivo VPN no local e um túnel IPsec S2S VPN ligar os dois. O fluxo de trabalho normal inclui os seguintes passos:

1. Criar e configurar um gateway de VPN do Azure (gateway de rede virtual)
2. Criar e configurar um gateway de rede local do Azure que representa a sua rede no local e o dispositivo VPN
3. Criar e configurar uma ligação de VPN do Azure entre o gateway de VPN do Azure e o gateway de rede local
4. Configurar o dispositivo VPN no local, representado pelo gateway de rede local para estabelecer o túnel S2S VPN real com o gateway de VPN do Azure

Pode concluir os passos 1 a 3 com o Azure [portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md), [PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md), ou [CLI](vpn-gateway-howto-site-to-site-resource-manager-cli.md). O último passo envolve a configuração de dispositivos VPN no local fora do Azure. Esta funcionalidade permite-lhe transferir um script de configuração para o dispositivo VPN com os valores correspondentes do seu gateway de VPN do Azure, rede virtual e prefixos de endereço de rede no local e propriedades de ligação de VPN, etc. já preenchidos. Pode utilizar o script como um ponto de partida ou aplicar o script diretamente para os dispositivos VPN no local através da consola de configuração.

> [!IMPORTANT]
> * A sintaxe para cada script de configuração do dispositivo VPN é diferente e descontos elevados depende de versões de firmware e modelos. Preste especial atenção às suas informações de versão e o modelo de dispositivo contra os modelos disponíveis.
> * Alguns valores de parâmetro tem de ser exclusivos no dispositivo e não não possível determinar sem aceder ao dispositivo. Os scripts de configuração gerado pelo Azure previamente preencher estes valores, mas certifique-se de que os valores são válidos no seu dispositivo. Para obter exemplos:
>    * Números de interface
>    * Lista de números de controlo de acesso
>    * Os nomes das políticas ou números, etc.
> * Procure a palavra-chave "**substituir**", incorporado no script para encontrar os parâmetros, terá de verificar antes de aplicar o script.
> * Alguns modelos incluem um "**limpeza**" secção pode aplicar a remover as configurações. As secções de limpeza são comentadas por predefinição.

## <a name="download-the-configuration-script-from-azure-portal"></a>Transferir o script de configuração a partir do portal do Azure

Crie um gateway de VPN do Azure, o gateway de rede local e um recurso de ligação ligar os dois. A página seguinte orienta-o ao longo dos passos:

* [Criar uma ligação Site a Site no portal do Azure](vpn-gateway-howto-site-to-site-resource-manager-portal.md)

Assim que o recurso de ligação for criado, siga as instruções abaixo para transferir os scripts de configuração do dispositivo VPN:

1. Num browser, navegue para o [portal do Azure](http://portal.azure.com) e, se necessário, inicie sessão com a sua conta do Azure
2. Ir para o recurso de ligação que criou. Pode encontrar a lista de todos os recursos de ligação ao clicar em "Todos os serviços", em seguida, "Redes" e "Ligações."

    ![lista de ligações](./media/vpn-gateway-download-vpndevicescript/connectionlist.png)

3. Clique na ligação que pretende configurar.

    ![Descrição geral da ligação](./media/vpn-gateway-download-vpndevicescript/connectionoverview.png)

4. Clique na ligação "Transferir a configuração de" conforme realçado vermelho na página de descrição geral da ligação; Esta ação abre a página "Configuração de transferência".

    ![download-script-1](./media/vpn-gateway-download-vpndevicescript/downloadscript-1.png)

5. Selecione a versão de firmware e família de modelo para o dispositivo VPN, em seguida, clique no botão "Transferir a configuração".

    ![download66-script-2](./media/vpn-gateway-download-vpndevicescript/downloadscript-2.PNG)

6. Lhe for pedido para guardar o script transferido (um ficheiro de texto) a partir do seu browser.
7. Assim que tiver transferido o script de configuração, abra-o com um editor de texto e procurar a palavra-chave "Substituir" para identificar e examinar os parâmetros que poderão ter de ser substituído.

    ![edit-script](./media/vpn-gateway-download-vpndevicescript/editscript.png)

## <a name="download-the-configuration-script-using-azure-powershell"></a>Transferir o script de configuração com o Azure PowerShell

Também pode transferir o script de configuração com o Azure PowerShell, conforme mostrado no exemplo seguinte:

```azurepowershell-interactive
$RG          = "TestRG1"
$GWName      = "VNet1GW"
$Connection  = "VNet1toSite1"

# List the available VPN device models and versions
Get-AzureRmVirtualNetworkGatewaySupportedVpnDevice -Name $GWName -ResourceGroupName $RG

# Download the configuration script for the connection
Get-AzureRmVirtualNetworkGatewayConnectionVpnDeviceConfigScript -Name $Connection -ResourceGroupName $RG -DeviceVendor Juniper -DeviceFamily Juniper_SRX_GA -FirmwareVersion Juniper_SRX_12.x_GA
```

## <a name="apply-the-configuration-script-to-your-vpn-device"></a>Aplicar o script de configuração para o seu dispositivo VPN

Depois de ter transferido e validar o script de configuração, o passo seguinte consiste em aplicar o script para o seu dispositivo VPN. O procedimento real varia com base no seu dispositivo VPN faz com que e modelos. Para os dispositivos VPN, consulte as páginas da instrução ou como a operação.

## <a name="next-steps"></a>Passos Seguintes

Continuar a configurar o [ligação Site a Site](vpn-gateway-howto-site-to-site-resource-manager-portal.md).