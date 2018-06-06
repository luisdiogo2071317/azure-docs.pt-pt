---
title: Resolução de problemas de instâncias de contentor do Azure
description: Saiba como resolver problemas com instâncias de contentor do Azure
services: container-instances
author: seanmck
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 03/14/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 39c43c079ea4d10686bd656ba2d451ff42aac9f6
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34700235"
---
# <a name="troubleshoot-common-issues-in-azure-container-instances"></a>Resolver problemas comuns em instâncias de contentor do Azure

Este artigo mostra como resolver problemas comuns para gerir ou implementar contentores para instâncias de contentor do Azure.

## <a name="naming-conventions"></a>Convenções de nomenclatura

Ao definir a especificação de contentor, determinados parâmetros requerem aderência às restrições de nomenclatura. Segue-se uma tabela com requisitos específicos para o contentor de propriedades do grupo.
Para obter mais informações sobre as convenções de nomenclatura do Azure, consulte [convenções de nomenclatura](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions#naming-rules-and-restrictions) no Centro de arquitetura do Azure.

| Âmbito | Comprimento | Maiúsculas e Minúsculas | Carateres válidos | Padrão de sugerida | Exemplo |
| --- | --- | --- | --- | --- | --- | --- |
| Nome do grupo de contentor | 1-64 |Não sensível a maiúsculas e minúsculas |Alfanumérico e hífen em qualquer lugar, exceto o primeiro nem último caráter |`<name>-<role>-CG<number>` |`web-batch-CG1` |
| Nome do contentor | 1-64 |Não sensível a maiúsculas e minúsculas |Alfanumérico e hífen em qualquer lugar, exceto o primeiro nem último caráter |`<name>-<role>-CG<number>` |`web-batch-CG1` |
| Portas de contentor | Entre 1 e 65535 |Número inteiro |Número inteiro entre 1 e 65535 |`<port-number>` |`443` |
| Etiqueta de nome DNS | 5-63 |Não sensível a maiúsculas e minúsculas |Alfanumérico e hífen em qualquer lugar, exceto o primeiro nem último caráter |`<name>` |`frontend-site1` |
| Variável de ambiente | 1-63 |Não sensível a maiúsculas e minúsculas |Alfanumérico e o chracter '_' em qualquer lugar, exceto o primeiro nem último caráter |`<name>` |`MY_VARIABLE` |
| Nome do volume | 5-63 |Não sensível a maiúsculas e minúsculas |Letras minúsculas, números e hífenes em qualquer lugar, exceto o primeiro nem último caráter. Não pode conter dois hífenes consecutivos. |`<name>` |`batch-output-volume` |

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

As imagens do Windows tem [considerações adicionais](#cached-windows-images).

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

### <a name="cached-windows-images"></a>Imagens do Windows em cache

Instâncias de contentor do Azure utiliza um mecanismo de colocação em cache para ajudar a acelerar o tempo de arranque contentor para imagens com base em determinados imagens do Windows.

Para garantir que o tempo de arranque de contentor mais rápido do Windows, utilize um do **três mais recente** versões dos seguintes **duas imagens** como a imagem de base:

* [Windows Server 2016] [ docker-hub-windows-core] (LTS apenas)
* [Servidor do Windows Server 2016 Nano][docker-hub-windows-nano]

### <a name="windows-containers-slow-network-readiness"></a>Preparação de rede lenta de contentores do Windows

Contentores do Windows podem implicar sem conectividade de entrada ou saída de até 5 segundos criação inicial. Após a configuração inicial das redes de contentor devem retomar adequadamente.

## <a name="resource-not-available-error"></a>Recurso erro não está disponível

Devido a vários recursos regional carregar no Azure, poderá receber o erro seguinte ao tentar implementar uma instância do contentor:

`The requested resource with 'x' CPU and 'y.z' GB memory is not available in the location 'example region' at this moment. Please retry with a different resource request or in another location.`

Este erro indica que, devido a sobrecarga na região na qual está a tentar implementar, não não possível alocar os recursos especificados para o contentor nessa altura. Utilize um ou mais dos seguintes passos de mitigação para ajudar a resolver o problema.

* Certifique-se de que as definições de implementação do contentor coincidir com parâmetros definidos na [Quotas e disponibilidade de região para instâncias de contentor do Azure](container-instances-quotas.md#region-availability)
* Especificar definições de CPU e memória inferiores para o contentor
* Implementar noutra região do Azure
* Implementar numa altura posterior

## <a name="next-steps"></a>Passos Seguintes
Saiba como [obter registos do contentor & eventos](container-instances-get-logs.md) para ajudar a depurar os contentores.

<!-- LINKS - External -->
[docker-multi-stage-builds]: https://docs.docker.com/engine/userguide/eng-image/multistage-build/
[docker-hub-windows-core]: https://hub.docker.com/r/microsoft/windowsservercore/
[docker-hub-windows-nano]: https://hub.docker.com/r/microsoft/nanoserver/

<!-- LINKS - Internal -->
[az-container-show]: /cli/azure/container#az_container_show