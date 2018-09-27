---
title: Conjuntos de dimensionamento de atualizações automáticas de imagem de SO com a máquina virtual do Azure | Documentos da Microsoft
description: Saiba como atualizar automaticamente a imagem do SO nas instâncias VM num conjunto de dimensionamento
services: virtual-machine-scale-sets
documentationcenter: ''
author: rajsqr
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2018
ms.author: rajraj
ms.openlocfilehash: 1ca0ec7185707d9b9f9712c2ace8dacb361f7b5b
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/27/2018
ms.locfileid: "47394374"
---
# <a name="azure-virtual-machine-scale-set-automatic-os-image-upgrades"></a>Atualizações automáticas da imagem de SO do conjunto de dimensionamento de máquina virtual do Azure

Atualização automática de imagem do SO é um recurso de conjuntos de dimensionamento de máquina virtual do Azure de que atualiza automaticamente todas as VMs para a imagem de SO mais recente.

Atualização automática de SO tem as seguintes características:

- Depois de configurada, a imagem de SO mais recente, publicada por editores de imagem é aplicada automaticamente para o conjunto sem intervenção do utilizador de dimensionamento.
- Atualiza lotes de instâncias de uma maneira sem interrupção sempre que uma nova imagem de plataforma é publicada pelo editor.
- Integra-se com a sonda de estado de funcionamento da aplicação.
- Funciona para todos os tamanhos VM e para imagens de plataforma do Windows e Linux.
- Pode desativar as atualizações automáticas em qualquer altura (atualizações de SO podem ser iniciadas manualmente também).
- O disco do SO de uma VM é substituído pelo novo disco de SO criado com a versão mais recente da imagem. Extensões configuradas e scripts de dados personalizados são executados ao mesmo tempo são retidos discos de dados persistentes.
- Encriptação de disco do Azure (em pré-visualização) não é atualmente suportada.  

## <a name="how-does-automatic-os-image-upgrade-work"></a>Como a SO automática trabalho de atualização de imagens?

Uma atualização funciona ao substituir o disco do SO de uma VM um novo, criado usando a versão mais recente da imagem. Qualquer configurado extensões e os scripts de dados personalizados são executados, ao mesmo tempo são retidos discos de dados persistentes. Para minimizar o período de indisponibilidade de aplicação, as atualizações ocorrem em lotes de máquinas, com mais do que 20% da escala definida a atualizar em qualquer altura. Tem também a opção para integrar uma sonda de estado de funcionamento da aplicação de Balanceador de carga do Azure. É altamente recomendado para incorporar um heartbeat do aplicativo e validar o êxito da atualização para cada lote no processo de atualização. Os passos de execução são: 

1. Antes de iniciar o processo de atualização, o orchestrator será Certifique-se de que não mais do que 20% das instâncias estão danificados. 
2. Identifique o lote de instâncias de VM para atualizar, com um lote de ter o máximo de 20% da contagem de instâncias total.
3. Atualize a imagem do SO deste lote de instâncias de VM.
4. Se o cliente tiver configurado as sondas de estado de funcionamento do aplicativo, a atualização tem de aguardar até 5 minutos para sondas para se tornar íntegros, antes de passar para atualizar o lote seguinte. 
5. Se é restantes que são instâncias para atualizar, goto passo 1) para o lote seguinte; caso contrário, a atualização foi concluída.

O conjunto de dimensionamento verificações de orchestrator de atualização de SO para o estado de funcionamento de instância VM geral antes de atualizar todos os lotes. Durante a atualização de um lote, pode haver outros planeada em simultâneo ou a manutenção não planeada acontecendo nos centros de dados do Azure que possa afetar a disponibilidade das suas VMs. Por conseguinte, é possível que temporariamente mais de 20% instâncias podem ficar inativo. Nesses casos, no final do lote atual, o conjunto de dimensionamento paradas de atualização.

## <a name="supported-os-images"></a>Imagens de SO suportadas
Atualmente são suportadas apenas determinadas imagens de plataforma de SO. Atualmente não é possível utilizar imagens personalizadas que tenha criado por mesmo. 

Os SKUs seguintes são atualmente suportados (serão adicionadas mais no futuro):
    
| Publicador               | Oferta de SO      |  Sku               |
|-------------------------|---------------|--------------------|
| Canónico               | UbuntuServer  | 16.04-LTS          |
| Canónico               | UbuntuServer  | 18.04 LTS *        | 
| Rogue Wave (OpenLogic)  | CentOS        | 7.5 *              | 
| CoreOS                  | CoreOS        | Estável             | 
| Microsoft Corporation   | WindowsServer | 2012-R2-Datacenter | 
| Microsoft Corporation   | WindowsServer | 2016 Datacenter    | 
| Microsoft Corporation   | WindowsServer | 2016-Datacenter-Smalldisk |
| Microsoft Corporation   | WindowsServer | 2016 Datacenter com contentores |

* Suporte para estas imagens está atualmente a implementar e estará disponível em todas as regiões do Azure em breve. 

