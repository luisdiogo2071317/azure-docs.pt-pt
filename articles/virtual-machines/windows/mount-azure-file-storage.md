---
title: Armazenamento de ficheiros do Azure de montagem de uma VM do Windows Azure | Microsoft Docs
description: "Armazene os ficheiros na nuvem com o file storage do Azure e montar a partilha de ficheiros na nuvem a partir de uma máquina virtual do Azure (VM)."
documentationcenter: 
author: cynthn
manager: jeconnoc
editor: tysonn
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: 
ms.tgt_pltfrm: 
ms.devlang: 
ms.topic: article
ms.date: 01/02/2018
ms.author: cynthn
ms.openlocfilehash: 8d537bdc882487784baef9f693e4677c76d3bd8d
ms.sourcegitcommit: 2e540e6acb953b1294d364f70aee73deaf047441
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/03/2018
---
# <a name="use-azure-file-shares-with-windows-vms"></a>Utilizar partilhas de ficheiros do Azure com VMs do Windows 

Pode utilizar partilhas de ficheiros do Azure como uma forma de armazenar e aceder aos ficheiros da VM. Por exemplo, pode armazenar um script ou um ficheiro de configuração de aplicação que pretende que todas as suas VMs para partilhar. Neste artigo, vamos mostrar-lhe como criar e montar uma partilha de ficheiros do Azure e como carregar e transferir ficheiros.

## <a name="connect-to-a-file-share-from-a-vm"></a>Ligar a uma partilha de ficheiros a partir de uma VM

Esta secção assume que já tiver uma partilha de ficheiros que pretende ligar. Se precisar de criar um, consulte [criar uma partilha de ficheiros](#create-a-file-share) posteriormente neste artigo.

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. No menu da esquerda, clique em **contas do Storage**.
3. Escolha a sua conta de armazenamento.
4. No **descrição geral** página **serviços**, selecione **ficheiros**.
5. Selecione uma partilha de ficheiros ou clique em **+ partilha de ficheiros** para criar uma nova partilha de ficheiros a utilizar.
6. Clique em **Connect** para abrir uma página que mostra a sintaxe da linha de comandos para montar a partilha de ficheiros do Windows ou Linux.
7. No **letra de unidade**, selecione a letra de que pretende utilizar para identificar a unidade.
8. Escolha qual sintaxe para utilizar e selecione o botão Copiar à direita para a copiar para a sua área de transferência. Cole-algumas local onde pode facilmente aceder. 
8. Ligar à VM e abra uma linha de comandos.
9. Cole a sintaxe de ligação editado e prima **Enter**.
10. Quando a ligação foi criada, obterá a mensagem **o comando foi concluído com êxito.**
11. Verifique a ligação ao escrever a letra de unidade para mudar para essa unidade e, em seguida, escreva **dir** vejam o conteúdo da partilha de ficheiros.



## <a name="create-a-file-share"></a>Criar uma partilha de ficheiros 
1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. No menu da esquerda, clique em **contas do Storage**.
3. Escolha a sua conta de armazenamento.
4. No **descrição geral** página **serviços**, selecione **ficheiros**.
5. Na página do serviço de ficheiros, clique em **+ partilha de ficheiros**.
6. Preencha o nome da partilha de ficheiros. Os nomes das partilhas de ficheiros podem utilizar letras minúsculas, números e hífenes único. O nome não pode começar com um hífen e não é possível utilizar várias hífenes consecutivos. 
7. Preencha um limite no qual o tamanho máximo do ficheiro pode ser, até 5120 GB.
8. Clique em **OK** para criar a partilha de ficheiros.
   
## <a name="upload-files"></a>Carregar ficheiros
1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. No menu da esquerda, clique em **contas do Storage**.
3. Escolha a sua conta de armazenamento.
4. No **descrição geral** página **serviços**, selecione **ficheiros**.
5. Selecione uma partilha de ficheiros.
6. Clique em **carregar** para abrir o **carregar ficheiros** página.
7. Clique no ícone de pasta para procurar o sistema de ficheiros local para um ficheiro a carregar.   
8. Clique em **carregar** para carregar o ficheiro para a partilha de ficheiros.

## <a name="download-files"></a>Transferir ficheiros
1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. No menu da esquerda, clique em **contas do Storage**.
3. Escolha a sua conta de armazenamento.
4. No **descrição geral** página **serviços**, selecione **ficheiros**.
5. Selecione uma partilha de ficheiros.
6. Faça duplo clique no ficheiro e escolha **transferir** para transferi-lo no seu computador local.
   

## <a name="next-steps"></a>Passos Seguintes

Também pode criar e gerir partilhas de ficheiros com o PowerShell. Para obter mais informações, consulte [introdução ao File storage do Azure no Windows](../../storage/files/storage-dotnet-how-to-use-files.md).
