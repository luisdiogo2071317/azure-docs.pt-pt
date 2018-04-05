---
title: Gerir partilhas de ficheiros do Azure com o Explorador de Armazenamento do Azure
description: Saiba como utilizar o Explorador de Armazenamento do Azure para gerir ficheiros do Azure.
services: storage
documentationcenter: ''
author: wmgries
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/27/2018
ms.author: wgries
ms.openlocfilehash: 0c16d3b0ef0b178f99eaafecd74eabb00cca5af9
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2018
---
# <a name="managing-azure-file-shares-with-azure-storage-explorer"></a>Gerir partilhas de ficheiros do Azure com o Explorador de Armazenamento do Azure 
Os [Ficheiros do Azure](storage-files-introduction.md) são o sistema de ficheiros na cloud fácil de utilizar da Microsoft. Este guia orienta-o pelas noções básicas de utilizar partilhas de ficheiros do Azure com o [Explorador de Armazenamento do Azure](https://azure.microsoft.com/features/storage-explorer/). O Explorador de Armazenamento do Azure é uma ferramenta de cliente popular disponível para Windows, macOS e Linux para gerir partilhas de ficheiros do Azure e outros recursos de armazenamento.

Este início rápido requer a instalação do Explorador de Armazenamento do Azure. Se quiser instalá-lo, visite [Explorador de Armazenamento do Azure](https://azure.microsoft.com/features/storage-explorer/) para transferi-lo.

Neste artigo, vai aprender a:

> [!div class="checklist"]
> * Criar um grupo de recursos e uma conta de armazenamento
> * Criar uma partilha de ficheiros do Azure 
> * Criar um diretório
> * Carregar um ficheiro
> * Transferir um ficheiro
> * Criar e utilizar um instantâneo de partilha

Se não tiver uma subscrição do Azure, pode criar uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="create-a-storage-account"></a>Criar uma conta do Storage
O Explorador de Armazenamento do Azure não tem a capacidade de criar novos recursos, por isso, para efeitos desta demonstração, crie a conta de armazenamento com o [Portal do Azure](https://portal.azure.com/). 

[!INCLUDE [storage-files-create-storage-account-portal](../../../includes/storage-files-create-storage-account-portal.md)]

## <a name="connecting-azure-storage-explorer-to-azure-resources"></a>Ligar o Explorador de Armazenamento do Azure a recursos do Azure
Na primeira execução, é mostrada a janela **Explorador de Armazenamento do Microsoft Azure – Ligar**. O Explorador de Armazenamento do Azure proporciona várias formas de ligar a contas de armazenamento: 

- **Iniciar sessão através da sua conta do Azure**: permite-lhe iniciar sessão com as suas credenciais de utilizador da sua organização ou da sua conta Microsoft. 
- **Ligar a uma conta de armazenamento específico com uma cadeia de ligação ou um token de SAS**: uma cadeia de ligação é uma cadeia especial que contém o nome da conta de armazenamento e a chave/token de SAS da conta de armazenamento que permitem ao Explorador de Armazenamento do Azure aceder diretamente a essa conta (em vez de, simplesmente, ver todas as contas de armazenamento numa conta do Azure). Para saber mais sobre as cadeias de ligação, veja [Configure Azure storage connection strings](../common/storage-configure-connection-string.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) (Configurar cadeias de ligação do Armazenamento do Azure).
- **Ligar a uma conta de armazenamento específica com o nome e a chave da mesma**: utilize o nome e a chave da conta de armazenamento da sua conta de armazenamento para ligar ao armazenamento do Azure.

Para efeitos deste início rápido, inicie sessão com a sua conta do Azure. Selecione **Adicionar uma Conta do Azure** e clique em **Iniciar sessão**. Siga as mensagens no ecrã para iniciar sessão na conta do Azure.

![Explorador de Armazenamento do Microsoft Azure – Janela Ligar](./media/storage-how-to-use-files-storage-explorer/connect-to-azure-storage-1.png)

### <a name="create-a-file-share"></a>Criar uma partilha de ficheiros
Para criar a sua primeira partilha de ficheiros do Azure na conta de armazenamento *storageacct<random number>*:

1. Expanda a conta de armazenamento que criou.
2. Clique com o botão direito do rato em **Partilhas de Ficheiros** e selecione **Criar Partilha de Ficheiros**.  
    ![Captura de ecrã da pasta de partilhas de ficheiros e do menu de contexto em contexto](media/storage-how-to-use-files-storage-explorer/create-file-share-1.png)

3. Escreva *myshare* na partilha de ficheiros e prima **Enter**.

> [!Important]  
> Os nomes das partilhas só podem ter letras minúsculas, números e hífenes, mas não podem começar com um hífen. Para obter detalhes completos sobre a nomenclatura de partilhas de ficheiros e ficheiros, veja [Naming and Referencing Shares, Directories, Files, and Metadata (Nomenclatura e Referência de Partilhas, Diretórios, Ficheiros e Metadados)](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Shares--Directories--Files--and-Metadata).

Quando a partilha de ficheiros estiver criada, é aberto um separador no painel do lado direito para a mesma. 

## <a name="manipulating-the-contents-of-the-azure-file-share"></a>Manipular os conteúdos da partilha de ficheiros do Azure
Agora que já criou uma partilha de ficheiros do Azure, pode montá-la com SMB em [Windows](storage-how-to-use-files-windows.md), [Linux](storage-how-to-use-files-linux.md) ou [macOS](storage-how-to-use-files-mac.md). Em alternativa, pode manipular a partilha de ficheiros com o Portal do Azure. Todos os pedidos realizados através do Portal do Azure são feitos com a API REST File, o que lhe permite criar, modificar e eliminar ficheiros e diretórios em clientes que não tenham acesso SMB.

### <a name="create-a-directory"></a>Criar um diretório
Adicionar um diretório proporciona uma estrutura hierárquica para gerir a sua partilha de ficheiros. Pode criar vários níveis. Contudo, antes de criar subdiretórios, tem de garantir que todos os diretórios existem. Por exemplo, para o caminho myDirectory/mySubDirectory, tem primeiro de criar o diretório *myDirectory* e só depois *mySubDirectory*. 

1. No separador da partilha de ficheiros, no menu superior, clique no botão **+ Nova Pasta** É aberta a página **Criar Novo Diretório**.
    ![Captura de ecrã do botão Nova Pasta em contexto](media/storage-how-to-use-files-storage-explorer/create-directory-1.png)

2. Escreva *myDirectory* como o nome e clique em **OK**. 

O diretório *myDirectory* é listado no separador da partilha de ficheiros *myshare*.

### <a name="upload-a-file"></a>Carregar um ficheiro 
Carregue um ficheiro a partir do seu computador local para o diretório novo na partilha de ficheiros. Pode carregar uma pasta inteira ou apenas um único ficheiro.

1. No menu na parte superior, selecione **Carregar**. Isto dá-lhe a opção de carregar uma pasta ou um ficheiro.

2. Selecione **Carregar Ficheiro** e, em seguida, escolha um ficheiro do seu computador local para carregar.

3. Em **Carregar para um diretório**, escreva *myDirectory* e clique em **Carregar**. 

Quando terminar, o ficheiro deve aparecer na lista na página **myDirectory**.

### <a name="download-a-file"></a>Transferir um ficheiro
Pode transferir uma cópia de um ficheiro na partilha de ficheiros ao clicar com o botão direito do rato no ficheiro e selecionar **Transferir**. Selecione onde pretende colocar o ficheiro no seu computador local e clique em **Guardar**.

Verá o progresso da transferência no painel **Atividades** na parte inferior da janela.

## <a name="create-and-modify-share-snapshots"></a>Criar e modificar instantâneos de partilha
Os instantâneos preservam uma cópia de ponto no tempo das partilhas de ficheiros do Azure. Os instantâneos de partilhas de ficheiros são semelhantes a outras tecnologias com as quais pode já estar familiarizado, tais como:
- [Serviço de Cópia Sombra de Volumes (VSS)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee923636) para sistemas de ficheiros Windows, como NTFS e ReFS.
- Instantâneos do [Gestor de Volumes Lógicos (LVM)](https://en.wikipedia.org/wiki/Logical_Volume_Manager_(Linux)#Basic_functionality) para sistemas Linux.
- Instantâneos do [Apple File System (APFS)](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/APFS_Guide/Features/Features.html) para macOS. 

Para criar um instantâneo de partilha:

1. Abra o separador da partilha de ficheiros *myshare*.
2. No menu na parte superior do separador, clique em **Criar Instantâneo** (o botão poderá estar ocultado atrás de **... Mais**, consoante as dimensões da janela do Explorador de Armazenamento do Azure).  
    ![Captura de ecrã do botão Criar Instantâneo no contexto](media/storage-how-to-use-files-storage-explorer/create-share-snapshot-1.png)

### <a name="list-and-browse-share-snapshots"></a>Listar e procurar instantâneos de partilhas
Quando o instantâneo estiver criado, pode clicar em **Ver Instantâneos da Partilha de Ficheiros** (este botão poderá estar ocultado atrás de **... Mais**, consoante as dimensões da janela do Explorador de Armazenamento do Azure) para listar os instantâneos da partilha. Faça duplo clique num instantâneo de partilha para navegar para o mesmo.

![Captura de ecrã do ecrã Procurar Instantâneos](media/storage-how-to-use-files-storage-explorer/list-browse-snapshots-1.png)

### <a name="restore-from-a-share-snapshot"></a>Restaurar a partir de um instantâneo de partilha
Para demonstrar o restauro de um ficheiro a partir de um instantâneo de partilha, primeiro é necessário eliminar um ficheiro da partilha de ficheiros do Azure em direto. Navegue para a pasta *myDirectory*, clique com o botão direito do rato no ficheiro que carregou e, em seguida, clique em **Eliminar**. Em seguida, para restaurar esse ficheiro a partir do instantâneo de partilha:

1. Clique em **Ver Instantâneos da Partilha de Ficheiros** (este botão poderá estar ocultado atrás de **... Mais**, consoante as dimensões da janela do Explorador de Armazenamento do Azure).
2. Selecione um instantâneo de partilha na lista e faça duplo clique para navegar para o mesmo.
3. Percorra o instantâneo até encontrar o ficheiro que eliminou, selecione-o e clique em **Restaurar Instantâneo** (este botão poderá estar ocultado atrás de **... Mais**, consoante as dimensões da janela do Explorador de Armazenamento do Azure). Receberá um aviso a dizer que o restauro do ficheiro substituirá os conteúdos da partilha de ficheiros e que não pode ser anulado. Selecione **OK**.
4. O ficheiro deverá estar agora no local original, abaixo da partilha de ficheiros do Azure ativa.

### <a name="delete-a-share-snapshot"></a>Eliminar um instantâneo de partilha
Para eliminar um instantâneo de partilha, [navegue para a lista de instantâneos de partilhas](#list-and-browse-share-snapshots). Clique com o botão direito do rato no instantâneo de partilha que pretende eliminar e selecione Eliminar.

## <a name="clean-up-resources"></a>Limpar recursos
O Explorador de Armazenamento do Azure não tem a capacidade d remover recursos; pode limpar este início rápido com o [Portal do Azure](https://portal.azure.com/). 

[!INCLUDE [storage-files-clean-up-portal](../../../includes/storage-files-clean-up-portal.md)]

## <a name="next-steps"></a>Passos seguintes
- [Gerir partilhas de ficheiros com o Portal do Azure](storage-how-to-use-files-portal.md)
- [Gerir partilhas de ficheiros com o Azure PowerShell](storage-how-to-use-files-powershell.md)
- [Managing file shares with the Azure CLI](storage-how-to-use-files-cli.md) (Gerir partilhas de ficheiros com a CLI do Azure)
- [Planning for an Azure Files deployment](storage-files-planning.md) (Planear uma implementação de Ficheiros do Azure)