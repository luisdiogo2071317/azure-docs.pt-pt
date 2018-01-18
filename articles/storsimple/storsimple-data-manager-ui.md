---
title: IU do Gestor de dados do Microsoft Azure StorSimple | Microsoft Docs
description: "Descreve como utilizar o serviço do Gestor de dados do StorSimple da IU"
services: storsimple
documentationcenter: NA
author: alkohli
manager: jeconnoc
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 01/16/2018
ms.author: alkohli
ms.openlocfilehash: d704cf8e6840c6a7b0a637c404d421f9f1497c46
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/17/2018
---
# <a name="manage-the-storsimple-data-manager-service-in-azure-portal"></a>Gerir o serviço StorSimple Manager de dados no portal do Azure

Este artigo explica como pode utilizar a IU do Gestor de dados do StorSimple para transformar os dados que residem nos dispositivos de série 8000 do StorSimple. Os dados transformados, em seguida, podem ser utilizados por outros serviços do Azure, tais como os Media Services do Azure, Azure HDInsight, do Azure Machine Learning e da Azure Search.


## <a name="use-storsimple-data-transformation"></a>Utilizar transformação de dados do StorSimple

O Gestor de dados do StorSimple é o recurso dentro do quais dados transformação é instanciada. O serviço de transformação de dados permite-lhe transformar dados do formato do StorSimple para o formato nativo em blobs ou ficheiros do Azure. Para transformar os dados de formato nativo do StorSimple, terá de especificar os detalhes sobre o dispositivo de série 8000 do StorSimple e os dados de interesse que pretende transformar.

### <a name="create-a-storsimple-data-manager-service"></a>Criar um serviço StorSimple Manager de dados

Execute os seguintes passos para criar um serviço StorSimple Manager de dados.

