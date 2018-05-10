---
title: Criar um gateway de aplicação do Azure redundante de zona
description: Saiba como criar um gateway de aplicação redundante de zona que não necessita de um gateway de seperarte cada zona.
services: application-gateway
author: amsriva
manager: jpconnock
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 5/8/2018
ms.author: victorh
ms.openlocfilehash: 03bc58db813534fdd17c9567f6425ab7357ed901
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
---
# <a name="create-a-zone-redundant-azure-application-gateway---private-preview"></a>Criar um gateway de aplicação do Azure redundante de zona - pré-visualização privada

A plataforma de redundante de zona do application gateway for um SKU novo que proporciona inúmeros melhoramentos através de existente application gateway SKU, incluindo:
- **Resiliência de zona** - uma implementação do gateway de aplicação agora pode abranger várias zonas de disponibilidade, eliminando a necessidade para aprovisionar e instâncias de gateway de aplicação separado de pin no cada zona com um Gestor de tráfego. Pode escolher uma zona única ou várias zonas em instâncias de gateway de aplicação estão implementadas, deste modo, resiliência de falha de zona, garantindo que. O conjunto de back-end para as aplicações pode ser da mesma forma distribuído através de zonas de disponibilidade.
- **Melhoramentos de desempenho**
- **VIP estático** -o gateway de aplicação VIP suporta agora o tipo de VIP estático exclusivamente. Isto garante que o VIP associado com o gateway de aplicação não se altera após um reinício.
- **Integração do Cofre de chaves para certificados SSL de cliente**
- **Tempo de implementação e atualização mais rápido**

> [!NOTE]
> O gateway de aplicação redundante de zona está atualmente em pré-visualização privada. Esta pré-visualização é fornecida sem um contrato de nível de serviço e não é recomendada para cargas de trabalho de produção. Algumas funcionalidades não podem ser suportadas ou poderão ter restrita capacidades. Veja os [Termos Suplementares de Utilização para Pré-visualizações do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) para obter mais informações.

## <a name="supported-regions"></a>Regiões suportadas

Gateways de aplicação redundante de zona são atualmente suportados na região EUA Leste 2. Logo que serão adicionadas mais regiões.

## <a name="topologies"></a>Topologias

Com a versão atual, já não precisam de criar a zona afixado gateways de aplicação para obter zonal redundância. A mesma implementação do gateway de aplicação agora pode abranger várias zonas.

Instâncias, pelo menos, três são necessárias para se certificar de que estes são distribuídos por todas as zonas de três. Gateway de aplicação distribui instâncias zonas são adicionadas mais instâncias.

Topologias de redundante zona anterior comparados como o diagrama a seguir:

![Topologia redundante antiga](media/create-zone-redundant/old-redundant.png)

Nova zona redundante topologia de aspeto neste diagrama:

![Novo topologia redundante de zona](media/create-zone-redundant/new-redundant.png)

## <a name="deployment"></a>Implementação

### <a name="prerequisites"></a>Pré-requisitos

Atualmente a capacidade de redundante de zona só está disponível na pré-visualização privada. Tem de correio eletrónico appgwxzone@microsoft.com na lista de permissões. Depois de receber uma confirmação, pode avançar para os passos seguintes. Inclua as seguintes informações no seu correio eletrónico para adicionar à lista branca:

- ID da subscrição
- Nome da região
- Número aproximado de gateways de aplicação necessário

### <a name="resource-manager-template-deployment"></a>Implementação de modelo do Resource Manager

1. Certifique-se a subscrição que utilizar na lista de permissões conforme mencionada anteriormente.
2. Depois de receber uma confirmação, inicie sessão para a conta do Azure e selecione a subscrição adequada, se existir mais do que uma subscrição. Certifique-se de que seleciona a subscrição que esteve na lista de permissões.

   ```PowerShell
   Login-AzureRmAccount

   Select-AzureRmSubscription -Subscription "<whitelisted subscription name>”
   ```
3. Criar um novo grupo de recursos

   ``` PowerShell
   New-AzureRmResourceGroup -Name <resource group name> -Location "East US 2"
   ```
