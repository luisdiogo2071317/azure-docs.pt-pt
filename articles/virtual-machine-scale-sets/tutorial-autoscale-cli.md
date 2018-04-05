---
title: Tutorial – Dimensionar automaticamente um conjunto de dimensionamento com a CLI 2.0 do Azure | Microsoft Docs
description: Saiba como utilizar a CLI 2.0 do Azure para dimensionar automaticamente um conjunto de dimensionamento de máquinas virtuais, à medida que a CPU exige aumentos e diminuições
services: virtual-machine-scale-sets
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/27/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 10e5b1a261f28391bed8cf3f111b1124b52d7816
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/28/2018
---
# <a name="tutorial-automatically-scale-a-virtual-machine-scale-set-with-the-azure-cli-20"></a>Tutorial: Dimensionar automaticamente um conjunto de dimensionamento de máquinas virtuais com a CLI 2.0 do Azure
Quando criar um conjunto de dimensionamento, pode definir o número de instâncias de VM que quer executar. À medida que a sua aplicação exige alterações, pode aumentar ou reduzir automaticamente o número de instâncias de VM. A capacidade de dimensionamento automático permite-lhe manter-se a par da exigência do cliente ou responder às alterações de desempenho durante todo o ciclo de vida da aplicação. Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Utilizar o dimensionamento automático com um conjunto de dimensionamento
> * Criar e utilizar regras de dimensionamento automático
> * Teste de esforço das instâncias de VM e acionar as regras de dimensionamento automático
> * Voltar ao dimensionamento automático à medida que a exigência diminui

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Se optar por instalar e utilizar a CLI localmente, este tutorial requer a execução da versão 2.0.29 ou posterior da CLI do Azure. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [instalar o Azure CLI 2.0]( /cli/azure/install-azure-cli). 


## <a name="create-a-scale-set"></a>Criar um conjunto de dimensionamento
Para ajudar a criar as regras de dimensionamento automático, defina alguns parâmetros para o seu ID de subscrição, o grupo de recursos e o nome do conjunto de dimensionamento, e a localização:

```azurecli-interactive
sub=$(az account show --query id -o tsv)
resourcegroup_name="myResourceGroup"
scaleset_name="myScaleSet"
location_name="eastus"
```

