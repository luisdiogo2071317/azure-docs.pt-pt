---
title: Executar tarefas em contentores no Azure Container Instances com as políticas de reinício
description: Saiba como utilizar o Azure Container Instances para executar tarefas que são executadas até a conclusão, tal como na compilação, teste ou trabalhos de composição de imagem.
services: container-instances
author: mmacy
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 07/26/2018
ms.author: marsma
ms.openlocfilehash: dd411ff38411c71cce2a8a63cc453c34e665a385
ms.sourcegitcommit: a5eb246d79a462519775a9705ebf562f0444e4ec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/26/2018
ms.locfileid: "39262740"
---
# <a name="run-containerized-tasks-with-restart-policies"></a>Executar tarefas em contentores com políticas de reinício

A facilidade e velocidade de implementação de contentores no Azure Container Instances fornece uma plataforma atraente para executar tarefas de execução única, como a compilação, teste e composição de imagem numa instância de contentor.

Com uma política de reinício configuráveis, pode especificar que os contentores são parados quando seus processos forem concluídas. Uma vez que as instâncias de contentor são faturadas ao segundo, é-lhe cobrado apenas pelos recursos de computação utilizados enquanto o contentor a executar a tarefa está em execução.

Os exemplos apresentados neste artigo uso a CLI do Azure. Tem de ter a CLI do Azure da versão 2.0.21 ou superior [instalado localmente][azure-cli-install], ou utilizar a CLI no [Azure Cloud Shell](../cloud-shell/overview.md).

## <a name="container-restart-policy"></a>Política de reinício do contentor

Quando cria um contentor no Azure Container Instances, pode especificar uma das três definições de política de reinício.

| Política de reinício   | Descrição |
| ---------------- | :---------- |
| `Always` | Contentores no grupo de contentores são sempre reiniciados. Este é o **predefinição** definição aplicada quando nenhuma política de reinício é especificada durante a criação do contentor. |
| `Never` | Contentores no grupo de contentores nunca são reiniciadas. Os contentores executam no máximo uma vez. |
| `OnFailure` | Contentores no grupo de contentores são reiniciadas apenas quando o processo executado no contentor de falha (quando termina com um código de saída diferente de zero). Os contentores são executados, pelo menos, uma vez. |

## <a name="specify-a-restart-policy"></a>Especifique uma política de reinício

Como especificar uma política de reinício depende de como criar as instâncias de contentor, tal como com a CLI do Azure, cmdlets do PowerShell do Azure, ou no portal do Azure. Na CLI do Azure, especifique a `--restart-policy` parâmetro, quando chama [criar contentor de az][az-container-create].

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer \
    --image mycontainerimage \
    --restart-policy OnFailure
```

## <a name="run-to-completion-example"></a>Executar o exemplo de conclusão

Para ver a política de reinício em ação, crie uma instância de contentor a partir do [aci/microsoft-wordcount] [ aci-wordcount-image] de imagem e especificar o `OnFailure` política de reinício. Este contentor de exemplo executa um script de Python que, por predefinição, analisa o texto do de Shakespeare [Mítico](http://shakespeare.mit.edu/hamlet/full.html), escreve as 10 palavras mais comuns para STDOUT e, em seguida, sai.

Execute o contentor de exemplo com o seguinte procedimento [criar contentor de az] [ az-container-create] comando:

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure
```

O Azure Container Instances inicia o contentor e, em seguida, pára-lo quando seu aplicativo (ou script, neste caso) é fechado. Quando o Azure Container Instances para um contentor de é cuja política de reinício `Never` ou `OnFailure`, estado do contentor é definido como **Terminated**. Pode verificar o estado de um contentor com o [show de contentor az] [ az-container-show] comando:

```azurecli-interactive
az container show --resource-group myResourceGroup --name mycontainer --query containers[0].instanceView.currentState.state
```

Exemplo de saída:

```bash
"Terminated"
```

Assim que o estado do contentor de exemplo mostra *Terminated*, pode ver o resultado da tarefa, visualizando os registos de contentor. Executar o [registos de contentor az] [ az-container-logs] o resultado de comando para ver o script:

