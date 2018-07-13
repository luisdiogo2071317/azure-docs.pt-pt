---
title: Gerir partilhas de ficheiros do Azure com o Explorador de Armazenamento do Azure
description: Saiba como utilizar o Explorador de Armazenamento do Azure para gerir Ficheiros do Azure.
services: storage
documentationcenter: ''
author: wmgries
manager: aungoo
editor: tamram
ms.assetid: ''
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/27/2018
ms.author: wgries
ms.openlocfilehash: ea401099ce90248be0ecb99109b845ff9a91f70d
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38308502"
---
# <a name="manage-azure-file-shares-with-azure-storage-explorer"></a>Gerir partilhas de ficheiros do Azure com o Explorador de Armazenamento do Azure 
Os [Ficheiros do Azure](storage-files-introduction.md) são o sistema de ficheiros na cloud fácil de utilizar da Microsoft. Este artigo orienta-o pelas noções básicas da utilização de partilhas de ficheiros do Azure com o [Explorador de Armazenamento do Azure](https://azure.microsoft.com/features/storage-explorer/). O Explorador de Armazenamento é uma ferramenta de cliente popular que está disponível para Windows, macOS e Linux. Pode utilizar o Explorador de Armazenamento para gerir partilhas de ficheiros do Azure e outros recursos de armazenamento.

Este início rápido requer a instalação do Explorador de Armazenamento. Para transferi-lo e instalá-lo, aceda ao [Explorador de Armazenamento do Azure](https://azure.microsoft.com/features/storage-explorer/).

Neste artigo, vai aprender a:

> [!div class="checklist"]
> * Criar um grupo de recursos e uma conta de armazenamento
> * Criar uma partilha de ficheiros do Azure 
> * Criar um diretório
> * Carregar um ficheiro
> * Transferir um ficheiro
> * Criar e utilizar um instantâneo de partilha

Se não tiver uma subscrição do Azure, pode criar uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="create-a-storage-account"></a>Criar uma conta de armazenamento
Não pode utilizar o Explorador de Armazenamento para criar novos recursos. Para efeitos desta demonstração, crie a conta de armazenamento no [portal do Azure](https://portal.azure.com/). 

[!INCLUDE [storage-files-create-storage-account-portal](../../../includes/storage-files-create-storage-account-portal.md)]

## <a name="connect-storage-explorer-to-azure-resources"></a>Ligar o Explorador de Armazenamento aos recursos do Azure
Ao iniciar o Explorador de Armazenamento, surge a janela **Explorador de Armazenamento do Microsoft Azure - Ligar**. O Explorador de Armazenamento proporciona várias formas de ligar a contas de armazenamento: 

- **Inicie sessão com a sua conta do Azure**: pode iniciar sessão com as credenciais de utilizador para a sua organização ou a sua conta Microsoft. 
- **Ligar a uma conta de armazenamento específica com uma cadeia de ligação ou um token SAS**: uma cadeia de ligação é uma cadeia especial que contém um nome de conta de armazenamento e o token de SAS/chave da conta de armazenamento. Com o token, o Explorador de Armazenamento acede diretamente à conta de armazenamento (em vez de simplesmente ver todas as contas de armazenamento numa conta do Azure). Para saber mais sobre as cadeias de ligação, veja [Configure Azure storage connection strings](../common/storage-configure-connection-string.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) (Configurar cadeias de ligação do Armazenamento do Azure).
- **Ligar a uma conta de armazenamento específica com o nome e a chave da mesma**: utilize o nome e a chave da conta de armazenamento da sua conta de armazenamento para ligar ao armazenamento do Azure.

Para efeitos deste início rápido, inicie sessão com a sua conta do Azure. Selecione **Adicionar uma Conta do Azure** e selecione **Iniciar sessão**. Siga os avisos para iniciar sessão na conta do Azure.

![Uma captura de ecrã do Explorador de Armazenamento do Microsoft Azure – Janela Ligar](./media/storage-how-to-use-files-storage-explorer/connect-to-azure-storage-1.png)

### <a name="create-a-file-share"></a>Criar uma partilha de ficheiros
Para criar a sua primeira partilha de ficheiros do Azure na conta de armazenamento *storageacct<random number>*:

1. Expanda a conta de armazenamento que criou.
2. Clique com o botão direito do rato em **Partilhas de Ficheiros** e, em seguida, selecione **Criar Partilha de Ficheiros**.  
    ![Captura de ecrã da pasta de partilhas de ficheiros e do menu de contexto em contexto](media/storage-how-to-use-files-storage-explorer/create-file-share-1.png)

3. Para a partilha de ficheiros, introduza *myshare* e, em seguida, prima Enter.

> [!IMPORTANT]  
> Os nomes das partilhas só podem ter letras minúsculas, números e hífenes (mas não podem começar com um hífen). Para obter detalhes completos sobre a nomenclatura de partilhas de ficheiros e ficheiros, veja [Naming and referencing shares, directories, files, and metadata (Nomenclatura e referência de partilhas, diretórios, ficheiros e metadados)](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Shares--Directories--Files--and-Metadata).

Depois de criar a partilha de ficheiros, é aberto um separador para a partilha de ficheiros no painel direito. 

## <a name="work-with-the-contents-of-an-azure-file-share"></a>Trabalhar com os conteúdos de uma partilha de ficheiros do Azure
Agora que já criou uma partilha de ficheiros do Azure, pode montá-la com SMB em [Windows](storage-how-to-use-files-windows.md), [Linux](storage-how-to-use-files-linux.md) ou [macOS](storage-how-to-use-files-mac.md). Em alternativa, pode trabalhar com a partilha de ficheiros do Azure com a CLI do Azure. A vantagem de utilizar a CLI do Azure em vez de montar a partilha de ficheiros com o SMB é que todos os pedidos realizados com a CLI do Azure são feitos através da API REST de Ficheiros. Pode utilizar a API REST do Ficheiro para criar, modificar e eliminar ficheiros e diretórios em clientes que não têm acesso ao SMB.

### <a name="create-a-directory"></a>Criar um diretório
Adicionar um diretório proporciona uma estrutura hierárquica para gerir a sua partilha de ficheiros. Pode criar vários níveis no seu diretório. No entanto, tem de se certificar que os diretórios principais existem antes de criar subdiretórios. Por exemplo, para o caminho myDirectory/mySubDirectory, tem primeiro de criar o diretório *myDirectory*. Em seguida, pode criar *mySubDirectory*. 

1. No separador da partilha de ficheiros, no menu superior, selecione o botão **Nova Pasta**. O painel **Criar Novo Diretório** abre.
    ![Uma captura de ecrã do botão Nova Pasta em contexto](media/storage-how-to-use-files-storage-explorer/create-directory-1.png)

2. Para o nome do diretório, introduza *myDirectory* e, em seguida, selecione **OK**. 

O diretório *myDirectory* é listado no separador da partilha de ficheiros *myshare*.

### <a name="upload-a-file"></a>Carregar um ficheiro 
Pode carregar um ficheiro a partir do seu computador local para o diretório novo na partilha de ficheiros. Pode carregar uma pasta inteira ou um único ficheiro.

1. No menu superior, selecione **Carregar**. Isto dá-lhe a opção de carregar uma pasta ou um ficheiro.
2. Selecione **Carregar Ficheiro** e, em seguida, selecione um ficheiro para carregar a partir do seu computador local.
3. Em **Carregar para um diretório**, introduza *myDirectory* e, em seguida, selecione **Carregar**. 

Quando tiver terminado, o ficheiro é apresentado na lista no painel *myDirectory*.

### <a name="download-a-file"></a>Transferir um ficheiro
Para transferir uma cópia de um ficheiro a partir da partilha de ficheiros, clique com o botão direito do rato no ficheiro e, em seguida, selecione **Transferir**. Selecione onde pretende colocar o ficheiro no seu computador local e, em seguida, selecione **Guardar**.

O progresso da transferência aparece no painel **Atividades** na parte inferior da janela.

## <a name="create-and-modify-share-snapshots"></a>Criar e modificar instantâneos de partilha
Os instantâneos preservam uma cópia de ponto no tempo das partilhas de ficheiros do Azure. Os instantâneos de partilhas de ficheiros são semelhantes a outras tecnologias com as quais pode já estar familiarizado:
- [Serviço de Cópia Sombra de Volumes (VSS)](https://docs.microsoft.com/en-us/windows/desktop/VSS/volume-shadow-copy-service-portal) para sistemas de ficheiros Windows, como NTFS e ReFS
- Instantâneos do [Gestor de Volumes Lógicos (LVM)](https://en.wikipedia.org/wiki/Logical_Volume_Manager_(Linux)#Basic_functionality) para sistemas Linux
- Instantâneos do [Apple File System (APFS)](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/APFS_Guide/Features/Features.html) para macOS

Para criar um instantâneo de partilha:

1. Selecione o separador da partilha de ficheiros *myshare*.
2. No menu superior, selecione **Criar Instantâneo**. (Poderá ter de selecionar primeiro **Mais** para ver esta opção, consoante as dimensões da janela do Explorador de Armazenamento.)  
    ![Uma captura de ecrã do botão Criar Instantâneo no contexto](media/storage-how-to-use-files-storage-explorer/create-share-snapshot-1.png)

### <a name="list-and-browse-share-snapshots"></a>Listar e procurar instantâneos de partilhas
Após a criação do instantâneo, para listar os instantâneos da partilha, selecione **Ver Instantâneos da Partilha de Ficheiros**. (Poderá ter de selecionar primeiro **Mais** para ver esta opção, consoante as dimensões da janela do Explorador de Armazenamento.) Para pesquisar um instantâneo de partilha, faça duplo clique no mesmo.

![Uma captura de ecrã da janela Procurar Instantâneos](media/storage-how-to-use-files-storage-explorer/list-browse-snapshots-1.png)

### <a name="restore-from-a-share-snapshot"></a>Restaurar a partir de um instantâneo de partilha
Para demonstrar como restaurar um ficheiro a partir de um instantâneo de partilha, primeiro tem de eliminar um ficheiro da partilha de ficheiros do Azure em direto. Vá para a pasta *myDirectory*, clique com o botão direito do rato no ficheiro que carregou e, em seguida, selecione **Eliminar**. Para restaurar esse ficheiro a partir do instantâneo de partilha:

1. Selecione **Ver Instantâneos da Partilha de Ficheiros**. (Poderá ter de selecionar primeiro **Mais** para ver esta opção, consoante as dimensões da janela do Explorador de Armazenamento.)
2. Na lista de instantâneos de partilha, faça duplo clique no instantâneo de partilha.
3. Procure o instantâneo até encontrar o ficheiro que eliminou. Selecione a partilha de ficheiros e, em seguida, selecione **Restaurar Instantâneos**. (Poderá ter de selecionar primeiro **Mais** para ver esta opção, consoante as dimensões da janela do Explorador de Armazenamento.) Uma janela é aberta e apresenta um aviso a dizer que o restauro do ficheiro substituirá os conteúdos da partilha de ficheiros e que este não pode ser anulado. Selecione **OK**.
4. O ficheiro deverá estar agora no local original, abaixo da partilha de ficheiros do Azure ativa.

### <a name="delete-a-share-snapshot"></a>Eliminar um instantâneo de partilha
Para eliminar um instantâneo de partilha, vá para a [lista de instantâneos de partilhas](#list-and-browse-share-snapshots). Clique com o botão direito do rato no instantâneo de partilha que pretende eliminar e, em seguida, selecione **Eliminar**.

## <a name="clean-up-resources"></a>Limpar recursos
Não pode utilizar o Explorador de Armazenamento para remover recursos. Para limpar este início rápido, pode utilizar o [portal do Azure](https://portal.azure.com/). 

[!INCLUDE [storage-files-clean-up-portal](../../../includes/storage-files-clean-up-portal.md)]

## <a name="next-steps"></a>Passos seguintes
- [Gerir partilhas de ficheiros com o portal do Azure](storage-how-to-use-files-portal.md)
- [Gerir partilhas de ficheiros com o Azure PowerShell](storage-how-to-use-files-powershell.md)
- [Gerir partilhas de ficheiros com a CLI do Azure](storage-how-to-use-files-cli.md)
- [Planear uma implementação de Ficheiros do Azure](storage-files-planning.md)