Crie um grupo de recursos com [az group create](/cli/azure/group#create), da seguinte forma:

```azurecli-interactive
az group create --name $resourcegroup_name --location $location_name
```

Agora, crie um conjunto de dimensionamento de máquinas virtuais com [az vmss create](/cli/azure/vmss#create). O exemplo seguinte cria um conjunto de dimensionamento com uma contagem de instâncias de *2*e gera chaves SSH, caso não existam:

```azurecli-interactive
az vmss create \
  --resource-group $resourcegroup_name \
  --name $scaleset_name \
  --image UbuntuLTS \
  --upgrade-policy-mode automatic \
  --instance-count 2 \
  --admin-username azureuser \
  --generate-ssh-keys
```


## <a name="define-an-autoscale-profile"></a>Definir um perfil de dimensionamento automático
As regras de dimensionamento automático são implementadas como JSON (JavaScript Object Notation) com a CLI 2.0 do Azure. Vamos examinar cada parte deste perfil de dimensionamento automático e, em seguida, criar um exemplo completo.

O início do perfil de dimensionamento automático define a capacidade predefinida, máxima e mínima do conjunto de dimensionamento. O exemplo seguinte define a capacidade predefinida e mínima de *2* instâncias de VM e um máximo de *10*:

```json
{
  "name": "autoscale rules",
  "capacity": {
    "minimum": "2",
    "maximum": "10",
    "default": "2"
  }
}
```


## <a name="create-a-rule-to-autoscale-out"></a>Criar uma regra para dimensionamento automático para aumentar horizontalmente
Se a exigência da aplicação aumentar, a carga sobre as instâncias de VM no conjunto de dimensionamento também aumenta. Se este aumento de carga for consistente, em vez de ser apenas uma breve exigência, pode configurar regras de dimensionamento automático para aumentar o número de instâncias de VM no conjunto de dimensionamento. Quando estas instâncias de VM forem criadas e as aplicações forem implementadas, o conjunto de dimensionamento começa a distribuir o tráfego pelas mesmas através do balanceador de carga. Controla as métricas a monitorizar, como a CPU ou o disco, quando a carga da aplicação tem de cumprir um determinado limiar, e quantas instâncias de VM devem ser adicionadas ao conjunto de dimensionamento.

Vamos criar uma regra que aumenta o número de instâncias de VM num conjunto de dimensionamento quando a carga média da CPU é superior a 70% durante um período de 5 minutos. Quando a regra for acionada, o número de instâncias de VM é aumentado em três.

Os parâmetros seguintes são utilizados para esta regra:

| Parâmetro         | Explicação                                                                                                         | Valor           |
|-------------------|---------------------------------------------------------------------------------------------------------------------|-----------------|
| *metricName*      | A métrica de desempenho para monitorizar e aplicar ações ao conjunto de dimensionamento.                                                   | Percentagem da CPU  |
| *timeGrain*       | Frequência com que as métricas são recolhidas para análise.                                                                   | 1 minuto        |
| *timeAggregation* | Define a forma como as métricas recolhidas devem ser agregadas para análise.                                                | Média         |
| *timeWindow*      | A quantidade de tempo monitorizado antes de os valores de métrica e limiar serem comparados.                                   | 5 minutos       |
| *operador*        | Operador utilizado para comparar os dados de métrica relativamente ao limiar.                                                     | Maior Que    |
| *limiar*       | O valor que faz com que a regra de dimensionamento automático acione uma ação.                                                      | 70%             |
| *direção*       | Define se o conjunto de dimensionamento deve aumentar e reduzir verticalmente quando a regra se aplicar.                                              | Aumentar        |
| *tipo*            | Indica que o número de instâncias de VM deve ser alterado por um valor específico.                                    | Alterar Contagem    |
| *valor*           | Quantas instâncias de VM devem ser aumentadas ou reduzidas horizontalmente quando a regra se aplicar.                                             | 3               |
| *tempo de arrefecimento*        | A quantidade de tempo de espera antes de a regra ser aplicada novamente, para que as ações de dimensionamento automático tenham tempo de entrar em vigor. | 5 minutos       |

O exemplo seguinte define a regra para aumentar horizontalmente o número de instâncias de VM. *metricResourceUri* utiliza as variáveis definidas anteriormente para o ID de subscrição, o nome do grupo de recursos e o nome do conjunto de dimensionamento:

```json
{
  "metricTrigger": {
    "metricName": "Percentage CPU",
    "metricNamespace": "",
    "metricResourceUri": "/subscriptions/'$sub'/resourceGroups/'$resourcegroup_name'/providers/Microsoft.Compute/virtualMachineScaleSets/'$scaleset_name'",
    "metricResourceLocation": "'$location_name'",
    "timeGrain": "PT1M",
    "statistic": "Average",
    "timeWindow": "PT5M",
    "timeAggregation": "Average",
    "operator": "GreaterThan",
    "threshold": 70
  },
  "scaleAction": {
    "direction": "Increase",
    "type": "ChangeCount",
    "value": "3",
    "cooldown": "PT5M"
  }
}
```


## <a name="create-a-rule-to-autoscale-in"></a>Criar uma regra para dimensionamento automático, para reduzir horizontalmente
À noite ou ao fim de semana, a exigência da aplicação pode diminuir. Se esta diminuição de carga for consistente durante um certo período de tempo, pode configurar regras de dimensionamento automático para diminuir o número de instâncias de VM no conjunto de dimensionamento. Esta ação de dimensionamento para reduzir horizontalmente reduz o custo de execução do conjunto de dimensionamento, uma vez que apenas executa o número de instâncias necessário para satisfazer a exigência atual.

Crie outra regra que diminui o número de instâncias de VM num conjunto de dimensionamento quando a carga média da CPU passa a ser inferior a 30% durante um período de 5 minutos. O exemplo seguinte define a regra para reduzir horizontalmente o número de instâncias de VM para um. *metricResourceUri* utiliza as variáveis definidas anteriormente para o ID de subscrição, o nome do grupo de recursos e o nome do conjunto de dimensionamento:

```json
{
  "metricTrigger": {
    "metricName": "Percentage CPU",
    "metricNamespace": "",
    "metricResourceUri": "/subscriptions/'$sub'/resourceGroups/'$resourcegroup_name'/providers/Microsoft.Compute/virtualMachineScaleSets/'$scaleset_name'",
    "metricResourceLocation": "'$location_name'",
    "timeGrain": "PT1M",
    "statistic": "Average",
    "timeWindow": "PT5M",
    "timeAggregation": "Average",
    "operator": "LessThan",
    "threshold": 30
  },
  "scaleAction": {
    "direction": "Decrease",
    "type": "ChangeCount",
    "value": "1",
    "cooldown": "PT5M"
  }
}
```


## <a name="apply-autoscale-rules-to-a-scale-set"></a>Aplicar regras de dimensionamento automático a um conjunto de dimensionamento
O último passo consiste em aplicar o perfil e as regras de dimensionamento automático ao conjunto de dimensionamento. O conjunto de dimensionamento passa a ter a capacidade de aumentar e reduzir horizontalmente de forma automática com base na exigência da aplicação. Aplique o perfil de dimensionamento automático com [az monitor autoscale-settings create](/cli/azure/monitor/autoscale-settings#az_monitor_autoscale_settings_create), da seguinte forma. O seguinte JSON completo utiliza o perfil e as regras indicadas nas secções anteriores:

```azurecli-interactive
az monitor autoscale-settings create \
    --resource-group $resourcegroup_name \
    --name autoscale \
    --parameters '{"autoscale_setting_resource_name": "autoscale",
      "enabled": true,
      "location": "'$location_name'",
      "notifications": [],
      "profiles": [
        {
          "name": "autoscale by percentage based on CPU usage",
          "capacity": {
            "minimum": "2",
            "maximum": "10",
            "default": "2"
          },
          "rules": [
            {
              "metricTrigger": {
                "metricName": "Percentage CPU",
                "metricNamespace": "",
                "metricResourceUri": "/subscriptions/'$sub'/resourceGroups/'$resourcegroup_name'/providers/Microsoft.Compute/virtualMachineScaleSets/'$scaleset_name'",
                "metricResourceLocation": "'$location_name'",
                "timeGrain": "PT1M",
                "statistic": "Average",
                "timeWindow": "PT5M",
                "timeAggregation": "Average",
                "operator": "GreaterThan",
                "threshold": 70
              },
              "scaleAction": {
                "direction": "Increase",
                "type": "ChangeCount",
                "value": "3",
                "cooldown": "PT5M"
              }
            },
            {
              "metricTrigger": {
                "metricName": "Percentage CPU",
                "metricNamespace": "",
                "metricResourceUri": "/subscriptions/'$sub'/resourceGroups/'$resourcegroup_name'/providers/Microsoft.Compute/virtualMachineScaleSets/'$scaleset_name'",
                "metricResourceLocation": "'$location_name'",
                "timeGrain": "PT1M",
                "statistic": "Average",
                "timeWindow": "PT5M",
                "timeAggregation": "Average",
                "operator": "LessThan",
                "threshold": 30
              },
              "scaleAction": {
                "direction": "Decrease",
                "type": "ChangeCount",
                "value": "1",
                "cooldown": "PT5M"
              }
            }
          ]
        }
      ],
      "tags": {},
      "target_resource_uri": "/subscriptions/'$sub'/resourceGroups/'$resourcegroup_name'/providers/Microsoft.Compute/virtualMachineScaleSets/'$scaleset_name'"
    }'
```


## <a name="generate-cpu-load-on-scale-set"></a>Gerar a carga da CPU no conjunto de dimensionamento
Para testar as regras de dimensionamento automático, gere a carga de CPU nas instâncias de VM do conjunto de dimensionamento. Esta carga de CPU simulada faz com que os dimensionamentos automáticos aumentem horizontalmente e aumentem o número de instâncias de VM. Uma vez que a carga de CPU simulada é diminuída, as regras de dimensionamento automático reduzem horizontalmente e reduzem o número de instâncias de VM.

Em primeiro lugar, liste o endereço e as portas para ligar a instâncias de VM num conjunto de dimensionamento com [az vmss list-instance-connection-info](/cli/azure/vmss#az_vmss_list_instance_connection_info):

```azurecli-interactive
az vmss list-instance-connection-info \
  --resource-group $resourcegroup_name \
  --name $scaleset_name
```

O seguinte resultado de exemplo mostra o nome da instância, o endereço IP público do balanceador de carga e o número da porta para o qual as regras NAT (Tradução de Endereços de Rede) encaminham o tráfego:

```json
{
  "instance 1": "13.92.224.66:50001",
  "instance 3": "13.92.224.66:50003"
}
```

SSH para a sua primeira instância de VM. Especifique o seu endereço IP público e o número da porta com o parâmetro `-p`, conforme mostrado no comando anterior:

```azurecli-interactive
ssh azureuser@13.92.224.66 -p 50001
```

Depois de iniciar sessão, instale o utilitário **stress**. Inicie *10* funções de trabalho de **stress** que geram carga da CPU. Estas funções de trabalho são executadas durante *420* segundos, que é o suficiente para fazer com que as regras de dimensionamento automático implementem a ação pretendida.

```azurecli-interactive
sudo apt-get -y install stress
sudo stress --cpu 10 --timeout 420 &
```

Quando o **stress** mostrar um resultado semelhante a *stress: info: [2688] dispatching hogs: 10 cpu, 0 io, 0 vm, 0 hdd*, prima a tecla *Enter* para regressar à linha de comandos.

Para confirmar que o **stress** gera carga de CPU, examine a carga de sistema ativa com o utilitário **top**:

```azuecli-interactive
top
```

Saia de **top** e, em seguida, feche a ligação à instância de VM. O **stress** continua a ser executado na instância de VM.

```azurecli-interactive
Ctrl-c
exit
```

Ligue à segunda instância de VM com o número de porta listado do anterior [az vmss list-instance-connection-info](/cli/azure/vmss#az_vmss_list_instance_connection_info):

```azurecli-interactive
ssh azureuser@13.92.224.66 -p 50003
```

Instale e execute o **stress** e, em seguida, inicie dez funções de trabalho nesta segunda instância de VM.

```azurecli-interactive
sudo apt-get -y install stress
sudo stress --cpu 10 --timeout 420 &
```

Novamente, quando o **stress** mostrar um resultado semelhante a *stress: info: [2713] dispatching hogs: 10 cpu, 0 io, 0 vm, 0 hdd*, prima a tecla *Enter* para regressar à linha de comandos.

Feche a ligação à segunda instância de VM. O **stress** continua a ser executado na instância de VM.

```azurecli-interactive
exit
```

## <a name="monitor-the-active-autoscale-rules"></a>Monitorizar as regras de dimensionamento automático ativas
Para monitorizar o número de instâncias de VM no conjunto de dimensionamento, utilize **watch**. Demora 5 minutos até as regras de dimensionamento automático iniciarem o processo de aumentar horizontalmente para a carga de CPU gerada por **stress** em cada uma das instâncias de VM:

```azurecli-interactive
watch az vmss list-instances \
  --resource-group $resourcegroup_name \
  --name $scaleset_name \
  --output table
```

Assim que tiver sido cumprido o limiar de CPU, as regras de dimensionamento automático aumentam o número de instâncias de VM no conjunto de dimensionamento. O resultado seguinte mostra três VMs criadas à medida que o conjunto de dimensionamento aumenta horizontalmente:

```bash
Every 2.0s: az vmss list-instances --resource-group myResourceGroup --name myScaleSet --output table

  InstanceId  LatestModelApplied    Location    Name          ProvisioningState    ResourceGroup    VmId
------------  --------------------  ----------  ------------  -------------------  ---------------  ------------------------------------
           1  True                  eastus      myScaleSet_1  Succeeded            MYRESOURCEGROUP  4f92f350-2b68-464f-8a01-e5e590557955
           2  True                  eastus      myScaleSet_2  Succeeded            MYRESOURCEGROUP  d734cd3d-fb38-4302-817c-cfe35655d48e
           4  True                  eastus      myScaleSet_4  Creating             MYRESOURCEGROUP  061b4c90-0d73-49fc-a066-19eab0b3d95c
           5  True                  eastus      myScaleSet_5  Creating             MYRESOURCEGROUP  4beff8b9-4e65-40cb-9652-43899309da27
           6  True                  eastus      myScaleSet_6  Creating             MYRESOURCEGROUP  9e4133dd-2c57-490e-ae45-90513ce3b336
```

Assim que o **stress** para nas instâncias de VM iniciais, a carga de CPU média volta ao normal. Após mais 5 minutos, as regras de dimensionamento automático reduzem horizontalmente o número de instâncias de VM. As ações para reduzir horizontalmente removem primeiro as instâncias de VM com os IDs mais elevados. O resultado de exemplo seguinte mostra uma instância de VM eliminada à medida que o conjunto de dimensionamento reduz horizontalmente de forma automática:

```bash
           6  True                  eastus      myScaleSet_6  Deleting             MYRESOURCEGROUP  9e4133dd-2c57-490e-ae45-90513ce3b336
```

Saia de *watch* com `Ctrl-c`. O conjunto de dimensionamento continua a reduzir horizontalmente a cada 5 minutos e remove uma instância de VM até ser atingida uma contagem mínima de duas instâncias.


## <a name="clean-up-resources"></a>Limpar recursos
Para remover o seu conjunto de dimensionamento e recursos adicionais, elimine o grupo de recursos e todos os respetivos recursos com [az group delete](/cli/azure/group#az_group_delete). O parâmetro `--no-wait` devolve o controlo à linha de comandos, sem aguardar a conclusão da operação. O parâmetro `--yes` confirma que pretende eliminar os recursos sem uma linha de comandos adicional para fazê-lo.

```azurecli-interactive
az group delete --name $resourcegroup_name --yes --no-wait
```


## <a name="next-steps"></a>Passos seguintes
Neste tutorial, aprendeu a aumentar e reduzir automaticamente um conjunto de dimensionamento com a CLI 2.0 do Azure:

> [!div class="checklist"]
> * Utilizar o dimensionamento automático com um conjunto de dimensionamento
> * Criar e utilizar regras de dimensionamento automático
> * Teste de esforço das instâncias de VM e acionar as regras de dimensionamento automático
> * Voltar ao dimensionamento automático à medida que a exigência diminui

Para obter mais exemplos de conjuntos de dimensionamento de máquinas virtuais em ação, veja os seguintes scripts de exemplo da CLI 2.0 do Azure:

> [!div class="nextstepaction"]
> [Exemplos de scripts de conjuntos de dimensionamento da CLI 2.0 do Azure](cli-samples.md)