1. Utilize as credenciais da sua conta Microsoft para iniciar sessão no [portal do Azure](https://portal.azure.com/).

2. Clique em **+ criar um recurso** e pesquisa para o Gestor de dados do StorSimple.

    ![Criar um serviço StorSimple Manager de dados 1](./media/storsimple-data-manager-ui/create-service-1.png)

3. Clique em Gestor de dados do StorSimple e, em seguida, clique em **criar**.
    
    ![Criar um serviço StorSimple Manager de dados 2](./media/storsimple-data-manager-ui/create-service-3.png)

3. Para o novo serviço, especifique o seguinte:

    1. Forneça um exclusivo **nome do serviço** para o Gestor de dados do StorSimple. Este é um nome amigável que pode ser utilizado para identificar o serviço. O nome pode ter entre 3 e 24 carateres que podem ser letras, números e hífenes. O nome tem de começar e terminar com uma letra ou um número.

    2. Escolha um **subscrição** na lista pendente. A subscrição está ligada à sua conta de faturação. Este campo é automaticamente preenchidos (e não selecionável) se tiver apenas uma subscrição.

    3. Selecione um grupo de recursos existente ou crie um novo grupo. Para obter mais informações, veja [Azure resource groups](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-infrastructure-resource-groups-guidelines/) (Grupos de recursos do Azure).

    4. Especifique o **localização** para o seu serviço que aloja as contas do storage e o serviço StorSimple Manager de dados. O serviço do Gestor de dispositivos do StorSimple, serviço Gestor de dados e a conta de armazenamento associado devem ser em regiões suportadas.
    
    5. Para obter uma ligação para este serviço no seu dashboard, selecione **afixar ao dashboard**.
    
    6. Clique em **Criar**.

    ![Criar um serviço StorSimple Manager de dados 3](./media/storsimple-data-manager-ui/create-service-4.png)

A criação do serviço demora alguns minutos. Verá uma notificação depois do serviço é criado com êxito e é apresentado o novo serviço.

### <a name="create-a-data-transformation-job-definition"></a>Criar uma definição de tarefa de transformação de dados

Dentro de um serviço do Gestor de dados do StorSimple, terá de criar uma definição de tarefa de transformação de dados. Uma definição de tarefa Especifica os detalhes dos dados StorSimple que está interessado em Mover para uma conta do storage no formato nativo. Depois de criar uma definição de tarefa, em seguida, pode executar esta tarefa novamente com as definições de tempo de execução diferente.

Execute os seguintes passos para criar uma definição de tarefa.

1. Navegue para o serviço que criou. Aceda a **gestão > definições da tarefa**.

2. Clique em **+ definição de tarefa**.

    ![Clique em + definição de tarefa](./media/storsimple-data-manager-ui/create-job-definition-1.png)

3. Forneça um nome para a sua definição de tarefa. O nome pode ter entre 3 e 63 carateres. O nome pode conter letras maiúsculas e minúsculas, números e hífenes.

4. Especifique uma localização onde é executada a tarefa. Esta localização pode ser diferente da localização onde o serviço é implementado.

5. Clique em **origem** para especificar o repositório de dados de origem.

    ![Configurar o repositório de dados de origem](./media/storsimple-data-manager-ui/create-job-definition-2.png)

6. Uma vez que este é um novo serviço do Gestor de dados, não repositórios de dados estão configurados. No **configurar origem de dados**, especifique os detalhes do seu dispositivo de série 8000 do StorSimple e os dados de interesse.

   Para adicionar o seu dispositivo do StorSimple Manager como um repositório de dados, clique em **adicionar novo** na lista pendente do repositório de dados e, em seguida, clique em **adicionar repositório de dados**.

    ![Adicionar novo repositório de dados](./media/storsimple-data-manager-ui/create-job-definition-3.png)
  
    1. Escolha **série 8000 do StorSimple Manager** como o tipo de dados de repositório.
    
    2. Introduza um nome amigável para o repositório de dados de origem.
    
    3. Na lista pendente, escolha uma subscrição associada com o serviço do Gestor de dispositivos do StorSimple.
    
    4. Forneça o nome do Gestor de dispositivos do StorSimple para os **recursos**.

    5. Introduza o **encriptação de dados do serviço** chave para o serviço do Gestor de dispositivos do StorSimple. 

    ![Configurar o repositório de dados de origem 1](./media/storsimple-data-manager-ui/create-job-definition-4.png)

    Clique em **OK** quando terminar. Isto poupa o seu repositório de dados. Reutilize este Gestor de dispositivos do StorSimple nas outras definições da tarefa sem introduzir estes parâmetros novamente. Demora alguns segundos depois de clicar em **OK** para o repositório de dados de origem criado recentemente a aparecer na lista pendente.

7. Na lista pendente para **repositório de dados**, selecione o repositório de dados que criou. 

    1. Introduza o nome do dispositivo de série 8000 do StorSimple que contém os dados de interesse.

    2. Especifique o nome do volume que reside no dispositivo StorSimple que tem os dados de interesse.

    3. No **filtro** subsecção, introduza o diretório de raiz que contenha os dados de interesse no _\MyRootDirectory\Data_ formato. Letras de unidade como _\C:\Data_ não são suportadas. Também pode adicionar aqui quaisquer filtros de ficheiro.

    4. O serviço de transformação de dados funciona nos dados que são enviados para o Azure através de instantâneos. Quando executar esta tarefa, pode optar por colocar uma cópia de segurança de cada vez que esta tarefa é executada (para trabalhar em dados mais recentes) ou utilizar a última cópia de segurança existente na nuvem (se estiver a trabalhar em alguns dados arquivados).

    5. Clique em **OK**.

    ![Configurar o repositório de dados de origem 2](./media/storsimple-data-manager-ui/create-job-definition-8.png)

8. Em seguida, o repositório de dados de destino tem de ser configurado. Escolha contas do storage para colocar ficheiros em blobs nessa conta. Na lista pendente, selecione **adicionar novo** e, em seguida, **configurar as definições**.

9. Selecione o tipo do repositório de destino que pretende adicionar e os outros parâmetros associados com o repositório.

    Se selecionar um destino de tipo de conta de armazenamento, pode especificar um nome amigável, a subscrição (escolher o mesmo que o serviço ou outros) e uma conta de armazenamento.
        ![Configurar o repositório de dados de destino 1](./media/storsimple-data-manager-ui/create-job-definition-10.png)

    Uma fila de armazenamento é criada quando a tarefa é executada. Esta fila é preenchida com mensagens sobre os blobs transformados, à medida que ficam prontos. O nome desta fila é igual ao nome da definição da tarefa.
    
10. Depois de adicionar o repositório de dados, aguarde alguns minutos.
    
    1. Selecione o repositório que criou como o destino na lista pendente no **nome da conta de destino**.

    2. Escolha o tipo de armazenamento como blobs ou ficheiros. Especifique o nome do contentor de armazenamento onde os dados transformados residem. Clique em **OK**.

        ![Configurar a conta de armazenamento do repositório de dados de destino](./media/storsimple-data-manager-ui/create-job-definition-16.png)

11. Também pode selecionar a opção para apresentar uma estimativa da duração de tarefa antes de executar a tarefa. Clique em **OK** para criar a definição de tarefa. A definição da tarefa está agora concluída. Pode utilizar esta definição de tarefa várias vezes através da IU com definições de tempo de execução diferente.

    ![Definição de tarefa concluídos](./media/storsimple-data-manager-ui/create-job-definition-13.png)

    A definição de tarefa criado recentemente, é adicionada à lista de definições de tarefas para este serviço.

### <a name="run-the-job-definition"></a>Executar a definição de tarefa

Sempre que precisar de mover dados do StorSimple para a conta de armazenamento que especificou na definição de tarefa, tem de executá-la. Em runtime, alguns parâmetros podem ser especificados de forma diferente. Os passos são os seguintes:

1. Selecione o serviço StorSimple Manager de dados e aceda a **gestão > definições da tarefa**. Selecione e clique na definição de tarefa que pretende executar.
     
     ![Iniciar a tarefa executar 1](./media/storsimple-data-manager-ui/start-job-run1.png)

2. Clique em **executar agora**.
     
     ![Iniciar a tarefa executar 2](./media/storsimple-data-manager-ui/start-job-run2.png)

3. Clique em **definições de execução** para modificar as definições que poderá pretender alterar esta execução da tarefa. Clique em **OK** e, em seguida, clique em **executar** para iniciar a tarefa.

    ![Iniciar a tarefa executar 3](./media/storsimple-data-manager-ui/start-job-run3.png)

4. Para monitorizar esta tarefa, aceda a **tarefas** no Gestor de dados do StorSimple. Para além de monitorização no **tarefas** painel, pode também escutar a fila de armazenamento onde uma mensagem é adicionada sempre que um ficheiro é movido do StorSimple para a conta de armazenamento.

    ![Iniciar a tarefa executar 4](./media/storsimple-data-manager-ui/start-job-run4.png)


## <a name="next-steps"></a>Passos Seguintes

[Utilizar o SDK do .NET para iniciar o Gestor de dados do StorSimple tarefas](storsimple-data-manager-dotnet-jobs.md).