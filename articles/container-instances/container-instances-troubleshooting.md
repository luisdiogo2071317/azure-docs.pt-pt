---
title: "Resolução de problemas de instâncias de contentor do Azure"
description: "Saiba como resolver problemas com instâncias de contentor do Azure"
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: article
ms.date: 03/14/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: a527939d6bc73e3dee5701bc53ef8312e68d2953
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/16/2018
---
# <a name="troubleshoot-deployment-issues-with-azure-container-instances"></a>Resolver problemas de implementação com instâncias de contentor do Azure

Este artigo mostra como resolver problemas quando implementar contentores para instâncias de contentor do Azure. Também descreve alguns dos problemas comuns que poderá ter.

## <a name="view-logs-and-stream-output"></a>Ver registos e a saída de fluxo

Quando tiver um contentor funcionar incorretamente, comece por visualizar os seus registos com [az contentor registos][az-container-logs]e a saída padrão e o erro padrão com transmissão em fluxo [contentor az anexar] [az-container-attach].

### <a name="view-logs"></a>Ver registos

Para ver registos a partir do código da aplicação num contentor, pode utilizar o [az contentor registos] [ az-container-logs] comando.

Segue-se a saída de registo do contentor de baseado em tarefas de exemplo do [executar uma tarefa de ACI](container-instances-restart-policy.md), depois de ter sejam fornecidas-processar um URL inválido:

```console
$ az container logs --resource-group myResourceGroup --name mycontainer
Traceback (most recent call last):
  File "wordcount.py", line 11, in <module>
    urllib.request.urlretrieve (sys.argv[1], "foo.txt")
  File "/usr/local/lib/python3.6/urllib/request.py", line 248, in urlretrieve
    with contextlib.closing(urlopen(url, data)) as fp:
  File "/usr/local/lib/python3.6/urllib/request.py", line 223, in urlopen
    return opener.open(url, data, timeout)
  File "/usr/local/lib/python3.6/urllib/request.py", line 532, in open
    response = meth(req, response)
  File "/usr/local/lib/python3.6/urllib/request.py", line 642, in http_response
    'http', request, response, code, msg, hdrs)
  File "/usr/local/lib/python3.6/urllib/request.py", line 570, in error
    return self._call_chain(*args)
  File "/usr/local/lib/python3.6/urllib/request.py", line 504, in _call_chain
    result = func(*args)
  File "/usr/local/lib/python3.6/urllib/request.py", line 650, in http_error_default
    raise HTTPError(req.full_url, code, msg, hdrs, fp)
urllib.error.HTTPError: HTTP Error 404: Not Found
```

### <a name="attach-output-streams"></a>Anexar fluxos de saída

O [contentor az anexar] [ az-container-attach] comando fornece informações de diagnóstico durante o arranque do contentor. Depois de ter iniciado o contentor, fluxos STDOUT e STDERR à consola local.

Por exemplo, o resultado é de contentor baseado em tarefas no [executar uma tarefa de ACI](container-instances-restart-policy.md), depois de ter de indicar um URL válido de um ficheiro de texto grandes para processar:

```console
$ az container attach --resource-group myResourceGroup --name mycontainer
Container 'mycontainer' is in state 'Unknown'...
Container 'mycontainer' is in state 'Waiting'...
Container 'mycontainer' is in state 'Running'...
(count: 1) (last timestamp: 2018-03-09 23:21:33+00:00) pulling image "microsoft/aci-wordcount:latest"
(count: 1) (last timestamp: 2018-03-09 23:21:49+00:00) Successfully pulled image "microsoft/aci-wordcount:latest"
(count: 1) (last timestamp: 2018-03-09 23:21:49+00:00) Created container with id e495ad3e411f0570e1fd37c1e73b0e0962f185aa8a7c982ebd410ad63d238618
(count: 1) (last timestamp: 2018-03-09 23:21:49+00:00) Started container with id e495ad3e411f0570e1fd37c1e73b0e0962f185aa8a7c982ebd410ad63d238618

Start streaming logs:
[('the', 22979),
 ('I', 20003),
 ('and', 18373),
 ('to', 15651),
 ('of', 15558),
 ('a', 12500),
 ('you', 11818),
 ('my', 10651),
 ('in', 9707),
 ('is', 8195)]
```

