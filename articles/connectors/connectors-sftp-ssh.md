---
title: Ligar a conta do SFTP a partir do Azure Logic Apps | Documentos da Microsoft
description: Automatizar tarefas e fluxos de trabalho que monitorizarem, criarem, gerirem, enviarem e recebem ficheiros para um servidor SFTP ao utilizar o Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
tags: connectors
ms.date: 09/24/2018
ms.openlocfilehash: 2250c6952aeac7b10dcb1a1a35419941e5cad507
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/30/2018
ms.locfileid: "50233213"
---
# <a name="monitor-create-and-manage-sftp-files-by-using-azure-logic-apps-and-sftp-ssh-connector"></a>Monitorizar, criar e gerir ficheiros SFTP utilizando o conector Azure Logic Apps e SFTP-SSH

Com o Azure Logic Apps e o conector SFTP-SSH, pode criar tarefas automatizadas e fluxos de trabalho que monitorizarem, criarem, enviarem e recebem ficheiros através da sua conta num [SFTP](https://www.ssh.com/ssh/sftp/) servidor, juntamente com outras ações como, por exemplo:

* Monitor de quando os ficheiros são adicionados ou alterados.
* Obter, criar, copiar, mudar o nome, atualizar, lista e eliminar ficheiros.
* Crie a pasta.
* Obter conteúdo do ficheiro e metadados.
* Extraia os arquivos para pastas.

Pode usar acionadores que obtém respostas a partir do seu servidor SFTP e disponibilizar a saída para outras ações. Pode utilizar ações nas suas aplicações lógicas para executar tarefas com arquivos no seu servidor SFTP. Pode também ter outras ações utilizar a saída de ações de SFTP. Por exemplo, se recuperar regularmente ficheiros a partir do seu servidor SFTP, pode enviar por e-mail sobre esses arquivos e seu conteúdo com o conector do Outlook do Office 365 ou o conector do Outlook.com.
Se estiver familiarizado com aplicações lógicas, reveja [o que é o Azure Logic Apps?](../logic-apps/logic-apps-overview.md)

## <a name="sftp-ssh-versus-sftp"></a>SFTP-SSH versus SFTP

Aqui estão algumas diferenças entre o conector SFTP-SSH e o [SFTP](../connectors/connectors-create-api-sftp.md) conector. O conector SFTP-SSH oferece estas capacidades:

* Utiliza a <a href="https://github.com/sshnet/SSH.NET" target="_blank"> **SSH.NET** </a> biblioteca, o que é uma biblioteca de Secure Shell (SSH) de código-fonte aberto para .NET.

* Fornece suporte para ficheiros grandes até **1GB**. O conector pode ler ou escrever ficheiros até 1 GB de tamanho.

* Fornece a **criar pasta** ação, que cria uma pasta no caminho especificado no servidor SFTP.

* Fornece a **mudança de nome de ficheiro** ação, o que muda o nome de um ficheiro no servidor SFTP.

* Coloca em cache a ligação ao servidor SFTP, o que melhora o desempenho e reduz o número de tentativas de ligação no servidor. 

  Pode controlar a duração utilizada para colocar em cache a ligação ao configurar o <a href="http://man.openbsd.org/sshd_config#ClientAliveInterval" target="_blank"> **ClientAliveInterval** </a> propriedade no seu servidor SFTP. 

## <a name="how-trigger-polling-works"></a>Como acionar funciona de consulta

Os acionadores SFTP-SSH trabalham consulta o sistema de ficheiros SFTP e está à procura de qualquer ficheiro que foi alterado desde a última consulta. Algumas ferramentas permitem-lhe preservar o carimbo de hora quando os ficheiros alterados, por isso nestes casos, terá de desativar esta funcionalidade para o acionador trabalhar. Seguem-se algumas definições comuns:

| Cliente SFTP | Ação | 
|-------------|--------| 
| Winscp | Preferências → de opções... Edição de transferência → →... Desativar → Preserve timestamp → |
| FileZilla | Desativar a transferência → Preserve carimbos de ficheiros transferidos → | 
||| 

Quando um acionador localiza um novo ficheiro, o acionador verifica que o novo ficheiro é completa e não parcialmente escrito. Por exemplo, um ficheiro pode ter as alterações em curso quando o acionador verifica o servidor de ficheiros. Para evitar o retorno de um arquivo parcialmente escrito, o acionador anota o carimbo de hora para o ficheiro que tem alterações recentes, mas não retorna imediatamente esse arquivo. O acionador devolver o ficheiro apenas quando consulta o servidor novamente. Às vezes, esse comportamento pode causar um atraso que é até duas vezes o acionador intervalo de consulta. 

Quando solicitar o conteúdo do ficheiro, o acionador não obter ficheiros maiores do que 50 MB. Para recolher ficheiros maiores do que 50 MB, siga este padrão:

* Utilizar um acionador que retorna as propriedades do ficheiro, tal como **quando um ficheiro é adicionado ou modificado (propriedades apenas)**. 
* Siga o acionador com uma ação que lê o arquivo completo, como **obter conteúdo do ficheiro através do caminho**.

## <a name="prerequisites"></a>Pré-requisitos

* Uma subscrição do Azure. Se não tiver uma subscrição do Azure, <a href="https://azure.microsoft.com/free/" target="_blank">inscreva-se para obter uma conta do Azure gratuita</a>. 

* Os SFTP anfitrião endereço e a conta de credenciais do servidor, que autoriza a aplicação lógica para criar uma ligação e aceder à sua conta do SFTP.

  Copie e cole o conteúdo completo para a chave privada SSH para o **chave privada SSH** propriedade seguindo o formato de várias linhas. 
  Eis os passos de exemplo que mostram como fornecer a chave privada SSH ao utilizar Notepad.exe:
    
  1. Abra o ficheiro de chave privada SSH no Notepad.exe
  2. Sobre o **edite** menu, selecione **Selecionar tudo**.
  3. Selecione **edite** > **cópia**.
  4. Quando cria a ligação na **chave privada SSH** propriedade, cole a chave. Não edite manualmente os **chave privada SSH** propriedade.

     O conector utiliza a biblioteca SSH.NET, que oferece suporte a esses formatos de chave privados SSH e apenas o MD5 impressão digital:

     * RSA 
     * DSA

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
* Para submeter ou votar em ideias para funcionalidades, visite o [site de comentários dos utilizadores do Logic Apps](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Passos Seguintes

* Saiba mais sobre outras [conectores do Logic Apps](../connectors/apis-list.md)