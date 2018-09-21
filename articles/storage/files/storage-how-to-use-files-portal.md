---
title: Gerir partilhas de ficheiros do Azure com o Portal do Azure
description: Aprenda a utilizar o portal do Azure para gerir os Ficheiros do Azure.
services: storage
author: wmgries
ms.service: storage
ms.topic: get-started-article
ms.date: 03/26/2018
ms.author: wgries
ms.component: files
ms.openlocfilehash: b4cace3922983ed93987069ac7cf59f2cb69403b
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/14/2018
ms.locfileid: "45578553"
---
# <a name="managing-azure-file-shares-with-the-azure-portal"></a>Gerir partilhas de ficheiros do Azure com o Portal do Azure 
[Ficheiros do Azure](storage-files-introduction.md) é o sistema de ficheiros na cloud fácil de utilizar da Microsoft. As partilhas de ficheiros do Azure podem ser montadas no Windows, Linux e macOS. Este guia orienta-o pelas noções básicas de utilizar partilhas de ficheiros do Azure com o [Portal do Azure](https://portal.azure.com/). Aprenda a:

> [!div class="checklist"]
> * Criar um grupo de recursos e uma conta de armazenamento
> * Criar uma partilha de ficheiros do Azure 
> * Criar um diretório
> * Carregar um ficheiro 
> * Transferir um ficheiro
> * Criar e utilizar um instantâneo de partilha

Se não tiver uma subscrição do Azure, pode criar uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="create-a-storage-account"></a>Criar uma conta de armazenamento
[!INCLUDE [storage-files-create-storage-account-portal](../../../includes/storage-files-create-storage-account-portal.md)]

## <a name="create-a-file-share"></a>Criar uma partilha de ficheiros
Para criar uma partilha de ficheiros:

1. Selecione a conta de armazenamento a partir do dashboard.
2. Na página da conta de armazenamento, na secção **Serviços**, selecione **Ficheiros**.
    ![Uma captura de ecrã da secção Serviços da conta de armazenamento; selecione o Serviço de Ficheiros](media/storage-how-to-use-files-portal/create-file-share-1.png)

3. No menu na parte superior da página **Serviço de Ficheiros**, clique em **+ Partilha de ficheiros**. É apresentada a página **Nova partilha de ficheiros**.
4. Em **Nome**, escreva *myshare*.
5. Clique em **OK** para criar a partilha de ficheiros do Azure.

## <a name="work-with-the-contents-of-the-azure-file-share"></a>Trabalhar com os conteúdos da partilha de ficheiros do Azure
Agora que já criou uma partilha de ficheiros do Azure, pode montá-la com SMB em [Windows](storage-how-to-use-files-windows.md), [Linux](storage-how-to-use-files-linux.md) ou [macOS](storage-how-to-use-files-mac.md). Em alternativa, pode trabalhar com a partilha de ficheiros do Azure com o portal do Azure. Todos os pedidos realizados através do Portal do Azure são feitos com a API REST File, o que lhe permite criar, modificar e eliminar ficheiros e diretórios em clientes que não tenham acesso SMB.

### <a name="create-a-directory"></a>Criar um diretório
Para criar um novo diretório com o nome *myDirectory* na raiz da partilha de ficheiros do Azure:

1. Na página **Serviço de Ficheiros**, selecione a partilha de ficheiros **myshare**. É aberta a página da partilha de ficheiros.
2. No menu na parte superior da página, selecione **+ Adicionar diretório**. É apresentada a página **Novo diretório**.
3. Escreva *myDirectory* e, em seguida, clique em **OK**.

### <a name="upload-a-file"></a>Carregar um ficheiro 
Para demonstrar o carregamento de um ficheiro, primeiro tem de criar ou selecionar um ficheiro a ser carregado. Pode fazê-lo da forma que considerar adequada. Depois de selecionar o ficheiro que quer carregar:

1. Clique no diretório **myDirectory**. É aberto o painel **myDirectory**.
2. No menu na parte superior, clique em **Carregar**. É aberto o painel **Carregar ficheiros**.  
    ![Uma captura de ecrã do painel Carregar ficheiros](media/storage-how-to-use-files-portal/upload-file-1.png)

3. Clique no ícone de pasta para abrir uma janela para procurar os ficheiros locais. 
4. Selecione um ficheiro e, em seguida, clique em **Abrir**. 
5. Na página **Carregar ficheiros**, verifique o nome de ficheiro e, em seguida, clique em **Carregar**.
6. Quando terminar, o ficheiro deve aparecer na lista na página **myDirectory**.

### <a name="download-a-file"></a>Transferir um ficheiro
Pode transferir uma cópia do ficheiro que carregou ao clicar com o botão direito do rato no ficheiro. Depois de clicar no botão de transferência, a experiência exata dependerá do sistema operativo e do browser que estiver a utilizar.

## <a name="create-and-modify-share-snapshots"></a>Criar e modificar instantâneos de partilha
Uma tarefa útil adicional que pode fazer com uma partilha de ficheiros do Azure é criar instantâneos de partilha. Um instantâneo preserva um ponto no tempo para uma partilha de ficheiros do Azure. Os instantâneos de partilha são semelhantes às tecnologias de sistema operativo com as quais pode já estar familiarizado, tais como:
- [Serviço de Cópia Sombra de Volumes (VSS)](https://docs.microsoft.com/windows/desktop/VSS/volume-shadow-copy-service-portal) para sistemas de ficheiros Windows, como NTFS e ReFS
- Instantâneos do [Gestor de Volumes Lógicos (LVM)](https://en.wikipedia.org/wiki/Logical_Volume_Manager_(Linux)#Basic_functionality) para sistemas Linux
- Instantâneos do [Apple File System (APFS)](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/APFS_Guide/Features/Features.html) para macOS. 

Para criar um instantâneo de partilha:

1. Abra a página da partilha de ficheiros ao abrir a conta de armazenamento no dashboard > **Ficheiros** > **myshare**. 
2. Na página da partilha de ficheiros, clique no botão **Instantâneo** no menu na parte superior da página e, em seguida, selecione **Criar um instantâneo**.  
    ![Uma captura de ecrã que ilustra como localizar o botão Criar instantâneo](media/storage-how-to-use-files-portal/create-snapshot-1.png)

### <a name="list-and-browse-share-snapshots"></a>Listar e procurar instantâneos de partilha
Após a criação do instantâneo, pode clicar novamente em **Instantâneo** e, em seguida, selecionar **Ver instantâneos** para listar os instantâneos da partilha. O painel resultante irá mostrar os instantâneos desta partilha. Clique num instantâneo de partilha para navegar no mesmo.

### <a name="restore-from-a-share-snapshot"></a>Restaurar a partir de um instantâneo de partilha
Para demonstrar o restauro de um ficheiro a partir de um instantâneo de partilha, primeiro é necessário eliminar um ficheiro da partilha de ficheiros do Azure em direto. Navegue para a pasta *myDirectory*, clique com o botão direito do rato no ficheiro que carregou e, em seguida, clique em **Eliminar**. Em seguida, para restaurar esse ficheiro a partir do instantâneo de partilha:

1. Clique em **Instantâneos** no menu superior e selecione **Ver instantâneos**. 
2. Clique no instantâneo que criou anteriormente e este abre o conteúdo numa nova página. 
3. Clique em **myDirectory** no instantâneo e deverá ver o ficheiro que eliminou. 
4. Clique com o botão direito do rato no ficheiro eliminado e selecione **Restaurar**.
5. É apresentado um pop-up com a opção de restaurar o ficheiro como uma cópia ou substituir o ficheiro original. Uma vez que o ficheiro original foi eliminado, pode selecionar **Substituir ficheiro original** para restaurar o ficheiro, tal como estava antes de ser eliminado. Clique em **OK** para restaurar o ficheiro para a partilha de ficheiros do Azure.  
    ![Uma captura de ecrã da caixa de diálogo Restaurar ficheiro](media/storage-how-to-use-files-portal/restore-snapshot-1.png)

6. Depois de o ficheiro ser restaurado, feche a página do instantâneo e volte a **myshare** > **myDirectory** e o ficheiro deverá estar no local original.

### <a name="delete-a-share-snapshot"></a>Eliminar um instantâneo de partilha
Para eliminar um instantâneo de partilha, [navegue para a lista de instantâneos de partilha](#list-and-browse-a-share-snapshot). Clique na caixa de verificação junto ao nome do instantâneo de partilha e selecione o botão **Eliminar**.

![Uma captura de ecrã da eliminação de um instantâneo de partilha](media/storage-how-to-use-files-portal/delete-snapshot-1.png)

## <a name="clean-up-resources"></a>Limpar recursos
[!INCLUDE [storage-files-clean-up-portal](../../../includes/storage-files-clean-up-portal.md)]

## <a name="next-steps"></a>Passos seguintes
- [Gerir partilhas de ficheiros com o Azure PowerShell](storage-how-to-use-files-powershell.md)
- [Gerir partilhas de ficheiros com a CLI do Azure](storage-how-to-use-files-cli.md)
- [Gerir partilhas de ficheiros com o Explorador de Armazenamento do Azure](storage-how-to-use-files-storage-explorer.md)
- [Planear uma implementação dos Ficheiros do Azure](storage-files-planning.md)