4. Transferir os modelos de [GitHub](https://github.com/amitsriva/CrossZonePreview) e tenha em atenção a pasta onde guardá-las.
5. Crie uma nova implementação no grupo de recursos que criou. Modificar o ficheiro de modelo e os parâmetros corretamente antes de implementar. 

   O diagrama seguinte mostra onde pode obter o ID do inquilino no portal do Azure:

   ![ID do inquilino no portal](media/create-zone-redundant/tenant-id.png)

O modelo cria os seguintes recursos:

- **Identidade do utilizador atribuída** -isto é utilizado para ativar as instâncias de gateway de aplicação para aceder ao Cofre de chaves e obter os certificados armazenados pelo utilizador.
- **KeyVault** – o Cofre de chaves onde está armazenado o certificado do utilizador. Isto pode ser um cofre de chaves já existente.
- **Segredo** – a chave privada que é armazenada no Cofre de chaves.
- **Política de acesso** – uma política de acesso aplicada no Cofre de chaves que concede permissão com a identidade do utilizador atribuída para que as implementações de gateway de aplicação podem obter certificados de utilizador.
- **Endereço IP público** – um reservado de endereços IP que é utilizado para aceder ao gateway de aplicação. Este endereço IP nunca muda para o ciclo de vida do gateway de aplicação.
- **Grupos de segurança de rede** – um NSG criada automaticamente na sub-rede de gateway da aplicação que abre o tráfego de porta de escuta configurado. Isto explicitamente é criado e gerido a SKU de novo em comparação comparada o SKU anterior onde este NSG foi implícita.
- **Rede virtual** – a vnet onde o gateway de aplicação e as aplicações são implementadas.
- **Gateway de aplicação** – cria um gateway de aplicação com instâncias as zonas necessárias. Por predefinição, estão selecionadas todas as zonas (1,2,3). O nome do SKU é alterado para *Standard_v2*. Este nome SKU está sujeita a alterações. Atualmente, a configuração de dimensionamento automático tem de min e max definido como o número necessário de instâncias. Depois de dimensionamento automático está ativado, este pode ser ajustado.

```PowerShell
New-AzureRmResourceGroupDeployment -Name Deployment1 -ResourceGroupName AmitVMSSLinuxTest9 -TemplateFile <complete path to template.json> -TemplateParameterFile <complete path to parameters.json>
```
### <a name="powershell-deployment"></a>Implementação de PowerShell

1. Certifique-se de que a subscrição utilizada na lista de permissões conforme mencionada anteriormente nos pré-requisitos.
2. Transfira e instale o MSI PowerShell privada do [GitHub](https://github.com/amitsriva/CrossZonePreview/blob/master/Azure-Cmdlets-5.7.0.19009-x64.msi).
3. Transfira o ficheiro zip do PowerShell privado da localização mencionada no e-mail de confirmação do registo de pré-visualização. Deszipe o ficheiro para a unidade e tome nota da localização.
4. Assim que a pré-visualização estiver ativada, carregar os módulos de pré-visualização primeiro antes de iniciar sessão sua conta:

   ```PowerShell
   $azurePSPath = "<complete path to Azure-PowerShell folder>"
   import-module "$azurePSPath\AzureRM.Profile\AzureRM.Profile.psd1"
   import-module "$azurePSPath\Azure.Storage\Azure.Storage.psd1"
   import-module "$azurePSPath\AzureRM.Resources\AzureRM.Resources.psd1"
   import-module "$azurePSPath\AzureRM.Network\AzureRM.Network.psd1"
   import-module "$azurePSPath\AzureRM.KeyVault\AzureRM.KeyVault.psd1"
   ```

4. Inicie sessão para a conta do Azure e selecione a subscrição pretendida, se existir mais do que uma subscrição. Certifique-se de que seleciona a subscrição adequada que estava na lista de permissões.
5. Execute os seguintes comandos para estabelecer constantes comuns que incluem os nomes para a maior parte das entidades a ser criadas. 

   Modificar as entradas conforme necessário para a sua preferência de nomenclatura.

   ```PowerShell
   $location = "eastus2"
   $version = "300"

   $rgname = "RG_A_$version"
   $appgwName = "AGW_A_$version"
   $vaultName = "KVA$version"
   $userAssignedIdentityName = "UI_A_$version"
   $certificateName = "KVCA$version"
   $nsgName = "NSG_A_$version"
   $vnetName = "VN_A_$version"
   $gwSubnetName = "SN_A_$version"
   $gipconfigname = "GC_A_$version"
   $publicIpName = "PIP_A_$version"
   $fipconfig01Name = "FC_A_$version"
   $poolName = "BP_A_$version"
   $frontendPort01Name = "FP1_A_$version"
   $frontendPort02Name = "FP2_A_$version"
   $poolSetting01Name = "BS_A_$version"
   $listener01Name = "HL1_A_$version"
   $listener02Name = "HL2_A_$version"
   $rule01Name = "RR1_A_$version"
   $rule02Name = "RR2_A_$version"
   $AddressPrefix = "111.111.222.0" 
   ```
6. Crie o grupo de recursos:

   ```PowerShell
   $resourceGroup = New-AzureRmResourceGroup -Name $rgname -Location $location -Force
   ```
7. Crie o utilizador atribuído identidade utilizada para dar acesso ao gateway de aplicação para obter certificados a partir do Cofre de chaves.

   ```PowerShell
   $userAssignedIdentity = New-AzureRmResource -ResourceGroupName $rgname -Location $location -ResourceName $userAssignedIdentityName -ResourceType Microsoft.ManagedIdentity/userAssignedIdentities -Force
   ```
8. Crie o Cofre de chave utilizada para armazenar os certificados:

   ```PowerShell
   $keyVault = New-AzureRmKeyVault -VaultName $vaultName -ResourceGroupName $rgname -Location $location -EnableSoftDelete
   ```
9. Carregue o certificado para o Cofre de chaves como um segredo:

   ```PowerShell
   $securepfxpwd = ConvertTo-SecureString -String "<password>" -AsPlainText -Force

   $cert = Import-AzureKeyVaultCertificate -VaultName $vaultName -Name         $certificateName -FilePath ‘<path to pfx file>'  -Password $securepfxpwd
   ```
10. Atribua a política de acesso para o Cofre de chaves utilizando a identidade do utilizador atribuído. Isto permite que as instâncias de gateway de aplicação para o Cofre de chaves de acesso secreta:

   ```PowerShell
   Set-AzureRmKeyVaultAccessPolicy -VaultName $vaultName -ResourceGroupName $rgname -PermissionsToSecrets get -ObjectId $userAssignedIdentity.Properties.principalId
   ```
11. Crie o grupo de segurança de rede (NSG) para permitir o acesso para a sub-rede do gateway de aplicação nas portas onde são criados os serviços de escuta de novo.

    Por exemplo, para HTTP/HTTPS predefinição portas NSG permita acesso de entrada para 80, 443 e 65200-65535 para operações de gestão.

   ```PowerShell
   $srule01 = New-AzureRmNetworkSecurityRuleConfig -Name "listeners" -Direction Inbound -SourceAddressPrefix * -SourcePortRange * -Protocol * -DestinationAddressPrefix * -DestinationPortRange 22,80,443 -Access Allow -Priority 100

   $srule02 = New-AzureRmNetworkSecurityRuleConfig -Name "managementPorts" -Direction Inbound -SourceAddressPrefix * -SourcePortRange * -Protocol * -DestinationAddressPrefix * -DestinationPortRange "65200-65535" -Access Allow -Priority 101

   $nsg = New-AzureRmNetworkSecurityGroup -Name $nsgName -ResourceGroupName $rgname -Location $location -SecurityRules $srule01,$srule02 -Force
   ```

12. Crie VNet e sub-redes:

   ```PowerShell
   $gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 
   $gwSubnetName -AddressPrefix "$AddressPrefix/24" -NetworkSecurityGroup $nsg

   $vnet = New-AzureRmvirtualNetwork -Name $vnetName -ResourceGroupName $rgname -Location $location -AddressPrefix "$AddressPrefix/24" -Subnet $gwSubnet -Force
   ```
13. Crie um endereço IP público reservado/estáticos do tipo:

   ```PowerShell
   $publicip = New-AzureRmPublicIpAddress -ResourceGroupName $rgname -name $publicIpName -location $location -AllocationMethod Static -Sku Standard -Force
   ```

14. Crie o gateway de aplicação:

   ```PowerShell
   $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name $gipconfigname -Subnet $gwSubnet

   $fipconfig01 = New-AzureRmApplicationGatewayFrontendIPConfig -Name $fipconfig01Name -PublicIPAddress $publicip

   $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name $poolName -BackendIPAddresses testbackend1.westus.cloudapp.azure.com, testbackend2.westus.cloudapp.azure.com

   $fp01 = New-AzureRmApplicationGatewayFrontendPort -Name $frontendPort01Name -Port 443

   $fp02 = New-AzureRmApplicationGatewayFrontendPort -Name $frontendPort02Name -Port 80

   $sslCert01 = New-AzureRmApplicationGatewaySslCertificate -Name "SSLCert" -KeyVaultSecretId $secret.Id

   $listener01 = New-AzureRmApplicationGatewayHttpListener -Name $listener01Name -Protocol Https -FrontendIPConfiguration
 $fipconfig01 -FrontendPort $fp01 -SslCertificate $sslCert01

   $listener02 = New-AzureRmApplicationGatewayHttpListener -Name $listener02Name -Protocol Http -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp02

   $poolSetting01 = New-AzureRmApplicationGatewayBackendHttpSettings -Name $poolSetting01Name -Port 80 -Protocol Http -CookieBasedAffinity Disabled

   $rule01 = New-AzureRmApplicationGatewayRequestRoutingRule -Name $rule01Name -RuleType basic -BackendHttpSettings $poolSetting01 -HttpListener $listener01 -BackendAddressPool $pool

   $rule02 = New-AzureRmApplicationGatewayRequestRoutingRule -Name $rule02Name -RuleType basic -BackendHttpSettings $poolSetting01 -HttpListener $listener02 -BackendAddressPool $pool

   $sku = New-AzureRmApplicationGatewaySku -Name Standard_v2 -Tier Standard_v2 -Capacity 2

   $listeners = @($listener02)

   $fps = @($fp01, $fp02)

   $fipconfigs = @($fipconfig01)

   $sslCerts = @($sslCert01)

   $rules = @($rule01, $rule02)

   $listeners = @($listener01, $listener02)

   $appgw = New-AzureRmApplicationGateway -Name $appgwName -ResourceGroupName $rgname -Location $location -UserAssignedIdentityId $userAssignedIdentity.ResourceId -Probes $probeHttps -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting01 -GatewayIpConfigurations $gipconfig -FrontendIpConfigurations $fipconfigs -FrontendPorts $fps -HttpListeners $listeners -RequestRoutingRules $rules -Sku $sku -SslPolicy $sslPolicy -sslCertificates $sslCerts -Force
   ```

15. Obter o endereço IP público do gateway de aplicação criado:

   ```PowerShell
   $pip = Get-AzureRmPublicIpAddress -Name $publicIpName -ResourceGroupName $rgname $pip.IpAddress
   ```

## <a name="frequently-asked-questions"></a>Perguntas mais frequentes

-  Será posso faturado por gateway de aplicação em pré-visualização?

   Durante a pré-visualização, há sem qualquer encargo. Será faturado por recursos que não seja o gateway de aplicação, tais como o Cofre de chaves, máquinas virtuais etc.
- Que regiões está disponível na pré-visualização?

   A pré-visualização está atualmente disponível na região EUA Leste 2. Logo que serão adicionadas mais regiões.
- O portal é suportado na pré-visualização?

   Não, o suporte está limitado a um módulo do PowerShell privado e modelo do Resource Manager durante a pré-visualização privada.

- Carga de trabalho de produção é suportada durante a pré-visualização privada?

   Não, não há nenhum SLA ou suporte durante a pré-visualização privada. Não é recomendado colocar cargas de trabalho de produção durante pré-visualizações. O suporte está limitado a interação direta com o grupo do produto a utilizar o alias de correio eletrónico para pré-visualização.

- Como posso comunicar problemas?

   A pré-visualização privada pode conter erros e pode ter implementações de código frequentes. Utilizar o alias de suporte appgwxzone@microsoft.com para relatórios de problemas e obter assistência.

## <a name="known-issues-and-limitations"></a>Problemas e limitações conhecidos


|Problema  |Detalhes  |
|---------|---------|---------|
|Faturação     |Não existem atualmente faturação|
|Registos de diagnóstico (não as métricas de)     |Registos de pedido/resposta e de desempenho não são apresentados atualmente|
|CLI/portal/SDK     |Não existe suporte para o portal, o CLI ou o SDK. O portal não deve ser utilizado para emitir as atualizações para pré-visualizar os gateways.|
|Falha de atualização através do modelo ocasionalmente     |Isto acontece devido a condição provável de antecipação com a política de acesso de KeyVault|Assim que o Cofre de chaves e a identidade do utilizador atribuída for criado, pode ser removida do modelo e apenas as referências ao segredo e identidade são necessárias no modelo.|
|Dimensionamento automático     |Não existe suporte para o dimensionamento automático atualmente|
|WAF     |Não é atualmente suportado WAF|
|Fornecido pelo utilizador certificados e VIPs dinâmica     |Estas não são suportadas no modelo de novo. Utilize o Cofre de chaves para armazenar certificados e VIPs estáticos.|
|Mesma sub-rede para antigo e a versão de pré-visualização do gateway de aplicação     |Uma sub-rede com um gateway de aplicação existente (modelo antigo) não pode ser utilizada para a versão de pré-visualização privada.|
|HTTP/2, FIPS modo, WebSocket, Web Apps do Azure como back-end     |Atualmente não suportado |


## <a name="support-and-feedback"></a>Suporte e comentários

Para suporte e comentários, contacte a appgwxzone@microsoft.com. O grupo de produtos do gateway de aplicação é satisfeito ouvir os seus comentários para melhoramentos e fornecer orientações sobre quando necessário.

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre outras funcionalidades do gateway de aplicação:

- [O que é o Gateway de aplicação do Azure?](overview.md)