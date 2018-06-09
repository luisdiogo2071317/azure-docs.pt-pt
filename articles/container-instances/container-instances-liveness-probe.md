---
title: Configurar as pesquisas liveness em instâncias de contentor do Azure
description: Saiba como configurar as pesquisas de liveness reiniciar contentores mau estado de funcionamento em instâncias de contentor do Azure
services: container-instances
author: jluk
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 06/08/2018
ms.author: juluk
ms.openlocfilehash: 76ca4db28d99702532ae656a19f0d54b479a13fe
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/08/2018
ms.locfileid: "35249383"
---
# <a name="configure-liveness-probes"></a>Configurar as pesquisas liveness

Aplicações de poderão ser executadas por períodos prolongados de tempo, resultando em Estados quebrados que poderão ter de ser reparados reiniciando o contentor. Instâncias de contentor do Azure suporta liveness sondas para incluir as configurações para que o contentor pode reiniciar se funcionalidade crítico não está a funcionar. 

Este artigo explica como implementar um grupo de contentor que inclui uma sonda liveness, que demonstra o reinício automático de um contentor de mau estado de funcionamento simulado.

## <a name="yaml-deployment"></a>Implementação de YAML

Criar um `liveness-probe.yaml` ficheiro pelo seguinte fragmento. Este ficheiro define um grupo de contentor que é composta por um contentor NGNIX acaba por fica danificado. 

```yaml
apiVersion: 2018-06-01
location: eastus
name: livenesstest
properties:
  containers:
  - name: mycontainer
    properties:
      image: nginx
      command:
        - "/bin/sh"
        - "-c"
        - "touch /tmp/healthy; sleep 30; rm -rf /tmp/healthy; sleep 600"
      ports: []
      resources:
        requests:
          cpu: 1.0
          memoryInGB: 1.5
      livenessProbe:
        exec:
            command: 
                - "cat"
                - "/tmp/healthy"
        periodSeconds: 5
  osType: Linux
  restartPolicy: Never
tags: null
type: Microsoft.ContainerInstance/containerGroups
```

Execute o seguinte comando para implementar este grupo de contentor com a configuração de YAML acima:

```azurecli-interactive
az container create --resource-group myResourceGroup --name livenesstest -f liveness-probe.yaml
```

### <a name="start-command"></a>Comando de início

A implementação define um comando inicial a ser executada quando o contentor é iniciado pela primeira vez em execução, definido pelo `command` propriedade que aceita uma matriz de cadeias. Neste exemplo, irá iniciar uma sessão de bash e crie um ficheiro chamado `healthy` dentro de `/tmp` diretório transferindo este comando:

```bash
/bin/sh -c "touch /tmp/healthy; sleep 30; rm -rf /tmp/healthy; sleep 600"
```

 Será, em seguida, no modo de suspensão durante 30 segundos antes de eliminar o ficheiro, em seguida, suspenso 10 minutos.

### <a name="liveness-command"></a>Comando liveness

Esta implementação define um `livenessProbe` que suporte um `exec` comando liveness que age como a verificação de liveness. Se este comando sai com um valor diferente de zero, o contentor será cancelado e reiniciado, sinalização o `healthy` não foi possível encontrar o ficheiro. Se este comando sai com sucesso com o código de saída 0, será efetuada nenhuma ação.

O `periodSeconds` propriedade designa o liveness comando deve executar a cada cinco segundos.

## <a name="verify-liveness-output"></a>Certifique-se a saída de liveness

Dentro os primeiros 30 segundos, o `healthy` existe ficheiro criado pelo comando de início. Quando o comando liveness verifica a existência de `healthy` existência do ficheiro, o código de estado devolve um zero, sinalização com êxito, pelo que não reiniciar ocorre.

Depois de 30 segundos, o `cat /tmp/healthy` começarão a falhar, fazendo com que os eventos mau estado de funcionamento e o cancelamento para ocorrer. 

Estes eventos podem ser visualizados a partir do portal do Azure ou do Azure CLI 2.0.

![Portal evento mau estado de funcionamento][portal-unhealthy]

Ao visualizar os eventos no portal do Azure, eventos do tipo `Unhealthy` será acionada após a falha do comando liveness. O evento subsequente irão ser do tipo `Killing`, que significa a eliminação de um contentor, pelo que pode iniciar um reinício. A contagem de reinício para o contentor irá incrementar sempre que isto ocorre.

Reinicia é concluída no local para recursos, como os endereços IP públicos e conteúdo de nó específicas será mantido.

![Contador de reinício do portal][portal-restart]

Se a pesquisa de liveness continuamente falha e aciona demasiados reinícios, contentor de introduzir um back exponencial desativar o atraso.

## <a name="liveness-probes-and-restart-policies"></a>As pesquisas liveness e políticas de reinício

Políticas de reinício substituem o comportamento de reinício acionado por liveness sondas. Por exemplo, se definir um `restartPolicy = Never` *e* uma sonda liveness, o grupo do contentor não reiniciará em caso de uma verificação de liveness falhada. O grupo de contentor em vez disso, irá cumprir a política de reinício do grupo contentor de `Never`.

## <a name="next-steps"></a>Passos Seguintes

Baseado em tarefas cenários podem exigir uma personalizada de sonda de liveness ativar reinícios automáticos se uma função de pré-requisito não está a funcionar corretamente. Para obter mais informações sobre como executar contentores baseado em tarefas, consulte [executar tarefas de em instâncias de contentor do Azure](container-instances-restart-policy.md).

<!-- IMAGES -->
[portal-unhealthy]: ./media/container-instances-liveness-probe/unhealthy-killing.png
[portal-restart]: ./media/container-instances-liveness-probe/portal-restart.png