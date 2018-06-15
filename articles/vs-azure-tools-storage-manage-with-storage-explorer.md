---
title: Introdução ao Explorador de armazenamento | Microsoft Docs
description: Gerir recursos de armazenamento do Azure com o Explorador de armazenamento
services: storage
documentationcenter: na
author: cawa
manager: paulyuk
editor: ''
ms.assetid: 1ed0f096-494d-49c4-ab71-f4164ee19ec8
ms.service: storage
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/17/2017
ms.author: cawa
ms.openlocfilehash: 2335872bcd7d3ea64e449d8b1a43f360d86bb4a0
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/17/2018
ms.locfileid: "34304635"
---
# <a name="get-started-with-storage-explorer"></a>Introdução ao Explorador de armazenamento
## <a name="overview"></a>Descrição geral
Explorador de armazenamento do Azure é uma aplicação autónoma que lhe permite trabalhar facilmente com dados de armazenamento do Azure no Windows, macOS e Linux. Neste artigo, saiba várias formas de ligar e gerir as contas do storage do Azure.

![Explorador de Armazenamento do Microsoft Azure][0]

## <a name="prerequisites"></a>Pré-requisitos

# <a name="windowstabwindows"></a>[Windows](#tab/windows)
Explorador de armazenamento do Azure é suportado nas seguintes versões do Windows:

* Windows 10 (recomendado)
* Windows 8
* Windows 7

Para todas as versões do Windows, .NET Framework 4.6.2 ou superior necessário.

