---
title: Utilize as funções do Azure para executar uma tarefa de limpeza da base de dados | Documentos da Microsoft
description: Utilize as funções do Azure para agendar uma tarefa que se liga a base de dados do Azure SQL para limpar periodicamente de linhas.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
ms.assetid: 076f5f95-f8d2-42c7-b7fd-6798856ba0bb
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 05/22/2017
ms.author: glenga
ms.openlocfilehash: 024958d8a548313b53fc24ade5805de036a89afb
ms.sourcegitcommit: 8e06d67ea248340a83341f920881092fd2a4163c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/16/2018
ms.locfileid: "49351920"
---
# <a name="use-azure-functions-to-connect-to-an-azure-sql-database"></a>Utilize as funções do Azure para ligar a uma base de dados do SQL do Azure
Este tópico mostra-lhe como utilizar as funções do Azure para criar uma tarefa agendada que limpa linhas numa tabela numa base de dados SQL do Azure. A nova função de script do c# é criada com base num modelo de Acionador de temporizador predefinidos no portal do Azure. Para suportar este cenário, também tem de definir uma cadeia de ligação de base de dados como uma definição de aplicação na aplicação de função. Este cenário utiliza uma operação em massa na base de dados. 

Para que sua pessoa de processo da função criar, leitura, atualização e operações de eliminação (CRUD) numa tabela de aplicações móveis, deve usar [enlaces de aplicações móveis](functions-bindings-mobile-apps.md).

> [!IMPORTANT]
> Os exemplos neste documento são aplicáveis ao 1.x runtime. [Pode encontrar aqui](./functions-versions.md#creating-1x-apps) informações sobre como criar uma aplicação de funções 1.x.

## <a name="prerequisites"></a>Pré-requisitos

+ Este tópico utiliza uma função acionada por temporizador. Conclua os passos no tópico [criar uma função no Azure que é acionado por um temporizador](functions-create-scheduled-function.md) para criar uma versão de c# dessa função.   

+ Este tópico demonstra um comando do Transact-SQL que executa uma operação de limpeza em massa no **SalesOrderHeader** tabela na base de dados de exemplo AdventureWorksLT. Para criar a base de dados de exemplo do AdventureWorksLT, conclua os passos no tópico [criar uma base de dados SQL do Azure no portal do Azure](../sql-database/sql-database-get-started-portal.md). 

## <a name="get-connection-information"></a>Obter informações da ligação

Tem de obter a cadeia de ligação da base de dados que criou quando concluiu [criar uma base de dados SQL do Azure no portal do Azure](../sql-database/sql-database-get-started-portal.md).

1. Inicie sessão no [Portal do Azure](https://portal.azure.com/).
 
3. Selecione **bases de dados SQL** no menu esquerdo e selecione a sua base de dados sobre o **bases de dados SQL** página.

4. Selecione **Mostrar cadeias de ligação de base de dados** e copie o completa **ADO.NET** cadeia de ligação. 

    ![Copie a cadeia de ligação do ADO.NET.](./media/functions-scenario-database-table-cleanup/adonet-connection-string.png)

## <a name="set-the-connection-string"></a>Definir a cadeia de ligação 

Uma aplicação de função aloja a execução das suas funções no Azure. É uma prática recomendada de armazenar cadeias de ligação e outros segredos nas suas definições de aplicação de função. Usar as configurações do aplicativo impede a divulgação acidental de cadeia de ligação com o seu código. 

1. Navegue para a aplicação de função que criou [criar uma função no Azure que é acionado por um temporizador](functions-create-scheduled-function.md).

2. Selecione **funcionalidades de plataforma** > **configurações de aplicativo**.
   
    ![Definições da aplicação para a aplicação de funções.](./media/functions-scenario-database-table-cleanup/functions-app-service-settings.png)

2. Desloque para baixo até **cadeias de ligação** e adicionar uma cadeia de ligação utilizando as definições conforme especificado na tabela.
   
    ![Adicione uma cadeia de ligação às definições da aplicação de função.](./media/functions-scenario-database-table-cleanup/functions-app-service-settings-connection-strings.png)

    | Definição       | Valor sugerido | Descrição             | 
    | ------------ | ------------------ | --------------------- | 
    | **Nome**  |  sqldb_connection  | Utilizado para aceder a cadeia de ligação armazenadas no código da função.    |
    | **Valor** | Cadeia de caracteres copiada  | Cole a cadeia de ligação que copiou na secção anterior e substitua `{your_username}` e `{your_password}` marcadores de posição pelos valores reais. |
    | **Tipo** | SQL Database | Utilize a ligação de base de dados SQL padrão. |   

3. Clique em **Guardar**.

Agora, pode adicionar o função código c# que se liga a base de dados SQL.

## <a name="update-your-function-code"></a>Atualizar o seu código de função

1. Na sua aplicação de função no portal, selecione a função acionada por temporizador.
 
3. Adicione as seguintes referências de assembly na parte superior c# script função código existente:

    ```cs
    #r "System.Configuration"
    #r "System.Data"
    ```
    >[!NOTE]
    >O código nestes exemplos são c# script a partir do portal. Quando estiver a desenvolver pré-compilados c# localmente uma função, em vez disso, tem de adicionar referências para cada uma delas monta em seu projeto local.  

3. Adicione o seguinte `using` instruções para a função:
    ```cs
    using System.Configuration;
    using System.Data.SqlClient;
    using System.Threading.Tasks;
    ```

4. Substituir o existente `Run` função com o código a seguir:
    ```cs
    public static async Task Run(TimerInfo myTimer, TraceWriter log)
    {
        var str = ConfigurationManager.ConnectionStrings["sqldb_connection"].ConnectionString;
        using (SqlConnection conn = new SqlConnection(str))
        {
            conn.Open();
            var text = "UPDATE SalesLT.SalesOrderHeader " + 
                    "SET [Status] = 5  WHERE ShipDate < GetDate();";

            using (SqlCommand cmd = new SqlCommand(text, conn))
            {
                // Execute the command and log the # rows affected.
                var rows = await cmd.ExecuteNonQueryAsync();
                log.Info($"{rows} rows were updated");
            }
        }
    }
    ```

    Este comando de exemplo atualiza o `Status` coluna com base na data de remessa. Ele deve atualizar 32 linhas de dados.

5. Clique em **salvar**, assista a **registos** windows para a próxima execução de função, em seguida, tenha em atenção o número de linhas atualizadas no **SalesOrderHeader** tabela.

    ![Ver os registos de função.](./media/functions-scenario-database-table-cleanup/functions-logs.png)

## <a name="next-steps"></a>Passos Seguintes

Em seguida, saiba como utilizar as funções com o Logic Apps para integrar com outros serviços.

> [!div class="nextstepaction"] 
> [Criar uma função que se integra com o Logic Apps](functions-twitter-email.md)

Para obter mais informações sobre as funções, consulte os seguintes tópicos:

* [Referência para programadores das Funções do Azure](functions-reference.md)  
  Referência para programadores para codificar funções e definir acionadores e enlaces.
* [Testar as Funções do Azure](functions-test-a-function.md)  
  Descreve várias ferramentas e técnicas para testar as suas funções.  
