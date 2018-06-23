---
title: FAQ – serviços cognitivos Microsoft | Microsoft Docs
titleSuffix: Azure
description: FAQs
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 04/21/2018
ms.author: saneppal
ms.openlocfilehash: a6bf32549715d0357771b3f3b0ff72f64788ec20
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354080"
---
# <a name="frequently-asked-questions"></a>Perguntas Mais Frequentes

## <a name="why-is-my-urlsfiles-is-not-extracting-question-answer-pairs"></a>Por que motivo é meu URLs / ficheiro não está a extrair pares de resposta de pergunta?

É possível que Maker de QnA não é automática-extrair algum conteúdo (FAQ) de pergunta e resposta de URLs de FAQ válido. Nestes casos, pode colar o conteúdo de QnA num ficheiro. txt e ver se a ferramenta pode ingeri-lo. Em alternativa, editorially pode adicionar conteúdo à sua base de dados de conhecimento.

## <a name="how-large-a-knowledge-base-can-i-create"></a>Que tamanho pode ter a base de dados de conhecimento que eu criar?

O tamanho da base de dados de conhecimento depende a pesquisa de SKU do Azure que escolha ao criar o serviço de QnA Maker. Leitura [aqui](./Tutorials/choosing-capacity-qnamaker-deployment.md) para obter mais detalhes.

## <a name="why-do-i-not-see-anything-in-the-drop-down-for-when-i-try-to-create-a-new-knowledge-base"></a>Por que motivo não vejo qualquer coisa na lista pendente ao tentar criar uma nova base de dados de conhecimento?

Ainda não criou quaisquer serviços de QnA Maker no Azure. Leitura [aqui](./How-To/set-up-qnamaker-service-azure.md) como fazê-lo.

## <a name="how-do-i-share-a-knowledge-base-with-other"></a>Como partilhar uma base de dados de conhecimento si?

Partilhar funciona ao nível de um serviço de QnA Maker, ou seja, todos os dados de conhecimento bases nos serviços de irá ser partilhados. Leitura [aqui](./How-To/collaborate-knowledge-base.md) como colaborar numa base de dados de conhecimento.

## <a name="how-can-i-change-the-default-message-when-no-good-match-is-found"></a>Como alterar a mensagem predefinida quando não é encontrada nenhuma correspondência boa?

A mensagem predefinida é parte integrante das definições no seu serviço de aplicações.
- Vá para o seu recurso de serviço de aplicação no portal do Azure

![qnamaker appservice](./media/qnamaker-faq/qnamaker-resource-list-appservice.png)
- Clique em de **definições** opção

![definições de serviço aplicacional qnamaker](./media/qnamaker-faq/qnamaker-appservice-settings.png)
- Altere o valor da **DefaultAnswer** definição
- Reinicie o serviço de aplicações

![reiniciar o serviço de aplicações qnamaker](./media/qnamaker-faq/qnamaker-appservice-restart.png)

## <a name="why-is-my-sharepoint-link-not-getting-extracted"></a>Por que motivo é que os meus ligação SharePoint obter extraída não?

A ferramenta analisa apenas os URLs públicos e não suporta origens de dados autenticados neste momento. Em alternativa, pode transferir o ficheiro e utilize a opção de carregamento de ficheiros para extrair as perguntas e respostas.


## <a name="the-updates-that-i-made-to-my-knowledge-base-are-not-reflected-on-publish-why-not"></a>As atualizações que efetuados a minha base de dados de conhecimento não são refletidas em publicar. Porquê?

Cada operação de edição, se uma atualização de tabela, de teste ou de definições, tem de ser guardada antes de poder publicar. Lembre-se de que clique dessa e formação botão após cada operação de edição.

## <a name="when-should-i-refresh-my-endpoint-keys"></a>Quando de atualização minhas chaves de ponto final?

Deve atualizar as chaves de ponto final se suspeitar que ter sido comprometidas.

## <a name="does-the-knowledge-base-support-rich-data-or-multimedia"></a>É a base de dados de conhecimento suporte avançado dados ou suporte?

A base de dados de conhecimento suporta Markdown. No entanto, a extração automática de URLs limitou a capacidade de conversão de HTML para Markdown. Se pretender utilizar Markdown totalmente funcional, pode modificar o conteúdo diretamente na tabela ou carregar uma base de dados de conhecimento com o conteúdo avançado.

Suporte, como imagens e vídeos, não é suportado neste momento.

## <a name="does-qna-maker-support-non-english-languages"></a>Maker de QnA suporta idiomas não inglesas?

Ver mais detalhes [idiomas suportados](./Overview/languages-supported.md).

Se tiver conteúdo a partir de vários idiomas, lembre-se de que crie um serviço separado para cada idioma.

## <a name="do-i-need-to-use-bot-framework-in-order-to-use-qna-maker"></a>Preciso de utilizar o Bot Framework para poder utilizar o QnA Maker?

Não, não terá de utilizar a estrutura de Bot com Maker de QnA. No entanto, Maker de QnA é fornecido como um dos vários modelos no serviço do Azure Bot. Serviço de bot permite um desenvolvimento rápido bot inteligente através do Microsoft Bot Framework, e é executado num servidor menos ambiente.

## <a name="how-can-i-create-a-bot-with-qna-maker"></a>Como criar um bot com Maker de QnA?

Siga as instruções em [isto](./Tutorials/create-qna-bot.md) documentação para criar a sua Bot com Bot do Azure.

## <a name="how-do-i-embed-the-qna-maker-service-in-my-website"></a>Como incorporo o serviço QnA Maker no meu site?

Siga estes passos para incorporar o serviço de QnA Maker como um controlo de web chat no seu Web site:

1. Criar a sua bot FAQ ao seguir as instruções [aqui](./Tutorials/create-qna-bot.md).
2. Ativar o chat na web, seguindo os passos [aqui](https://docs.microsoft.com/en-us/azure/bot-service/bot-service-channel-connect-webchat)


