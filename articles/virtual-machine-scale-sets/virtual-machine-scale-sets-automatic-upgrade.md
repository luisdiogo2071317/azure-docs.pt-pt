---
title: Conjuntos de dimensionamento de atualizações automáticas de SO com a máquina virtual do Azure | Documentos da Microsoft
description: Saiba como atualizar automaticamente o sistema operacional nas instâncias VM num conjunto de dimensionamento
services: virtual-machine-scale-sets
documentationcenter: ''
author: yeki
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/03/2018
ms.author: yeki
ms.openlocfilehash: 6b20ef98e008d9c5d984ba29eed894b1c5ec8c09
ms.sourcegitcommit: a5eb246d79a462519775a9705ebf562f0444e4ec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/26/2018
ms.locfileid: "39263253"
---
# <a name="azure-virtual-machine-scale-set-automatic-os-upgrades"></a>Atualizações automáticas de SO do conjunto de dimensionamento de máquina virtual do Azure

Atualização automática de imagem do SO é uma funcionalidade de pré-visualização para conjuntos de dimensionamento de máquina virtual do Azure que atualiza automaticamente todas as VMs para a imagem de SO mais recente.

Atualização automática de SO tem as seguintes características:

- Depois de configurada, a imagem de SO mais recente, publicada por editores de imagem é aplicada automaticamente para o conjunto sem intervenção do utilizador de dimensionamento.
- Atualiza lotes de instâncias de uma maneira sem interrupção sempre que uma nova imagem de plataforma é publicada pelo editor.
- Integra-se com a sonda de estado de funcionamento da aplicação (opcional, mas altamente recomendado de segurança).
- Funciona para todos os tamanhos VM.
- Imagens da plataforma funciona para Windows e Linux.
- Pode desativar as atualizações automáticas em qualquer altura (atualizações de SO pode ser iniciadas manualmente também).
- O disco do SO de uma VM é substituído pelo novo disco de SO criado com a versão mais recente da imagem. Extensões configuradas e scripts de dados personalizados são executados ao mesmo tempo são retidos discos de dados persistentes.


## <a name="preview-notes"></a>Notas de pré-visualização 
Enquanto está em pré-visualização, as seguintes limitações e restrições aplicam-se:

