---
title: "Resolução de problemas de instâncias de contentor do Azure"
description: "Saiba como resolver problemas com instâncias de contentor do Azure"
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: article
ms.date: 01/02/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 1fd3b2c251860e883519744b11fcfc2b925cd2fa
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/05/2018
---
# <a name="troubleshoot-deployment-issues-with-azure-container-instances"></a>Resolver problemas de implementação com instâncias de contentor do Azure

Este artigo mostra como resolver problemas quando implementar contentores para instâncias de contentor do Azure. Também descreve alguns dos problemas comuns que poderá ter.

## <a name="get-diagnostic-events"></a>Obter eventos de diagnóstico

Para ver registos a partir do código da aplicação num contentor, pode utilizar o [az contentor registos] [ az-container-logs] comando. Mas se o contentor não implementar com êxito, terá de rever as informações de diagnóstico fornecidas pelo fornecedor de recursos de instâncias de contentor do Azure. Para ver os eventos para o contentor, execute o [mostrar de contentor az] [ az-container-show] comando:

```azurecli-interactive
az container show --resource-group myResourceGroup --name mycontainer
```

O resultado inclui as propriedades de núcleo do seu contentor, juntamente com os eventos de implementação (mostrados aqui truncada):

```JSON
{
  "containers": [
    {
      "command": null,
      "environmentVariables": [],
      "image": "microsoft/aci-helloworld",
      ...
        "events": [
          {
            "count": 1,
            "firstTimestamp": "2017-12-21T22:50:49+00:00",
            "lastTimestamp": "2017-12-21T22:50:49+00:00",
            "message": "pulling image \"microsoft/aci-helloworld\"",
            "name": "Pulling",
            "type": "Normal"
          },
          {
            "count": 1,
            "firstTimestamp": "2017-12-21T22:50:59+00:00",
            "lastTimestamp": "2017-12-21T22:50:59+00:00",
            "message": "Successfully pulled image \"microsoft/aci-helloworld\"",
            "name": "Pulled",
            "type": "Normal"
          },
          {
            "count": 1,
            "firstTimestamp": "2017-12-21T22:50:59+00:00",
            "lastTimestamp": "2017-12-21T22:50:59+00:00",
            "message": "Created container with id 2677c7fd54478e5adf6f07e48fb71357d9d18bccebd4a91486113da7b863f91f",
            "name": "Created",
            "type": "Normal"
          },
          {
            "count": 1,
            "firstTimestamp": "2017-12-21T22:50:59+00:00",
            "lastTimestamp": "2017-12-21T22:50:59+00:00",
            "message": "Started container with id 2677c7fd54478e5adf6f07e48fb71357d9d18bccebd4a91486113da7b863f91f",
            "name": "Started",
            "type": "Normal"
          }
        ],
        "previousState": null,
        "restartCount": 0
      },
      "name": "mycontainer",
      "ports": [
        {
          "port": 80,
          "protocol": null
        }
      ],
      ...
    }
  ],
  ...
}
```

## <a name="common-deployment-issues"></a>Problemas comuns de implementação

Existem alguns problemas comuns essa conta para a maioria dos erros na implementação.

## <a name="unable-to-pull-image"></a>Não é possível a imagem de solicitação

Se as instâncias de contentor do Azure não é possível solicitar a sua imagem inicialmente, repete as tentativas durante um determinado período antes de falhar, eventualmente. Se a imagem não pode ser solicitada, eventos semelhante ao seguinte são apresentados na saída do [mostrar de contentor az][az-container-show]:

```bash
"events": [
  {
    "count": 3,
    "firstTimestamp": "2017-12-21T22:56:19+00:00",
    "lastTimestamp": "2017-12-21T22:57:00+00:00",
    "message": "pulling image \"microsoft/aci-hellowrld\"",
    "name": "Pulling",
    "type": "Normal"
  },
  {
    "count": 3,
    "firstTimestamp": "2017-12-21T22:56:19+00:00",
    "lastTimestamp": "2017-12-21T22:57:00+00:00",
    "message": "Failed to pull image \"microsoft/aci-hellowrld\": rpc error: code 2 desc Error: image t/aci-hellowrld:latest not found",
    "name": "Failed",
    "type": "Warning"
  },
  {
    "count": 3,
    "firstTimestamp": "2017-12-21T22:56:20+00:00",
    "lastTimestamp": "2017-12-21T22:57:16+00:00",
    "message": "Back-off pulling image \"microsoft/aci-hellowrld\"",
    "name": "BackOff",
    "type": "Normal"
  }
],
```

Para resolver, eliminar o contentor e repita a implementação, pagar especial atenção se introduziu o nome da imagem corretamente.

## <a name="container-continually-exits-and-restarts"></a>Contentor continuamente sai e reinicia

Se o contentor é executada para conclusão e reinicia automaticamente, poderá ter de definir um [reiniciar política](container-instances-restart-policy.md) de **OnFailure** ou **nunca**. Se especificar **OnFailure** e consulte ainda expedição é reiniciado, podem existir um problema com a aplicação ou script executado no seu contentor.

