---
title: Ligar ao servidor FTP - Azure Logic Apps | Documentos da Microsoft
description: Criar, monitorizar e gerir os ficheiros num servidor FTP no Azure Logic Apps
author: ecfan
manager: jeconnoc
ms.author: estfan
ms.date: 07/22/2016
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: 4355a767d2ecd500662cdf4522e8a7e12de86b80
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/06/2018
ms.locfileid: "37866156"
---
# <a name="get-started-with-the-ftp-connector"></a>Começar com o conector FTP
Utilize o conector FTP para monitorizar, gerir e criar arquivos num servidor FTP. 

Para utilizar [qualquer conector](apis-list.md), tem primeiro de criar uma aplicação lógica. Pode começar a utilizar pelo [criar uma aplicação lógica agora](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="connect-to-ftp"></a>Ligar ao FTP
Antes da aplicação lógica pode aceder a qualquer serviço, tem primeiro de criar uma *ligação* ao serviço. R [ligação](connectors-overview.md) fornece conectividade entre uma aplicação lógica e outro serviço.  

### <a name="create-a-connection-to-ftp"></a>Criar uma ligação para FTP
> [!INCLUDE [Steps to create a connection to FTP](../../includes/connectors-create-api-ftp.md)]
> 
> 

## <a name="use-a-ftp-trigger"></a>Utilizar um acionador FTP
Um acionador é um evento que pode ser utilizado para iniciar o fluxo de trabalho definido numa aplicação lógica. [Saiba mais sobre os acionadores](../logic-apps/logic-apps-overview.md#logic-app-concepts).  

> [!IMPORTANT]
> O conector FTP requer um servidor FTP que está acessível a partir da Internet e está configurado para operar com o modo passivo. Além disso, é o conector FTP **não é compatível com FTPS implícito (FTP sobre SSL)**. O conector FTP só suporta FTPS explícita (FTP sobre SSL).  
> 
> 

Neste exemplo, mostrarei como utilizar o **FTP - quando um ficheiro é adicionado ou modificado** acionador para iniciar um fluxo de trabalho de aplicação lógica quando um ficheiro é adicionado ou modificado num servidor FTP. Um exemplo de enterprise, poderia usar este acionador para monitorizar uma pasta FTP para novos ficheiros que representam as encomendas de clientes.  Em seguida, pode utilizar como uma ação de conector FTP **obter conteúdo do ficheiro** para obter o conteúdo do pedido para processamento adicional e o armazenamento na sua base de dados de encomendas.

1. Introduza *ftp* na caixa de pesquisa no designer de aplicações lógicas, em seguida, selecione a **FTP - quando um ficheiro é adicionado ou modificado** acionador   
   ![Imagem de Acionador FTP 1](./media/connectors-create-api-ftp/ftp-trigger-1.png)  
   O **quando um ficheiro é adicionado ou modificado** controlo, abre-se  
   ![Imagem de Acionador FTP 2](./media/connectors-create-api-ftp/ftp-trigger-2.png)  
2. Selecione o **...**  localizado no lado direito do controle. Esta ação abre o controlo de Seletor de pasta  
   ![Imagem de Acionador FTP 3](./media/connectors-create-api-ftp/ftp-trigger-3.png)  
3. Selecione o **>** (seta para a direita) e procure para localizar a pasta que pretende monitorizar ficheiros novos ou modificados. Selecione a pasta e observe que a pasta é apresentada no **pasta** controle.  
   ![Imagem de Acionador FTP 4](./media/connectors-create-api-ftp/ftp-trigger-4.png)   

Neste momento, a aplicação lógica foi configurada com um acionador que começará uma execução de outros acionadores e ações do fluxo de trabalho quando um ficheiro é modificado ou criado na pasta FTP específica. 

> [!NOTE]
> Para uma aplicação de lógica para ser funcional, tem de conter, pelo menos, um acionador e uma ação. Siga os passos na secção seguinte para adicionar uma ação.  
> 
> 

## <a name="use-a-ftp-action"></a>Utilize uma ação de FTP
Uma ação é uma operação levada a cabo pelo fluxo de trabalho definido numa aplicação lógica. [Saiba mais sobre as ações](../logic-apps/logic-apps-overview.md#logic-app-concepts).  

Agora que adicionou um acionador, siga estes passos para adicionar uma ação que irá obter o conteúdo do arquivo novo ou modificado encontrado pelo acionador.    

1. Selecione **+ novo passo** para adicionar a ação para obter o conteúdo do ficheiro no servidor de FTP  
2. Selecione o **adicionar uma ação** ligação.  
   ![Imagem da ação de FTP 1](./media/connectors-create-api-ftp/ftp-action-1.png)  
3. Introduza *FTP* para procurar todas as ações relacionadas com o FTP.
4. Selecione **FTP - obter conteúdo do ficheiro** como a ação a tomar quando um ficheiro de novo ou modificado está localizado na pasta de FTP.      
   ![Imagem da ação de FTP 2](./media/connectors-create-api-ftp/ftp-action-2.png)  
   O **obter conteúdo do ficheiro** controlar abre-se. **Tenha em atenção**: será solicitado para autorizar a aplicação lógica para aceder à sua conta de servidor FTP, se não o tiver feito isso anteriormente.  
   ![Imagem da ação de FTP 3](./media/connectors-create-api-ftp/ftp-action-3.png)   
5. Selecione o **ficheiro** controle (o espaço em branco localizada abaixo * * ficheiro ***). Aqui, pode utilizar qualquer uma das várias propriedades do ficheiro novo ou modificado encontrado no servidor de FTP.  
6. Selecione o **conteúdo do ficheiro** opção.  
   ![Imagem da ação de FTP 4](./media/connectors-create-api-ftp/ftp-action-4.png)   
7. O controle é atualizado, indicando que o **FTP - obter conteúdo do ficheiro** ação irá obter a *conteúdo do ficheiro* do arquivo novo ou modificado no servidor de FTP.      
   ![Imagem da ação de FTP 5](./media/connectors-create-api-ftp/ftp-action-5.png)     
8. Guarde o seu trabalho, em seguida, adicione um ficheiro para a pasta FTP para testar o fluxo de trabalho.    

Neste momento, a aplicação lógica foi configurada com um acionador para monitorizar uma pasta num servidor FTP e iniciar o fluxo de trabalho quando encontra um novo ficheiro ou um ficheiro modificado no servidor de FTP. 

A aplicação lógica também foi configurada com uma ação para obter o conteúdo do arquivo novo ou modificado.

Agora pode adicionar outra ação, tal como o [SQL Server - Inserir linha](connectors-create-api-sqlazure.md) ação para inserir o conteúdo do arquivo novo ou modificado numa tabela de base de dados SQL.  

## <a name="connector-specific-details"></a>Detalhes específicos do conector

Ver os acionadores e as ações definidas no swagger e também ver quaisquer limites na [detalhes do conector](/connectors/ftpconnector/). 

## <a name="next-steps"></a>Próximos Passos
[Criar uma aplicação lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md)

