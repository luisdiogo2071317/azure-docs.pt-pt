---
title: Limites e limites - QnA Maker
titleSuffix: Azure Cognitive Services
description: Lista completa dos limites entre a ferramenta QnA Maker.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: tulasim
ms.openlocfilehash: efdcf57fb0f175be51e6b9f8e72e4a02fcbeb5f3
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/25/2018
ms.locfileid: "47162326"
---
# <a name="qna-maker-limits"></a>Limites do QnA Maker
Lista completa dos limites entre a ferramenta QnA Maker.

## <a name="knowledge-bases"></a>Bases de dados de conhecimento

* Número máximo de bases de dados de conhecimento com base em [limites do escalão de Azure Search](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity)

|**Escalão de pesquisa do Azure** | **Gratuito** | **Básica** |**S1** | **S2**| **S3** |**S3 HD**|
|---|---|---|---|---|---|----|
|Número máximo de bases de dados de conhecimento publicados permitido (máx. de índices - 1 (reservado para teste)|2|14|49|199|199|2999|

## <a name="extraction-limits"></a>Limites de extração
* Número máximo de ficheiros que podem ser extraídos e o tamanho máximo do ficheiro: veja [preços QnAMaker](https://azure.microsoft.com/en-in/pricing/details/cognitive-services/qna-maker/)
* Número máximo de ligações profunda que podem ser pesquisados para extração de QnAs de páginas HTML de FAQ: 20

## <a name="metadata-limits"></a>Limites de metadados
* Número máximo de campos de metadados por base de dados de conhecimento, com base em [limites do escalão de Azure Search](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity)

|**Escalão de pesquisa do Azure** | **Gratuito** | **Básica** |**S1** | **S2**| **S3** |**S3 HD**|
|---|---|---|---|---|---|----|
|Campos de metadados máximo por serviço QnA Maker (em todos os KBs)|1000|100 *|1000|1000|1000|1000|

## <a name="knowledge-base-content-limits"></a>Limites de conteúdo de Base de dados de conhecimento
Limites geral para o conteúdo na base de dados de conhecimento:
* Comprimento do texto de resposta: 250000
* Comprimento do texto da pergunta: 1000
* Comprimento do texto de chave/valor de metadados: 100
* Carateres suportados pelo nome de metadados: letras do alfabeto, dígitos e _  
* Carateres suportados pelo valor de metadados: todos, exceto: e | 
* Comprimento do nome de ficheiro: 200
* Formatos de ficheiro suportados: ". tsv", ". pdf", ". txt", ". docx", ". xlsx".
* Número máximo de perguntas alternativos: 100
* Número máximo de pares de perguntas respostas: depende da [escalão do Azure Search](https://docs.microsoft.com/en-in/azure/search/search-limits-quotas-capacity#document-limits) escolhido 

## <a name="create-knowledge-base-call-limits"></a>Crie limites de chamada de base de dados de conhecimento:
Estes representam os limites para cada criar ação de base de dados; ou seja, clicar *KB criar* ou chamar a API de CreateKnowledgeBase.
* Número máximo de perguntas alternativos por resposta: 100
* Número máximo de URLs: 10
* Número máximo de ficheiros: 10

## <a name="update-knowledge-base-call-limits"></a>Atualizar limites de chamada de base de dados de conhecimento
Estes representam os limites para cada ação de atualização; ou seja, clicar *guardar e treinar* ou chamar a API de UpdateKnowledgeBase.
* Comprimento de cada nome de origem: 300
* Número máximo de perguntas alternativos adicionados ou excluídos: 100
* Número máximo de campos de metadados adicionados ou excluídos: 10
* Número máximo de URLs que podem ser atualizados: 5
