---
title: Ligar a conta do SFTP a partir do Azure Logic Apps | Documentos da Microsoft
description: Automatizar tarefas e fluxos de trabalho que monitorizarem, criarem, gerirem, enviarem e recebem ficheiros para um servidor SFTP ao utilizar o Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.assetid: 697eb8b0-4a66-40c7-be7b-6aa6b131c7ad
ms.topic: article
tags: connectors
ms.date: 10/11/2018
ms.openlocfilehash: 77a76aa4fbb051e5999053279798c1b0147ae8e9
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/12/2018
ms.locfileid: "49166785"
---
# <a name="monitor-create-and-manage-sftp-files-by-using-azure-logic-apps"></a>Monitorizar, criar e gerir ficheiros de SFTP ao utilizar o Azure Logic Apps

Com o Azure Logic Apps e o conector do SFTP, pode criar tarefas automatizadas e fluxos de trabalho que monitorizarem, criarem, enviarem e recebem ficheiros através da sua conta num [SFTP](https://www.ssh.com/ssh/sftp/) servidor, juntamente com outras ações como, por exemplo:

* Monitor de quando os ficheiros são adicionados ou alterados.
* Obter, criar, copiar, atualizar, lista e eliminar ficheiros.
* Obter conteúdo do ficheiro e metadados.
* Extraia os arquivos para pastas.

Pode usar acionadores que obtém respostas a partir do seu servidor SFTP e disponibilizar a saída para outras ações. Pode utilizar ações nas suas aplicações lógicas para executar tarefas com arquivos no seu servidor SFTP. Pode também ter outras ações utilizar a saída de ações de SFTP. Por exemplo, se recuperar regularmente ficheiros a partir do seu servidor SFTP, pode enviar por e-mail sobre esses arquivos e seu conteúdo com o conector do Outlook do Office 365 ou o conector do Outlook.com. Se estiver familiarizado com aplicações lógicas, reveja [o que é o Azure Logic Apps?](../logic-apps/logic-apps-overview.md)

> [!NOTE]
> Para ficheiros com mais de 50 MB e a cópia de segurança para 1 GB, utilize o [conector do SFTP-SSH](../connectors/connectors-sftp-ssh.md). O conector do SFTP suporta apenas os ficheiros que estão a 50 MB ou mais pequeno, a menos que utilize [segmentação para lidar com mensagens grandes](../logic-apps/logic-apps-handle-large-messages.md). 

## <a name="prerequisites"></a>Pré-requisitos

* Uma subscrição do Azure. Se não tiver uma subscrição do Azure, <a href="https://azure.microsoft.com/free/" target="_blank">inscreva-se para obter uma conta do Azure gratuita</a>. 

* Os SFTP anfitrião endereço e a conta de credenciais do servidor

   As suas credenciais autorizar a aplicação lógica para criar uma ligação e aceder à sua conta do SFTP.

* Conhecimento básico sobre [como criar aplicações lógicas](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* A aplicação de lógica onde pretende aceder à sua conta do SFTP. Para começar com um acionador SFTP [criar uma aplicação lógica em branco](../logic-apps/quickstart-create-first-logic-app-workflow.md). Para utilizar uma ação de SFTP, inicie a aplicação lógica com outro acionador, por exemplo, o **periodicidade** acionador.

## <a name="connect-to-sftp"></a>Ligue-se ao SFTP

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Inicie sessão para o [portal do Azure](https://portal.azure.com)e abra a aplicação lógica no Estruturador da aplicação lógica, se não estiver já abrir.

1. Para aplicações de lógica em branco, na caixa de pesquisa, introduza "sftp" como o filtro. Abaixo da lista de disparadores, selecione o acionador que pretende. 

   -ou-

   Para logic apps existentes, sob a última etapa em que pretende adicionar uma ação, escolha **novo passo**. 
   Na caixa de pesquisa, introduza "sftp" como o filtro. 
   Abaixo da lista de ações, selecione a ação que pretende.

   Para adicionar uma ação entre passos, mova o ponteiro do mouse sobre a seta entre passos. 
   Selecione o sinal de adição (**+**) que é apresentada e, em seguida, selecione **adicionar uma ação**.

1. Forneça os detalhes necessários para a sua ligação e, em seguida, escolha **criar**.

1. Forneça os detalhes necessários para o seu acionador selecionado ou a ação e continuar a criar o fluxo de trabalho da sua aplicação lógica.

## <a name="examples"></a>Exemplos

### <a name="sftp-trigger-when-a-file-is-added-or-modified"></a>Acionador SFTP: quando um ficheiro é adicionado ou modificado

Este acionador é iniciado um fluxo de trabalho de aplicação lógica quando o acionador detetar quando um ficheiro é adicionado ou alterado num servidor SFTP. Por exemplo, que pode adicionar uma condição que verifica o conteúdo do arquivo e decide se obter esse conteúdo, com base em se esse conteúdo cumpre uma condição especificada. Por fim, pode adicionar uma ação que obtém o conteúdo do arquivo e colocar esse conteúdo numa pasta no servidor SFTP. 

**Exemplo de Enterprise**: pode utilizar este acionador para monitorizar uma pasta SFTP para novos ficheiros que representam as encomendas de cliente. Em seguida, pode utilizar como uma ação de SFTP **obter conteúdo do ficheiro**, para que possa obter conteúdo do pedido para processamento adicional e armazenar essa ordem numa base de dados de encomendas.

### <a name="sftp-action-get-content"></a>Ação de SFTP: obter o conteúdo

Esta ação obtém o conteúdo de um arquivo num servidor SFTP. Por exemplo, pode adicionar o acionador do exemplo anterior e uma condição que o conteúdo do ficheiro tem de cumprir. Se a condição for verdadeira, pode executar a ação que obtém o conteúdo. 

## <a name="connector-reference"></a>Referência do conector

Para obter detalhes técnicos sobre os limites, ações e acionadores, que é descrito através OpenAPI do conector (anteriormente Swagger) descrição, reveja o conector [página de referência](/connectors/sftpconnector/).

## <a name="get-support"></a>Obter suporte

* Relativamente a dúvidas, visite o [fórum do Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Para submeter ou votar em ideias para funcionalidades, visite o [site de comentários dos utilizadores do Logic Apps](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Passos Seguintes

* Saiba mais sobre outras [conectores do Logic Apps](../connectors/apis-list.md)