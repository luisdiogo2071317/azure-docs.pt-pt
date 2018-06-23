---
title: Escolher a capacidade para a implementação de QnA Maker - serviços cognitivos Microsoft | Microsoft Docs
titleSuffix: Azure
description: um guia para escolher a capacidade para a implementação de QnA Maker
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/07/2018
ms.author: saneppal
ms.openlocfilehash: b0219b9f7dbbee52406dab9d808134fa2e2a689d
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354062"
---
# <a name="choosing-capacity-for-your-qna-maker-deployment"></a>Escolher a capacidade para a implementação de QnA Maker

O serviço de QnA Maker, demora uma dependência em três recursos do Azure:
1.  Serviço de aplicações (para o tempo de execução)
2.  Pesquisa do Azure (para armazenar QnAs)
3.  Informações de aplicação (opcionais, para armazenar a telemetria e chatlogs)

Antes de criar o serviço de QnA Maker, deve decidir quais as camadas de serviços acima é adequada para si. 

Normalmente, existem três parâmetros, que terá de considerar:
1. **O débito precisa do serviço**: selecione apropriados [plano do App](https://azure.microsoft.com/en-in/pricing/details/app-service/plans/) para o serviço de aplicações com base nas suas necessidades. Pode [aumentar verticalmente](https://docs.microsoft.com/en-us/azure/app-service/web-sites-scale) ou reduzir verticalmente a aplicação. Isto deve também influenciar a seleção de SKU de pesquisa do Azure, consulte os detalhes mais [aqui](https://docs.microsoft.com/en-us/azure/search/search-sku-tier).

2. **Tamanho e o número de bases de dados de conhecimento**: escolha apropriados [SKU de pesquisa do Azure](https://azure.microsoft.com/en-in/pricing/details/search/) para o seu cenário. Pode publicar bases de dados de conhecimento N-1 numa camada específica, em que N é os índices máximos permitidos na camada. Consulte também o tamanho máximo e o número de documentos permitido por camada.

3. **Número de documentos como origens**: O SKU do serviço de gestão Maker de QnA livre limita o número de documentos que pode gerir através do portal e as APIs para 3 (de 1 MB tamanho cada). O padrão de SKU tem não limites para o número de documentos que pode gerir. Ver mais detalhes [aqui](https://aka.ms/qnamaker-pricing).

A tabela seguinte fornece algumas diretrizes de alto nível.

|                        | Gestão de QnA Maker | Serviço de Aplicações | Azure Search | Limitações                      |
| ---------------------- | -------------------- | ----------- | ------------ | -------------------------------- |
| Experimentação        | SKU livre             | Escalão Gratuito   | Escalão Gratuito    | Publicar até 2 KBs, tamanho de 50 MB  |
| Ambiente de desenvolvimento/teste   | SKU Standard         | Partilhado      | Básica        | Publicar até 4 KBs, tamanho de 2GB    |
| Ambiente de produção | SKU Standard         | Básica       | Standard     | Publicar até 49 KBs, tamanho de 25 GB |

Para atualizar a pilha de QnA Maker, consulte [Atualize o serviço de QnA Maker](../How-To/upgrade-qnamaker-service.md).

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Atualize o serviço de QnA Maker](../How-To/upgrade-qnamaker-service.md)
