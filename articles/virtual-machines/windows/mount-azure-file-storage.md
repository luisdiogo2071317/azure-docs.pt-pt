---
title: Armazenamento de ficheiros do Azure de montagem de uma VM do Windows Azure | Documentos da Microsoft
description: Store ficheiros na cloud com o armazenamento de ficheiros do Azure e Monte a partilha de ficheiros na cloud a partir de uma máquina virtual do Azure (VM).
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: tysonn
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 01/02/2018
ms.author: cynthn
ms.subservice: files
ms.openlocfilehash: d79a2d33157d9e2a6f56eb18d061f71a7dcd635c
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/31/2019
ms.locfileid: "55462244"
---
# <a name="use-azure-file-shares-with-windows-vms"></a>Utilizar partilhas de ficheiros do Azure com VMs do Windows 

Pode utilizar partilhas de ficheiros do Azure como uma forma de armazenar e aceder aos ficheiros da VM. Por exemplo, pode armazenar um script ou um arquivo de configuração que pretende que todas as VMs para partilhar. Neste artigo, mostraremos como criar e montar uma partilha de ficheiros do Azure e como carregar e transferir ficheiros.

## <a name="connect-to-a-file-share-from-a-vm"></a>Ligar a uma partilha de ficheiros a partir de uma VM

Esta secção assume que já tem uma partilha de ficheiros que pretende ligar a. Se precisar de criar um, veja [criar uma partilha de ficheiros](#create-a-file-share) mais adiante neste artigo.

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. No menu da esquerda, clique em **contas de armazenamento**.
3. Escolha a sua conta de armazenamento.
4. Na **descrição geral** página, em **serviços**, selecione **ficheiros**.
5. Selecione uma partilha de ficheiros ou clique em **+ partilha de ficheiros** para criar uma nova partilha de ficheiros a utilizar.
6. Clique em **Connect** para abrir uma página que mostra a sintaxe da linha de comandos para montar a partilha de ficheiros do Windows ou Linux.
7. Na **letra de unidade**, selecione a letra que pretende utilizar para identificar a unidade.
8. Escolha qual sintaxe para utilizar e selecione o botão de copiar no lado direito para copiá-lo para a área de transferência. Cole-a algum lugar onde pode facilmente aceder à mesma. 
8. Ligar à VM e abra um prompt de comando.
9. Cole a sintaxe de ligação editado e de acessos **Enter**.
10. Quando a ligação tiver sido criada, obterá a mensagem **o comando foi concluído com êxito.**
11. Verifique a ligação ao escrever na letra de unidade para mudar para essa unidade e, em seguida, escreva **dir** para ver os conteúdos da partilha de ficheiros.



## <a name="create-a-file-share"></a>Criar uma partilha de ficheiros 
1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. No menu da esquerda, clique em **contas de armazenamento**.
3. Escolha a sua conta de armazenamento.
4. Na **descrição geral** página, em **serviços**, selecione **ficheiros**.
5. Na página do serviço de ficheiros, clique em **+ partilha de ficheiros**.
6. Preencha o nome da partilha de ficheiros. Os nomes das partilhas de ficheiros podem utilizar letras minúsculas, números e hífenes. O nome não pode começar com um hífen e não é possível utilizar várias hífenes consecutivos. 
7. Preencha um limite no tamanho do arquivo pode ser, até 5120 GB.
8. Clique em **OK** para criar a partilha de ficheiros.
   
## <a name="upload-files"></a>Carregar ficheiros
1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. No menu da esquerda, clique em **contas de armazenamento**.
3. Escolha a sua conta de armazenamento.
4. Na **descrição geral** página, em **serviços**, selecione **ficheiros**.
5. Selecione uma partilha de ficheiros.
6. Clique em **carregue** para abrir o **carregar ficheiros** página.
7. Clique no ícone de pasta para procurar o seu sistema de ficheiros local para um ficheiro a carregar.   
8. Clique em **carregar** para carregar o ficheiro para a partilha de ficheiros.

## <a name="download-files"></a>Transferir ficheiros
1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. No menu da esquerda, clique em **contas de armazenamento**.
3. Escolha a sua conta de armazenamento.
4. Na **descrição geral** página, em **serviços**, selecione **ficheiros**.
5. Selecione uma partilha de ficheiros.
6. Com o botão direito no ficheiro e escolha **transferir** para baixá-lo no seu computador local.
   

## <a name="next-steps"></a>Passos Seguintes

Também pode criar e gerir partilhas de ficheiros com o PowerShell. Para obter mais informações, consulte [introdução ao armazenamento de ficheiros do Azure no Windows](../../storage/files/storage-dotnet-how-to-use-files.md).