## <a name="get-diagnostic-events"></a>Obter eventos de diagnóstico

Se o contentor não conseguir implementar com êxito, terá de rever as informações de diagnóstico fornecidas pelo fornecedor de recursos de instâncias de contentor do Azure. Para ver os eventos para o contentor, execute o [mostrar de contentor az] [ az-container-show] comando:

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

As secções seguintes descrevem problemas comuns que essa conta para a maioria dos erros na implementação do contentor:

* [Versão da imagem não suportada](#image-version-not-supported)
* [Não é possível a imagem de solicitação](#unable-to-pull-image)
* [Contentor continuamente sai e reinicia](#container-continually-exits-and-restarts)
* [Contentor demora muito tempo a iniciar](#container-takes-a-long-time-to-start)
* [Erro de "Recurso não está disponível"](#resource-not-available-error)

## <a name="image-version-not-supported"></a>Versão da imagem não suportada

Se especificar uma imagem que não suportem a instâncias de contentor do Azure, uma `ImageVersionNotSupported` é devolvido o erro. O valor do erro é `The version of image '{0}' is not supported.`e aplica-se atualmente para imagens de 1709 do Windows. Para atenuar este problema, utilize uma imagem do LTS Windows. Suporte para imagens do Windows 1709 está em curso.

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

Os dois fatores primários que contribuem para o tempo de arranque de contentor em instâncias de contentor do Azure são:

* [Tamanho da imagem](#image-size)
* [Localização da imagem](#image-location)

As imagens do Windows tem [considerações adicionais](#use-recent-windows-images).

### <a name="image-size"></a>Tamanho da imagem

Se o seu contentor demora muito tempo a iniciar, eventualmente, mas for bem sucedida, comece por observar o tamanho da imagem do contentor. Porque as instâncias de contentor do Azure obtém a imagem do contentor a pedido, o tempo de arranque ocorrer está diretamente relacionada com o tamanho.

Pode ver o tamanho da imagem do contentor utilizando o `docker images` da CLI do Docker:

```console
$ docker images
REPOSITORY                  TAG       IMAGE ID        CREATED        SIZE
microsoft/aci-helloworld    latest    7f78509b568e    13 days ago    68.1MB
```

A chave para manter o tamanho de imagem pequeno é garantir que a imagem final não contém tudo o que não é necessária no tempo de execução. Uma forma para fazer isto é com [fase multi compilações][docker-multi-stage-builds]. Fase multi baseia-se disponibilizar fácil de garantir que a imagem final contém apenas os artefactos que é necessário para a sua aplicação, e não quaisquer o extra de conteúdos que era necessário no momento da compilação.

### <a name="image-location"></a>Localização da imagem

Outra forma de reduzir o impacto da solicitação de imagem no tempo de arranque do contentor é para alojar a imagem do contentor no [registo de contentor do Azure](/azure/container-registry/) na mesma região onde pretende implementar instâncias de contentor. Isto reduz o caminho de rede que a imagem do contentor tem de viajam, encurtar significativamente o tempo de transferência.

### <a name="use-recent-windows-images"></a>Utilizar imagens recentes do Windows

Instâncias de contentor do Azure utiliza um mecanismo de colocação em cache para ajudar a acelerar o tempo de arranque contentor para imagens com base em determinados imagens do Windows.

Para garantir que o tempo de arranque de contentor mais rápido do Windows, utilize um do **três mais recente** versões dos seguintes **duas imagens** como a imagem de base:

* [Windows Server 2016] [ docker-hub-windows-core] (LTS apenas)
* [Windows Server 2016 Nano Server][docker-hub-windows-nano]

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
[docker-hub-windows-core]: https://hub.docker.com/r/microsoft/windowsservercore/
[docker-hub-windows-nano]: https://hub.docker.com/r/microsoft/nanoserver/

<!-- LINKS - Internal -->
[az-container-attach]: /cli/azure/container#az_container_attach
[az-container-logs]: /cli/azure/container#az_container_logs
[az-container-show]: /cli/azure/container#az_container_show