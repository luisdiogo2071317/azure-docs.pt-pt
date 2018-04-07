---
title: Cópia de segurança de VMs do Azure Windows | Microsoft Docs
description: Proteger as VMs do Windows através de cópias de segurança utilizando a cópia de segurança do Azure.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/27/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 12859bf967cf8de1b57ab9dfd5c0bd080806f2eb
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2018
---
# <a name="back-up-windows-virtual-machines-in-azure"></a>Cópia de segurança de máquinas virtuais do Windows no Azure

Pode criar cópias de segurança em intervalos regulares para manter os seus dados protegidos. O Azure Backup cria pontos de recuperação que são armazenados em cofres de recuperação georredundantes. Quando restaura a partir de um ponto de recuperação, pode restaurar a VM completa ou apenas ficheiros específicos. Este artigo explica como restaurar um ficheiro único para uma VM com o Windows Server e o IIS. Se ainda não tiver uma VM a utilizar, pode criar um utilizando o [início rápido do Windows](quick-create-portal.md). Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Criar uma cópia de segurança de uma VM
> * Agendar uma cópia de segurança diária
> * Restaurar um ficheiro a partir de uma cópia de segurança




## <a name="backup-overview"></a>Descrição geral da Cópia de Segurança

Quando o serviço de cópia de segurança do Azure inicia uma tarefa de cópia de segurança, aciona a extensão de cópia de segurança para criar um instantâneo de ponto no tempo. As utilizações do serviço de cópia de segurança do Azure a _VMSnapshot_ extensão. A extensão é instalada durante a primeira cópia de segurança da VM se a VM estiver em execução. Se a VM não estiver em execução, o serviço Backup cria um instantâneo do armazenamento subjacente (uma vez que não ocorrem escritas da aplicação enquanto a VM está parada).

Quando um instantâneo de VMs do Windows, o serviço de cópia de segurança coordena com o serviço do Volume de cópia de sombra de volumes (VSS) para obter um instantâneo consistente dos discos da máquina virtual. Assim que o serviço Azure Backup tira o instantâneo, os dados são transferidos para o cofre. Para maximizar a eficiência, o serviço identifica e transfere apenas os blocos de dados que foram alterados desde a cópia de segurança anterior.

Quando a transferência de dados estiver concluída, o instantâneo é removido e é criado um ponto de recuperação.


## <a name="create-a-backup"></a>Criar uma cópia de segurança
Crie uma cópia de segurança diária simples para um Cofre dos Serviços de Recuperação. 

