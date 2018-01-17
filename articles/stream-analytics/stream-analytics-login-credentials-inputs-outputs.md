---
title: "Análise de fluxo: Rodar credenciais de início de sessão de entradas e saídas | Microsoft Docs"
description: "Saiba como atualizar as credenciais de Stream Analytics entradas e saídas."
keywords: "credenciais de início de sessão"
services: stream-analytics
documentationcenter: 
author: SnehaGunda
manager: kfile
editor: cgronlun
ms.assetid: 42ae83e1-cd33-49bb-a455-a39a7c151ea4
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 01/11/2018
ms.author: sngun
ms.openlocfilehash: c1aded8fefc7b56acd2e9ff36bb2c9641665db76
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/16/2018
---
# <a name="rotate-login-credentials-for-inputs-and-outputs-of-a-stream-analytics-job"></a>Roda credenciais de início de sessão para entradas e saídas da tarefa do Stream Analytics

Sempre que voltar a gerar as credenciais para uma entrada ou saída de uma tarefa de Stream Analytics, deve atualizar a tarefa com novas credenciais. Tem de parar a tarefa antes de atualizar as credenciais, não é possível substituir as credenciais enquanto a tarefa está em execução. Para reduzir o atraso entre a parar e reiniciar a tarefa, o Stream Analytics suporta retomar uma tarefa a partir do respetivo resultado último. Este tópico descreve o processo de rodar as credenciais de início de sessão e reiniciar a tarefa com novas credenciais.

## <a name="regenerate-new-credentials-and-update-your-job-with-the-new-credentials"></a>Voltar a gerar novas credenciais e atualizar a tarefa com as novas credenciais 

Nesta secção, iremos irá guiá-lo através de regenerar as credenciais para o armazenamento de BLOBs, os Event Hubs, base de dados SQL e o Table Storage. 

### <a name="blob-storagetable-storage"></a>Armazenamento de/tabela de armazenamento de BLOBs
1. Inicie sessão no portal do Azure > procurar a conta de armazenamento que é utilizado como entrada/saída para a tarefa de Stream Analytics.    
2. A partir da secção de definições, abra o **chaves de acesso**. Entre as chaves de predefinidos (chave1, key2), escolha a que não é utilizado pelo seu trabalho e voltar a gerá-lo:  
   ![Voltar a gerar chaves de conta de armazenamento](media/stream-analytics-login-credentials-inputs-outputs/image1.png)
3. Copie a chave recentemente criada.    
4. No portal do Azure, navegue até a tarefa de Stream Analytics > selecione **parar** e aguarde que a tarefa parar.    
5. Localizar o Blob/tabela de entrada/saída de armazenamento para o qual pretende atualizar as credenciais.    
6. Localizar o **chave da conta de armazenamento** campo e cole a chave recentemente criada > clique **guardar**.    
7. Um teste de ligação irá iniciar automaticamente quando guardar as alterações, pode vê-la a partir do separador notificações. Existem duas notificações para um corresponde ao guardar a atualização e outros corresponde a testar a ligação:  
   ![Notificações depois de editar a chave](media/stream-analytics-login-credentials-inputs-outputs/image4.png)
