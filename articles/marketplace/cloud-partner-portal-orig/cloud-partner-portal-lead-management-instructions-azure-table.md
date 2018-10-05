---
title: Tabela do Azure | Documentos da Microsoft
description: Configure a gestão de oportunidades potenciais para a tabela do Azure.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/14/2018
ms.author: pbutlerm
ms.openlocfilehash: 60e3e3d81b07bf7ae681b5cef2d6d9681877a35f
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/05/2018
ms.locfileid: "48810891"
---
<a name="lead-management-instructions-for-azure-table"></a>Levar as instruções de gestão para a tabela do Azure
============================================

Este artigo descreve como configurar tabelas do Azure para armazenar oportunidades potenciais de venda. Tabela do Azure permite-lhe armazenar e personalizar as informações do cliente.

## <a name="to-configure-azure-table"></a>Para configurar tabelas do Azure

1.  Se não tiver uma conta do Azure, pode [criar uma conta de avaliação gratuita](https://azure.microsoft.com/pricing/free-trial/).

2.  Após a sua conta do Azure Active Directory, inicie sessão para o [portal do Azure](https://portal.azure.com).
3.  No portal do Azure, crie uma conta de armazenamento. Captura de ecrã seguinte mostra como criar uma conta de armazenamento. Para obter mais informações sobre os preços de armazenamento, consulte [preços de armazenamento](https://azure.microsoft.com/pricing/details/storage/).

    ![Passos para criar uma conta de armazenamento do Azure](./media/cloud-partner-portal-lead-management-instructions-azure-table/azurestoragecreate.png)

4.  Copie a cadeia de ligação de conta de armazenamento para a chave e cole-o para o **cadeia de ligação de conta de armazenamento** campo no Portal de parceiros de nuvem. É um exemplo de um sting de ligação `DefaultEndpointsProtocol=https;AccountName=myAccountName;AccountKey=myAccountKey;EndpointSuffix=core.windows.net `
    
    ![Chave de armazenamento do Azure](./media/cloud-partner-portal-lead-management-instructions-azure-table/azurestoragekeys.png)

Pode usar [Explorador de armazenamento do Azure](http://azurestorageexplorer.codeplex.com/) ou qualquer outra ferramenta para ver os dados na sua tabela de armazenamento. Também pode exportar os dados na tabela do Azure.
dados.

## <a name="optional-to-use-azure-functions-with-an-azure-table"></a>**(Opcional)**  Para utilizar as funções do Azure com uma tabela do Azure

Se pretender personalizar a forma como está a receber oportunidades potenciais, utilize [as funções do Azure](https://azure.microsoft.com/services/functions/) com uma tabela do Azure. O serviço de funções do Azure permite-lhe automatizar o processo de geração de oportunidades potenciais.

Os passos seguintes mostram como criar uma função do Azure que utiliza um temporizador. A cada cinco minutos a função aspeto na tabela do Azure para novos registros e, em seguida, utiliza o serviço SendGrid para enviar uma notificação por e-mail.


1.  [Criar](https://portal.azure.com/#create/SendGrid.SendGrid) uma conta de serviço SendGrid gratuita na subscrição do Azure.

    ![Criar o SendGrid](./media/cloud-partner-portal-lead-management-instructions-azure-table/createsendgrid.png)

2.  Criar uma chave de API de SendGrid 
    - Selecione **gerir** para aceder à interface do Usuário do SendGrid
    - Selecione **configurações**, **chaves de API**e, em seguida, crie uma chave com o envio de correio -\> acesso total
    - Guardar a chave de API


    ![Chave de API de SendGrid](./media/cloud-partner-portal-lead-management-instructions-azure-table/sendgridkey.png)


3.  [Criar](https://portal.azure.com/#create/Microsoft.FunctionApp) uma aplicação de função do Azure com a opção de plano de alojamento com o nome "Plano de consumo".

    ![Criar aplicação de funções do Azure](./media/cloud-partner-portal-lead-management-instructions-azure-table/createfunction.png)


4.  Crie uma nova definição de função.

    ![Criar definição de função do Azure](./media/cloud-partner-portal-lead-management-instructions-azure-table/createdefinition.png)
 

5.  Para obter a função para enviar uma atualização numa hora específica, selecione o **TimerTrigger-CSharp** como a opção de arranque.

     ![Opção de Acionador de tempo de função do Azure](./media/cloud-partner-portal-lead-management-instructions-azure-table/timetrigger.png)


6.  Substitua o código de "Desenvolver" com o seguinte exemplo de código. Edite os endereços de e-mail com os endereços que pretende utilizar para o remetente e o receptor.

        #r "Microsoft.WindowsAzure.Storage"
        #r "SendGrid"
        using Microsoft.WindowsAzure.Storage.Table;
        using System;
        using SendGrid;
        using SendGrid.Helpers.Mail;
        public class MyRow : TableEntity
        {
            public string Name { get; set; }
        }
        public static void Run(TimerInfo myTimer, IQueryable<MyRow> inputTable, out Mail message, TraceWriter log)
        {
            // UTC datetime that is 5.5 minutes ago while the cron timer schedule is every 5 minutes
            DateTime dateFrom = DateTime.UtcNow.AddSeconds(-(5 * 60 + 30));
            var emailFrom = "YOUR EMAIL";
            var emailTo = "YOUR EMAIL";
            var emailSubject = "Azure Table Notification";
            // Look in the table for rows that were added recently
            var rowsList = inputTable.Where(r => r.Timestamp > dateFrom).ToList();
            // Check how many rows were added
            int rowsCount = rowsList.Count;
            if (rowsCount > 0)
            {
                log.Info($"Found {rowsCount} rows added since {dateFrom} UTC");
                // Configure the email message describing how many rows were added
                message = new Mail
                {
                    From = new Email(emailFrom),
                    Subject = emailSubject + " (" + rowsCount + " new rows)"
                };
                var personalization = new Personalization();
                personalization.AddTo(new Email(emailTo));
                message.AddPersonalization(personalization);
                var content = new Content
                {
                    Type = "text/plain",
                    Value = "Found " + rowsCount + " new rows added since " + dateFrom.ToString("yyyy-MM-dd HH:mm:ss") + " UTC"
                };
                message.AddContent(content);
            }
            else
            {
                // Do not send the email if no new rows were found
                message = null;
            }
        }

    ![Trecho de código de função do Azure](./media/cloud-partner-portal-lead-management-instructions-azure-table/code.png)


7.  Selecione **integrar** e **entradas** para definir a ligação de tabelas do Azure.

    ![Integrar a função do Azure](./media/cloud-partner-portal-lead-management-instructions-azure-table/integrate.png)


8.  Introduza o nome da tabela e definir a cadeia de ligação selecionando **novo**.


    ![Ligação de tabela da função do Azure](./media/cloud-partner-portal-lead-management-instructions-azure-table/configtable.png)

9.  Agora, definir a saída como o SendGrid e manter todas as predefinições.

    ![Saída de SendGrid](./media/cloud-partner-portal-lead-management-instructions-azure-table/sendgridoutput.png)

    ![Predefinições de saída do SendGrid](./media/cloud-partner-portal-lead-management-instructions-azure-table/sendgridoutputdefaults.png)

10. Adicionar chave de API do SendGrid às definições de aplicação de função com o nome "SendGridApiKey" e o valor obtido a partir das chaves de API na interface do Usuário do SendGrid

    ![Gerir o SendGrid](./media/cloud-partner-portal-lead-management-instructions-azure-table/sendgridmanage.png)
    ![SendGrid Gerir chave](./media/cloud-partner-portal-lead-management-instructions-azure-table/sendgridmanagekey.png)

Depois de ter de configurar a função, o código na secção Integrate deve ter um aspeto semelhante ao seguinte exemplo.

    {
      "bindings": [
        {
          "name": "myTimer",
          "type": "timerTrigger",
          "direction": "in",
          "schedule": "0 */5 * * * *"
        },
        {
          "type": "table",
          "name": "inputTable",
          "tableName": "MarketplaceLeads",
          "take": 50,
          "connection": "yourstorageaccount_STORAGE",
          "direction": "in"
        },
        {
          "type": "sendGrid",
          "name": "message",
          "apiKey": "SendGridApiKey",
          "direction": "out"
        }
      ],
      "disabled": false
    }

11. A etapa final é navegar para a interface do Usuário desenvolver da função e, em seguida, selecione **executar** para iniciar o temporizador. Agora vai receber uma notificação sempre que uma nova oportunidade potencial entra em ação.
