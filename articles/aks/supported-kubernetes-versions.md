---
title: Versões suportadas do Kubernetes no Azure Kubernetes Service
description: Compreender a política de suporte de versão do Kubernetes e o ciclo de vida de clusters no Azure Kubernetes Service (AKS)
services: container-service
author: sauryadas
ms.service: container-service
ms.topic: article
ms.date: 09/21/2018
ms.author: saudas
ms.openlocfilehash: 6b55825107ae8872b146b3ad4fde0ef4b917b71d
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47047994"
---
# <a name="supported-kubernetes-versions-in-azure-kubernetes-service-aks"></a>Versões suportadas do Kubernetes no Azure Kubernetes Service (AKS)

Comunidade do Kubernetes libera a versões secundárias aproximadamente a cada três meses. Estas versões incluem novas funcionalidades e melhorias. Versões de patch (por vezes, semanalmente) são mais freqüentes e destinam-se apenas de correções de erros críticas numa versão secundária. Estas versões de patch incluem correções para vulnerabilidades de segurança ou bugs principais afetar um grande número de clientes e produtos em execução na produção com base no Kubernetes.

Kubernetes nova versão secundária é disponibilizada no [motor de acs] [ acs-engine] no primeiro dia. Os destinos de objetivo de nível de serviço (SLO) do AKS a lançar a versão secundária para clusters do AKS dentro de 30 dias, sujeita a estabilidade da versão.

## <a name="kubernetes-version-support-policy"></a>Política de suporte de versão do Kubernetes

AKS oferece suporte a quatro versões secundárias do Kubernetes:

- A atual versão secundária que é lançado a montante (n)
- Três versões secundárias anteriores. Cada versão secundária suportado também oferece suporte a dois patches estáveis.

Por exemplo, se apresenta o AKS *1.11.x* hoje em dia, também é fornecido suporte para *1.10.a* + *1.10.b*, *1.9.c*  +  *1!d 1,9*, *1.8.e* + *1.8F* (em que as versões de patch letras são duas compilações estáveis mais recente).

Quando uma nova versão secundária é introduzida, as mais antigas pequenas versão e patch em versões suportadas são descontinuadas. 15 dias antes do lançamento da nova versão secundária e extinção de versão futura, um anúncio é feito por meio dos canais de atualização do Azure. No exemplo acima onde *1.11.x* é lançado, as versões retiradas são *1.7.g* + *1.7.h*.

Quando implementa um cluster do AKS no portal ou com a CLI do Azure, o cluster está sempre definido como a versão secundária n-1 e o patch mais recente. Por exemplo, se suportar do AKS *1.11.x*, *1.10.a* + *1.10.b*, *1.9.c* + *1!d 1.9* , *1.8.e* + *1.8F*, é a versão predefinida para novos clusters *1.10.b*.

## <a name="faq"></a>FAQ

**O que acontece quando um cliente é atualizado um cluster do Kubernetes com uma versão secundária que não é suportada?**

Se estiver a utilizar o *n-4* versão, está fora do SLO. Se a atualização da versão n-4 para n-3 for bem-sucedida, em seguida, está novamente no SLO. Por exemplo:

- Se forem as versões suportadas do AKS *1.10.a* + *1.10.b*, *1.9.c* + *1!d 1,9*,  *1.8.e* + *1.8F* e estiver a utilizar *1.7.g* ou *1.7.h*, está fora do SLO.
- Se a atualização a partir de *1.7.g* ou *1.7.h* para *1.8.e* ou *1.8.f* tiver êxito, está novamente no SLO.

As atualizações para versões mais antigas que *n-4* não são suportadas. Nesses casos, recomendamos aos clientes criar novos clusters do AKS e voltar a implementar as cargas de trabalho.

**O que acontece quando um cliente dimensiona um cluster de Kubernetes com uma versão secundária que não é suportada?**

Para as versões secundárias não suportadas pelo AKS, dimensionamento e reduzir continua a funcionar sem problemas.

**Um cliente manter-numa versão do Kubernetes para sempre?**

Sim. No entanto, se o cluster não estiver em uma das versões suportadas pelo AKS, o cluster está fora do SLO o AKS. Azure automaticamente não atualizar o seu cluster ou eliminá-lo.

**Qual versão é que o suporte principal se o cluster de agente não está em uma das versões suportadas do AKS?**

O mestre é atualizado automaticamente para a versão mais recente suportada.

## <a name="next-steps"></a>Passos Seguintes

Para obter informações sobre como atualizar o seu cluster, consulte [atualizar um cluster do Azure Kubernetes Service (AKS)][aks-upgrade].

<!-- LINKS - External -->
[acs-engine]: https://github.com/Azure/acs-engine

<!-- LINKS - Internal -->
[aks-upgrade]: upgrade-cluster.md