---
title: Procurar e gerir recursos de armazenamento utilizando o Server Explorer | Documentos da Microsoft
description: Procurar e gerir recursos de armazenamento ao utilizar o Explorador de servidores
services: visual-studio-online
author: ghogen
manager: douge
assetId: 658dc064-4a4e-414b-ae5a-a977a34c930d
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 8/24/2017
ms.author: ghogen
ms.openlocfilehash: 74f5508586d073bcccc54894cce6fcde1b83fe18
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/18/2018
ms.locfileid: "42056047"
---
# <a name="browse-and-manage-storage-resources-by-using-server-explorer"></a>Procurar e gerir recursos de armazenamento ao utilizar o Explorador de servidores

[!INCLUDE [storage-try-azure-tools](../includes/storage-try-azure-tools.md)]

## <a name="overview"></a>Descrição geral

Se instalou ferramentas do Azure para Microsoft Visual Studio, pode ver dados de blob, fila e tabela das suas contas de armazenamento para o Azure. O Azure **armazenamento** nó no Server Explorer mostra os dados que está na sua conta do emulador de armazenamento local e as outras contas de armazenamento do Azure.

Para ver o Explorador de servidores no Visual Studio, na barra de menus, selecione **View** > **Explorador de servidores**. O **armazenamento** nó mostra todas as contas de armazenamento que existem em cada subscrição do Azure ou o certificado que está ligado a. Se a sua conta de armazenamento não aparecer, pode adicioná-lo ao seguir as instruções [mais adiante neste artigo](#add-storage-accounts-by-using-server-explorer).

A partir do Azure SDK 2.7, pode também utilizar o Cloud Explorer para ver e gerir recursos do Azure. Para obter mais informações, consulte [recursos de gerir o Azure com o Cloud Explorer](vs-azure-tools-resources-managing-with-cloud-explorer.md).

## <a name="view-and-manage-storage-resources-in-visual-studio"></a>Ver e gerir recursos de armazenamento no Visual Studio

Explorador de servidores automaticamente mostra uma lista de blobs, filas e tabelas na sua conta do emulador de armazenamento. A conta de emulador de armazenamento está listada no Explorador de servidores sob o **armazenamento** nó como o **desenvolvimento** nó.

Para ver os recursos da conta de emulador de armazenamento, expanda o **desenvolvimento** nó. Se o emulador de armazenamento ainda não foi iniciado, expandindo a **desenvolvimento** nó, ele inicia automaticamente. Este processo pode demorar vários segundos. Pode continuar a trabalhar em outras áreas do Visual Studio, enquanto inicia o emulador de armazenamento.

Para ver os recursos numa conta de armazenamento, expanda o nó da conta de armazenamento no Explorador de servidores, onde vir **Blobs**, **filas**, e **tabelas** nós.

## <a name="work-with-blob-resources"></a>Trabalhar com recursos de blob

O **Blobs** nó apresenta uma lista de contentores para a conta de armazenamento selecionada. Contentores de BLOBs contém ficheiros de blob, e pode organizar estas blobs em pastas e subpastas. Para obter mais informações, consulte [como utilizar o armazenamento de Blobs do .NET](storage/blobs/storage-dotnet-how-to-use-blobs.md).

### <a name="to-create-a-blob-container"></a>Para criar um contentor de BLOBs

1. Abra o menu de atalho para o **Blobs** nó e, em seguida, selecione **criar contentor de BLOBs**.
1. Na **criar contentor de BLOBs** caixa de diálogo, introduza o nome do novo contentor.  
1. Selecione Enter no teclado, ou clique ou toque fora o campo de nome para guardar o contentor de Blobs.

   > [!NOTE]
   > O nome do contentor de blob tem de começar com uma letra minúscula (a-z) ou um número (0-9).

### <a name="to-delete-a-blob-container"></a>Para eliminar um contentor de BLOBs

Abra o menu de atalho para o contentor de BLOBs que pretende remover e, em seguida, selecione **eliminar**.

### <a name="to-display-a-list-of-the-items-in-a-blob-container"></a>Para apresentar uma lista dos itens num contentor de BLOBs

Abra o menu de atalho para um nome de contentor do blob na lista e, em seguida, selecione **aberto**.

Quando exibe o conteúdo de um contentor de BLOBs, aparece um separador conhecido como o modo de exibição do contentor de Blobs.

![Vista do contentor de BLOBs](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC749016.png)

Pode executar as seguintes operações nos blobs ao utilizar os botões no canto superior direito da vista do contentor de BLOBs:

* Introduza um valor de filtro e aplicá-la.
* Atualize a lista de blobs no contentor.
* Carregar um ficheiro.
* Elimine um blob. (A eliminar um ficheiro de um contentor de BLOBs não elimina o ficheiro subjacente. Ele apenas remove do contentor de Blobs.)
* Abra um blob.
* Guarde um blob para o computador local.

### <a name="to-create-a-folder-or-subfolder-in-a-blob-container"></a>Para criar uma pasta ou subpasta num contentor de BLOBs

1. Escolha o contentor de BLOBs no Cloud Explorer. Na janela do contentor, selecione o **carregar Blob** botão.

1. Na **carregar o ficheiro novo** caixa de diálogo, selecione a **procurar** botão para especificar o ficheiro que pretende carregar e, em seguida, introduza um nome de pasta no **pasta (opcional)** caixa.

   ![Carregar um ficheiro para uma pasta de BLOBs](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766037.png)

   Pode adicionar subpastas em pastas do contentor, seguindo o mesmo passo. Se não especificar um nome de pasta, o ficheiro é carregado para o nível superior do contentor de Blobs. O ficheiro é apresentada na pasta especificada no contentor.

   ![Pasta adicionada a um contentor de BLOBs](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766038.png)

1. Faça duplo clique na pasta ou selecione Enter para ver o conteúdo da pasta. Quando estiver na pasta do contentor, pode voltar para a raiz do contentor, selecionando o **Open Directory principal** botão (seta).

### <a name="to-delete-a-container-folder"></a>Para eliminar uma pasta de contentor

Elimine todos os ficheiros na pasta.

Como as pastas em contentores de BLOBs são pastas virtuais, não é possível criar uma pasta vazia. Também não é possível eliminar uma pasta para eliminar o seu conteúdo do ficheiro, mas em vez disso, tem de eliminar todo o conteúdo de uma pasta a eliminar a pasta em si.

### <a name="to-filter-blobs-in-a-container"></a>Para filtrar os blobs num contentor

Pode filtrar os blobs que são apresentados ao especificar um prefixo comum.

Por exemplo, se introduzir o prefixo **hello** na caixa de texto de filtro e, em seguida, selecione a **Execute** (**!**) aparecem do botão, apenas os blobs que começam com "Olá".

![Caixa de texto de filtro](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC519076.png)

A caixa de texto de filtro diferencia maiúsculas de minúsculas e não suporta a filtragem com carateres universais. BLOBs podem ser filtrados apenas pelo prefixo. O prefixo pode incluir um delimitador, se estiver a utilizar um delimitador para organizar os blobs numa hierarquia virtual. Por exemplo, a filtragem no prefixo "HelloFabric /" devolve todos os blobs que começam com essa cadeia de caracteres.

### <a name="to-download-blob-data"></a>Para transferir dados de blob

No Cloud Explorer, use qualquer um dos seguintes métodos:

* Abra o menu de atalho para um ou mais blobs e, em seguida, selecione **aberto**.
* Escolha o nome do blob e, em seguida, selecione o **aberto** botão.
* Faça duplo clique no nome do blob.

O progresso de um download de blob é apresentado na **registo de atividades do Azure** janela.

O blob é aberto no editor predefinido para esse tipo de ficheiro. Se o sistema operativo reconhece o tipo de ficheiro, o ficheiro abre-se num aplicativo instalado localmente. Caso contrário, lhe for pedido para escolher uma aplicação que é adequada para o tipo de ficheiro do blob. O ficheiro local que é criado quando transferir um blob é marcado como só de leitura.

Dados de BLOBs são armazenados em cache localmente e verificados relativamente ao tempo de última modificação do blob no armazenamento de Blobs do Azure. Se o blob foi atualizado, uma vez que este foi transferido a última, ele é transferido novamente. Caso contrário, o blob carregado a partir de disco local.

Por predefinição, um blob é transferido para um diretório temporário. Para transferir blobs para um diretório específico, abra o menu de atalho para os nomes de blob selecionados e selecione **guardar como**. Quando guardar um blob dessa maneira, não é possível abrir o ficheiro de blob e o ficheiro local é criado com atributos de leitura/escrita.

### <a name="to-upload-blobs"></a>Para carregar blobs

Para carregar blobs, selecione o **carregar Blob** botão quando o contentor está aberto para visualização na vista de contentor de Blobs.

Pode escolher um ou mais ficheiros para carregar e pode carregar ficheiros de qualquer tipo. O **registo de atividades do Azure** janela mostra o progresso do carregamento. Para obter mais informações sobre como trabalhar com dados de BLOBs, veja [como utilizar o armazenamento de Blobs do Azure no .NET](http://go.microsoft.com/fwlink/p/?LinkId=267911).

### <a name="to-view-logs-transferred-to-blobs"></a>Para ver os registos transferidos para blobs

Se estiver a utilizar o diagnóstico do Azure para registar dados a partir da sua aplicação do Azure e transferiu os registos para a sua conta de armazenamento, verá contentores Azure criado para estes registos. Ver estes registos no Explorador de servidores é uma forma fácil de identificar problemas com a sua aplicação, especialmente se for foi implementada no Azure.

Para obter mais informações sobre o diagnóstico do Azure, consulte [recolher dados do registo de diagnóstico do Azure utilizando](https://msdn.microsoft.com/library/azure/gg433048.aspx).

### <a name="to-get-the-url-for-a-blob"></a>Para obter o URL para um blob

Abra o menu de atalho do blob e, em seguida, selecione **URL de cópia**.

### <a name="to-edit-a-blob"></a>Para editar um blob

Selecione o blob e, em seguida, selecione o **BLOBs aberto** botão.

O ficheiro é transferido para uma localização temporária e aberto no computador local. Carregar blob novamente depois de efetuar alterações.

## <a name="work-with-queue-resources"></a>Trabalhar com recursos de fila

Filas de serviços de armazenamento estão alojadas numa conta de armazenamento do Azure. Pode usá-los para permitir que a sua cloud funções do serviço comunicar entre si e com outros serviços, um mecanismo de passagem de mensagens. Pode acessar a fila por meio de programação, por meio de um serviço em nuvem e ao longo de um serviço da web para clientes externos. Também pode aceder a fila diretamente, usando o Server Explorer no Visual Studio.

Ao desenvolver um serviço cloud que utiliza as filas, pode querer utilizar o Visual Studio para criar filas e trabalhar com eles interativamente enquanto desenvolve e testar o seu código.

No Explorador de servidores, pode ver as filas numa conta de armazenamento, criar e eliminar filas, abrir uma fila para ver suas mensagens e adicionar mensagens a uma fila. Quando abre uma fila para visualização, pode ver as mensagens individuais e pode efetuar as seguintes ações na fila utilizando os botões no canto superior esquerdo:

* Atualize a vista da fila.
* Adicione uma mensagem à fila.
* Remover da fila a mensagem mais superior.
* Limpe a fila completa.

A imagem seguinte mostra uma fila que contém duas mensagens:

![Visualização de uma fila](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC651470.png)

Para obter mais informações sobre o armazenamento de filas de serviços, consulte [introdução ao armazenamento de filas do Azure com o .NET](http://go.microsoft.com/fwlink/?LinkID=264702). Para obter informações sobre o serviço web para filas de serviços de armazenamento, consulte [conceitos do serviço de fila](http://go.microsoft.com/fwlink/?LinkId=264788). Para obter informações sobre como enviar mensagens a uma fila de serviços de armazenamento com o Visual Studio, consulte [envio de mensagens numa fila de serviços de armazenamento](https://docs.microsoft.com/azure/visual-studio/vs-storage-cloud-services-getting-started-queues).

> [!NOTE]
> Filas de serviços de armazenamento são diferentes dos filas de Azure Service Bus. Para obter mais informações sobre as filas do Service Bus, consulte [filas do Service Bus, tópicos e subscrições](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-queues-topics-subscriptions).

## <a name="work-with-table-resources"></a>Trabalhar com recursos de tabela

O armazenamento de Tabelas do Azure armazena grandes quantidades de dados estruturados. O serviço é um arquivo de dados NoSQL que aceita chamadas autenticadas de dentro e fora da cloud do Azure. As tabelas do Azure são ideais para armazenar dados estruturados não relacionais.

### <a name="to-create-a-table"></a>Para criar uma tabela

1. No Cloud Explorer, selecione o **tabelas** nó da conta de armazenamento e, em seguida, selecione **Create Table**.
1. Na **Create Table** caixa de diálogo, introduza um nome para a tabela.

### <a name="to-view-table-data"></a>Para ver os dados de tabela

1. No Cloud Explorer, abra a **Azure** nó e, em seguida, abra o **armazenamento** nó.
1. Abra o nó de conta de armazenamento que está interessado e, em seguida, abra a **tabelas** nó para ver uma lista de tabelas para a conta de armazenamento.
1. Abra o menu de atalho para uma tabela e, em seguida, selecione **vista de tabela**.

    ![Uma tabela do Azure no Explorador de soluções](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC744165.png)

A tabela é organizada, entidades (mostradas em linhas) e propriedades (mostradas nas colunas). Por exemplo, a ilustração seguinte mostra as entidades indicadas no Designer de tabela.

### <a name="to-edit-table-data"></a>Para editar os dados da tabela

No Designer de tabela, abra o menu de atalho para uma entidade (uma única linha) ou uma propriedade (uma única célula) e, em seguida, selecione **editar**.

    ![Add or edit a table entity](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC656238.png)

Entidades numa única tabela não são necessárias para ter o mesmo conjunto de propriedades (colunas). Tenha em atenção as seguintes restrições no visualizar e editar dados de tabela:

* Não é possível ver ou editar dados binários (`type byte[]`), mas pode armazená-lo numa tabela.
* Não é possível editar a **PartitionKey** ou **RowKey** valores, porque o armazenamento de tabelas no Azure não suporta essa operação.
* Não é possível criar uma propriedade chamada **Timestamp**. Serviços de armazenamento do Azure utilizam uma propriedade com esse nome.
* Se introduzir um **DateTime** valor, tem de seguir um formato adequado para as definições de idioma e região do seu computador (por exemplo, DD/MM/AAAA HH: mm: [AM | PM] para inglês dos Estados Unidos).

### <a name="to-add-entities"></a>Adicionar entidades

1. No Designer de tabela, selecione o **adicionar entidade** botão.

    ![Adicionar botão de entidade](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC655336.png)

1. Na **adicionar entidade** caixa de diálogo, introduza os valores da **PartitionKey** e **RowKey** propriedades.

    ![Adicionar a caixa de diálogo de entidade](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC655335.png)

    Introduza os valores com cuidado. Não é possível alterá-los depois de fechar a caixa de diálogo a menos que a eliminar a entidade e adicioná-lo novamente.

### <a name="to-filter-entities"></a>Para filtrar as entidades

Pode personalizar o conjunto de entidades que aparecem numa tabela, se usar o construtor de consultas.

1. Para abrir o construtor de consultas, abra uma tabela para visualização.
1. Selecione o **Query Builder** botão na barra de ferramentas da exibição de tabela.

    O **Query Builder** é apresentada a caixa de diálogo. A ilustração seguinte mostra uma consulta que está sendo criada no construtor de consultas.

    ![Construtor de consultas](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC652231.png)
1. Quando concluir a criação de consulta, feche a caixa de diálogo. O formato de texto resultante da consulta é apresentado numa caixa de texto como um filtro de WCF Data Services.
1. Para executar a consulta, selecione o ícone do triângulo verde.

Também pode filtrar os dados de entidades que aparece no Designer de tabela se introduzir uma cadeia de filtro de WCF Data Services diretamente na caixa de texto de filtro. Esse tipo de cadeia de caracteres é semelhante a uma cláusula WHERE de SQL, mas é enviado para o servidor como um pedido HTTP. Para obter informações sobre como construir as cadeias de caracteres de filtro, consulte [Constructing cadeias de caracteres de filtro para o designer de tabela](https://docs.microsoft.com/azure/vs-azure-tools-table-designer-construct-filter-strings).

A ilustração seguinte mostra um exemplo de uma cadeia de filtro válido:

![Cadeia de filtro](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC655337.png)

## <a name="refresh-storage-data"></a>Atualizar os dados de armazenamento

Ao Explorador de servidores se conecta ao ou obtém dados a partir de uma conta de armazenamento, a operação poderá demorar até um minuto para concluir. Se não conseguir ligar o Explorador de servidores, a operação poderá tempo limite. Enquanto os dados são recuperados, pode continuar a trabalhar em outras partes do Visual Studio. Para cancelar a operação se está a demorar demasiado tempo, selecione o **parar atualizar** botão na barra de ferramentas do Explorador de servidores.

### <a name="to-refresh-blob-container-data"></a>Para atualizar os dados de contentor de BLOBs

* Selecione o **Blobs** nó abaixo **armazenamento**e, em seguida, selecione o **atualizar** botão na barra de ferramentas do Explorador de servidores.
* Para atualizar a lista de blobs, que é apresentada, selecione o **Execute** botão.

### <a name="to-refresh-table-data"></a>Para atualizar os dados de tabela

* Selecione o **tabelas** nó abaixo **armazenamento**e, em seguida, selecione o **atualizar** botão na barra de ferramentas do Explorador de servidores.
* Para atualizar a lista de entidades que é apresentada no Designer de tabela, selecione o **Execute** botão no Designer de tabela.

### <a name="to-refresh-queue-data"></a>Para atualizar os dados de fila

Selecione o **filas** nó abaixo **armazenamento**e, em seguida, selecione o **atualizar** botão na barra de ferramentas do Explorador de servidores.

### <a name="to-refresh-all-items-in-a-storage-account"></a>Para atualizar todos os itens numa conta de armazenamento

Escolha o nome da conta e, em seguida, selecione o **atualizar** botão na barra de ferramentas do Explorador de servidores.

## <a name="add-storage-accounts-by-using-server-explorer"></a>Adicionar contas de armazenamento ao utilizar o Explorador de servidores

Existem duas formas de adicionar contas de armazenamento ao utilizar o Explorador de servidores. Pode criar uma conta de armazenamento na sua subscrição do Azure, ou pode anexar uma conta de armazenamento existente.

### <a name="to-create-a-storage-account-by-using-server-explorer"></a>Para criar uma conta de armazenamento utilizando o Explorador de servidores

1. No Explorador de servidores, abra o menu de atalho para o **armazenamento** nó e, em seguida, selecione **criar conta de armazenamento**.

1. Na **criar conta de armazenamento** diálogo caixa, selecione ou introduza as seguintes informações:

   * A subscrição do Azure para o qual pretende adicionar a conta de armazenamento.
   * O nome que pretende utilizar para a nova conta de armazenamento.
   * A região ou o grupo de afinidade, (por exemplo, E.U.A. oeste ou Leste asiático).
   * O tipo de replicação que pretende utilizar para a conta de armazenamento, tais como localmente redundante.

   ![Criar uma conta do Storage do Azure](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC744166.png)

1. Selecione **Criar**.

A nova conta de armazenamento é apresentado na **armazenamento** lista no Explorador de soluções.

### <a name="to-attach-an-existing-storage-account-by-using-server-explorer"></a>Para anexar uma conta de armazenamento existente com o Explorador de servidores

1. No Explorador de servidores, abra o menu de atalho para o Azure **armazenamento** nó e, em seguida, selecione **anexar armazenamento externo**.

    ![Adicionar uma conta de armazenamento existente](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766039.png)
1. Na **criar conta de armazenamento** diálogo caixa, selecione ou introduza as seguintes informações:

   * O nome da conta de armazenamento existente que pretende anexar.
   * A chave para a conta de armazenamento selecionada. Este valor é, geralmente, fornecido por ao selecionar uma conta de armazenamento. Se pretender que o Visual Studio lembrar-se a chave de conta de armazenamento, selecione o **Lembre-se a chave da conta** caixa de verificação.
   * O protocolo a utilizar para ligar à conta de armazenamento, como HTTP, HTTPS ou um ponto final personalizado. Para obter mais informações sobre os pontos finais personalizados, consulte [como configurar as cadeias de ligação](https://msdn.microsoft.com/library/azure/ee758697.aspx).

### <a name="to-view-the-secondary-endpoints"></a>Para ver os pontos de extremidade secundários

Se tiver criado uma conta de armazenamento utilizando o **Georredundante com acesso de leitura** opção de replicação, pode ver os seus pontos de extremidade secundários ao abrir o menu de atalho para o nome da conta e, em seguida, selecione **propriedades**.

![Pontos finais secundários de armazenamento](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766040.png)

### <a name="to-remove-a-storage-account-from-server-explorer"></a>Para remover uma conta de armazenamento no Explorador de servidores

No Explorador de servidores, abra o menu de atalho para o nome da conta e, em seguida, selecione **eliminar**. 

Se eliminar uma conta de armazenamento, quaisquer informações da chave guardadas para essa conta também são removidas.

Se eliminar uma conta de armazenamento no Explorador de servidores, não afeta a sua conta de armazenamento ou de todos os dados nele contidos. Ela simplesmente retira a referência no Explorador de servidores. Para eliminar permanentemente uma conta de armazenamento, utilize o [portal do Azure](https://portal.azure.com/).

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre como utilizar os serviços de armazenamento do Azure, veja [aceder aos serviços de armazenamento do Azure](https://msdn.microsoft.com/library/azure/ee405490.aspx).
