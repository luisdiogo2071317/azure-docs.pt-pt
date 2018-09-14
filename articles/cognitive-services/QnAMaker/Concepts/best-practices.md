---
title: Melhores práticas - QnA Maker
titlesuffix: Azure Cognitive Services
description: Utilize estas melhores práticas para melhorar a sua base de dados de conhecimento e fornecer os melhores resultados para os utilizadores finais do seu bot de aplicação/bate-papo.
services: cognitive-services
author: nstulasi
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: saneppal
ms.openlocfilehash: c82c117d149da39fba7b9a243aebb3e127540881
ms.sourcegitcommit: f983187566d165bc8540fdec5650edcc51a6350a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45542936"
---
# <a name="best-practices"></a>Melhores práticas
O [ciclo de vida de desenvolvimento de base de dados de conhecimento](../Concepts/development-lifecycle-knowledge-base.md) orienta-o sobre como gerir as KB ponto-a-ponto. Utilize estas melhores práticas para melhorar a sua base de dados de conhecimento e fornecer os melhores resultados para os utilizadores finais do seu bot de aplicação/bate-papo.

## <a name="extraction"></a>Extração
A ferramenta QnA Maker é continuamente melhorar os algoritmos que extrair QnAs de conteúdo e expandir a lista de ficheiros e formatos de página HTML suportados. Siga os [diretrizes](../Concepts/data-sources-supported.md) para extração com base no tipo de documento está a extrair do. 

Em geral, as páginas de FAQ devem ser autónomo e não combinada com outras informações. Os manuais de produto devem ter títulos claros e preferencialmente, uma página de índice. 

## <a name="rankingmatching"></a>Classificação/correspondência
Certifique-se de que está fazendo o melhor uso dos recursos de classificação que do QnA Maker suporta. Se o fizer, irá melhorar a probabilidade de que consulta um determinado usuário foi respondida com uma resposta adequada.

### <a name="add-alternate-questions"></a>Adicionar perguntas alternativas
[Alternate perguntas](../How-To/edit-knowledge-base.md) melhorar a probabilidade de uma correspondência com uma consulta de utilizador. Perguntas alternativas são úteis quando existem múltiplas formas em que pode ser solicitada a mesma pergunta. Isto pode incluir alterações na estrutura de sentença (por exemplo, *"É estacionamento disponível?"* versus *"Tem parque de carro?"* ) ou alterações no estilo do word e gíria (por exemplo, *"Hi"* versus *"Yo"*, *"EI lá!"* ).

### <a name="use-metadata-filters"></a>Utilizar filtros de metadados
[Metadados](../How-To/edit-knowledge-base.md) acrescenta a capacidade para restringir os resultados de uma consulta de utilizador com base nos filtros. A resposta da base de dados de conhecimento pode ser diferente consoante a marca de metadados, mesmo que a consulta é o mesmo. Por exemplo, *"onde está localizado de estacionamento"* pode ter uma resposta diferente se a localização do ramo restaurante for diferente, ou seja, os metadados são *localização: Seattle* versus *localização: Redmond*.)

### <a name="use-synonyms"></a>Utilizar sinónimos
Embora haja algum suporte para sinónimos em inglês, utilize [alterações de palavras](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75fd) para adicionar sinónimos para palavras-chave que assumir a forma diferente (exemplo: *comprar* -> *comprar*  ou *netbanking* -> *net banca*. Sinónimos devem ser adicionados ao nível de serviço QnA Maker e partilhados por todas as bases de dados de conhecimento no serviço.

### <a name="use-distinct-words-to-differentiate-questions"></a>Utilizar palavras diferentes para diferenciar perguntas
Algoritmos de correspondência de classificação do QnA Maker que correspondem a uma consulta de utilizador com uma pergunta na base de dados de conhecimento funcionam melhor se cada pergunta endereços necessidades diferentes. Repetição da mesma palavra definido entre perguntas reduz a probabilidade de que a resposta certa é escolhida para uma consulta de determinado usuário com essas palavras.

## <a name="collaborate"></a>Colaboração
A ferramenta QnA Maker permite que os usuários [colaborar](../How-to/collaborate-knowledge-base.md) numa base de dados de conhecimento. Os utilizadores necessitam de acesso para o grupo de recursos do Azure QnA Maker para acessar as bases de dados de conhecimento. Algumas organizações poderão querer terceirizar a edição de base de dados de conhecimento e a manutenção e continuará a poder proteger o acesso aos seus recursos do Azure. Esse modelo de aprovador de editor pode ser feito ao configurar dois idêntico [serviços do QnA Maker](../How-to/set-up-qnamaker-service-azure.md) em subscrições diferentes e designar um para o ciclo de teste de edição. Assim que o teste estiver concluído, o conteúdo de base de dados pode ser transferido com um [importação-exportação](../Tutorials/migrate-knowledge-base.md) processos para o serviço QnA Maker do aprovador que finalmente publicar a base de dados de conhecimento e atualizar o ponto final.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Editar uma base de dados de conhecimento](../How-to/edit-knowledge-base.md)