A API de instâncias de contentor inclui um `restartCount` propriedade. Para verificar o número de reinícios para um contentor, pode utilizar o [mostrar de contentor az] [ az-container-show] comando no 2.0 de CLI do Azure. No seguinte exemplo de saída (que foi truncado de uma forma abreviada), pode ver o `restartCount` propriedade no final de saída.

```json
...
 "events": [
   {
     "count": 1,
     "firstTimestamp": "2017-11-13T21:20:06+00:00",
     "lastTimestamp": "2017-11-13T21:20:06+00:00",
     "message": "Pulling: pulling image \"myregistry.azurecr.io/aci-tutorial-app:v1\"",
     "type": "Normal"
   },
   {
     "count": 1,
     "firstTimestamp": "2017-11-13T21:20:14+00:00",
     "lastTimestamp": "2017-11-13T21:20:14+00:00",
     "message": "Pulled: Successfully pulled image \"myregistry.azurecr.io/aci-tutorial-app:v1\"",
     "type": "Normal"
   },
   {
     "count": 1,
     "firstTimestamp": "2017-11-13T21:20:14+00:00",
     "lastTimestamp": "2017-11-13T21:20:14+00:00",
     "message": "Created: Created container with id bf25a6ac73a925687cafcec792c9e3723b0776f683d8d1402b20cc9fb5f66a10",
     "type": "Normal"
   },
   {
     "count": 1,
     "firstTimestamp": "2017-11-13T21:20:14+00:00",
     "lastTimestamp": "2017-11-13T21:20:14+00:00",
     "message": "Started: Started container with id bf25a6ac73a925687cafcec792c9e3723b0776f683d8d1402b20cc9fb5f66a10",
     "type": "Normal"
   }
 ],
 "previousState": null,
 "restartCount": 0
...
}
```

> [!NOTE]
> A maioria das imagens de contentor para as distribuições do Linux definir uma shell, tais como bash, como o comando de predefinição. Uma vez que uma shell no seu próprio não é um serviço de execução longa, estes imediatamente sair e contentores enquadram-se um ciclo de reinício quando configurado com a predefinição **sempre** reiniciar política.

## <a name="container-takes-a-long-time-to-start"></a>Contentor demora muito tempo a iniciar

Se o seu contentor demora muito tempo a iniciar, eventualmente, mas for bem sucedida, comece por observar o tamanho da imagem do contentor. Porque as instâncias de contentor do Azure obtém a imagem do contentor a pedido, o tempo de arranque ocorrer está diretamente relacionada com o tamanho.

Pode ver o tamanho da imagem do contentor utilizando a CLI do Docker:

```bash
docker images
```

Saída:

```bash
REPOSITORY                             TAG                 IMAGE ID            CREATED             SIZE
microsoft/aci-helloworld               latest              7f78509b568e        13 days ago         68.1MB
```

A chave para manter o tamanho de imagem pequeno é garantir que a imagem final não contém tudo o que não é necessária no tempo de execução. Uma forma para fazer isto é com [fase multi compilações][docker-multi-stage-builds]. Fase multi baseia-se disponibilizar fácil de garantir que a imagem final contém apenas os artefactos que é necessário para a sua aplicação, e não quaisquer o extra de conteúdos que era necessário no momento da compilação.

É a forma como para reduzir o impacto da solicitação de imagem no tempo de arranque do contentor para alojar a imagem de contentor com o registo de contentor do Azure na mesma região em que pretende utilizar instâncias de contentor do Azure. Isto reduz o caminho de rede que a imagem do contentor tem de viajam, encurtar significativamente o tempo de transferência.

## <a name="resource-not-available-error"></a>Recurso erro não está disponível

Devido a vários recursos regional carregar no Azure, poderá receber o erro seguinte ao tentar implementar uma instância do contentor:

`The requested resource with 'x' CPU and 'y.z' GB memory is not available in the location 'example region' at this moment. Please retry with a different resource request or in another location.`

Este erro indica que, devido a sobrecarga na região na qual está a tentar implementar, não não possível alocar os recursos especificados para o contentor nessa altura. Utilize um ou mais dos seguintes passos de mitigação para ajudar a resolver o problema.

* Certifique-se de que as definições de implementação do contentor coincidir com parâmetros definidos na [Quotas e disponibilidade de região para instâncias de contentor do Azure](container-instances-quotas.md#region-availability)
* Especificar definições de CPU e memória inferiores para o contentor
* Implementar noutra região do Azure
* Implementar numa altura posterior

<!-- LINKS - External -->
[docker-multi-stage-builds]: https://docs.docker.com/engine/userguide/eng-image/multistage-build/

<!-- LINKS - Internal -->
[az-container-logs]: /cli/azure/container#az_container_logs
[az-container-show]: /cli/azure/container#az_container_show