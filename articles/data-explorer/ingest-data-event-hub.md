---
title: 'Início Rápido: Ingerir dados do Hub de Eventos para o Azure Data Explorer'
description: Neste início rápido, irá aprender a ingerir (carregar) dados para o Azure Data Explorer a partir do Hub de Eventos.
services: data-explorer
author: mgblythe
ms.author: mblythe
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: quickstart
ms.date: 09/24/2018
ms.openlocfilehash: acc18b6414e62a1f8239562158b4b80219c5d7c1
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46976827"
---
# <a name="quickstart-ingest-data-from-event-hub-into-azure-data-explorer"></a>Início Rápido: Ingerir dados do Hub de Eventos para o Azure Data Explorer

O Azure Data Explorer é um serviço de exploração de dados rápido e altamente dimensionável para dados telemétricos e de registo. O Azure Data Explorer fornece ingestão (carregamento de dados) a partir dos Hubs de Eventos, uma plataforma de transmissão de macrodados e um serviço de ingestão de eventos. Os Hubs de Eventos podem processar milhões de eventos por segundo quase em tempo real. Neste início rápido, vai criar um hub de eventos, ligar ao mesmo a partir do Azure Data Explorer e ver o fluxo de dados através do sistema.

Se não tiver uma subscrição do Azure, crie uma [conta do Azure gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Além de uma subscrição do Azure, precisa do seguinte para concluir este início rápido:

* [Um cluster e uma base de dados de teste](create-cluster-database-portal.md)

* [Uma aplicação de exemplo](https://github.com/Azure-Samples/event-hubs-dotnet-ingest) que gere dados

* [Visual Studio 2017 Versão 15.3.2 ou superior](https://www.visualstudio.com/vs/) para executar a aplicação de exemplo

## <a name="sign-in-to-the-azure-portal"></a>Iniciar sessão no portal do Azure

Inicie sessão no [portal do Azure](https://portal.azure.com/).

## <a name="create-an-event-hub"></a>Criar um hub de eventos

Neste início rápido, vai gerar dados de exemplo e enviá-los para um hub de eventos. O primeiro passo é criar um hub de eventos. Pode fazê-lo através de um modelo do Azure Resource Manager (ARM) no portal do Azure.

1. Selecione o botão seguinte para iniciar a implementação.

    [![Implementar no Azure](media/ingest-data-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-event-hubs-create-event-hub-and-consumer-group%2Fazuredeploy.json)

    O botão **Implementar no Azure** leva-o para o portal do Azure para preencher um formulário de implementação.

    ![Implementar no Azure](media/ingest-data-event-hub/deploy-to-azure.png)

1. Selecione a subscrição em que quer criar o hub de eventos e crie um grupo de recursos chamado *test-hub-rg*.

    ![Criar um grupo de recursos](media/ingest-data-event-hub/create-resource-group.png)

1. Preencha o formulário com as seguintes informações.

    ![Formulário de implementação](media/ingest-data-event-hub/deployment-form.png)

    Utilize as predefinições para todas as definições não listadas na tabela seguinte.

    **Definição** | **Valor sugerido** | **Descrição do campo**
    |---|---|---|
    | Subscrição | A sua subscrição | Selecione a subscrição do Azure que quer utilizar para o hub de eventos.|
    | Grupo de recursos | *test-hub-rg* | Crie um novo grupo de recursos. |
    | Localização | *E.U.A. Oeste* | Selecione *E.U.A. Oeste* para este início rápido. Para um sistema de produção, selecione a região que melhor se adequa às suas necessidades.
    | Nome do espaço de nomes | Um nome de espaço de nomes exclusivo | Escolha um nome exclusivo que identifique o seu espaço de nomes. Por exemplo, *mytestnamespace*. O nome de domínio *servicebus.windows.net* é anexado ao nome que indicar. O nome só pode conter letras, números e hífenes. O nome tem de começar com uma letra e terminar com uma letra ou número. O valor deve ter entre 6 e 50 carateres.
    | Nome do hub de eventos | *test-hub* | O hub de eventos encontra-se no espaço de nomes, que fornece um contentor de âmbito exclusivo. O nome do hub de eventos tem de ser exclusivo no espaço de nomes. |
    | Nome do grupo de consumidores | *test-group* | Os grupos de consumidores permitem que cada aplicação de consumo tenha uma vista separada do fluxo de eventos. |
    | | |

1. Selecione **Comprar**, que reconhece que está a criar recursos na sua subscrição.

1. Selecione **Notificações** na barra de ferramentas (o ícone da campainha) para monitorizar o processo de aprovisionamento. Poderá demorar alguns minutos até que a implementação seja bem-sucedida, mas pode avançar para o passo seguinte agora.

## <a name="create-a-target-table-in-azure-data-explorer"></a>Criar uma tabela de destino no Azure Data Explorer

Agora, vai criar uma tabela no Azure Data Explorer, para onde os Hubs de Eventos vão enviar os dados. Cria a tabela no cluster e na base de dados aprovisionada em **Pré-requisitos**.

1. No portal do Azure, no seu cluster, selecione **Consulta**.

    ![Ligação da aplicação de consulta](media/ingest-data-event-hub/query-explorer-link.png)

1. Copie o comando seguinte para a janela e selecione **Executar**.

    ```Kusto
    .create table TestTable (TimeStamp: datetime, Name: string, Metric: int, Source:string)
    ```

    ![Executar criação da consulta](media/ingest-data-event-hub/run-create-query.png)

1. Copie o comando seguinte para a janela e selecione **Executar**.

    ```Kusto
    .create table TestTable ingestion json mapping 'TestMapping' '[{"column":"TimeStamp","path":"$.timeStamp","datatype":"datetime"},{"column":"Name","path":"$.name","datatype":"string"},{"column":"Metric","path":"$.metric","datatype":"int"},{"column":"Source","path":"$.source","datatype":"string"}]'
    ```
    Este comando mapeia os dados JSON recebidos para os nomes de coluna e tipos de dados utilizados ao criar a tabela.

## <a name="connect-to-the-event-hub"></a>Ligar ao hub de eventos

Agora, ligue ao hub de eventos a partir do Azure Data Explorer, para que os dados que fluem para o hub de eventos sejam transmitidos à tabela de teste.

1. Selecione **Notificações** na barra de ferramentas para verificar se a implementação do hub de eventos foi concluída com êxito.

1. No cluster que criou, selecione **Bases de dados** e, em seguida, **TestDatabase**.

    ![Selecionar a base de dados de teste](media/ingest-data-event-hub/select-test-database.png)

1. Selecione **Ingestão de dados** e, em seguida, **Adicionar ligação de dados**.

    ![Ingestão de dados](media/ingest-data-event-hub/data-ingestion-create.png)

1. Preencha o formulário com as seguintes informações e, em seguida, selecione **Criar**.

    ![Ligação ao hub de eventos](media/ingest-data-event-hub/event-hub-connection.png)

    **Definição** | **Valor sugerido** | **Descrição do campo**
    |---|---|---|
    | Nome da ligação de dados | *test-hub-connection* | O nome da ligação que quer criar no Azure Data Explorer.|
    | Espaço de nomes do hub de eventos | Um nome de espaço de nomes exclusivo | O nome que escolheu anteriormente que identifica o seu espaço de nomes. |
    | Hub de eventos | *test-hub* | O hub de eventos que criou. |
    | Grupo de consumidores | *test-group* | O grupo de consumidores definido no hub de eventos que criou. |
    | Tabela | *TestTable* | A tabela que criou em **TestDatabase**. |
    | Formato de dados | *JSON* | São suportados os formatos JSON e CSV. |
    | Mapeamento de colunas | *TestMapping* | O mapeamento que criou em **TestDatabase**. |

    Neste início rápido, vai utilizar *encaminhamento estático* no hub de eventos, onde especifica o nome da tabela, o formato de ficheiro e o mapeamento. Também pode utilizar o encaminhamento dinâmico, onde a sua aplicação define estas propriedades.

## <a name="copy-the-connection-string"></a>Copiar a cadeia de ligação

Quando executa a aplicação para gerar dados de exemplo, precisa da cadeia de ligação para o espaço de nomes do hub de eventos.

1. No espaço de nomes do hub de eventos que criou, selecione **Políticas de acesso partilhado** e, em seguida, **RootManageSharedAccessKey**.

    ![Políticas de acesso partilhado](media/ingest-data-event-hub/shared-access-policies.png)

1. Copie **Cadeia de ligação - chave primária**.

    ![Cadeia de ligação](media/ingest-data-event-hub/connection-string.png)

## <a name="generate-sample-data"></a>Gerar dados de exemplo

Agora que o Azure Data Explorer e o hub de eventos etsão ligados, utilize a aplicação de exemplo que transferiu para gerar dados.

1. Abra a solução de aplicação de exemplo no Visual Studio.

1. No ficheiro *program.cs*, atualize a constante `connectionString` para a cadeia de ligação que copiou do espaço de nomes do hub de eventos.

    ```csharp
    const string eventHubName = "test-hub";
    // Copy the connection string ("Connection string-primary key") from your Event Hub namespace.
    const string connectionString = @"<YourConnectionString>";
    ```

1. Compile e execute a aplicação. A aplicação envia mensagens para o hub de eventos e apresenta o estado a cada dez segundos.

1. Depois de a aplicação enviar algumas mensagens, avance para o próximo passo: rever o fluxo de dados no hub de eventos e testar a tabela.

## <a name="review-the-data-flow"></a>Rever o fluxo de dados

1. No portal do Azure, no seu hub de eventos, verá o pico de atividade enquanto a aplicação está em execução.

    ![Gráfico do hub de eventos](media/ingest-data-event-hub/event-hub-graph.png)

1. Volte à aplicação e pare-a depois de atingir a mensagem 99.

1. Execute a consulta seguinte na base de dados de teste para verificar quantas mensagens chegaram à base de dados até ao momento.

    ```Kusto
    TestTable
    | count
    ```

1. Execute a consulta seguinte para ver o conteúdo das mensagens.

    ```Kusto
    TestTable
    ```

    O conjunto de resultados deve ter um aspeto semelhante ao seguinte.

    ![Conjunto de resultados das mensagens](media/ingest-data-event-hub/message-result-set.png)

## <a name="clean-up-resources"></a>Limpar recursos

Se não planear voltar a utilizar o hub de eventos, limpe **test-hub-rg**, para evitar incorrer em custos.

1. No portal do Azure, selecione **Grupos de recursos** à esquerda e, em seguida, selecione o grupo de recursos que criou.  

    Se o menu à esquerda estiver fechado, selecione ![botão Expandir](media/ingest-data-event-hub/expand.png) para expandi-lo.

   ![Selecionar grupo de recursos a eliminar](media/ingest-data-event-hub/delete-resources-select.png)

1. Em **test-resource-group**, selecione **Eliminar grupo de recursos**.

1. Na nova janela, escreva o nome do grupo de recursos a eliminar (*test-hub-rg*) e, em seguida, selecione **Eliminar**.

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Início Rápido: Consultar dados no Azure Data Explorer](web-query-data.md)