- Atualizações de SO automática só suportam [quatro SKUs de SO](#supported-os-images). Não existe SLA ou garantias. Recomendamos que não usar as atualizações automáticas em cargas de trabalho críticas de produção durante a pré-visualização.
- A encriptação de disco do Azure é **não** atualmente suportada com a máquina virtual dimensionamento conjunto atualização automática de SO.


## <a name="register-to-use-automatic-os-upgrade"></a>Registre-se para utilizar a atualização automática do SO
Para utilizar a funcionalidade de atualização do sistema operacional automatizada, registe o fornecedor de pré-visualização com o Azure Powershell ou a CLI 2.0 do Azure.

### <a name="powershell"></a>PowerShell

1. Registe-se com [Register-AzureRmProviderFeature](/powershell/module/azurerm.resources/register-azurermproviderfeature):

     ```powershell
     Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Compute -FeatureName AutoOSUpgradePreview
     ```

2. Demora cerca de 10 minutos para o estado de registo para o relatório como *registado*. Pode verificar o estado do registo atual com [Get-AzureRmProviderFeature](/powershell/module/AzureRM.Resources/Get-AzureRmProviderFeature). 

3. Depois de registado, confirme que o *Microsoft. Compute* fornecedor está registado. O exemplo seguinte utiliza o Azure Powershell com [Register-AzureRmResourceProvider](/powershell/module/AzureRM.Resources/Register-AzureRmResourceProvider):

     ```powershell
     Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute
     ```


### <a name="cli-20"></a>CLI 2.0

1. Registe-se com [Registre-se de funcionalidade de az](/cli/azure/feature#az-feature-register):

     ```azurecli
     az feature register --name AutoOSUpgradePreview --namespace Microsoft.Compute
     ```

2. Demora cerca de 10 minutos para o estado de registo para o relatório como *registado*. Pode verificar o estado do registo atual com [show de funcionalidade de az](/cli/azure/feature#az-feature-show). 
 
3. Depois de registado, certifique-se de que o *Microsoft. Compute* fornecedor está registado. O exemplo seguinte utiliza a CLI do Azure (2.0.20 ou posterior) com [Registre-se fornecedor de az](/cli/azure/provider#az-provider-register):

     ```azurecli
     az provider register --namespace Microsoft.Compute
     ```

> [!NOTE]
> Clusters do Service Fabric tem seus próprios noção de estado de funcionamento do aplicativo, mas os conjuntos de dimensionamento sem o Service Fabric utilizar a sonda de estado de funcionamento do Balanceador de carga para monitorizar o estado de funcionamento do aplicativo. 
>
> ### <a name="azure-powershell"></a>Azure PowerShell
>
> 1. Registar a funcionalidade do fornecedor, para as sondas de estado de funcionamento com [Register-AzureRmProviderFeature](/powershell/module/azurerm.resources/register-azurermproviderfeature):
>
>      ```powershell
>      Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Network -FeatureName AllowVmssHealthProbe
>      ```
>
> 2. Novamente, demora cerca de 10 minutos para o estado de registo para o relatório como *registado*. Pode verificar o estado do registo atual com [Get-AzureRmProviderFeature](/powershell/module/AzureRM.Resources/Get-AzureRmProviderFeature)
>
> 3. Uma vez registado Certifique-se de que o *Network* fornecedor é registrado usando [Register-AzureRmResourceProvider](/powershell/module/AzureRM.Resources/Register-AzureRmResourceProvider):
>
>      ```powershell
>      Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
>      ```
>
>
> ### <a name="cli-20"></a>CLI 2.0
>
> 1. Registar a funcionalidade do fornecedor, para as sondas de estado de funcionamento com [Registre-se de funcionalidade de az](/cli/azure/feature#az-feature-register):
>
>      ```azurecli
>      az feature register --name AllowVmssHealthProbe --namespace Microsoft.Network
>      ```
>
> 2. Novamente, demora cerca de 10 minutos para o estado de registo para o relatório como *registado*. Pode verificar o estado do registo atual com [show de funcionalidade de az](/cli/azure/feature#az-feature-show). 
>
> 3. Uma vez registado Certifique-se de que o *Network* fornecedor é registrado usando [Registre-se fornecedor de az](/cli/azure/provider#az-provider-register) da seguinte forma:
>
>      ```azurecli
>      az provider register --namespace Microsoft.Network
>      ```

## <a name="portal-experience"></a>Experiência do portal
Depois de seguir os passos de registo acima, pode aceder à [do portal do Azure](https://aka.ms/managed-compute) para ativar atualizações automáticas de SO em conjuntos de dimensionamento e para ver o progresso das atualizações:

![](./media/virtual-machine-scale-sets-automatic-upgrade/automatic-upgrade-portal.png)


## <a name="supported-os-images"></a>Imagens de SO suportadas
Atualmente são suportadas apenas determinadas imagens de plataforma de SO. Atualmente não é possível utilizar imagens personalizadas que tenha criado por conta própria. O *versão* propriedade da imagem de plataforma tem de ser definida como *mais recente*.

Os SKUs seguintes são atualmente suportados (serão adicionadas mais):
    
| Publicador               | Oferta         |  Sku               | Versão  |
|-------------------------|---------------|--------------------|----------|
| Canónico               | UbuntuServer  | 16.04-LTS          | mais recente   |
| MicrosoftWindowsServer  | WindowsServer | 2012-R2-Datacenter | mais recente   |
| MicrosoftWindowsServer  | WindowsServer | 2016 Datacenter    | mais recente   |
| MicrosoftWindowsServer  | WindowsServer | 2016-Datacenter-Smalldisk | mais recente   |
| MicrosoftWindowsServer  | WindowsServer | 2016 Datacenter com contentores | mais recente   |



## <a name="application-health-without-service-fabric"></a>Estado de funcionamento do aplicativo sem o Service Fabric
> [!NOTE]
> Esta secção aplica-se apenas para conjuntos de dimensionamento sem o Service Fabric. Service Fabric tem seu próprio noção de estado de funcionamento do aplicativo. Ao utilizar as atualizações automáticas de SO com o Service Fabric, a nova imagem de sistema operacional é lançada domínio de atualização por domínio de atualização para manter uma elevada disponibilidade dos serviços em execução no Service Fabric. Para obter mais informações sobre as características de durabilidade de clusters do Service Fabric, veja [esta documentação](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-durability-characteristics-of-the-cluster).

Durante uma atualização de SO, instâncias de VM num conjunto de dimensionamento são atualizadas um lote a cada vez. A atualização deve continuar apenas se a aplicação de cliente está em bom estada nas instâncias de VM atualizadas. Por esse motivo, a aplicação tem de fornecer sinais de estado de funcionamento para o mecanismo de atualização de SO de conjunto de dimensionamento. Durante as atualizações de SO, a plataforma considera o estado de energia da VM e para determinar se uma instância de VM está em bom estada após uma atualização, o estado de aprovisionamento de extensão. Durante a atualização de SO de uma instância VM, o disco do SO numa instância VM é substituído por um novo disco com base na versão mais recente da imagem. Depois de concluída a atualização do sistema operacional, as extensões configuradas são executadas nestas VMS. Apenas quando todas as extensões numa VM estão aprovisionadas com êxito, é o aplicativo considerado em bom estado. 

Além disso, o conjunto de dimensionamento *tem* ser configurado com sondas de estado de funcionamento da aplicação para fornecer a plataforma com as informações corretas sobre o estado em curso do aplicativo. Sondas de estado de funcionamento do aplicativo são personalizado Load Balancer sonda que são utilizadas como um sinal de estado de funcionamento. O aplicativo em execução numa instância VM do conjunto de dimensionamento pode responder a pedidos HTTP ou TCP externos, que indica se está em bom estado. Para obter mais informações sobre como funcionam os sondas do Balanceador de carga personalizado, consulte a [as sondas do Balanceador de carga compreender](../load-balancer/load-balancer-custom-probe-overview.md).

Se o conjunto de dimensionamento está configurado para utilizar vários grupos de colocação, sondas usando um [Balanceador de carga Standard](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview) tenha de ser utilizada.

### <a name="important-keep-credentials-up-to-date"></a>Importantes: Credenciais de manter atualizadas
Se o conjunto de dimensionamento utiliza quaisquer credenciais para aceder a recursos externos, terá de certificar-se de que as credenciais são mantidas atualizadas. Por exemplo, uma extensão de VM pode ser configurada para utilizar um token SAS para a conta de armazenamento. Se quaisquer credenciais, incluindo certificados e tokens tiverem expirado, a atualização irá falhar e o primeiro batch de VMs será deixado no estado de falha.

Os passos recomendados para recuperar as VMs e volte a ativar a atualização automática do SO, se houver uma falha de autenticação de recursos são:

* Voltar a gerar o token (ou quaisquer outras credenciais) passados para sua extensão ou extensões.
* Certifique-se de que qualquer credencial utilizada de dentro da VM para comunicar com entidades externas está atualizada.
* Atualize extensão ou extensões de no modelo de conjunto de dimensionamento com quaisquer tokens de novo.
* Implemente o conjunto de dimensionamento atualizada, o qual irá atualizar todas as instâncias de VM, incluindo aqueles com falha. 

### <a name="configuring-a-custom-load-balancer-probe-as-application-health-probe-on-a-scale-set"></a>Configurar uma sonda de Balanceador de carga do personalizada como a sonda de estado de funcionamento do aplicativo numa escala definido
*Tem* criar explicitamente uma sonda de Balanceador de carga, para o estado de funcionamento do conjunto de dimensionamento. Pode ser utilizado o mesmo ponto final para uma sonda HTTP existente ou uma sonda TCP, mas uma sonda de estado de funcionamento pode exigir um comportamento diferente de uma sonda de Balanceador de carga tradicional. Por exemplo, uma sonda de Balanceador de carga tradicional poderia retornar danificada se a carga na instância é demasiado elevada. Por outro lado que pode não ser adequado para determinar o estado de funcionamento da instância durante uma atualização de SO automática. Configure a sonda de ter uma elevada taxa de pesquisa de menos de dois minutos.

A sonda de Balanceador de carga pode ser referenciada a *networkProfile* da escala definida e pode ser associado a qualquer um dos interno ou público com acesso-balanceadores de carga da seguinte forma:

```json
"networkProfile": {
  "healthProbe" : {
    "id": "[concat(variables('lbId'), '/probes/', variables('sshProbeName'))]"
  },
  "networkInterfaceConfigurations":
  ...
```


## <a name="enforce-an-os-image-upgrade-policy-across-your-subscription"></a>Impor uma política de atualização de imagem de sistema operacional em sua assinatura
Para atualizações de segurança, recomendamos para impor uma política de atualização. Esta política pode exigir que as sondas de estado de funcionamento da aplicação na sua subscrição. A seguinte política do Azure Resource Manager rejeita as implementações que não têm a imagem do sistema operacional automatizada atualizar as definições configuradas:

### <a name="powershell"></a>PowerShell
1. Obter a definição de política incorporada do Azure Resource Manager com [Get-AzureRmPolicyDefinition](/powershell/module/AzureRM.Resources/Get-AzureRmPolicyDefinition) da seguinte forma:

    ```powershell
    $policyDefinition = Get-AzureRmPolicyDefinition -Id "/providers/Microsoft.Authorization/policyDefinitions/465f0161-0087-490a-9ad9-ad6217f4f43a"
    ```

2. Atribuir a política a uma subscrição com [New-AzureRmPolicyAssignment](/powershell/module/AzureRM.Resources/New-AzureRmPolicyAssignment) da seguinte forma:

    ```powershell
    New-AzureRmPolicyAssignment `
        -Name "Enforce automatic OS upgrades with app health checks" `
        -Scope "/subscriptions/<SubscriptionId>" `
        -PolicyDefinition $policyDefinition
    ```

### <a name="cli-20"></a>CLI 2.0
Atribua a política a uma subscrição com a política do Azure Resource Manager incorporada:

```azurecli
az policy assignment create --display-name "Enforce automatic OS upgrades with app health checks" --name "Enforce automatic OS upgrades" --policy 465f0161-0087-490a-9ad9-ad6217f4f43a --scope "/subscriptions/<SubscriptionId>"
```

## <a name="configure-auto-updates"></a>Configurar atualizações automáticas
Para configurar as atualizações automáticas, certifique-se de que o *automaticOSUpgrade* estiver definida como *verdadeiro* na escala de definição de modelo do conjunto. Pode configurar esta propriedade com o Azure PowerShell ou a CLI 2.0 do Azure.

### <a name="powershell"></a>PowerShell
O exemplo seguinte utiliza o Azure PowerShell (4.4.1 ou posterior) para configurar as atualizações automáticas para o conjunto nomeado de dimensionamento *myVMSS* no grupo de recursos com o nome *myResourceGroup*:

```powershell
$rgname = myResourceGroup
$vmssname = myVMSS
$vmss = Get-AzureRmVMss -ResourceGroupName $rgname -VmScaleSetName $vmssname
$vmss.UpgradePolicy.AutomaticOSUpgrade = $true
Update-AzureRmVmss -ResourceGroupName $rgname -VMScaleSetName $vmssname -VirtualMachineScaleSet $vmss
```

### <a name="cli-20"></a>CLI 2.0
O exemplo seguinte utiliza a CLI do Azure (2.0.20 ou posterior) para configurar as atualizações automáticas para o conjunto nomeado de dimensionamento *myVMSS* no grupo de recursos com o nome *myResourceGroup*:

```azurecli
rgname="myResourceGroup"
vmssname="myVMSS"
az vmss update --name $vmssname --resource-group $rgname --set upgradePolicy.AutomaticOSUpgrade=true
```


## <a name="check-the-status-of-an-automatic-os-upgrade"></a>Verificar o estado de uma atualização de SO automática
Pode verificar o estado da atualização do SO mais recente efetuada no seu conjunto de dimensionamento com o Azure PowerShell, CLI 2.0 do Azure ou as APIs REST.

### <a name="powershell"></a>PowerShell
Para o exemplo seguinte utiliza o Azure PowerShell (4.4.1 ou posterior) para verificar o estado para o conjunto nomeado de dimensionamento *myVMSS* no grupo de recursos com o nome *myResourceGroup*:

```powershell
Get-AzureRmVmssRollingUpgrade -ResourceGroupName myResourceGroup -VMScaleSetName myVMSS
```

### <a name="cli-20"></a>CLI 2.0
O exemplo seguinte utiliza a CLI do Azure (2.0.20 ou posterior) para verificar o estado para o conjunto nomeado de dimensionamento *myVMSS* no grupo de recursos com o nome *myResourceGroup*:

```azurecli
az vmss rolling-upgrade get-latest --resource-group myResourceGroup --name myVMSS
```

### <a name="rest-api"></a>API REST
O exemplo seguinte utiliza a API REST para verificar o estado para o conjunto nomeado de dimensionamento *myVMSS* no grupo de recursos com o nome *myResourceGroup*:

```
GET on `/subscriptions/subscription_id/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/rollingUpgrades/latest?api-version=2017-03-30`
```

A chamada GET retorna propriedades semelhantes à saída de exemplo seguinte:

```json
{
  "properties": {
    "policy": {
      "maxBatchInstancePercent": 20,
      "maxUnhealthyInstancePercent": 5,
      "maxUnhealthyUpgradedInstancePercent": 5,
      "pauseTimeBetweenBatches": "PT0S"
    },
    "runningStatus": {
      "code": "Completed",
      "startTime": "2017-06-16T03:40:14.0924763+00:00",
      "lastAction": "Start",
      "lastActionTime": "2017-06-22T08:45:43.1838042+00:00"
    },
    "progress": {
      "successfulInstanceCount": 3,
      "failedInstanceCount": 0,
      "inprogressInstanceCount": 0,
      "pendingInstanceCount": 0
    }
  },
  "type": "Microsoft.Compute/virtualMachineScaleSets/rollingUpgrades",
  "location": "southcentralus"
}
```


## <a name="automatic-os-upgrade-execution"></a>Execução de atualização automática de SO
Para expandir o uso de sondas de estado de funcionamento do aplicativo, atualizações de SO do conjunto de dimensionamento executar passos seguintes:

1. Se tiver mais do que 20% das instâncias são mau estado de funcionamento, pare a atualização; caso contrário, continue.
2. Identifique o lote seguinte de instâncias de VM para atualizar, com um lote ter máximo 20% do total de instâncias.
3. Atualize o sistema operativo do lote seguinte de instâncias de VM.
4. Se tiver mais do que 20% das instâncias atualizadas são mau estado de funcionamento, pare a atualização; caso contrário, continue.
5. Para conjuntos de dimensionamento que não fazem parte de um cluster do Service Fabric, a atualização tem de aguardar até 5 minutos para sondas para se tornar íntegros, em seguida, continua a imediatamente para o lote seguinte. Para conjuntos de dimensionamento que fazem parte de um cluster do Service Fabric, o conjunto de dimensionamento aguarda 30 minutos antes de passar para o lote seguinte.
6. Se é restantes que são instâncias para atualizar, goto passo 1) para o lote seguinte; caso contrário, a atualização foi concluída.

O conjunto de dimensionamento verificações de mecanismo de atualização de SO para o estado de funcionamento de instância VM geral antes de atualizar todos os lotes. Durante a atualização de um lote, pode haver outros planeada em simultâneo ou a manutenção não planeada acontecendo nos centros de dados do Azure que possa afetar a disponibilidade das suas VMs. Portanto, é possível que temporariamente mais de 20% instâncias podem estar inativa. Nesses casos, no final do lote atual, o conjunto de dimensionamento paradas de atualização.


## <a name="deploy-with-a-template"></a>Implementar com um modelo

Pode utilizar o modelo seguinte para implementar um conjunto de dimensionamento que utiliza as atualizações automáticas de <a href='https://github.com/Azure/vm-scale-sets/blob/master/preview/upgrade/autoupdate.json'>cuidam do andamento automática atualiza - o Ubuntu 16.04-LTS</a>

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fvm-scale-sets%2Fmaster%2Fpreview%2Fupgrade%2Fautoupdate.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>


## <a name="next-steps"></a>Passos Seguintes
Para obter mais exemplos sobre como utilizar as atualizações automáticas de SO com conjuntos de dimensionamento, veja a [repositório do GitHub para funcionalidades de pré-visualização](https://github.com/Azure/vm-scale-sets/tree/master/preview/upgrade).