## <a name="requirements-for-configuring-automatic-os-image-upgrade"></a>Requisitos para configurar a atualização automática de imagem do SO

- O *versão* propriedade da imagem de plataforma tem de ser definida como *mais recente*.
- Utilize sondas de estado de funcionamento de aplicações não Service Fabric para conjuntos de dimensionamento.
- Certifique-se de que os recursos que o modelo de conjunto de dimensionamento se refere a está disponível e mantido atualizado. 
  URI de Exa.SAS para inicialização payload nas propriedades de extensão VM, payload na conta de armazenamento referenciar a segredos no modelo. 

## <a name="configure-automatic-os-image-upgrade"></a>Configurar a atualização automática de imagem do SO
Para configurar a atualização automática de imagem do SO, certifique-se de que o *automaticOSUpgradePolicy.enableAutomaticOSUpgrade* estiver definida como *verdadeiro* na escala de definição de modelo do conjunto. 

```
PUT or PATCH on `/subscriptions/subscription_id/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet?api-version=2018-10-01`
```

```json
{ 
  "properties": { 
    "upgradePolicy": { 
      "automaticOSUpgradePolicy": { 
        "enableAutomaticOSUpgrade":  true 
      } 
    } 
  } 
} 
```

Suporte para configurar esta propriedade através do Azure PowerShell e CLI 2.0 será lançado em 10/09.

## <a name="using-application-health-probes"></a>Sondas de estado de funcionamento da aplicação a utilizar 

Durante uma atualização de SO, instâncias de VM num conjunto de dimensionamento são atualizadas um lote a cada vez. A atualização deve continuar apenas se a aplicação de cliente está em bom estada nas instâncias de VM atualizadas. Recomendamos que o aplicativo fornece sinais de estado de funcionamento para o mecanismo de atualização de SO de conjunto de dimensionamento. Por predefinição, durante as atualizações de SO a plataforma considera o estado de energia VM e para determinar se uma instância de VM está em bom estada após uma atualização, o estado de aprovisionamento de extensão. Durante a atualização de SO de uma instância VM, o disco do SO numa instância VM é substituído por um novo disco com base na versão mais recente da imagem. Depois de concluída a atualização do sistema operacional, as extensões configuradas são executadas nestas VMS. Apenas quando todas as extensões numa VM estão aprovisionadas com êxito, é o aplicativo considerado em bom estado. 

Opcionalmente, pode ser configurado um conjunto de dimensionamento com sondas de estado de funcionamento da aplicação para fornecer a plataforma com informações precisas sobre o estado atual do aplicativo. Sondas de estado de funcionamento do aplicativo são personalizado Load Balancer sonda que são utilizadas como um sinal de estado de funcionamento. O aplicativo em execução numa instância VM do conjunto de dimensionamento pode responder a pedidos HTTP ou TCP externos, que indica se está em bom estado. Para obter mais informações sobre como funcionam os sondas do Balanceador de carga personalizado, consulte a [as sondas do Balanceador de carga compreender](../load-balancer/load-balancer-custom-probe-overview.md). Uma sonda de estado de funcionamento de aplicação não é necessário para atualizações automáticas de SO, mas é altamente recomendável.

Se o conjunto de dimensionamento está configurado para utilizar vários grupos de colocação, sondas usando um [Balanceador de carga Standard](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview) tenha de ser utilizada.

### <a name="configuring-a-custom-load-balancer-probe-as-application-health-probe-on-a-scale-set"></a>Configurar uma sonda de Balanceador de carga do personalizada como a sonda de estado de funcionamento do aplicativo numa escala definido
Como melhor prática, crie uma sonda de Balanceador de carga explicitamente para o estado de funcionamento do conjunto de dimensionamento. Pode ser utilizado o mesmo ponto final para uma sonda HTTP existente ou uma sonda TCP, mas uma sonda de estado de funcionamento pode exigir um comportamento diferente de uma sonda de Balanceador de carga tradicional. Por exemplo, uma sonda de Balanceador de carga tradicional pode retornar danificada se a carga na instância é demasiado elevada, enquanto que pode não ser apropriado para determinar o estado de funcionamento da instância durante uma atualização de SO automática. Configure a sonda de ter uma elevada taxa de pesquisa de menos de dois minutos.

A sonda de Balanceador de carga pode ser referenciada a *networkProfile* da escala definida e pode ser associado a qualquer um dos interno ou público com acesso-balanceadores de carga da seguinte forma:

```json
"networkProfile": {
  "healthProbe" : {
    "id": "[concat(variables('lbId'), '/probes/', variables('sshProbeName'))]"
  },
  "networkInterfaceConfigurations":
  ...
```
> [!NOTE]
> Ao utilizar as atualizações automáticas de SO com o Service Fabric, a nova imagem de sistema operacional é lançada domínio de atualização por domínio de atualização para manter uma elevada disponibilidade dos serviços em execução no Service Fabric. Para obter mais informações sobre as características de durabilidade de clusters do Service Fabric, veja [esta documentação](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-durability-characteristics-of-the-cluster).

