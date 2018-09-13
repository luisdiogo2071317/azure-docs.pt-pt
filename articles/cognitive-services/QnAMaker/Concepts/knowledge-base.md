---
title: Base de dados de conhecimento - serviços cognitivos da Microsoft | Documentos da Microsoft
titleSuffix: Azure
description: Sobre uma base de dados de conhecimento
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/07/2018
ms.author: saneppal
ms.openlocfilehash: 5dfb96f454bf5ce3f030ebfc6a97482fcfc9469b
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/12/2018
ms.locfileid: "35760269"
---
# <a name="knowledge-base"></a>Base de dados de conhecimento

Uma base de dados de conhecimento do QnA Maker é composta por um conjunto de pares de pergunta/resposta (FAQ) e metadados opcionais associados a cada par de QnA.

## <a name="key-knowledge-base-concepts"></a>Conceitos chave base de dados de conhecimento

* **Perguntas** -uma pergunta contém o texto que melhor representa uma consulta de utilizador. 
* **Respostas** -uma resposta é a resposta retornada quando uma consulta de utilizador é correspondida com a pergunta associada.  
* **Metadados** -metadados são as marcas associadas com um par de QnA e são representados como pares chave-valor. Metadados são utilizados para filtrar os pares de QnA e limitar o conjunto ao longo do que consulta correspondente é executada.

Um único QnA, representada por um ID numérico do QnA, tem várias variantes de uma pergunta (perguntas alternativas) que todos mapeiam para uma única resposta. Além disso, cada par de tal pode ter vários campos de metadados associados à mesma.

![Bases de dados de conhecimento do QnA Maker](../media/qnamaker-concepts-knowledgebase/knowledgebase.png) 

## <a name="knowledge-base-content-format"></a>Formato de conteúdo de base de dados de conhecimento

Quando ingerir conteúdo avançado para uma base de dados de conhecimento, QnA Maker tenta converter o conteúdo em markdown. Leia [isso](https://aka.ms/qnamaker-docs-markdown-support) blogue para compreender o markdown formatos reconhecidos pela maioria dos clientes de bate-papo.

Campos de metadados são compostas por pares chave-valor, separados por vírgula **(produto: Shredder)**. Chave e valor tem de ser só de texto. A chave de metadados não pode conter quaisquer espaços.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Ciclo de vida do desenvolvimento de uma base de dados de conhecimento](./development-lifecycle-knowledge-base.md)

## <a name="see-also"></a>Consulte também

[Descrição geral do QnA Maker](../Overview/overview.md)