1. Inicie sessão no [portal do Azure](https://portal.azure.com/).
2. No menu do lado esquerdo, selecione **Máquinas virtuais**. 
3. Na lista, selecione uma VM da qual pretende criar uma cópia de segurança.
4. No painel da VM, na secção **Definições**, clique em **Cópia de Segurança**. O painel **Ativar cópia de segurança** abre.
5. Em **Cofre dos Serviços de Recuperação**, clique em **Criar novo** e indique o nome do cofre novo. É criado um cofre novo no mesmo Grupo de Recursos e na mesma localização que a máquina virtual.
6. Clique em **Política de cópia de segurança**. Para este exemplo, mantenha as predefinições e clique em **OK**.
7. No painel **Ativar cópia de segurança**, clique em **Ativar Cópia de Segurança**. Esta ação cria uma cópia de segurança diária, com base na agenda predefinida.
10. Para criar um ponto de recuperação inicial, no painel **Cópia de segurança**, clique em **Fazer cópia de segurança agora**.
11. No painel **Fazer Cópia de Segurança Agora**, clique no ícone de calendário, utilize o controlo do calendário para selecionar o último dia em que este ponto de recuperação é mantido e clique em **Cópia de Segurança**.
12. No painel **Cópia de Segurança** para a VM, verá o número de pontos de recuperação que estão concluídos.

    ![Pontos de recuperação](./media/tutorial-backup-vms/backup-complete.png)
    
A primeira cópia de segurança demora cerca de 20 minutos. Avance para a parte seguinte deste tutorial depois de concluída a cópia de segurança.

## <a name="recover-a-file"></a>Recuperar um ficheiro

Se eliminar ou fizer alterações acidentalmente a um ficheiro, pode utilizar a Recuperação de Ficheiros para recuperar o ficheiro do cofre de cópias de segurança. Recuperação de ficheiros utiliza um script que é executado na VM, para o ponto de recuperação como unidade local de montagem. Estas unidades permanecerão montadas durante 12 horas para que possa copiar os ficheiros do ponto de recuperação e restaurá-los para a VM.  

Neste exemplo, mostramos como recuperar o ficheiro de imagem que é utilizado na página web predefinido do IIS. 

1. Abra um browser e ligar para o endereço IP da VM para mostrar a página predefinida de IIS.

    ![IIS predefinido de página web](./media/tutorial-backup-vms/iis-working.png)

2. Ligar à VM.
3. Na VM, abra **Explorador de ficheiros** e navegue para \inetpub\wwwroot e elimine o ficheiro **iisstart.png**.
4. No computador local, atualize o browser para ver que a imagem na página IIS predefinido é removida.

    ![IIS predefinido de página web](./media/tutorial-backup-vms/iis-broken.png)

5. No computador local, abra um novo separador e aceda ao [portal do Azure](https://portal.azure.com).
6. No menu à esquerda, selecione **máquinas virtuais** e selecione a VM a partir da lista.
8. No painel da VM, na secção **Definições**, clique em **Cópia de Segurança**. O painel **Cópia de Segurança** abre. 
9. No menu na parte superior do painel, selecione **Recuperação de Ficheiros**. O painel **Recuperação de Ficheiros** abre.
10. Em **Passo 1: selecionar o ponto de recuperação**, selecione um ponto de recuperação na lista pendente.
11. Em **Passo 2: descarregar o script para procurar e recuperar ficheiros**, clique no botão **Transferir Executável**. Guarde o ficheiro para o **transfere** pasta.
12. No seu computador local, abra **Explorador de ficheiros** e navegue até à sua **transfere** pasta e copie o ficheiro de .exe transferido. O nome de ficheiro será adicionado como prefixo pelo seu nome VM. 
13. Na sua VM (através da ligação de RDP) cole o ficheiro de .exe no ambiente de trabalho da sua VM. 
14. Navegue para o ambiente de trabalho da sua VM e faça duplo clique no .exe. Este procedimento irá iniciar uma linha de comandos e, em seguida, Monte o ponto de recuperação como uma partilha de ficheiros que pode aceder. Quando terminar a criar a partilha, escreva **q** para fechar a linha de comandos.
15. Na sua VM, abra **Explorador de ficheiros** e navegue para a letra de unidade que foi utilizada para a partilha de ficheiros.
16. Navegue para \inetpub\wwwroot e copie **iisstart.png** do ficheiro partilhar e cole-o \inetpub\wwwroot. Por exemplo, copie F:\inetpub\wwwroot\iisstart.png e cole-o c:\inetpub\wwwroot para recuperar o ficheiro.
17. No computador local, abra o separador do browser onde estão ligados ao endereço IP da VM que mostra a página predefinida do IIS. Prima CTRL + F5 para atualizar a página do browser. Deverá ver a imagem foi restaurada.
18. No seu computador local, volte ao separador do browser relativo ao portal do Azure e, em **Passo 3: desmontar os discos depois da recuperação**, clique no botão **Desmontar Discos**. Caso se esqueça efetuar este passo, a ligação para a pontodemontagem é fechar automaticamente após 12 horas. Após essas 12 horas, terá de transferir um novo script para criar uma novo ponto de montagem.


## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Criar uma cópia de segurança de uma VM
> * Agendar uma cópia de segurança diária
> * Restaurar um ficheiro a partir de uma cópia de segurança

Avance para o tutorial seguinte para saber mais sobre a monitorização de máquinas virtuais.

> [!div class="nextstepaction"]
> [Governar máquinas virtuais](tutorial-govern-resources.md)









