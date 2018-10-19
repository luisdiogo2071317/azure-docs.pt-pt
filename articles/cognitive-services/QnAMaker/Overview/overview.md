---
title: O que é o Criador de FAQ?
titleSuffix: Azure Cognitive Services
description: A ferramenta Criador de FAQ é um serviço de API com base na cloud que aplica a inteligência de aprendizagem automática personalizada à pergunta no idioma natural do utilizador para fornecer a melhor resposta.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: overview
ms.date: 10/09/2018
ms.author: tulasim
ms.openlocfilehash: 742c18815445b038e85c33a96743790491976945
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/10/2018
ms.locfileid: "48901112"
---
# <a name="what-is-qna-maker"></a>O que é o Criador de FAQ?

A ferramenta Criador de FAQ é um serviço de base de dados de conhecimento (KB) de perguntas e respostas, que aplica a inteligência de aprendizagem automática personalizada à pergunta no idioma natural do utilizador para determinar a melhor resposta.

O Criador de FAQ permite incorporar o serviço com base na cloud a partir de conteúdo semi-estruturado, como documentos de FAQ, URLs, manuais de produtos, e perguntas e respostas personalizadas. O [portal Web](https://qnamaker.ai) de fácil utilização permite criar, gerir, preparar e publicar o serviço sem qualquer experiência como programador. Assim que o serviço é publicado num ponto final, uma aplicação cliente, como um chatbot, pode gerir a conversa com um utilizador para obter as perguntas e fornecer as respostas. 

![Descrição geral](../media/qnamaker-overview-learnabout/overview.png)

## <a name="key-qna-maker-processes"></a>Principais processos do Criador de FAQ

Um Criador de FAQ fornece dois serviços-chave para os seus dados:

* **Extração**: os dados de pergunta/resposta estruturados são extraídos de [origens de dados](../Concepts/data-sources-supported.md) semiestruturadas, como FAQs e manuais de produtos. A extração pode ser feita como parte da [criação](https://aka.ms/qnamaker-docs-createkb) da KB ou posteriormente, como parte do processo de edição.

* **Correspondência**: assim que a base de dados de conhecimento tiver sido [preparada e testada](https://aka.ms/qnamaker-docs-trainkb), pode [publicá-la](https://aka.ms/qnamaker-docs-publishkb). Isto permite ter um ponto final da base de dados de conhecimento do Criador de FAQ, que pode utilizar no bot ou na aplicação cliente. Este ponto final aceita uma pergunta do utilizador e responde com a melhor resposta existente na base de dados de conhecimento, juntamente com uma pontuação de confiança para a correspondência.

```JSON
{
    "answers": [
        {
            "questions": [
                "How do I share a knowledge base with other?"
            ],
            "answer": "Sharing works at the level of a QnA Maker service, i.e. all knowledge bases in the services will be shared. Read [here](https://docs.microsoft.com/azure/cognitive-services/qnamaker/how-to/collaborate-knowledge-base)how to collaborate on a knowledge base.",
            "score": 70.95,
            "id": 4,
            "source": "https://docs.microsoft.com/azure/cognitive-services/qnamaker/faqs",
            "metadata": []
        }
    ]
}

```

## <a name="qna-maker-architecture"></a>Arquitetura do Criador de FAQ

O Criador de FAQ é constituído pelos seguintes serviços de API:

1. **Serviços de gestão do Criador de FAQ**: experiência de gestão para uma base de dados de conhecimento do Criador de FAQ, que inclui a criação inicial, atualização, preparação e publicação. Estas atividades podem ser efetuadas através do [portal](https://qnamaker.ai) ou das [APIs de gestão](https://aka.ms/qnamaker-v4-apis). 

2. **Serviço de predição do Criador de FAQ**: é implementado na sua subscrição do Azure na região especificada. O conteúdo da KB do cliente está armazenado no [Azure Search](https://azure.microsoft.com/services/search/) e o ponto final é implementado como um [Serviço de aplicações](https://azure.microsoft.com/services/app-service/). Também pode optar por implementar um recurso do [Application Insights](https://azure.microsoft.com/services/application-insights/) para análise.

![Arquitetura](../media/qnamaker-overview-learnabout/architecture.png)


## <a name="service-highlights"></a>Destaques do serviço

- Uma total experiência **sem código** para [criar um bot de FAQ](https://aka.ms/qnamaker-docs-create-faqbot).
- **Não existe limitação de largura de banda para predições**. Paga pelo alojamento do serviço e não pelo número de transações. Veja a [página de preços](https://aka.ms/qnamaker-docs-pricing) para obter mais informações.
- **Dimensione conforme necessário**. Escolha os SKUs apropriados dos componentes individuais que melhor correspondem ao seu cenário. Veja como [escolher a capacidade](https://aka.ms/qnamaker-docs-capacity) para o seu serviço Criador de FAQ.
- **Total conformidade de dados**. Os componentes do serviço de predição são implementados na sua subscrição do Azure e dentro do respetivo limite de conformidade.


## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Criar o serviço Criador de FAQ](../how-to/set-up-qnamaker-service-azure.md)