```azurecli-interactive
az container logs --resource-group myResourceGroup --name mycontainer
```

Saída:

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

Este exemplo mostra a saída que o script enviado para STDOUT. As tarefas em contentores, no entanto, poderão em vez disso, escrever o resultado no armazenamento persistente para recuperação posterior. Por exemplo, para um [partilha de ficheiros do Azure](container-instances-mounting-azure-files-volume.md).

## <a name="configure-containers-at-runtime"></a>Configurar os contentores em tempo de execução

Quando cria uma instância de contentor, pode definir seus **variáveis de ambiente**, bem como especificar um personalizado **linha de comandos** para ser executado quando o contentor é iniciado. Pode utilizar estas definições nas suas tarefas de lote para preparar cada contentor com a configuração de tarefa específica.

## <a name="environment-variables"></a>Variáveis de ambiente

Definir variáveis de ambiente no seu contentor para fornecer a configuração dinâmica da aplicação ou o contentor de execução do script. Isso é semelhante a `--env` argumento da linha de comandos para `docker run`.

Por exemplo, pode modificar o comportamento do script no contentor de exemplo ao especificar as seguintes variáveis de ambiente, quando cria a instância de contentor:

*NumWords*: O número de palavras enviado para STDOUT.

*MinLength*: O número mínimo de carateres de uma palavra para a mesma a contar. Um número mais alto ignora palavras comuns, como "de" e "a".

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer2 \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure \
    --environment-variables NumWords=5 MinLength=8
```

Ao especificar `NumWords=5` e `MinLength=8` das variáveis de ambiente do contentor, os registos de contentor devem apresentar um resultado diferente. Assim que o estado do contentor é apresentado como *Terminated* (utilizar `az container show` para verificar o estado), apresentar os respetivos registos para ver a nova saída:

```azurecli-interactive
az container logs --resource-group myResourceGroup --name mycontainer2
```

Saída:

```bash
[('CLAUDIUS', 120),
 ('POLONIUS', 113),
 ('GERTRUDE', 82),
 ('ROSENCRANTZ', 69),
 ('GUILDENSTERN', 54)]
```

## <a name="command-line-override"></a>Substituição de linha de comandos

Especifique uma linha de comandos quando cria uma instância de contentor para substituir a linha de comandos embutida na imagem de contentor. Isso é semelhante a `--entrypoint` argumento da linha de comandos para `docker run`.

Por exemplo, pode ter o contentor de exemplo, analisar o texto que *Mítico* ao especificar uma linha de comandos diferente. O script de Python executado pelo contêiner, *wordcount.py*, aceita um URL como um argumento e irão processar o conteúdo dessa página em vez da predefinição.

Por exemplo, para determinar as palavras de cinco letras 3 principais na *Romeu e Julieta*:

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer3 \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure \
    --environment-variables NumWords=3 MinLength=5 \
    --command-line "python wordcount.py http://shakespeare.mit.edu/romeo_juliet/full.html"
```

Novamente, assim que o contentor estiver *Terminated*, ver o resultado, mostrando os registos do contentor:

```azurecli-interactive
az container logs --resource-group myResourceGroup --name mycontainer3
```

Saída:

```bash
[('ROMEO', 177), ('JULIET', 134), ('CAPULET', 119)]
```

## <a name="next-steps"></a>Passos Seguintes

### <a name="persist-task-output"></a>Manter a saída de tarefa

Para obter detalhes sobre como manter a saída de seus contentores que são executados até a conclusão, consulte [montar uma partilha de ficheiros do Azure com o Azure Container Instances](container-instances-mounting-azure-files-volume.md).

<!-- LINKS - External -->
[aci-wordcount-image]: https://hub.docker.com/r/microsoft/aci-wordcount/

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container?view=azure-cli-latest#az_container_create
[az-container-logs]: /cli/azure/container?view=azure-cli-latest#az_container_logs
[az-container-show]: /cli/azure/container?view=azure-cli-latest#az_container_show
[azure-cli-install]: /cli/azure/install-azure-cli
