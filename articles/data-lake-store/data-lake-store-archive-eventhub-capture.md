---
title: Capturar dados dos Hubs de eventos para o Azure Data Lake Store | Documentos da Microsoft
description: Utilização do Azure Data Lake Store para capturar dados dos Hubs de eventos
services: data-lake-store
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: nitinme
ms.openlocfilehash: bda52acc12aad3cad20143c319f557f11d760c42
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39435157"
---
# <a name="use-azure-data-lake-store-to-capture-data-from-event-hubs"></a>Utilização do Azure Data Lake Store para capturar dados dos Hubs de eventos

Saiba como utilizar o Azure Data Lake Store para capturar os dados recebidos pelo Event Hubs do Azure.

## <a name="prerequisites"></a>Pré-requisitos

* **Uma subscrição do Azure**. Consulte [Obter uma avaliação gratuita do Azure](https://azure.microsoft.com/pricing/free-trial/).

* **Uma conta do Azure Data Lake Store**. Para obter instruções sobre como criar uma, veja [Introdução ao Azure Data Lake Store](data-lake-store-get-started-portal.md).

*  **Um espaço de nomes de Hubs de eventos**. Para obter instruções, consulte [criar um espaço de nomes de Hubs de eventos](../event-hubs/event-hubs-create.md#create-an-event-hubs-namespace). Certifique-se de que a conta de Data Lake Store e o espaço de nomes de Hubs de eventos estão na mesma subscrição do Azure.


## <a name="assign-permissions-to-event-hubs"></a>Atribuir permissões aos Hubs de eventos

Nesta secção, vai criar uma pasta dentro da conta em que pretende capturar os dados dos Hubs de eventos. Também atribuir permissões aos Hubs de eventos para que possa escrever dados para uma conta do Data Lake Store. 

1. Abra a conta do Data Lake Store para capturar dados dos Hubs de eventos e, em seguida, clique em onde pretende **Data Explorer**.

    ![Explorador de dados do Data Lake Store](./media/data-lake-store-archive-eventhub-capture/data-lake-store-open-data-explorer.png "Explorador de dados do Data Lake Store")

1.  Clique em **nova pasta** e, em seguida, introduza um nome para a pasta onde pretende capturar os dados.

    ![Criar uma nova pasta no Data Lake Store](./media/data-lake-store-archive-eventhub-capture/data-lake-store-create-new-folder.png "criar uma nova pasta no Data Lake Store")

1. Atribua permissões na raiz do Store de Lake dados. 

    a. Clique em **Data Explorer**, selecione a raiz da conta do Data Lake Store e, em seguida, clique em **acesso**.

    ![Atribuir permissões para a raiz do Data Lake Store](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-permissions-to-root.png "atribuir permissões para a raiz do Data Lake Store")

    b. Sob **acesso**, clique em **Add**, clique em **selecionar utilizador ou grupo**e, em seguida, procure `Microsoft.EventHubs`. 

    ![Atribuir permissões para a raiz do Data Lake Store](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp.png "atribuir permissões para a raiz do Data Lake Store")
    
    Clique em **Selecionar**.

    c. Sob **atribuir permissões**, clique em **selecione permissões**. Definir **permissões** ao **executar**. Definir **adicionar ao** ao **esta pasta e todos os filhos**. Definir **adicionar como** ao **uma entrada de permissão de acesso e uma entrada de permissão predefinida**.

    > [!IMPORTANT]
    > Ao criar uma nova hierarquia de pasta para capturar os dados recebidos pelo Event Hubs do Azure, esta é uma forma fácil de garantir o acesso para a pasta de destino.  No entanto, a adicionar permissões a todos os filhos de uma pasta de nível superior com muitos ficheiros e pastas subordinados poderá demorar muito tempo.  Se a sua pasta de raiz contém um grande número de ficheiros e pastas, talvez seja mais rápido adicionar **Execute** permissões para `Microsoft.EventHubs` individualmente para cada pasta no caminho para a pasta de destino final. 

    ![Atribuir permissões para a raiz do Data Lake Store](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp1.png "atribuir permissões para a raiz do Data Lake Store")

    Clique em **OK**.

1. Atribua permissões para a pasta na conta do Data Lake Store, onde pretende capturar os dados.

    a. Clique em **Data Explorer**, selecione a pasta na conta do Data Lake Store e, em seguida, clique em **acesso**.

    ![Atribuir permissões para a pasta do Data Lake Store](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-permissions-to-folder.png "atribuir permissões para a pasta do Data Lake Store")

    b. Sob **acesso**, clique em **Add**, clique em **selecionar utilizador ou grupo**e, em seguida, procure `Microsoft.EventHubs`. 

    ![Atribuir permissões para a pasta do Data Lake Store](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp.png "atribuir permissões para a pasta do Data Lake Store")
    
    Clique em **Selecionar**.

    c. Sob **atribuir permissões**, clique em **selecione permissões**. Definir **permissões** ao **ler, escrever,** e **executar**. Definir **adicionar ao** ao **esta pasta e todos os filhos**. Por fim, defina **adicionar como** ao **uma entrada de permissão de acesso e uma entrada de permissão predefinida**.

    ![Atribuir permissões para a pasta do Data Lake Store](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp-folder.png "atribuir permissões para a pasta do Data Lake Store")
    
    Clique em **OK**. 

## <a name="configure-event-hubs-to-capture-data-to-data-lake-store"></a>Configurar os Hubs de eventos para capturar dados que o Data Lake Store

Nesta secção, vai criar um Hub de eventos dentro de um espaço de nomes de Hubs de eventos. Também é configurar o Hub de eventos para capturar dados para uma conta do Azure Data Lake Store. Esta secção assume que já criou um espaço de nomes de Hubs de eventos.

1. Partir do **descrição geral** painel de espaço de nomes dos Hubs de eventos, clique em **+ Hub de eventos**.

    ![Criar Hub de eventos](./media/data-lake-store-archive-eventhub-capture/data-lake-store-create-event-hub.png "criar Hub de eventos")

1. Forneça os valores seguintes para configurar os Hubs de eventos para capturar dados que o Data Lake Store.

    ![Criar Hub de eventos](./media/data-lake-store-archive-eventhub-capture/data-lake-store-configure-eventhub.png "criar Hub de eventos")

    a. Forneça um nome para o Hub de eventos.
    
    b. Para este tutorial, defina **contagem de partições** e **retenção de mensagens** para os valores predefinidos.
    
    c. Definir **capturar** ao **no**. Definir o **janela de tempo** (como com frequência capturar) e **janela de tamanho** (tamanho de dados para capturar). 
    
    d. Para **capturar fornecedor**, selecione **do Azure Data Lake Store** e selecionar o Store de Lake de dados que criou anteriormente. Para **caminho do Data Lake**, introduza o nome da pasta que criou na conta do Data Lake Store. Apenas terá de fornecer o caminho relativo para a pasta.

    e. Deixe o **formatos de nome de arquivo de captura de exemplo** para o valor predefinido. Esta opção controla a estrutura de pastas criada sob a pasta de captura.

    f. Clique em **Criar**.

## <a name="test-the-setup"></a>A configuração de teste

Agora pode testar a solução ao enviar dados para o Hub de eventos do Azure. Siga as instruções em [enviar eventos para Hubs de eventos](../event-hubs/event-hubs-dotnet-framework-getstarted-send.md). Depois de começar a enviar os dados, verá os dados serão refletidos no Data Lake Store com a estrutura de pasta que especificou. Por exemplo, verá uma estrutura de pastas, como mostrado na captura de ecrã seguinte, em sua Store de Lake de dados.

![Exemplo de dados de EventHub no Data Lake Store](./media/data-lake-store-archive-eventhub-capture/data-lake-store-eventhub-data-sample.png "dados de EventHub de exemplo no Data Lake Store")

> [!NOTE]
> Mesmo que não tem mensagens recebidas nos Hubs de eventos, os Hubs de eventos escreve ficheiros vazios com apenas os cabeçalhos para a conta do Data Lake Store. Os ficheiros são escritos no mesmo intervalo de tempo que forneceu ao criar os Hubs de eventos.
> 
>

## <a name="analyze-data-in-data-lake-store"></a>Analise dados no Data Lake Store

Quando os dados estiverem no Data Lake Store, pode executar tarefas analíticas para o processo e processe os dados. Ver [u-SQL Avro exemplo](https://github.com/Azure/usql/tree/master/Examples/AvroExamples) sobre como fazer isto com o Azure Data Lake Analytics.
  

## <a name="see-also"></a>Consulte também
* [Secure data in Data Lake Store (Proteger dados no Data Lake Store)](data-lake-store-secure-data.md)
* [Copiar dados dos Blobs de armazenamento do Azure para o Data Lake Store](data-lake-store-copy-data-azure-storage-blob.md)
