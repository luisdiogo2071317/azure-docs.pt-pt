---
title: "Melhores práticas no Azure Container Registry"
description: "Saiba como utilizar o registo de contentor do Azure de forma eficiente, ao seguir estas melhores práticas."
services: container-registry
author: mmacy
manager: timlt
ms.service: container-registry
ms.topic: quickstart
ms.date: 11/05/2017
ms.author: marsma
ms.openlocfilehash: 5ccbb3022dc38f13eed9b5aa24beb14dfdb3b5b6
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/05/2017
---
# <a name="best-practices-for-azure-container-registry"></a>Melhores práticas do Azure Container Registry

Ao seguir estas melhores práticas, pode ajudar a maximizar o desempenho e a utilização rentável do seu registo privado do Docker no Azure.

## <a name="network-close-deployment"></a>Implementação sem rede

Crie o registo de contentor na mesma região do Azure em que implementa contentores. Colocar o seu registo numa região sem rede para os anfitriões do seu contentor pode ajudar a reduzir a latência e o custo.

A implementação sem rede é uma das principais razões para utilizar um registo de contentor privado. As imagens do Docker têm uma ótima [construção em camadas](https://docs.docker.com/engine/userguide/storagedriver/imagesandcontainers/) que permite implementações incrementais. No entanto, os novos nós têm de solicitar todas as camadas necessárias para uma determinada imagem. Este `docker pull` inicial pode adicionar rapidamente até vários gigabytes. Ter um registo privado próximo à sua implementação minimiza a latência de rede.
Além disso, todas as clouds públicas, o Azure incluído, implementam as taxas de saída da rede. Extrair imagens de um datacenter para outro, adiciona taxas de saída da rede além da latência.

## <a name="geo-replicate-multi-region-deployments"></a>Georreplicar implementações em várias regiões

Utilize a funcionalidade de [georreplicação](container-registry-geo-replication.md) do Azure Container Registry, se estiver a implementar contentores em várias regiões. Se estiver a servir clientes globais de datacenters locais ou a sua equipa de desenvolvimento estiver em diferentes localizações, pode simplificar a gestão dos registos e minimizar a latência através da georreplicação do seu registo. Atualmente em pré-visualização, esta funcionalidade está disponível nos registos [Premium](container-registry-skus.md#premium).

Para saber como utilizar a georreplicação, veja o tutorial de três partes [Geo-replication in Azure Container Registry (Georreplicação no Azure Container Registry)](container-registry-tutorial-prepare-registry.md).

## <a name="repository-namespaces"></a>Espaços de nomes do repositório

Ao tirar partido dos espaços de nomes do repositório, pode permitir a partilha de um único registo em vários grupos na sua organização. Os registos podem ser partilhados em implementações e equipas. O Azure Container Registry suporta espaços de nomes aninhados, ao ativar o isolamento de grupo.

Por exemplo, considere as seguintes etiquetas da imagem de contentor. As imagens que são utilizadas a nível empresarial, como `aspnetcore`, são colocadas no espaço de nomes de raiz, enquanto as imagens de contentor pertencentes a grupos de Produção e Marketing utilizam os seus próprios espaços de nomes.

```
contoso.azurecr.io/aspnetcore:2.0
contoso.azurecr.io/products/widget/web:1
contoso.azurecr.io/products/bettermousetrap/refundapi:12.3
contoso.azurecr.io/marketing/2017-fall/concertpromotions/campaign:218.42
```

## <a name="dedicated-resource-group"></a>Grupo de recursos dedicado

Dado que os registos do contentor são recursos que são utilizados em vários anfitriões do contentor, um registo deve residir o seu próprio grupo de recursos.

Embora possa experimentar um tipo de anfitrião específico, como o Azure Container Instances, irá provavelmente eliminar a instância do contentor quando tiver terminado. No entanto, também pode manter a coleção de imagens enviadas para o Azure Container Registry. Ao colocar o seu registo no seu próprio grupo de recursos, está a minimizar o risco de eliminar acidentalmente a coleção de imagens no registo, ao eliminar o grupo de recursos de instância do contentor.

## <a name="authentication"></a>Autenticação

Ao autenticar com um registo de contentor do Azure, existem dois cenários principais: autenticação individual e autenticação de serviço (ou "sem interface"). A tabela seguinte apresenta uma breve descrição geral destes cenários e o método de autenticação recomendado para cada um.

| Tipo | Cenário de exemplo | Método recomendado |
|---|---|---|
| Identidade individual | Um programador a extrair imagens ou enviar imagens a partir da respetiva máquina de desenvolvimento. | [az acr login](/cli/azure/acr?view=azure-cli-latest#az_acr_login) |
| Identidade de serviço/sem interface | Compile e implemente pipelines onde o utilizador não esteja diretamente envolvido. | [Principal de serviço](container-registry-authentication.md#service-principal) |

Para obter informações aprofundadas sobre a autenticação do Azure Container Registry, veja [Authenticate with an Azure container registry (Autenticar com um registo de contentor do Azure)](container-registry-authentication.md).

## <a name="next-steps"></a>Passos seguintes

O Azure Container Registry está disponível em vários escalões, denominadas SKUs, e cada uma dispõe de funcionalidades diferentes. Para obter detalhes sobre as SKUs disponíveis, veja [Azure Container Registry SKUs (SKUs do Azure Container Registry)](container-registry-skus.md).