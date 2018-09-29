---
title: O que é o Criador de FAQ?
titleSuffix: Azure Cognitive Services
description: O Criador de FAQ permite incorporar um serviço de perguntas e respostas a partir do seu conteúdo semiestruturado, como URLs ou documentos de FAQ e manuais de produtos.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: overview
ms.date: 09/12/2018
ms.author: tulasim
ms.openlocfilehash: eba7355faf304721fdac4bfdb88f8a69ed41fcba
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47038158"
---
# <a name="what-is-qna-maker"></a>O que é o Criador de FAQ?

O [Criador de FAQ](https://qnamaker.ai) permite incorporar um serviço de perguntas e respostas a partir do seu conteúdo semiestruturado, como URLs ou documentos de FAQ (Perguntas Mais Frequentes) e manuais de produtos. Pode criar um modelo de perguntas e respostas flexível para consultas de utilizador, fornecendo as respostas utilizadas por um bot preparado de forma natural e conversacional.

Uma interface de utilizador gráfica de fácil utilização permite criar, gerir, preparar e utilizar o serviço sem qualquer experiência de programação.

![Descrição geral](../media/qnamaker-overview-learnabout/overview.png)

## <a name="highlights"></a>Destaques

- Uma total experiência **sem código** para [criar um bot de FAQ](https://aka.ms/qnamaker-docs-create-faqbot).
- **Sem limitação de rede**. Paga pelo alojamento do serviço e não pelo número de transações. Veja a [página de preços](https://aka.ms/qnamaker-docs-pricing) para obter mais informações.
- **Dimensionamento de acordo com as suas necessidades**. Escolha os SKUs apropriados dos componentes individuais que melhor correspondem ao seu cenário. Veja como [escolher a capacidade](https://aka.ms/qnamaker-docs-capacity) para o seu serviço Criador de FAQ.
- **Total conformidade de dados**. Os componentes de dados e de runtime são implementados na subscrição do Azure do utilizador e dentro do respetivo limite de conformidade. Leia os detalhes mais abaixo.

## <a name="key-qna-maker-processes"></a>Principais processos do Criador de FAQ

Um Criador de FAQ fornece dois serviços-chave para os seus dados:

* **Extração**: os dados de pergunta/resposta estruturados são extraídos de origens de dados semiestruturados, como FAQs e manuais de produtos. Esta extração é feita ao criar a base de dados de conhecimento. Crie a sua base de dados de conhecimento [aqui](https://aka.ms/qnamaker-docs-createkb).

* **Correspondência**: assim que a base de dados de conhecimento tiver sido [preparada e testada](https://aka.ms/qnamaker-docs-trainkb), pode [publicá-la](https://aka.ms/qnamaker-docs-publishkb). Isto permite ter um ponto final da base de dados de conhecimento do Criador de FAQ, que pode utilizar no bot ou na aplicação. Este ponto final aceita uma pergunta do utilizador e responde com a melhor correspondência de pergunta/resposta existente na base de dados de conhecimento, juntamente com uma pontuação de confiança para a correspondência.

## <a name="qna-maker-architecture"></a>Arquitetura do Criador de FAQ

A pilha do Criador de FAQ é constituída pelas seguintes partes:

1. **Serviços de gestão do Criador de FAQ (plano de controlo)**: experiência de gestão para uma base de dados de conhecimento do Criador de FAQ, que inclui a criação inicial, atualização, preparação e publicação. Estas atividades podem ser efetuadas através do [portal](https://qnamaker.ai) ou das [APIs de gestão](https://aka.ms/qnamaker-v4-apis). Os serviços de gestão comunicam com o componente de runtime abaixo.

2. **Runtime do Criador de FAQ (plano de dados)**: os dados e o runtime são implementados na subscrição do Azure do utilizador numa região à escolha. O conteúdo de perguntas/respostas do cliente está armazenado no [Azure Search](https://azure.microsoft.com/services/search/) e o runtime é implementado como um [Serviço de aplicações](https://azure.microsoft.com/services/app-service/). Opcionalmente, também pode optar por implementar um recurso do [Application Insights](https://azure.microsoft.com/services/application-insights/) para análise.

![Arquitetura](../media/qnamaker-overview-learnabout/architecture.png)

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Criar o serviço Criador de FAQ](../how-to/set-up-qnamaker-service-azure.md)
