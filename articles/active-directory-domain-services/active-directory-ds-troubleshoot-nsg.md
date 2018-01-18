---
title: "Serviços do Azure Active Directory domínio: Configuração do grupo de segurança de rede de resolução de problemas | Microsoft Docs"
description: "Resolver problemas de configuração de NSG para serviços de domínio do Azure AD"
services: active-directory-ds
documentationcenter: 
author: eringreenlee
manager: 
editor: 
ms.assetid: 95f970a7-5867-4108-a87e-471fa0910b8c
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2018
ms.author: ergreenl
ms.openlocfilehash: 447f9119ea379e278be77d8699c7dcb751ea3085
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/18/2018
---
# <a name="azure-ad-domain-services---troubleshooting-network-security-group-configuration"></a>Serviços de domínio do Azure AD - configuração do grupo de segurança de rede de resolução de problemas



## <a name="aadds104-network-error"></a>AADDS104: Erro de rede

**Mensagem de alerta:** *Microsoft é não é possível aceder aos controladores de domínio para este domínio gerido. Isto pode acontecer se um grupo de segurança de rede (NSG) configurado na sua rede virtual bloqueia o acesso a domínio gerido. Outra razão possível é se houver uma rota definida pelo utilizador que tráfego de entrada de blocos da internet.*

