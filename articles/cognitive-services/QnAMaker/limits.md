---
title: Limites de QnA Maker - serviços cognitivos do Azure | Microsoft Docs
description: Limites de Maker de QnA
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/07/2018
ms.author: saneppal
ms.openlocfilehash: 4d2bafad08ffab76743cb802733a5d2f01a97f06
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "35356031"
---
# <a name="qna-maker-limits"></a>Limites de Maker de QnA
Lista completa dos limites através de QnA Maker.

## <a name="knowledge-bases"></a>Bases de dados de conhecimento

* Número máximo de bases de dados de conhecimento com base no [limites da camada de pesquisa do Azure](https://docs.microsoft.com/en-us/azure/search/search-limits-quotas-capacity)

|**Camada de pesquisa do Azure** | **Livre** | **Básica** |**S1** | **S2**| **S3** |**S3 HD**|
|---|---|---|---|---|---|----|
|Número máximo de bases de dados de conhecimento publicados permitidos (máx. os índices - 1 (reservado para teste)|2|14|49|199|199|2999|

## <a name="extraction-limits"></a>Limites de extração
* Número máximo de ficheiros que podem ser extraídos e o tamanho máximo do ficheiro: consulte [QnAMaker de preços](https://azure.microsoft.com/en-in/pricing/details/cognitive-services/qna-maker/)
* Número máximo de ligações avançadas que podem ser pesquisados para extração de QnAs de páginas HTML de FAQ: 20

## <a name="metadata-limits"></a>Limites de metadados
* Número máximo de campos de metadados por base de dados de conhecimento, com base no [limites da camada de pesquisa do Azure](https://docs.microsoft.com/en-us/azure/search/search-limits-quotas-capacity)

|**Camada de pesquisa do Azure** | **Livre** | **Básica** |**S1** | **S2**| **S3** |**S3 HD**|
|---|---|---|---|---|---|----|
|Campos de metadados máximo por serviço de QnA Maker (em todos os KBs)|1000|100 *|1000|1000|1000|1000|

## <a name="knowledge-base-content-limits"></a>Limites de conteúdo de Base de dados de conhecimento
Limites gerais sobre o conteúdo da base de dados de conhecimento:
* Comprimento do texto de resposta: 250000
* Comprimento do texto da pergunta: 1000
* Comprimento do texto de chave/valor de metadados: 100
* Carateres suportados para o nome de metadados: europeus, dígitos e _  
* Carateres suportados para o valor de metadados: tudo, exceto: e | 
* Comprimento do nome de ficheiro: 200
* Formatos de ficheiro suportados: ".tsv", ". pdf", "txt", ". docx", "ou".
* Número máximo de perguntas alternativos: 100
* Número máximo de pares de resposta da pergunta: depende o [camada da Azure Search](https://docs.microsoft.com/en-in/azure/search/search-limits-quotas-capacity#document-limits) escolhido 

## <a name="create-knowledge-base-call-limits"></a>Crie limites de chamada de base de dados de conhecimento:
Estes representam os limites para cada criar base de dados de conhecimento ação; ou seja, clicando em *criar KB* ou chamar a API de CreateKnowledgeBase.
* Número máximo de perguntas alternativos por resposta: 100
* Número máximo de URLs: 10
* Número máximo de ficheiros: 10

## <a name="update-knowledge-base-call-limits"></a>Atualizar limites de chamada de base de dados de conhecimento
Estes representam os limites para cada ação de atualização; ou seja, clicando em *guardar e formação* ou chamar a API de UpdateKnowledgeBase.
* Comprimento de cada nome de origem: 300
* Número máximo de perguntas alternativa perguntas adicionado ou eliminado: 100
* Número máximo de campos de metadados adicionado ou eliminado: 10
* Número máximo de URLs que podem ser atualizadas: 5
