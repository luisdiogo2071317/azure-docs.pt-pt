---
title: Captura de dados a partir de Hubs de eventos para o Azure Data Lake Store | Microsoft Docs
description: Utilização do Azure Data Lake Store para capturar os dados dos Event Hubs
services: data-lake-store
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 02/21/2018
ms.author: nitinme
ms.openlocfilehash: cff2ca87ab2916c9d5aa8bb308325cb7a0227d75
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
---
# <a name="use-azure-data-lake-store-to-capture-data-from-event-hubs"></a>Utilização do Azure Data Lake Store para capturar os dados dos Event Hubs

Saiba como utilizar o Azure Data Lake Store para capturar os dados recebidos pelo Event Hubs do Azure.

## <a name="prerequisites"></a>Pré-requisitos

* **Uma subscrição do Azure**. Consulte [Obter uma avaliação gratuita do Azure](https://azure.microsoft.com/pricing/free-trial/).

* **Uma conta do Azure Data Lake Store**. Para obter instruções sobre como criar uma, veja [Introdução ao Azure Data Lake Store](data-lake-store-get-started-portal.md).

*  **Um espaço de nomes de Event Hubs**. Para obter instruções, consulte [criar um espaço de nomes de Event Hubs](../event-hubs/event-hubs-create.md#create-an-event-hubs-namespace). Certifique-se a conta de Data Lake Store e o espaço de nomes de Event Hubs na mesma subscrição do Azure.


## <a name="assign-permissions-to-event-hubs"></a>Atribuir permissões para os Event Hubs

Nesta secção, vai criar uma pasta dentro da conta em que pretenda capturar os dados a partir do Event Hubs. Também atribuir permissões para os Event Hubs para que este pode escrever dados para uma conta do Data Lake Store. 

1. Abra a conta de Data Lake Store, onde pretende capturar os dados dos Event Hubs e, em seguida, clique em **Explorador de dados**.

    ![Explorador de dados do Data Lake Store](./media/data-lake-store-archive-eventhub-capture/data-lake-store-open-data-explorer.png "Explorador de dados do Data Lake Store")

2.  Clique em **nova pasta** e, em seguida, introduza um nome para a pasta onde pretende capturar os dados.

    ![Crie uma nova pasta no Data Lake Store](./media/data-lake-store-archive-eventhub-capture/data-lake-store-create-new-folder.png "crie uma nova pasta no Data Lake Store")

3. Atribua permissões na raiz do Data Lake Store. 

    a. Clique em **Explorador de dados**, selecione a raiz da conta do Data Lake Store e, em seguida, clique em **acesso**.

    ![Atribuir permissões para a raiz do Data Lake Store](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-permissions-to-root.png "atribuir permissões para a raiz do Data Lake Store")

    b. Em **acesso**, clique em **adicionar**, clique em **selecionar utilizador ou grupo**e, em seguida, procure `Microsoft.EventHubs`. 

    ![Atribuir permissões para a raiz do Data Lake Store](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp.png "atribuir permissões para a raiz do Data Lake Store")
    
    Clique em **Selecionar**.

    c. Em **atribuir permissões**, clique em **selecionar permissões**. Definir **permissões** para **executar**. Definir **adicionar ao** para **esta pasta e todos os elementos subordinados**. Definir **adicionar como** para **uma entrada de permissão de acesso e uma entrada de permissão predefinidas**.

> [!IMPORTANT]
> Ao criar uma nova hierarquia de pastas para capturar os dados recebidos pelo Event Hubs do Azure, esta é uma forma fácil de garantir o acesso à pasta de destino.  No entanto, se adicionar permissões a todos os elementos subordinados de uma pasta de nível superior com muitos ficheiros de subordinados e pastas, pode demorar muito tempo.  Se a pasta de raiz contém um grande número de ficheiros e pastas, poderá ser mais rápida Adicionar **executar** permissões para `Microsoft.EventHubs` individualmente para cada pasta no caminho para a pasta de destino final. 

    ![Assign permissions for Data Lake Store root](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp1.png "Assign permissions for Data Lake Store root")

    Click **OK**.

4. Atribua as permissões da pasta na conta do Data Lake Store, onde pretende capturar os dados.

    a. Clique em **Explorador de dados**, selecione a pasta na conta do Data Lake Store e, em seguida, clique em **acesso**.

    ![Atribuir permissões para a pasta do Data Lake Store](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-permissions-to-folder.png "atribuir permissões para a pasta do Data Lake Store")

    b. Em **acesso**, clique em **adicionar**, clique em **selecionar utilizador ou grupo**e, em seguida, procure `Microsoft.EventHubs`. 

    ![Atribuir permissões para a pasta do Data Lake Store](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp.png "atribuir permissões para a pasta do Data Lake Store")
    
    Clique em **Selecionar**.

    c. Em **atribuir permissões**, clique em **selecionar permissões**. Definir **permissões** para **ler, escrever,** e **executar**. Definir **adicionar ao** para **esta pasta e todos os elementos subordinados**. Finalmente, defina o **adicionar como** para **uma entrada de permissão de acesso e uma entrada de permissão predefinidas**.

    ![Atribuir permissões para a pasta do Data Lake Store](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp-folder.png "atribuir permissões para a pasta do Data Lake Store")
    
    Clique em **OK**. 

## <a name="configure-event-hubs-to-capture-data-to-data-lake-store"></a>Configurar os Hubs de eventos para capturar os dados ao Data Lake Store

Nesta secção, vai criar um Hub de eventos dentro de um espaço de nomes de Event Hubs. Também é configurar o Hub de eventos para capturar os dados para uma conta do Azure Data Lake Store. Esta secção assume que já criou um espaço de nomes de Event Hubs.

2. Do **descrição geral** painel do espaço de nomes dos Event Hubs, clique em **+ Hub de eventos**.

    ![Criar o Hub de eventos](./media/data-lake-store-archive-eventhub-capture/data-lake-store-create-event-hub.png "criar o Hub de eventos")

3. Forneça os valores seguintes para configurar os Hubs de eventos para capturar os dados ao Data Lake Store.

    ![Criar o Hub de eventos](./media/data-lake-store-archive-eventhub-capture/data-lake-store-configure-eventhub.png "criar o Hub de eventos")

    a. Forneça um nome para o Hub de eventos.
    
    b. Para este tutorial, defina **contagem da partição** e **mensagem retenção** para os valores predefinidos.
    
    c. Definir **capturar** para **no**. Definir o **janela de tempo** (como frequentemente capturar) e **tamanho da janela** (tamanho de dados para capturar). 
    
    d. Para **capturar fornecedor**, selecione **Azure Data Lake Store** e a Data Lake Store, selecione o que criou anteriormente. Para **caminho do Data Lake**, introduza o nome da pasta que criou na conta do Data Lake Store. Só tem de fornecer o caminho relativo para a pasta.

    e. Deixe o **formatos de nome de ficheiro de captura de exemplo** para o valor predefinido. Esta opção é regida pela estrutura de pasta que é criada na pasta de captura.

    f. Clique em **Criar**.

## <a name="test-the-setup"></a>Testar o programa de configuração

Pode agora testar a solução ao enviar dados para o Hub de eventos do Azure. Siga as instruções apresentadas em [enviar eventos para Event Hubs do Azure](../event-hubs/event-hubs-dotnet-framework-getstarted-send.md). Depois de começar a enviar os dados, consulte os dados serão refletidos no Data Lake Store utilizando a estrutura de pasta que especificou. Por exemplo, verá uma estrutura de pasta, conforme mostrado na captura de ecrã seguinte, no Data Lake Store.

![Exemplo de dados de EventHub no Data Lake Store](./media/data-lake-store-archive-eventhub-capture/data-lake-store-eventhub-data-sample.png "EventHub de amostra de dados no Data Lake Store")

> [!NOTE]
> Mesmo se não tiver mensagens entra de Event Hubs, os Event Hubs escreve ficheiros vazios com apenas os cabeçalhos de para a conta de Data Lake Store. Os ficheiros são escritos no mesmo intervalo de tempo que forneceu ao criar os Event Hubs.
> 
>

## <a name="analyze-data-in-data-lake-store"></a>Analisar dados no Data Lake Store

Depois dos dados no Data Lake Store, pode executar tarefas analíticas a processar e crunch os dados. Consulte [USQL Avro exemplo](https://github.com/Azure/usql/tree/master/Examples/AvroExamples) sobre como fazê-lo utilizando o Azure Data Lake Analytics.
  

## <a name="see-also"></a>Consulte também
* [Secure data in Data Lake Store (Proteger dados no Data Lake Store)](data-lake-store-secure-data.md)
* [Copiar dados de Blobs de armazenamento do Azure para o Data Lake Store](data-lake-store-copy-data-azure-storage-blob.md)