A causa mais comum de erros de rede para serviços de domínio do Azure AD pode ser atribuída para as configurações de NSG incorretas. Para garantir que a Microsoft pode service e manter geridos domínio, tem de utilizar um grupo de segurança de rede (NSG) para permitir o acesso ao [portas específicas](active-directory-ds-networking.md#ports-required-for-azure-ad-domain-services) dentro da sub-rede. Se estas portas são bloqueadas, Microsoft não é possível aceder os recursos que necessita de, que se poderá dever detrimental ao seu serviço. Ao criar o NSG, mantém estas portas abertas para garantir que nenhum interrupção no serviço.

## <a name="sample-nsg"></a>Exemplo NSG
A tabela seguinte ilustra um NSG manteria seu domínio gerido segura ao permitir que a Microsoft para monitorizar, gerir e atualizar as informações de exemplo.

![exemplo NSG](.\media\active-directory-domain-services-alerts\default-nsg.png)

>[!NOTE]
> Serviços de domínio do Azure AD requer acesso de saída sem restrições. É recomendado para criar qualquer regra outboud adicionais para os seus NSGs.

## <a name="adding-a-rule-to-a-network-security-group-using-the-azure-portal"></a>Adicionar uma regra a um grupo de segurança de rede através do portal do Azure
Se não pretender utilizar o PowerShell, pode adicionar manualmente regras único para os NSGs no portal do Azure. Siga estes passos para criar regras no seu grupo de segurança de rede.

1. Navegue para o [grupos de segurança de rede](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.Network%2FNetworkSecurityGroups) página no portal do Azure
2. Escolha o NSG associado o domínio da tabela.
3. Em definições no painel de navegação esquerdo, clique em **regras de segurança de entrada** ou **regras de segurança de saída**.
4. Criar a regra clicando **adicionar** e preencher as informações. Clique em **OK**.
5. Certifique-se de que a regra foi criada através da localização-lo na tabela de regras.


## <a name="create-a-default-nsg-using-powershell"></a>Criar uma predefinição NSG através do PowerShell

Anterior se os passos que pode utilizar para criar um novo NSG através do PowerShell que mantém todas as portas necessitam para executar o domínio do Azure AD serviços abrir ao negar qualquer outro acesso indesejável.


Esta resolução requer a instalação e execução [Azure AD Powershell](https://docs.microsoft.com/en-us/powershell/azure/active-directory/install-adv2?toc=%2Fazure%2Factive-directory-domain-services%2Ftoc.json&view=azureadps-2.0).

1. Ligar ao seu diretório do Azure AD.

  ```PowerShell
  # Connect to your Azure AD directory.
  Connect-AzureAD
  ```
2. Inicie sessão sua subscrição do Azure.

  ```PowerShell
  # Log in to your Azure subscription.
  Login-AzureRmAccount
  ```

3. Crie um NSG com três regras. O script seguinte define três regras para o NSG que permite o acesso para as portas necessárias para executar os serviços de domínio do Azure AD. Em seguida, o script cria um novo NSG contém essas regras. Está bem-vindo ao adicionar regras adicionais como julgar utilizando o mesmo formato.

  ```PowerShell
  # Create the rules needed
  $rule1 = New-AzureRmNetworkSecurityRuleConfig -Name https-rule -Description "Allow HTTP" `
  -Access Allow -Protocol Tcp -Direction Inbound -Priority 101 `
  -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
  -DestinationPortRange 443
  $rule2 = New-AzureRmNetworkSecurityRuleConfig -Name manage-3389 -Description "Manage domain through port 3389" `
  -Access Allow -Protocol Tcp -Direction Inbound -Priority 102 `
  -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
  -DestinationPortRange 3389
  $rule3 = New-AzureRmNetworkSecurityRuleConfig -Name manage-5986 -Description "Manage domain through port 5986" `
  -Access Allow -Protocol Tcp -Direction Inbound -Priority 103 `
  -SourceAddressPrefix $serviceIPs -SourcePortRange * -DestinationAddressPrefix * `
  -DestinationPortRange 5986
  # Create the NSG with the 3 rules above
  $nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $resourceGroup -Location westus `
  -Name "AADDS-NSG" -SecurityRules $rule1,$rule2,$rule3
  ```

4. Por último, o script associa o NSG a vnet e sub-rede à escolha.

  ```PowerShell
  # Find vnet and subnet
  $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $resourceGroup -Name $vnetName
  $subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name $subnetName
  # Set the nsg to the subnet and save the changes
  $subnet.NetworkSecurityGroup = $nsg
  Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```

### <a name="full-script"></a>Script completo

```PowerShell
#Change the following values to match your deployment
$resourceGroup = "ResourceGroupName"
$vnetName = "exampleVnet"
$subnetName = "exampleSubnet"

$serviceIPs = "52.180.183.8, 23.101.0.70, 52.225.184.198, 52.179.126.223, 13.74.249.156, 52.187.117.83, 52.161.13.95, 104.40.156.18, 104.40.87.209, 52.180.179.108, 52.175.18.134, 52.138.68.41, 104.41.159.212, 52.169.218.0, 52.187.120.237, 52.161.110.169, 52.174.189.149, 13.64.151.161"

# Create the rules needed
$rule1 = New-AzureRmNetworkSecurityRuleConfig -Name https-rule -Description "Allow HTTP" `
-Access Allow -Protocol Tcp -Direction Inbound -Priority 101 `
-SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 443

$rule2 = New-AzureRmNetworkSecurityRuleConfig -Name manage-3389 -Description "Manage domain through port 3389" `
-Access Allow -Protocol Tcp -Direction Inbound -Priority 102 `
-SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 3389

$rule3 = New-AzureRmNetworkSecurityRuleConfig -Name manage-5986 -Description "Manage domain through port 5986" `
-Access Allow -Protocol Tcp -Direction Inbound -Priority 103 `
-SourceAddressPrefix $serviceIPs -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 3389


# Connect to your Azure AD directory.
Connect-AzureAD

# Log in to your Azure subscription.
Login-AzureRmAccount

# Create the NSG with the 3 rules above
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $resourceGroup -Location westus `
-Name "NSG-Default" -SecurityRules $rule1,$rule2,$rule3

# Find vnet and subnet
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $resourceGroup -Name $vnetName
$subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name $subnetName

# Set the nsg to the subnet and save the changes
$subnet.NetworkSecurityGroup = $nsg
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

> [!NOTE]
>Esta predefinição NSG não bloquear acesso para a porta utilizada para proteger o LDAP. Para saber como criar uma regra para esta porta, visite [neste artigo](active-directory-ds-troubleshoot-ldaps.md).
>

## <a name="contact-us"></a>Contacte-nos
Contacte a equipa de produto do Azure Active Directory Domain Services para [partilhar comentários ou para suporte](active-directory-ds-contact-us.md).
