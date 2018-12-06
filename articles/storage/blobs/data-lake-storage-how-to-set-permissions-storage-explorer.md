---
title: 'Procedimentos: ACLs de conjunto em arquivos e diretórios com o Explorador de armazenamento do Azure'
description: Isso como, saiba como definir ACLs em arquivos e diretórios
services: storage
author: roygara
ms.custom: mvc
ms.service: storage
ms.topic: quickstart
ms.date: 12/05/2018
ms.author: rogarana
ms.openlocfilehash: c1bcb373fcf21906cbce9b7276e2a7dd7ebb7ed8
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/06/2018
ms.locfileid: "52975912"
---
# <a name="how-to-set-file-and-directory-level-permissions-using-azure-storage-explorer"></a>Como: definir permissões ao nível ficheiro e de diretório com o Explorador de armazenamento do Azure

Este artigo mostra como definir o nível de ficheiros e diretórios ACLs por meio de versão para desktop do Explorador de armazenamento do Azure.

Este artigo requer que instale o Explorador de armazenamento do Azure. Para instalar o Explorador de Armazenamento do Azure para Windows, Macintosh ou Linux, consulte [Explorador de Armazenamento do Azure](https://azure.microsoft.com/features/storage-explorer/).

## <a name="sign-in-to-storage-explorer"></a>Inicie sessão no Explorador de armazenamento

Na primeira execução, é mostrada a janela **Explorador de Armazenamento do Microsoft Azure – Ligar**. Embora o Explorador de armazenamento proporciona várias formas de ligar a contas de armazenamento, apenas uma forma é atualmente suportada para gerenciar ACLs.

|Tarefa|Objetivo|
|---|---|
|Adicionar uma Conta do Azure | Redireciona-o para a página de início de sessão de organizações para o autenticar no Azure. Atualmente, é o único método de autenticação, que permite-lhe gerir ACLs de utilizar o Explorador de armazenamento.|

Selecione **adicionar uma conta do Azure** e clique em **início de sessão.**. Siga as mensagens no ecrã para iniciar sessão na conta do Azure.

![Explorador de Armazenamento do Microsoft Azure – Janela Ligar](media/storage-quickstart-blobs-storage-explorer/connect.png)

Quando a ligação for concluída, o Explorador de Armazenamento do Azure é carregado com o separador **Explorador** mostrado. Esta vista fornece-lhe informações detalhadas de todas as contas de armazenamento do Azure, bem como do armazenamento local configurado através do [Emulador de Armazenamento do Azure](../common/storage-use-emulator.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json), contas do [Cosmos DB](../../cosmos-db/storage-explorer.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) ou ambientes do [Azure Stack](../../azure-stack/user/azure-stack-storage-connect-se.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

![Explorador de Armazenamento do Microsoft Azure – Janela Ligar](media/storage-quickstart-blobs-storage-explorer/mainpage.png)

## <a name="managing-access"></a>Gerir o acesso

Pode definir permissões na raiz do seu sistema de ficheiros. Para fazer isso, seu sistema de ficheiros com o botão direito e selecione **gerir permissões**.

Este irá abrir o **permissão Gerir** prompt.

![Explorador de armazenamento do Microsoft Azure – gerir o acesso ao diretório](media/storage-quickstart-blobs-storage-explorer/manageperms.png)

O **gerir acesso** prompt permitirá para gerir as permissões de proprietário e o grupo proprietários, bem como adicionar novos utilizadores para a lista de controlo de acesso para o qual, em seguida, pode gerir as permissões. Para saber mais sobre as permissões, incluindo as permissões predefinidas, as permissões de acesso e seu comportamento, consulte nosso artigo sobre [controlo de acesso no data lake storage gen2](data-lake-storage-access-control.md#access-control-lists-on-files-and-directories). Efetuar seleções aqui não definir ACLs em qualquer item atualmente existente dentro do diretório.

É recomendável criar grupos de segurança e manter as permissões do grupo em vez de utilizadores individuais. Para obter detalhes sobre esta recomendação e outras práticas recomendadas, consulte nosso [melhores práticas para a geração 2 de armazenamento do data lake](data-lake-storage-best-practices.md) artigo.

Pode gerir as permissões em diretórios individuais como arquivos individuais, permitindo que controle de acesso refinado. O processo de gerenciamento de permissões em ficheiros e diretórios é igual à descrita acima.

## <a name="next-steps"></a>Passos Seguintes

Este procedimento, ficou a saber como definir permissões em ficheiros e diretórios usando **Explorador de armazenamento do Azure**. Para saber mais sobre listas de controlo de acesso e permissões, avance para o nosso artigo conceitual sobre o assunto.

> [!div class="nextstepaction"]
> [Controlo de HAccess na geração 2 de armazenamento do Azure Data Lake](data-lake-storage-access-control.md)