---
title: Referência da API de Moderator conteúdo do Azure | Microsoft Docs
description: Saiba mais sobre a moderação de interrupção de conteúdo e reveja as APIs para Moderator de conteúdo.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 06/25/2017
ms.author: sajagtap
ms.openlocfilehash: 94de9b91cc242e8b7e5479cacab8380adac551f6
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351565"
---
# <a name="content-moderator-api-reference"></a>Referência da API do Content Moderator

Começar com Azure conteúdo Moderator APIs das seguintes formas: (Consulte também [gerir credenciais](review-tool-user-guide/credentials.md).)

- No portal do Azure, [subscrever as APIs de Moderator conteúdo](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesContentModerator).
- Inscrever-se a [ferramenta conteúdo Moderator revisão](http://contentmoderator.cognitive.microsoft.com/). Consulte [início rápido](quick-start.md).

## <a name="moderation-apis"></a>APIs de Moderação

Pode utilizar as APIs de Moderator conteúdo seguinte para configurar os fluxos de trabalho de pós-moderação de interrupção.

| Descrição | Referência |
| -------------------- |-------------|
| **Imagem moderação de interrupção API**<br /><br />Análise de imagens e detetar potencial conteúdo para adultos e racy utilizando etiquetas, pontuações de confiança e outras informações extraídas. <br /><br />Utilize estas informações para publicar, rejeitar ou analisar o conteúdo no seu fluxo de trabalho de pós-moderação de interrupção. <br /><br />| [Imagem de referência da API de moderação interrupção](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66c "referência da API de moderação interrupção de imagem")   |
| **Texto moderação de interrupção API**<br /><br />Análise de conteúdo de texto. Termos de Profanity e informações de identificação pessoal (PII) são devolvidos. <br /><br />Utilize estas informações para publicar, rejeitar ou analisar o conteúdo no seu fluxo de trabalho de pós-moderação de interrupção.<br /><br /> | [Referência da API de moderação interrupção de texto](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66f "referência da API de moderação interrupção de texto")   |
| **Moderação de interrupção gráfica API**<br /><br />Análise de vídeos e detetar potencial conteúdo para adultos e racy. <br /><br />Utilize estas informações para publicar, rejeitar ou analisar o conteúdo no seu fluxo de trabalho de pós-moderação de interrupção.<br /><br /> | [Descrição geral de API de moderação interrupção gráfica](video-moderation-api.md "descrição geral da API de moderação interrupção de vídeo")   |
| **API de gestão da lista**<br /><br />Criar e gerir listas de exclusão ou inclusão personalizadas de imagens e texto. Se estiver ativada, o **imagem - corresponder** e **texto - ecrã** operações fazer a correspondência por semelhantes do conteúdo submetido contra as listas personalizadas. <br /><br />Para uma eficiência, pode ignorar o passo de moderação de interrupção com base na aprendizagem máquina.<br /><br /> | [Referência da API de gestão de lista](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f675 "referência da API de gestão da lista")   |

## <a name="review-api"></a>API de revisão

A API de revisão tem os seguintes componentes:

| Descrição | Referência |
| -------------------- |-------------|
| **Tarefas**<br /><br /> Inicie os fluxos de trabalho de moderação de interrupção de análise e revisão para o conteúdo de texto e imagem. Uma tarefa de moderação interrupção analisa o conteúdo utilizando a API de moderação interrupção de imagem e a API de moderação interrupção de texto. Tarefas de moderação interrupção utilizam o definido e predefinido fluxos de trabalho para gerar revisões. <br /><br />Depois de um moderator humana tem revisto a etiquetas atribuídos e os dados de predição e submetida uma decisão de moderação de interrupção de conteúdo, a API de revisão submete todas as informações para o ponto final de API.<br /><br /> | [Referência da tarefa](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c5 "referência da tarefa")   |
| **Revisões**<br /><br />Utilize a ferramenta de revisão para criar diretamente revisões de imagem ou de texto para moderators humanos.<br /><br /> Depois de um moderator humana tem revisto a etiquetas atribuídos e os dados de predição e submetida uma decisão de moderação de interrupção de conteúdo, a API de revisão submete todas as informações para o ponto final de API.<br /><br /> | [Reveja a referência](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c4 "rever referência")   |
| **Fluxos de trabalho**<br /><br />Criar, atualizar e obter detalhes sobre os fluxos de trabalho personalizados que cria a sua equipa. Definir fluxos de trabalho utilizando a ferramenta de revisão. <br /> <br />Fluxos de trabalho, normalmente, utilizam Moderator conteúdo, mas também podem utilizar determinadas APIs que estão disponíveis como conectores na ferramenta de revisão.<br /><br /> | [Referência de fluxo de trabalho](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59 "referência de fluxo de trabalho")   |