[Transferir e instalar o Explorador de Armazenamento](http://www.storageexplorer.com)

# <a name="macostabmacos"></a>[macOS](#tab/macos)
Explorador de armazenamento do Azure é suportado nos seguintes versões do macOS:

* macOS 10.12 "Sierra" e versões posteriores

[Transferir e instalar o Explorador de Armazenamento](http://www.storageexplorer.com)

# <a name="linuxtablinux"></a>[Linux](#tab/linux)
Explorador de armazenamento do Azure é suportado a distros seguintes do Linux:

* Ubuntu 16.04 x64 (recomendado)
* Ubuntu 17.10 x64
* Ubuntu 14.04 x64

Explorador de armazenamento do Azure pode funcionar em outros distros, mas os únicos listados acima são suportados oficialmente.

Também tem de ter as seguintes dependências/bibliotecas de instalada para ser executada Exploer de armazenamento do Azure no Linux:

* [.NET core 2. x](https://docs.microsoft.com/dotnet/core/linux-prerequisites?tabs=netcore2x)
* libsecret (Nota: libsecret 1.so.0 tem de estar disponível no seu computador. Se tiver uma versão diferente do libsecret instalado, pode experimentar soft respetivo ficheiro do SO da associação ao incidente libsecret 1.so.0)
* libgconf-2-4
* GCC atualizado

O Explorador de armazenamento do Azure [notas de versão](https://go.microsoft.com/fwlink/?LinkId=838275&clcid=0x409) contém passos específicos para algumas distros.

[Transferir e instalar o Explorador de Armazenamento](http://www.storageexplorer.com)

---

## <a name="connect-to-a-storage-account-or-service"></a>Ligar a um serviço ou a uma conta do Storage
O Explorador de Armazenamento proporciona várias formas de ligar a contas de armazenamento. Pode, por exemplo:
* Ligar a contas de armazenamento associadas às suas subscrições do Azure.
* Ligar a contas e serviços de armazenamento que são partilhados com outras subscrições do Azure.
* Ligar e gerir o armazenamento local através do Emulador do Armazenamento do Azure. 

Além disso, pode trabalhar com contas de armazenamento no Azure global e nacional:

* [Ligar a uma subscrição do Azure](#connect-to-an-azure-subscription): faça a gestão dos recursos de armazenamento que pertencem à sua subscrição do Azure.
* [Trabalhar com o armazenamento de desenvolvimento local](#work-with-local-development-storage): faça a gestão do armazenamento local com o Emulador do Armazenamento do Azure.
* [Anexar ao armazenamento externo](#attach-or-detach-an-external-storage-account): faça a gestão dos recursos de armazenamento que pertencem a outra subscrição do Azure ou a clouds nacionais do Azure com o nome, a chave e os pontos finais da conta de armazenamento.
* [Anexar uma conta de armazenamento com SAS](#attach-storage-account-using-sas): faça a gestão dos recursos de armazenamento que pertencem a outra subscrição do Azure com uma assinatura de acesso partilhado (SAS).
* [Anexar um serviço com SAS](#attach-service-using-sas): faça a gestão de um serviço de armazenamento específico (contentor de blobs, filas ou tabelas) que pertence a outra subscrição do Azure com SAS.
* [Ligar a uma conta de base de dados do Azure Cosmos utilizando uma cadeia de ligação](#connect-to-an-azure-cosmos-db-account-by-using-a-connection-string): Gerir BD do Cosmos conta através da utilização de uma cadeia de ligação.

## <a name="connect-to-an-azure-subscription"></a>Ligar a uma subscrição do Azure
> [!NOTE]
> Se não tiver uma conta do Azure, pode [inscrever-se numa avaliação gratuita](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) ou [ativar os benefícios de subscritor do Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).
>
>

1. No Explorador de armazenamento, selecione **gerir contas** para ir para o **conta de gestão de painel**.

    ![Gerir contas][1]

2. O painel esquerdo apresenta agora todas as contas do Azure que tiver sessão iniciada. Para ligar a outra conta, selecione **adicionar uma conta**

3. Se pretender iniciar sessão na nuvem nacional ou uma pilha do Azure, clique em de **ambiente do Azure** pendente para selecionar qual em nuvem do Azure que pretende utilizar. Assim que tiver escolhido o seu ambiente, clique em de **iniciar sessão...**  botão. Se está a iniciar sessão com a pilha do Azure, consulte o artigo [Explorador de armazenamento de ligar a uma subscrição do Azure pilha](azure-stack/user/azure-stack-storage-connect-se.md) para obter mais informações.

    ![Inicie sessão na opção][2]

3. Depois de se inscrever com êxito com uma conta do Azure, a conta e as subscrições Azure associadas com essa conta são adicionadas ao painel da esquerda. Selecione as subscrições do Azure que pretende trabalhar com o e, em seguida, selecione **aplicar** (selecionar **todas as subscrições:** alterna entre selecionar todas ou nenhuma das subscrições Azure listadas).

    ![Selecionar subscrições do Azure][3]

    O painel esquerdo apresenta as contas de armazenamento associadas às subscrições do Azure selecionadas.

    ![Subscrições do Azure selecionadas][4]

## <a name="work-with-local-development-storage"></a>Trabalhar com armazenamento de desenvolvimento local
Com o Explorador de armazenamento, pode trabalhar com o armazenamento local utilizando o emulador do Storage do Azure. Esta abordagem permite-lhe simular trabalhar com o Storage do Azure sem ter necessariamente uma conta do storage implementada no Azure, porque a conta de armazenamento está a ser emulada pelo emulador do Storage do Azure.

> [!NOTE]
> Atualmente, o Emulador do Storage do Azure é suportado apenas para o Windows.
>
>

> [!NOTE]
> O emulador do Storage do Azure não suporta partilhas de ficheiros.
>
>

1. No painel esquerdo do Explorador de armazenamento, expanda o **(locais e anexadas)** > **contas do Storage** > **(desenvolvimento)**  >  **Contentores de BLOBs** nós.

    ![Nó de desenvolvimento local][5]

2. Se ainda não instalou o emulador do Storage do Azure, precisará de fazê-lo através de uma barra de informações. Se a barra de informações for apresentada, selecione **Transferir a versão mais recente** e instale o emulador.

    ![Transferir o pedido de Emulador de Armazenamento do Azure][6]

3. Quando o emulador estiver instalado, pode criar e trabalhar com tabelas, filas e blobs locais. Para saber como trabalhar com cada tipo de conta de armazenamento, consulte os guias seguintes:

    * [Gerir recursos do armazenamento de blobs do Azure](vs-azure-tools-storage-explorer-blobs.md)

## <a name="attach-or-detach-an-external-storage-account"></a>Anexar ou desanexar uma conta do Storage externo
Com o Explorador de armazenamento, poderá anexar a contas de armazenamento externo para que as contas do storage possam ser partilhadas facilmente. Esta secção explica como anexar (e desanexar) de contas do Storage externo.

### <a name="get-the-storage-account-credentials"></a>Obter as credenciais da conta do Storage
Para partilhar uma conta de armazenamento externo, o proprietário dessa conta tem primeiro de obter as credenciais (nome de conta e chave) para a conta e, em seguida, partilhar que informações com a pessoa que pretende anexar ao consiga aceder tal conta. Pode obter as credenciais da conta de armazenamento através do portal do Azure, efetuando os seguintes passos:

1. Inicie sessão no [portal do Azure](https://portal.azure.com).

2. Selecione **Procurar**.

3. Selecione **Contas do Storage**.

4. Na lista de **contas do Storage**, selecione a conta de armazenamento pretendido.

5. Em **Definições**, selecione **Chaves de acesso**.

    ![Opção Chaves de acesso][7]

6. Copiar o **nome da conta de armazenamento** e **chave1**.

    ![Chaves de acesso][8]

### <a name="attach-to-an-external-storage-account"></a>Anexar a uma conta do Storage externo
Para anexar a uma conta de armazenamento externa, é necessário o nome e a chave da conta. A secção “Obter as credenciais da conta de armazenamento” explica como obter estes valores a partir do portal do Azure. No entanto, no portal, a chave de conta é designada por **key1**. Por isso, quando o Explorador de armazenamento pede-lhe uma chave de conta, introduza o **chave1** valor.

1. No Explorador de armazenamento, abra o **ligar a caixa de diálogo**.

    ![Ligar à opção de armazenamento do Azure][9]

2. No **ligar a caixa de diálogo**, escolha **utilizar um nome de conta de armazenamento e a chave**

    ![Adicionar com o nome e a opção de chave][10]

3. Cole o nome da sua conta na **nome da conta** texto caixa e cole a chave de conta (o **chave1** valor do portal do Azure) para o **chave da conta** caixa de texto e, em seguida, selecione **Seguinte**.

    ![Nome e a página de chave][11]

    > [!NOTE]
    > Para utilizar um nome e chave de uma nuvem nacional, utilize o **domínio de pontos finais de armazenamento:** pendente para selecionar o domínio de pontos finais adequadas: 
    >
    >

4. Na caixa de diálogo **Resumo da Ligação**, verifique as informações. Se quiser alterar alguma informação, selecione **Voltar** e reintroduza as definições pretendidas. 

5. Selecione **Ligar**.

6. Depois da conta de armazenamento foi anexada com êxito, a conta de armazenamento é apresentada com **(externo)** anexado ao respetivo nome.

    ![Resultado da ligação a uma conta de armazenamento externo][12]

### <a name="detach-from-an-external-storage-account"></a>Desanexar de uma conta do Storage externo
1. Clique com o botão direito do rato na conta de armazenamento externo que pretende desanexar e selecione **Desanexar**.

    ![Anular a exposição da opção de armazenamento][13]

2. Na mensagem de confirmação, selecione **Sim** para confirmar a desanexação da conta de armazenamento externo.

## <a name="attach-a-storage-account-by-using-a-shared-access-signature-sas"></a>Anexe uma conta do storage utilizando uma assinatura de acesso partilhado (SAS)
A assinatura de acesso partilhado, ou [SAS](storage/common/storage-dotnet-shared-access-signature-part-1.md), permite que o administrador de uma subscrição do Azure, conceder acesso temporário a uma conta de armazenamento sem ter de fornecer credenciais de subscrição do Azure.

Para ilustrar este cenário, vamos supor que o UtilizadorA é um administrador de uma subscrição do Azure e que pretende permitir que o UtilizadorB aceda a uma conta de armazenamento por um período limitado de tempo com determinadas permissões:

1. O utilizador a gera uma cadeia de ligação de SAS para um período de tempo específico e com as permissões pretendidas.

2. O utilizador a partilha o SAS com a pessoa (UserB, neste exemplo) que quiser ter acesso à conta de armazenamento.  

3. B utiliza o Explorador de armazenamento para anexar à conta que pertença a UserA através da SAS fornecida.

### <a name="generate-a-sas-connection-string-for-the-account-you-want-to-share"></a>Gerar uma cadeia de ligação de SAS para a conta que pretende partilhar
1. No Explorador de armazenamento, clique com botão direito na conta de armazenamento que pretende partilhar e, em seguida, selecione **obter assinatura de acesso partilhado...** .

    ![Obter a opção de menu de contexto SAS][14]

2. No **gerar assinatura de acesso partilhado** diálogo caixa, especifique o período de tempo e as permissões que pretende para a conta e, em seguida, clique em de **criar** botão.

    ![Obter a caixa de diálogo SAS][15]  

3. Junto a **cadeia de ligação** caixa de texto, selecione **cópia** copiá-lo para a sua área de transferência e, em seguida, clique em **fechar**.

### <a name="attach-to-a-storage-account-by-using-a-sas-connection-string"></a>Anexar a uma conta de armazenamento através da utilização de uma cadeia de ligação de SAS
1. No Explorador de armazenamento, abra o **ligar a caixa de diálogo**.

    ![Ligar à opção de armazenamento do Azure][9]

2. No **ligar a caixa de diálogo** caixa de diálogo, escolha **utilizar uma cadeia de ligação ou a assinatura de acesso partilhado URI** e, em seguida, clique em **seguinte**.

    ![Caixa de diálogo Ligar ao armazenamento do Azure][16]

3. Escolha **utilizam uma cadeia de ligação** e cole a cadeia de ligação para o **cadeia de ligação:** campo. Clique em de **seguinte** botão.

    ![Caixa de diálogo Ligar ao armazenamento do Azure][17]

4. Na caixa de diálogo **Resumo da Ligação**, verifique as informações. Para fazer alterações, selecione **Regressar** e introduza as definições que pretende. 

5. Selecione **Ligar**.

6. Depois da conta de armazenamento foi anexada com êxito, a conta de armazenamento é apresentada com **(SAS)** anexado ao respetivo nome.

    ![Resultado de anexação a uma conta com SAS][18]

## <a name="attach-a-service-by-using-a-shared-access-signature-sas"></a>Anexe um serviço utilizando uma assinatura de acesso partilhado (SAS)
A secção "Ligar uma conta de armazenamento ao utilizar um SAS" explica como um administrador da subscrição do Azure pode conceder acesso temporário a uma conta do storage ao gerar e um SAS para a conta de armazenamento de partilha. Da mesma forma, é possível gerar uma SAS para um serviço específico (contentor de blob, fila, tabela ou partilha de ficheiros) dentro de uma conta de armazenamento.  

### <a name="generate-an-sas-for-the-service-that-you-want-to-share"></a>Gerar uma SAS para o serviço que pretende partilhar
Neste contexto, um serviço pode ser um contentor de blob, fila, tabela ou partilha de ficheiros. Para gerar a SAS para um serviço listado, veja:

* [Obter a SAS para um contentor de blobs](vs-azure-tools-storage-explorer-blobs.md#get-the-sas-for-a-blob-container)

### <a name="attach-to-the-shared-account-service-by-using-a-sas-uri"></a>Anexar ao serviço da conta partilhada através da utilização de um URI de SAS
1. No Explorador de armazenamento, abra o **ligar a caixa de diálogo**.

    ![Ligar à opção de armazenamento do Azure][9]

2. No **ligar a caixa de diálogo** diálogo caixa, escolha **utilizar uma cadeia de ligação ou a assinatura de acesso partilhado URI** e, em seguida, clique em **seguinte**.

    ![Caixa de diálogo Ligar ao armazenamento do Azure][16]

3. Escolha **utilizar um URI de SAS** e cole o URI para o **URI:** campo. Clique em de **seguinte** botão.

    ![Caixa de diálogo Ligar ao armazenamento do Azure][19]

3. Na caixa de diálogo **Resumo da Ligação**, verifique as informações. Para fazer alterações, selecione **Regressar** e introduza as definições que pretende. 

4. Selecione **Ligar**.

5. Depois do serviço é anexado com êxito, o serviço é apresentado sob o **(SAS-Attached serviços)** nós.

    ![Resultado da anexação a um serviço partilhado com SAS][20]

## <a name="connect-to-an-azure-cosmos-db-account-by-using-a-connection-string"></a>Ligar a uma conta de base de dados do Azure Cosmos utilizando uma cadeia de ligação
Besides gerir contas de base de dados do Azure Cosmos através de subscrição do Azure, uma maneira de ligar a uma base de dados do Azure Cosmos consiste em utilizar uma cadeia de ligação. Utilize os seguintes passos para ligar através de uma cadeia de ligação.

1. Localize **Local e Ligada** na árvore à esquerda, clique com o botão direito do rato em **Contas do Azure Cosmos DB** e selecione **Ligar ao Azure Cosmos DB...**

    ![ligar à base de dados do Azure Cosmos pela cadeia de ligação][21]

2. Escolha a API do Azure Cosmos DB, cole o **cadeia de ligação**e, em seguida, clique em **OK** para ligar a conta de base de dados do Azure Cosmos. Para obter informações sobre como obter a cadeia de ligação, veja [Obter a cadeia de ligação](https://docs.microsoft.com/azure/cosmos-db/manage-account#get-the--connection-string).

    ![connection-string][22]

 ## <a name="connect-to-azure-data-lake-store-by-uri"></a>Ligar ao Azure Data Lake Store pelo URI
Se pretender obter acesso a recursos que não existem na sua subscrição. Mas outros concedem-lhe permissão para obter o Uri dos recursos. Neste caso, pode ligar ao Data Lake Store com o Uri, depois de ter sessão iniciada. Consulte os seguintes passos.
1. Abra o Explorador de Armazenamento.
2. No painel esquerdo, expanda **Local e Ligado**.
3. Clique com botão direito do rato em **Data Lake Store** e, no menu de contexto, selecione **Ligar ao Data Lake Store...**.

    ![menu de contexto Ligar ao Data Lake Store](./media/vs-azure-tools-storage-manage-with-storage-explorer/storageexplorer-adls-uri-attach.png)

4. Introduza o Uri e, em seguida, a ferramenta navega para a localização do URL que acabou de introduzir.

    ![caixa de diálogo de contexto Ligar ao Data Lake Store](./media/vs-azure-tools-storage-manage-with-storage-explorer/storageexplorer-adls-uri-attach-dialog.png)

    ![resultado de Ligar ao Data Lake Store](./media/vs-azure-tools-storage-manage-with-storage-explorer/storageexplorer-adls-attach-finish.png)

## <a name="search-for-storage-accounts"></a>Procurar contas do Storage
Se precisar de localizar um recurso de armazenamento e não souber qual é, pode utilizar a caixa de pesquisa na parte superior do painel esquerdo para procurar o recurso.

À medida que escreve na caixa de pesquisa, o painel esquerdo apresenta todos os recursos que correspondem o valor de pesquisa que introduziu até esse ponto. Por exemplo, uma pesquisa de **pontos finais** é mostrado na captura de ecrã seguinte:

![Procurar conta de armazenamento][23]

> [!NOTE]
> Utilize o **conta de gestão de painel** para anular a seleção de quaisquer subscrições que contém o item de procura para melhorar o tempo de execução da sua pesquisa. Também pode com o botão direito num nó e escolha **pesquisa de aqui** para iniciar a pesquisa a partir de um nó específico.
>
>

## <a name="next-steps"></a>Passos Seguintes
* [Gerir recursos de armazenamento de Blobs do Azure com o Explorador de armazenamento](vs-azure-tools-storage-explorer-blobs.md)
* [Gerir o Azure Cosmos DB no Explorador de armazenamento do Azure (pré-visualização)](./cosmos-db/storage-explorer.md)
* [Gerir recursos do Azure Data Lake Store com o Explorador de armazenamento](./data-lake-store/data-lake-store-in-storage-explorer.md)

[0]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/Overview.png
[1]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ManageAccounts.png
[2]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog-SignInSelected.png
[3]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/AccountPanel.png
[4]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/SubscriptionNode.png
[5]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/DevelopmentNode.png
[6]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/EmulatorNotInstalled.png
[7]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/PortalAccessKeys.png
[8]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/AccessKeys.png
[9]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog.png
[10]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog-AddWithKeySelected.png
[11]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog-NameAndKeyPage.png
[12]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/AttachedWithKeyAccount.png
[13]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/AttachedWithKeyAccount-Detach.png
[14]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/GetSharedAccessSignature.png
[15]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/SharedAccessSignatureDialog.png
[16]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog-WithConnStringOrSASSelected.png
[17]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog-ConnStringOrSASPage-1.png
[18]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/AttachedWithSASAccount.png
[19]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog-ConnStringOrSASPage-2.png
[20]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ServiceAttachedWithSAS.png
[21]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-db-by-connection-string.png
[22]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connection-string.png
[23]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/Search.png
