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
ms.date: 11/08/2018
ms.author: tulasim
ms.openlocfilehash: 2e4a5d9b7ee2a1a88bcfe819be6540385458108f
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/14/2018
ms.locfileid: "51622368"
---
# <a name="frequently-asked-questions"></a>Perguntas Mais Frequentes

## <a name="manage-the-knowledge-base"></a>Gerir a base de dados de conhecimento

### <a name="i-accidentally-deleted-a-part-of-my-qna-maker-what-should-i-do"></a>Eliminei acidentalmente uma parte do meu QnA Maker, o que devo fazer? 

Todas as exclusões são permanentes, incluindo os pares de perguntas e respostas, ficheiros, URLs, personalizadas perguntas e respostas, bases de dados de conhecimento ou recursos do Azure. Certifique-se de exportar a sua base de dados de conhecimento a partir da **definições** página antes de eliminar qualquer parte da sua base de dados de conhecimento. 

### <a name="why-is-my-urlsfiles-is-not-extracting-question-answer-pairs"></a>Por que motivo está o meu URLs / ficheiro (s) não é a extração de pares de pergunta-resposta?

É possível que a ferramenta QnA Maker não é automática extrair algum conteúdo de (FAQ) de perguntas e respostas de URLs de FAQ válidos. Nesses casos, pode colar o conteúdo de QnA num ficheiro. txt e ver se a ferramenta pode ingeri-lo. Em alternativa, forma editorial pode adicionar conteúdo à sua base de dados de conhecimento através de [portal do QnA Maker](https://qnamaker.ai).

### <a name="how-large-a-knowledge-base-can-i-create"></a>Que tamanho pode ter a base de dados de conhecimento que eu criar?

O tamanho da base de dados de conhecimento depende da pesquisa de SKU do Azure que escolher ao criar o serviço QnA Maker. Leia [aqui](./Tutorials/choosing-capacity-qnamaker-deployment.md) para obter mais detalhes.

### <a name="why-cant-i-see-anything-in-the-drop-down-when-i-try-to-create-a-new-knowledge-base"></a>Por que motivo não vejo qualquer coisa no menu pendente ao tentar criar uma nova base de dados de conhecimento?

Ainda não criou quaisquer serviços do QnA Maker no Azure. Leia [aqui](./How-To/set-up-qnamaker-service-azure.md) para saber como fazê-lo.

### <a name="how-do-i-share-a-knowledge-base-with-others"></a>Como posso partilhar uma base de dados de conhecimento com outras pessoas?

Partilha funciona no nível de um serviço QnA Maker, ou seja, todas as bases de dados de conhecimento no serviço serão partilhadas. Leia [aqui](./How-To/collaborate-knowledge-base.md) como colaborar numa base de dados de conhecimento.

### <a name="can-you-share-a-kb-with-a-contributor-that-is-not-in-the-same-aad-tenant-to-modify-a-kb"></a>Pode partilhar uma BDC com um colaborador que não se encontra no mesmo inquilino do AAD, para modificar um KB? 

Partilha baseia-se no controlo de acesso baseado em função do Azure (RBAC). Se pode compartilhar _qualquer_ recursos no Azure com outro utilizador, também pode partilhar o QnA Maker.

### <a name="if-you-have-an-app-service-plan-with-5-qnamaker-kbs-can-you-assign-readwrite-rights-to-5-different-users-so-each-of-them-can-access-only-1-qnamaker-kb"></a>Se tiver um plano de serviço de aplicações com 5 KBs de QnAMaker. Pode atribuir direitos de leitura/escrita para 5 utilizadores diferentes para que cada um deles possa aceder apenas 1 QnAMaker KB?

Pode partilhar um serviço QnAMaker completo, KBs não individuais.

### <a name="how-can-i-change-the-default-message-when-no-good-match-is-found"></a>Como posso alterar a mensagem predefinida quando for encontrada nenhuma correspondência bom?

A mensagem predefinida é parte integrante das definições no seu serviço de aplicações.
- Aceda ao seu recurso de serviço de aplicações no portal do Azure

![serviço de aplicações do qnamaker](./media/qnamaker-faq/qnamaker-resource-list-appservice.png)
- Clique nas **definições** opção

![definições de serviço de aplicações do qnamaker](./media/qnamaker-faq/qnamaker-appservice-settings.png)
- Alterar o valor do **DefaultAnswer** definição
- Reinicie o serviço de aplicações

![reiniciar o serviço de aplicações qnamaker](./media/qnamaker-faq/qnamaker-appservice-restart.png)

### <a name="why-is-my-sharepoint-link-not-getting-extracted"></a>Por que minha ligação do SharePoint não é obter extraída?

A ferramenta analisa apenas dos URLs públicos e não suporta origens de dados autenticados neste momento. Em alternativa, pode transferir o ficheiro e utilize a opção de carregamento de ficheiros para extrair perguntas e respostas.


### <a name="the-updates-that-i-made-to-my-knowledge-base-are-not-reflected-on-publish-why-not"></a>As atualizações que fiz minha base de dados de conhecimento não são refletidas publicar. Porquê?

Cada operação de edição, seja numa atualização da tabela, teste ou definição, tem de ser guardadas antes de poderem ser publicadas. Certifique-se de que clique a **guardar e treinar** botão após cada operação de edição.

### <a name="does-the-knowledge-base-support-rich-data-or-multimedia"></a>Faz a base de dados de conhecimento suporta dados avançados ou a multimídia?

A base de dados de conhecimento suporta Markdown. No entanto, a extração automática a partir de URLs limitou a capacidade de conversão de HTML para Markdown. Se pretender utilizar o Markdown completo, pode modificar o seu conteúdo diretamente na tabela, ou carregar uma base de dados de conhecimento com o conteúdo.

Multimídia, como imagens e vídeos, não é suportada neste momento.

### <a name="does-qna-maker-support-non-english-languages"></a>O QnA Maker suporta idiomas inglês?

Ver mais detalhes [idiomas suportados](./Overview/languages-supported.md).

Se tiver conteúdo a partir de vários idiomas, certifique-se de que criar um serviço separado para cada idioma.

## <a name="manage-service"></a>Gerir serviço

### <a name="when-should-i-restart-my-app-service"></a>Quando eu reiniciar o meu serviço de aplicações? 

Atualizar o serviço de aplicações quando o ícone de cuidado estiver junto ao valor da versão para a base de dados de conhecimento no **chaves de ponto final** da tabela no **definições do utilizador** [página](https://www.qnamaker.ai/UserSettings).

### <a name="when-should-i-refresh-my-endpoint-keys"></a>Quando razão devo atualizar as minhas chaves de ponto final?

Atualize as suas chaves de ponto final, se suspeitar que tenham sido comprometidos.

### <a name="can-i-use-the-same-azure-search-resource-for-kbs-using-multiple-languages"></a>Pode utilizar o mesmo recurso do Azure Search para KBs com vários idiomas?

Para utilizar múltiplos idiomas e vários KBs, o utilizador tem de criar um recurso do QnA Maker para cada idioma. Esta ação irá criar dos serviços de pesquisa do Azure separada por idioma. Misturar as KBs de idioma diferente num serviço de pesquisa do Azure única irá resultar em degradação relevância dos resultados.

## <a name="integrate-with-other-services-including-bots"></a>Integrar com outros serviços incluindo Bots

### <a name="do-i-need-to-use-bot-framework-in-order-to-use-qna-maker"></a>Preciso de utilizar o Bot Framework para poder utilizar o QnA Maker?

Não, não é necessário utilizar o Bot Framework com a ferramenta QnA Maker. No entanto, o QnA Maker é disponibilizado como um dos vários modelos do Azure Bot Service. Bot Service permite um desenvolvimento rápido de bots inteligentes através do Microsoft Bot Framework, e é executado num ambiente sem servidor.

### <a name="how-can-i-create-a-bot-with-qna-maker"></a>Como criar um bot com a ferramenta QnA Maker?

Siga as instruções em [isso](./Tutorials/create-qna-bot.md) documentação para criar o seu Bot com o Azure Bot Service.

### <a name="how-do-i-embed-the-qna-maker-service-in-my-website"></a>Como incorporo o serviço QnA Maker no meu site?

Siga estes passos para incorporar o serviço QnA Maker como um controle de web chat no seu Web site:

1. Criar o seu bot de FAQ, seguindo as instruções [aqui](./Tutorials/create-qna-bot.md).
2. Ativar o chat web ao seguir os passos [aqui](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-webchat)

## <a name="data-storage"></a>Armazenamento de dados

### <a name="what-data-is-stored-and-where-is-it-stored"></a>Que dados são armazenados e onde ele é armazenado? 

Ao criar o serviço QnA Maker, selecionou uma região do Azure. As suas bases de dados de conhecimento e ficheiros de registo são armazenados nesta região. 
