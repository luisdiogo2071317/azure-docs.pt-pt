---
title: Serviços cognitivos do Azure - Maker de QnA - de melhores práticas | Microsoft Docs
description: Utilize estas melhores práticas para melhorar a sua base de dados de conhecimento e fornecer os melhores resultados para os utilizadores finais do seu bot de aplicação/chat.
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/07/2018
ms.author: saneppal
ms.openlocfilehash: 7a85ebbc3892a90e98e73a73425c1f8ec1de0b35
ms.sourcegitcommit: 680964b75f7fff2f0517b7a0d43e01a9ee3da445
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "35355927"
---
# <a name="best-practices"></a>Melhores práticas
O [ciclo de vida de desenvolvimento de base de dados de conhecimento](../Concepts/development-lifecycle-knowledge-base.md) orienta-o sobre como gerir a KB ponto-a-ponto. Utilize estas melhores práticas para melhorar a sua base de dados de conhecimento e fornecer os melhores resultados para os utilizadores finais do seu bot de aplicação/chat.

## <a name="extraction"></a>Extração
Maker de QnA é continuamente melhorar os algoritmos que extrair QnAs do conteúdo e expandir a lista de ficheiros e formatos de página HTML suportados. Siga o [diretrizes](../Concepts/data-sources-supported.md) para extração com base no tipo de documento sua tem extrair do. 

Em geral, as páginas de FAQ devem ser autónomo e não combinado com outras informações. Como produto deve ter cabeçalhos encriptados e, de preferência, uma página de índice. 

## <a name="rankingmatching"></a>Classificação/correspondente
Certifique-se que estiver a efetuar a melhor utilização das funcionalidades de classificação que Maker de QnA suporta. Se o fizer, irá melhorar a probabilidade que uma consulta de utilizador especificado é respondida com uma resposta adequada.

### <a name="add-alternate-questions"></a>Adicionar perguntas alternativas
[Alternate perguntas](../How-To/edit-knowledge-base.md) melhorar a probabilidade de uma correspondência com uma consulta do utilizador. Perguntas alternativas são úteis quando existem várias formas em que a mesma pergunta poderá ser-lhe pedida. Isto pode incluir as alterações na estrutura de frases (por exemplo, *"É parking disponível?"* versus *"Tem carro park?"* ) ou alterações de palavra-estilo e slang (por exemplo, *"Hi"* versus *"Yo"*, *"Existe Hey!"* ).

### <a name="use-metadata-filters"></a>Utilizar filtros de metadados
[Metadados](../How-To/edit-knowledge-base.md) adiciona a capacidade para restringir os resultados de uma consulta de utilizador com base em filtros. A resposta de base de dados de conhecimento pode divergir com base na metadata tag, mesmo se a consulta é o mesmo. Por exemplo, *"onde está parking localizado"* pode ter uma resposta diferente se a localização do ramo restaurante diferentes - ou seja, os metadados são *localização: Seattle* versus *localização: Redmond*.)

### <a name="use-synonyms"></a>Utilizar sinónimos
Enquanto existir algumas suporte para sinónimos no idioma do inglês, utilize [word expansões](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75fd) adicionar sinónimos para palavras-chave que tomar forma diferentes (exemplo: *comprar* -> *compra*  ou *netbanking* -> *net banca*. Sinónimos devem ser adicionados, o nível de serviço de QnA Maker e partilhados por todas as bases de dados de conhecimento no serviço.

### <a name="use-distinct-words-to-differentiate-questions"></a>Utilize palavras para diferenciar perguntas
Algoritmos de correspondência de classificação de Maker de QnA que correspondem a uma consulta de utilizador com uma pergunta na base de dados de conhecimento funcionam melhor se cada pergunta endereços necessidades diferentes. Repetição da mesma word definido entre perguntas reduz a probabilidade de que a resposta à direita é escolhida para uma consulta de determinado utilizador com as palavras.

## <a name="collaborate"></a>Colaboração
Maker de QnA permite aos utilizadores [colaborar](../How-to/collaborate-knowledge-base.md) uma base de dados de conhecimento. Os utilizadores necessitam de acesso para o grupo de recursos do Azure Maker de QnA para poder aceder as bases de dados de conhecimento. Algumas organizações poderão querer outsource a edição da base de dados de conhecimento e a manutenção e continuará a poder proteger o acesso aos respetivos recursos do Azure. Este modelo de editor aprovador pode ser conseguido através de configurar dois idênticos [serviços de QnA Maker](../How-to/set-up-qnamaker-service-azure.md) em diferentes subscrições e designar um para o ciclo de testes de edição. Quando o teste estiver concluído, o conteúdo da base de dados de conhecimento pode ser transferido com um [importar exportar](../Tutorials/migrate-knowledge-base.md) processo ao serviço Maker de QnA do aprovador que irá publicar a base de dados de conhecimento, finalmente e atualizar o ponto final.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Editar uma base de dados de conhecimento](../How-to/edit-knowledge-base.md)

