---
title: Definir variáveis de ambiente em instâncias de contentor do Azure
description: Saiba como definir variáveis de ambiente de instâncias de contentor do Azure
services: container-instances
author: david-stanford
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 03/13/2018
ms.author: dastanfo
ms.openlocfilehash: 37fde41b6dc2ea0a4d3b4b38a0e3df81a297c125
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="set-environment-variables"></a>Definir variáveis de ambiente

A definição de variáveis de ambiente nas instâncias de contentor permite-lhe fornecer a configuração dinâmica da aplicação ou script execute ao contentor.

Está atualmente consegue definir variáveis de ambiente do CLI e PowerShell. Em ambos os casos, utilizaria um sinalizador de comandos para definir as variáveis de ambiente. A definição de variáveis de ambiente no ACI é semelhante para o `--env` argumento da linha de comandos para `docker run`. Por exemplo, se utilizar a imagem de aci/microsoft-wordcount contentor pode modificar o comportamento ao especificar as seguintes variáveis de ambiente:

*NumWords*: O número de palavras enviado para STDOUT.

*MinLength*: O número mínimo de carateres de uma palavra-lo a contar. Um número mais alto ignora palavras comuns, como "de" e "a".

## <a name="azure-cli-example"></a>Exemplo CLI do Azure

Para ver a saída predefinida do contentor, execute o seguinte comando:

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer1 \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure
```

Ao especificar `NumWords=5` e `MinLength=8` nas variáveis de ambiente do contentor, os registos do contentor devem apresentar um resultado diferente.

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer2 \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure \
    --environment-variables NumWords=5 MinLength=8
```

Assim que mostra o estado do contentor como *Terminated* (utilizar [mostrar de contentor az] [ az-container-show] para verificar o estado do mesmo), apresentar os seus registos para ver o resultado.  Para ver o resultado do contentor com nenhum conjunto de variáveis de ambiente – nome ser mycontainer1 em vez de mycontainer2.

```azurecli-interactive
az container logs --resource-group myResourceGroup --name mycontainer2
```

## <a name="azure-powershell-example"></a>Exemplo do PowerShell do Azure

Para ver a saída predefinida do contentor, execute o seguinte comando:

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer1 \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure
```

Ao especificar `NumWords=5` e `MinLength=8` nas variáveis de ambiente do contentor, os registos do contentor devem apresentar um resultado diferente.

```azurepowershell-interactive
$envVars = @{NumWord=5;MinLength=8}
New-AzureRmContainerGroup `
    -ResourceGroupName myResourceGroup `
    -Name mycontainer2 `
    -Image microsoft/aci-wordcount:latest `
    -RestartPolicy OnFailure `
    -EnvironmentVariable $envVars
```

Depois do Estado do contentor é *Terminated* (utilizar [Get-AzureRmContainerInstanceLog] [ azure-instance-log] para verificar o estado do mesmo), apresentar os seus registos para ver o resultado. Para ver o contentor nos registos com nenhuma variável de ambiente definidas ContainerGroupName ser mycontainer1 em vez de mycontainer2.

```azurepowershell-interactive
Get-AzureRmContainerInstanceLog `
    -ResourceGroupName myResourceGroup `
    -ContainerGroupName mycontainer2
```

## <a name="example-output-without-environment-variables"></a>Exemplo de saída sem variáveis de ambiente

```bash
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
```

## <a name="example-output-with-environment-variables"></a>Saída de exemplo com variáveis de ambiente

```bash
[('CLAUDIUS', 120),
 ('POLONIUS', 113),
 ('GERTRUDE', 82),
 ('ROSENCRANTZ', 69),
 ('GUILDENSTERN', 54)]
```

## <a name="next-steps"></a>Passos Seguintes

Agora que sabe como personalizar a entrada para o contentor, saiba como manter o resultado de contentores serem concluídas.
> [!div class="nextstepaction"]
> [Montar uma partilha de ficheiros do Azure com instâncias de contentor do Azure](container-instances-mounting-azure-files-volume.md)

<!-- LINKS Internal -->
[azure-cloud-shell]: ../cloud-shell/overview.md
[azure-cli-install]: /cli/azure/
[azure-powershell-install]: /powershell/azure/install-azurerm-ps
[azure-instance-log]: /powershell/module/azurerm.containerinstance/get-azurermcontainerinstancelog
[az-container-show]: /cli/azure/container?view=azure-cli-latest#az_container_show