### <a name="keep-credentials-up-to-date"></a>Manter credenciais atualizados
Se o conjunto de dimensionamento utiliza quaisquer credenciais para aceder a recursos externos, por exemplo, se estiver configurada uma extensão de VM que utiliza um token SAS para a conta de armazenamento, terá de certificar-se de que as credenciais são mantidas atualizadas. Se quaisquer credenciais, incluindo certificados e tokens tiverem expirado, a atualização irá falhar e o primeiro batch de VMs será deixado no estado de falha.

Os passos recomendados para recuperar as VMs e volte a ativar a atualização automática do SO, se houver uma falha de autenticação de recursos são:

* Voltar a gerar o token (ou quaisquer outras credenciais) passados para sua extensão ou extensões.
* Certifique-se de que qualquer credencial utilizada de dentro da VM para comunicar com entidades externas está atualizada.
* Atualize extensão ou extensões de no modelo de conjunto de dimensionamento com quaisquer tokens de novo.
* Implemente o conjunto de dimensionamento atualizada, o qual irá atualizar todas as instâncias de VM, incluindo aqueles com falha. 

## <a name="get-the-history-of-automatic-os-image-upgrades"></a>Obter o histórico de atualizações automáticas da imagem de SO 
Pode verificar o histórico de atualização de SO mais recente efetuada no seu conjunto de dimensionamento com o Azure PowerShell, CLI 2.0 do Azure ou as APIs REST. Pode obter o histórico para as tentativas de atualização de SO últimos cinco nos últimos dois meses.

### <a name="azure-powershell"></a>Azure PowerShell
Para o exemplo seguinte utiliza o Azure PowerShell para verificar o estado para o conjunto nomeado de dimensionamento *myVMSS* no grupo de recursos com o nome *myResourceGroup*:

```powershell
Get-AzureRmVmss -ResourceGroupName myResourceGroup -VMScaleSetName myVMSS -OSUpgradeHistory
```

### <a name="azure-cli-20"></a>CLI 2.0 do Azure
O exemplo seguinte utiliza a CLI do Azure (2.0.20 ou posterior) para verificar o estado para o conjunto nomeado de dimensionamento *myVMSS* no grupo de recursos com o nome *myResourceGroup*:

```azurecli
az vmss rolling-upgrade get-latest --resource-group myResourceGroup --name myVMSS
```

### <a name="rest-api"></a>API REST
O exemplo seguinte utiliza a API REST para verificar o estado para o conjunto nomeado de dimensionamento *myVMSS* no grupo de recursos com o nome *myResourceGroup*:

```
GET on `/subscriptions/subscription_id/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/osUpgradeHistory?api-version=2018-10-01`
```
Consulte a documentação para esta API aqui: https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/getosupgradehistory.

A chamada GET retorna propriedades semelhantes à saída de exemplo seguinte:

```json
{
  "value": [
    {
      "properties": {
        "runningStatus": {
          "code": "RollingForward",
          "startTime": "2018-07-24T17:46:06.1248429+00:00",
          "completedTime": "2018-04-21T12:29:25.0511245+00:00"
        },
        "progress": {
          "successfulInstanceCount": 16,
          "failedInstanceCount": 0,
          "inProgressInstanceCount": 4,
          "pendingInstanceCount": 0
        },
        "startedBy": "Platform",
        "targetImageReference": {
          "publisher": "MicrosoftWindowsServer",
          "offer": "WindowsServer",
          "sku": "2016-Datacenter",
          "version": "2016.127.20180613"
        },
        "rollbackInfo": {
          "successfullyRolledbackInstanceCount": 0,
          "failedRolledbackInstanceCount": 0
        }
      },
      "type": "Microsoft.Compute/virtualMachineScaleSets/rollingUpgrades",
      "location": "westeurope"
    }
  ]
} 
```

## <a name="how-to-get-the-latest-version-of-a-platform-os-image"></a>Como obter a versão mais recente de uma imagem de plataforma de SO? 

Pode obter as versões de imagem para a atualização de SO automática de SKUs suportados usando os cmdlets do PowerShell abaixo: 

```powershell
Get-AzureRmVmImage -Location "westus" -PublisherName "Canonical" -Offer "UbuntuServer" -Skus "16.04-LTS"
```

## <a name="deploy-with-a-template"></a>Implementar com um modelo

Pode utilizar o modelo seguinte para implementar um conjunto de dimensionamento que utiliza as atualizações automáticas de <a href='https://github.com/Azure/vm-scale-sets/blob/master/preview/upgrade/autoupdate.json'>cuidam do andamento automática atualiza - o Ubuntu 16.04-LTS</a>

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fvm-scale-sets%2Fmaster%2Fpreview%2Fupgrade%2Fautoupdate.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>

## <a name="next-steps"></a>Passos Seguintes
Para obter mais exemplos sobre como utilizar as atualizações automáticas de SO com conjuntos de dimensionamento, veja a [repositório do GitHub para funcionalidades de pré-visualização](https://github.com/Azure/vm-scale-sets/tree/master/preview/upgrade).
