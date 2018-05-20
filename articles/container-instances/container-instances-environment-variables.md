---
title: Definir variáveis de ambiente em instâncias de contentor do Azure
description: Saiba como definir variáveis de ambiente nos contentores executadas em instâncias de contentor do Azure
services: container-instances
author: mmacy
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 05/16/2018
ms.author: marsma
ms.openlocfilehash: 1a025ce647cb3c071a6549a433e6505b85409fdc
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
---
# <a name="set-environment-variables"></a>Definir variáveis de ambiente

A definição de variáveis de ambiente nas instâncias de contentor permite-lhe fornecer a configuração dinâmica da aplicação ou script execute ao contentor. Para definir as variáveis de ambiente num contentor, especifique-os ao criar uma instância de contentor. Pode definir variáveis de ambiente quando inicia um contentor com o [CLI do Azure](#azure-cli-example), [Azure PowerShell](#azure-powershell-example)e o [portal do Azure](#azure-portal-example).

Por exemplo, se executar o [aci/microsoft-wordcount] [ aci-wordcount] imagem de contentor, pode modificar o comportamento ao especificar as seguintes variáveis de ambiente:

*NumWords*: O número de palavras enviado para STDOUT.

*MinLength*: O número mínimo de carateres de uma palavra-lo a contar. Um número mais alto ignora palavras comuns, como "de" e "a".

## <a name="azure-cli-example"></a>Exemplo CLI do Azure

Para ver o resultado predefinido a [aci/microsoft-wordcount] [ aci-wordcount] contentor, executá-lo primeiro com esta [criar contentor de az] [ az-container-create] comando (não variáveis de ambiente especificadas):

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer1 \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure
```

Para modificar o resultado, inicie um segundo contentor com o `--environment-variables` argumento adicionado, especificar valores para o *NumWords* e *MinLength* variáveis:

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer2 \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure \
    --environment-variables NumWords=5 MinLength=8
```

Assim que mostra o estado dos ambos os contentores como *Terminated* (utilizar [mostrar de contentor az] [ az-container-show] para verificar o estado), apresentar os respetivos registos com [os registos do contentor de az] [ az-container-logs] para ver o resultado.

```azurecli-interactive
az container logs --resource-group myResourceGroup --name mycontainer1
az container logs --resource-group myResourceGroup --name mycontainer2
```

O resultado dos contentores Mostrar como tiver modificado o comportamento de script do contentor segundo definindo as variáveis de ambiente.

```console
azureuser@Azure:~$ az container logs --resource-group myResourceGroup --name mycontainer1
[('the', 990),
 ('and', 702),
 ('of', 628),
 ('to', 610),
 ('I', 544),
 ('you', 495),
 ('a', 453),
 ('my', 441),
 ('in', 399),
 ('HAMLET', 386)]

azureuser@Azure:~$ az container logs --resource-group myResourceGroup --name mycontainer2
[('CLAUDIUS', 120),
 ('POLONIUS', 113),
 ('GERTRUDE', 82),
 ('ROSENCRANTZ', 69),
 ('GUILDENSTERN', 54)]
```

## <a name="azure-powershell-example"></a>Exemplo do PowerShell do Azure

A definição de variáveis de ambiente no PowerShell semelhante para a CLI, mas utiliza o `-EnvironmentVariable` argumento da linha de comandos.

Em primeiro lugar, inicie o [aci/microsoft-wordcount] [ aci-wordcount] contentor na sua configuração predefinida com esta [New-AzureRmContainerGroup] [ new-azurermcontainergroup]comando:

```azurepowershell-interactive
New-AzureRmContainerGroup `
    -ResourceGroupName myResourceGroup `
    -Name mycontainer1 `
    -Image microsoft/aci-wordcount:latest
```

Agora, execute o seguinte [New-AzureRmContainerGroup] [ new-azurermcontainergroup] comando. Este Especifica o *NumWords* e *MinLength* variáveis de ambiente depois de preencher uma variável de matriz, `envVars`:

```azurepowershell-interactive
$envVars = @{NumWords=5;MinLength=8}
New-AzureRmContainerGroup `
    -ResourceGroupName myResourceGroup `
    -Name mycontainer2 `
    -Image microsoft/aci-wordcount:latest `
    -RestartPolicy OnFailure `
    -EnvironmentVariable $envVars
```

Depois do estado dos ambos os contentores é *Terminated* (utilizar [Get-AzureRmContainerInstanceLog] [ azure-instance-log] para verificar o estado), extraia os seus registos com o [ Get-AzureRmContainerInstanceLog] [ azure-instance-log] comando.

```azurepowershell-interactive
Get-AzureRmContainerInstanceLog -ResourceGroupName myResourceGroup -ContainerGroupName mycontainer1
Get-AzureRmContainerInstanceLog -ResourceGroupName myResourceGroup -ContainerGroupName mycontainer2
```

O resultado para cada contentor mostra como tiver modificado o script a executar ao contentor definindo as variáveis de ambiente.

```console
PS Azure:\> Get-AzureRmContainerInstanceLog -ResourceGroupName myResourceGroup -ContainerGroupName mycontainer1
[('the', 990),
 ('and', 702),
 ('of', 628),
 ('to', 610),
 ('I', 544),
 ('you', 495),
 ('a', 453),
 ('my', 441),
 ('in', 399),
 ('HAMLET', 386)]

Azure:\
PS Azure:\> Get-AzureRmContainerInstanceLog -ResourceGroupName myResourceGroup -ContainerGroupName mycontainer2
[('CLAUDIUS', 120),
 ('POLONIUS', 113),
 ('GERTRUDE', 82),
 ('ROSENCRANTZ', 69),
 ('GUILDENSTERN', 54)]

Azure:\
```

## <a name="azure-portal-example"></a>Exemplo de portal do Azure

Para definir as variáveis de ambiente quando inicia um contentor no portal do Azure, especifique-os no **configuração** página quando criar o contentor.

Quando implementar com o portal, está limitado a três variáveis e tem de introduzi-las neste formato: `"variableName":"value"`

Para ver um exemplo, inicie o [aci/microsoft-wordcount] [ aci-wordcount] contentor com o *NumWords* e *MinLength* variáveis.

1. No **configuração**, defina o **reiniciar política** para *em caso de falha*
2. Introduza `"NumWords":"5"` para a variável primeiro, selecione **Sim** em **adicionar variáveis de ambiente adicionais**e introduza `"MinLength":"8"` para a variável segundo. Selecione **OK** para verificar e, em seguida, implementa o contentor.

![Página do portal que mostra o ambiente variável caixas de texto e no botão de ativação][portal-env-vars-01]

Para ver registos do contentor, em **definições** selecione **contentores**, em seguida, **registos**. É semelhante à saída mostrado na CLI anterior e PowerShell secções, que pode ver o comportamento do script foi modificado pelas variáveis de ambiente. São apresentadas apenas cinco palavras, cada um com um comprimento mínimo de oito carateres.

![Saída de registo de contentor do portal que mostra][portal-env-vars-02]

## <a name="next-steps"></a>Passos Seguintes

Cenários e baseado em tarefas, tais como um grande conjunto de dados com vários contentores, de processamento em lote podem beneficiar de variáveis de ambiente personalizadas no tempo de execução. Para obter mais informações sobre como executar contentores baseado em tarefas, consulte [executar tarefas de em instâncias de contentor do Azure](container-instances-restart-policy.md).

<!-- IMAGES -->
[portal-env-vars-01]: ./media/container-instances-environment-variables/portal-env-vars-01.png
[portal-env-vars-02]: ./media/container-instances-environment-variables/portal-env-vars-02.png

<!-- LINKS - External -->
[aci-wordcount]: https://hub.docker.com/r/microsoft/aci-wordcount/

<!-- LINKS Internal -->
[az-container-create]: /cli/azure/container#az-container-create
[az-container-logs]: /cli/azure/container#az-container-logs
[az-container-show]: /cli/azure/container#az-container-show
[azure-cli-install]: /cli/azure/
[azure-instance-log]: /powershell/module/azurerm.containerinstance/get-azurermcontainerinstancelog
[azure-powershell-install]: /powershell/azure/install-azurerm-ps
[new-azurermcontainergroup]: /powershell/module/azurerm.containerinstance/new-azurermcontainergroup
[portal]: https://portal.azure.com