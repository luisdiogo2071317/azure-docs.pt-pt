---
title: Capacidade de recursos para a implementação - QnA Maker
titleSuffix: Azure Cognitive Services
description: um guia para escolher a capacidade para a sua implementação do QnA Maker
services: cognitive-services
author: nstulasi
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: nstulasi
ms.openlocfilehash: e2c9239ccd42e2464c85172be0e91492bd8f6718
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/17/2018
ms.locfileid: "45736789"
---
# <a name="choosing-capacity-for-your-qna-maker-deployment"></a>Escolher a capacidade para a sua implementação do QnA Maker

O serviço QnA Maker, utiliza uma dependência em três recursos do Azure:
1.  Serviço de aplicações (para o tempo de execução)
2.  O Azure Search (para armazenar QnAs)
3.  Informações da aplicação (opcionais, para armazenar registos de chat e telemetria)

Antes de criar o serviço QnA Maker, deve decidir qual dos escalões de serviços acima é adequada para si. 

Normalmente, existem três parâmetros, que precisa considerar:
1. **O débito precisa do serviço**: selecione o adequado [plano de aplicação](https://azure.microsoft.com/en-in/pricing/details/app-service/plans/) para o serviço de aplicações com base nas suas necessidades. Pode [aumentar verticalmente](https://docs.microsoft.com/azure/app-service/web-sites-scale) ou reduzir verticalmente a aplicação. Isso deve também influenciar a seleção de SKU de pesquisa do Azure, veja mais detalhes [aqui](https://docs.microsoft.com/azure/search/search-sku-tier).

2. **Tamanho e o número de bases de dados de conhecimento**: escolha apropriado [SKU de pesquisa do Azure](https://azure.microsoft.com/en-in/pricing/details/search/) para o seu cenário. Pode publicar o bases de dados de conhecimento de n-1 numa camada específica, onde N é os índices máximos permitidos na camada. Também pode verificar o tamanho máximo e o número de documentos permitidas por camada.

3. **Número de documentos como fontes**: O SKU gratuito do serviço de gestão do QnA Maker limita o número de documentos, pode gerenciar através do portal e as APIs para 3 (de 1 MB de tamanho de cada). O padrão de SKU não tem limites ao número de documentos, que pode gerir. Ver mais detalhes [aqui](https://aka.ms/qnamaker-pricing).

A tabela seguinte fornece algumas diretrizes de alto nível.

|                        | Gestão do QnA Maker | Serviço de Aplicações | Azure Search | Limitações                      |
| ---------------------- | -------------------- | ----------- | ------------ | -------------------------------- |
| Experimentação        | SKU gratuito             | Escalão Gratuito   | Escalão Gratuito    | Publicar até 2 KBs, tamanho de 50 MB  |
| Ambiente de desenvolvimento/teste   | SKU Standard         | Partilhado      | Básica        | Publicar até 4 KBs, tamanho de 2 GB    |
| Ambiente de produção | SKU Standard         | Básica       | Standard     | Publicar até 49 KBs, 25 GB de tamanho |

Para atualizar a sua pilha de QnA Maker, consulte [atualizar o serviço QnA Maker](../How-To/upgrade-qnamaker-service.md).

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Atualizar o serviço QnA Maker](../How-To/upgrade-qnamaker-service.md)