8. Avance para [iniciar a tarefa a partir da última vez parada] secção (#start-your-job-from-the-last-stopped-time).

### <a name="event-hubs"></a>Hubs de Eventos

1. Inicie sessão no portal do Azure > procurar o Hub de eventos que utilizou como entrada/saída para a tarefa de Stream Analytics.    
2. A partir da secção de definições, abra o **políticas de acesso partilhado** e selecione a política de acesso necessária. Entre o **chave primária** e **chave secundária**, escolha a que não é utilizado pelo seu trabalho e voltar a gerá-lo:  
   ![Voltar a gerar chaves para o Hub de eventos](media/stream-analytics-login-credentials-inputs-outputs/image2.png)
3. Copie a chave recentemente criada.    
4. No portal do Azure, navegue até a tarefa de Stream Analytics > selecione **parar** e aguarde que a tarefa parar.    
5. Localize o evento de hubs de entradam/saídam para os quais pretende atualizar as credenciais.    
6. Localizar o **chave de política de Hub de eventos** campo e cole a chave recentemente criada > clique **guardar**.    
7. Um teste de ligação irá iniciar automaticamente quando guardar as alterações, certifique-se de que tem passou com êxito.    
8. Avance para [iniciar a tarefa a partir da última vez parada](#start-your-job-from-the-last-stopped-time) secção.

### <a name="sql-database"></a>SQL Database

Terá de ligar à base de dados do SQL Server para atualizar as credenciais de início de sessão de um utilizador existente. Pode atualizar as credenciais utilizando o portal do Azure ou uma ferramenta do lado do cliente como o SQL Server Management Studio. Esta secção demonstra o processo de atualização de credenciais utilizando o portal do Azure.

1. Inicie sessão no portal do Azure > Procurar na base de dados do SQL Server que utilizou como saída para a tarefa de Stream Analytics.    
2. De **Explorador de dados**, início de sessão/ligar à base de dados > selecione o tipo de autorização como **autenticação do SQL Server** > Escreva no seu **início de sessão** e  **Palavra-passe** detalhes > selecione **Ok**.  
   ![Voltar a gerar as credenciais para a base de dados SQL](media/stream-analytics-login-credentials-inputs-outputs/image3.png)

3. No separador de consulta, alterar a palavra-passe para um dos seus utilizadores, executando a seguinte consulta (Certifique-se de que substitui `<user_name>` com o nome de utilizador e `<new_password>` com a sua nova palavra-passe):  

   ```SQL
   Alter user `<user_name>` WITH PASSWORD = '<new_password>'
   Alter role db_owner Add member `<user_name>`
   ```

4. Anote a nova palavra-passe.    
5. No portal do Azure, navegue até a tarefa de Stream Analytics > selecione **parar** e aguarde que a tarefa parar.    
6. Localize a saída de base de dados SQL para os quais pretende rodar credenciais. Atualize a palavra-passe e guardar as alterações.    
7. Um teste de ligação irá iniciar automaticamente quando guardar as alterações, certifique-se de que tem passou com êxito.    
8. Avance para [iniciar a tarefa a partir da última vez parada](#start-your-job-from-the-last-stopped-time) secção.

### <a name="power-bi"></a>Power BI
1. Inicie sessão no portal do Azure > procurar a tarefa de Stream Analytics > selecione **parar** e aguarde que a tarefa parar.    
2. Localize a saída do Power BI para o qual pretende renovar credenciais > clique o **renovar autorização** (deverá ver uma mensagem de êxito) > **guardar** as alterações.    
3. Um teste de ligação irá iniciar automaticamente quando guardar as alterações, certifique-se de passou com êxito.    
4. Avance para [iniciar a tarefa a partir da última vez parada](#start-your-job-from-the-last-stopped-time) secção.

## <a name="start-your-job-from-the-last-stopped-time"></a>Iniciar a tarefa a partir da última vez parada

1. Navegue para a tarefa **descrição geral** painel > selecione **iniciar** para iniciar a tarefa.    
2. Selecione **quando a última paragem** > clique **iniciar**. Tenha em atenção que a opção "quando a última paragem" só é apresentada se anteriormente executou a tarefa e tinha algumas saídas de gerado. A tarefa for reiniciada com base na hora da última resultado do valor.
   ![Iniciar a tarefa](media/stream-analytics-login-credentials-inputs-outputs/image5.png)

## <a name="next-steps"></a>Passos Seguintes
* [Introdução ao Azure Stream Analytics](stream-analytics-introduction.md)
* [Começar a utilizar o Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Tarefas de escala do Azure Stream Analytics](stream-analytics-scale-jobs.md)
* [Referência do idioma de consulta do Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referência de API do REST de gestão do Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)
