---
title: FAQ - QnA Maker
titleSuffix: Azure Cognitive Services
description: Lista de perguntas frequentes sobre o serviço QnA Maker
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: tulasim
ms.openlocfilehash: efefd595c43d7f46ff1ead91577d070cf8fb90e4
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/25/2018
ms.locfileid: "47164621"
---
# <a name="frequently-asked-questions"></a>Perguntas Mais Frequentes

## <a name="why-is-my-urlsfiles-is-not-extracting-question-answer-pairs"></a>Por que motivo está o meu URLs / ficheiro (s) não é a extração de pares de pergunta-resposta?

É possível que a ferramenta QnA Maker não é automática extrair algum conteúdo de (FAQ) de perguntas e respostas de URLs de FAQ válidos. Nesses casos, pode colar o conteúdo de QnA num ficheiro. txt e ver se a ferramenta pode ingeri-lo. Em alternativa, pode adicionar conteúdo forma editorial à sua base de dados de conhecimento.

## <a name="how-large-a-knowledge-base-can-i-create"></a>Que tamanho pode ter a base de dados de conhecimento que eu criar?

O tamanho da base de dados de conhecimento depende da pesquisa de SKU do Azure que escolher ao criar o serviço QnA Maker. Leia [aqui](./Tutorials/choosing-capacity-qnamaker-deployment.md) para obter mais detalhes.

## <a name="why-do-i-not-see-anything-in-the-drop-down-for-when-i-try-to-create-a-new-knowledge-base"></a>Por que motivo não vejo qualquer coisa no menu pendente ao tentar criar uma nova base de dados de conhecimento?

Ainda não criou quaisquer serviços do QnA Maker no Azure. Leia [aqui](./How-To/set-up-qnamaker-service-azure.md) como fazer isso.

## <a name="how-do-i-share-a-knowledge-base-with-other"></a>Como posso partilhar uma base de dados de conhecimento com as outras?

Partilha funciona no nível de um serviço QnA Maker, serão partilhadas, ou seja, todas as bases de conhecimento nos serviços. Leia [aqui](./How-To/collaborate-knowledge-base.md) como colaborar numa base de dados de conhecimento.

## <a name="how-can-i-change-the-default-message-when-no-good-match-is-found"></a>Como posso alterar a mensagem predefinida quando for encontrada nenhuma correspondência bom?

A mensagem predefinida é parte integrante das definições no seu serviço de aplicações.
- Vá para o seu recurso de serviço de aplicações no portal do Azure

![serviço de aplicações do qnamaker](./media/qnamaker-faq/qnamaker-resource-list-appservice.png)
- Clique nas **definições** opção

![definições de serviço de aplicações do qnamaker](./media/qnamaker-faq/qnamaker-appservice-settings.png)
- Alterar o valor do **DefaultAnswer** definição
- Reinicie o serviço de aplicações

![reiniciar o serviço de aplicações qnamaker](./media/qnamaker-faq/qnamaker-appservice-restart.png)

## <a name="why-is-my-sharepoint-link-not-getting-extracted"></a>Por que minha ligação do SharePoint não é obter extraída?

A ferramenta analisa apenas dos URLs públicos e não suporta origens de dados autenticados neste momento. Em alternativa, pode transferir o ficheiro e utilize a opção de carregamento de ficheiros para extrair perguntas e respostas.


## <a name="the-updates-that-i-made-to-my-knowledge-base-are-not-reflected-on-publish-why-not"></a>As atualizações que fiz minha base de dados de conhecimento não são refletidas publicar. Porquê?

Cada operação de edição, seja numa atualização da tabela, de teste ou de definições, tem de ser guardadas antes de poderem ser publicadas. Certifique-se de que clique em salvar e treinar botão após cada operação de edição.

## <a name="when-should-i-refresh-my-endpoint-keys"></a>Quando razão devo atualizar as minhas chaves de ponto final?

Deve atualizar as suas chaves de ponto final, se suspeitar que tenham sido comprometidos.

## <a name="does-the-knowledge-base-support-rich-data-or-multimedia"></a>Faz a base de dados de conhecimento suporta dados avançados ou a multimídia?

A base de dados de conhecimento suporta Markdown. No entanto, a extração automática a partir de URLs limitou a capacidade de conversão de HTML para Markdown. Se pretender utilizar o Markdown completo, pode modificar o seu conteúdo diretamente na tabela, ou carregar uma base de dados de conhecimento com o conteúdo.

Multimídia, como imagens e vídeos, não é suportada neste momento.

## <a name="does-qna-maker-support-non-english-languages"></a>O QnA Maker suporta idiomas inglês?

Ver mais detalhes [idiomas suportados](./Overview/languages-supported.md).

Se tiver conteúdo a partir de vários idiomas, certifique-se de que criar um serviço separado para cada idioma.

## <a name="do-i-need-to-use-bot-framework-in-order-to-use-qna-maker"></a>Preciso de utilizar o Bot Framework para poder utilizar o QnA Maker?

Não, não é necessário utilizar o Bot Framework com a ferramenta QnA Maker. No entanto, o QnA Maker é disponibilizado como um dos vários modelos do Azure Bot Service. Bot Service permite um desenvolvimento rápido de bots inteligentes através do Microsoft Bot Framework, e é executado num servidor de ambiente de menos.

## <a name="how-can-i-create-a-bot-with-qna-maker"></a>Como criar um bot com a ferramenta QnA Maker?

Siga as instruções em [isso](./Tutorials/create-qna-bot.md) documentação para criar o seu Bot com o Azure Bot.

## <a name="how-do-i-embed-the-qna-maker-service-in-my-website"></a>Como incorporo o serviço QnA Maker no meu site?

Siga estes passos para incorporar o serviço QnA Maker como um controle de web chat no seu Web site:

1. Criar o seu bot de FAQ, seguindo as instruções [aqui](./Tutorials/create-qna-bot.md).
2. Ativar o chat web ao seguir os passos [aqui](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-webchat)


