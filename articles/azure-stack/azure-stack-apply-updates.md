---
title: Aplicar atualizações no Azure Stack | Documentos da Microsoft
description: Saiba como importar e instalar pacotes de atualização da Microsoft para um sistema integrado do Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 449ae53e-b951-401a-b2c9-17fee2f491f1
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/20/2017
ms.author: mabrigg
ms.openlocfilehash: da8261d27ae7fad3c5ff30e4e1cce3f1bca2b70a
ms.sourcegitcommit: 04fc1781fe897ed1c21765865b73f941287e222f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2018
ms.locfileid: "39035338"
---
# <a name="apply-updates-in-azure-stack"></a>Aplicar atualizações no Azure Stack

*Aplica-se a: sistemas integrados do Azure Stack*

Como um operador do Azure Stack, pode aplicar Microsoft ou mosaico de pacotes de atualização do OEM para o Azure Stack, utilizando a atualização no portal do administrador. Tem de transferir o pacote de atualização, importe os ficheiros de pacote para o Azure Stack e, em seguida, instale o pacote de atualização. 

## <a name="download-the-update-package"></a>Transferir o pacote de atualização

Quando um pacote de atualização de OEM da Microsoft ou para o Azure Stack está disponível, transferir o pacote para uma localização que esteja acessível a partir do Azure Stack e rever o conteúdo do pacote. Um pacote de atualização consiste, normalmente, os seguintes ficheiros:

- Extração automática *PackageName*.exe ficheiro. Este ficheiro contém a carga para a atualização, por exemplo a atualização cumulativa mais recente para o Windows Server.   
- Correspondente *PackageName*arquivos. bin. Estes ficheiros fornecem compactação para o payload que está associada a *PackageName*.exe ficheiro. 
- Um arquivo METADATA XML. Esse arquivo contém informações essenciais sobre a atualização, por exemplo o publicador, nome, pré-requisito, tamanho e URL do caminho de suporte.

## <a name="import-and-install-updates"></a>Importar e instalar atualizações

O procedimento seguinte mostra como importar e instalar pacotes de atualização no portal do administrador.

> [!IMPORTANT]
> Recomendamos vivamente que notifique os utilizadores de quaisquer operações de manutenção e agendar a janelas de manutenção normal durante o horário não comercial tanto quanto possível. Operações de manutenção podem afetar as cargas de trabalho de utilizador e de operações do portal.

1. No portal do administrador, selecione **mais serviços**. Em seguida, no **dados + armazenamento** categoria, selecione **contas de armazenamento**. (Ou, na caixa de filtro, comece a escrever **contas de armazenamento**e selecioná-lo.)

    ![Mostra onde encontrar as contas de armazenamento no portal](media/azure-stack-apply-updates/ApplyUpdates1.png)

2. Na caixa de filtro, escreva **atualizar**e selecione o **updateadminaccount** conta de armazenamento.

    ![Mostra como procurar updateadminaccount](media/azure-stack-apply-updates/ApplyUpdates2.png)

3. No armazenamento de detalhes da conta, em **serviços**, selecione **Blobs**.
 
    ![Mostra como obter para Blobs para a conta de armazenamento](media/azure-stack-apply-updates/ApplyUpdates3.png) 
 
4. Sob **serviço Blob**, selecione **+ contentor** para criar um contentor. Introduza um nome (por exemplo *atualização 1709*) e, em seguida, selecione **OK**.
 
     ![Mostra como adicionar um contentor na conta de armazenamento](media/azure-stack-apply-updates/ApplyUpdates4.png)

5. Depois do contentor é criado, clique no nome de contentor e, em seguida, clique em **carregar** para carregar os ficheiros de pacote para o contentor.
 
    ![Mostra como carregar os ficheiros do pacote](media/azure-stack-apply-updates/ApplyUpdates5.png)

6. Sob **carregar blob**, clique no ícone de pasta, procure .exe ficheiro o pacote de atualização e, em seguida, clique em **aberto** na janela do Explorador de ficheiros.
  
7. Sob **carregar blob**, clique em **carregar**. 
 
    ![Mostra onde carregar cada ficheiro de pacote](media/azure-stack-apply-updates/ApplyUpdates6.png)

8. Repita os passos 6 e 7 para o *PackageName*. bin e arquivos de METADATA. Não importe o arquivo notice suplementares se incluído.
9. Quando terminar, pode rever as notificações (ícone de sino no canto superior direito do portal). As notificações devem indicar que concluiu o carregamento. 
10. Navegue de volta para o atualizar mosaico no dashboard. O mosaico deve indicar que está disponível uma atualização. Clique no mosaico para rever o pacote de atualização adicionada recentemente.
11. Para instalar a atualização, selecione o pacote que está marcado como **pronto** e um pacote com o botão direito e selecione **atualizar agora**, ou clique nas **atualizar agora** ação perto da parte superior .
12. Ao clicar o pacote de atualização de instalação, pode ver o estado dos **detalhes de execução de atualização** área. A partir daqui, pode também clicar **transferir registos completos** para transferir os ficheiros de registo.
13. Quando a atualização estiver concluída, o mosaico de atualização mostra a versão atualizada do Azure Stack.

## <a name="next-steps"></a>Passos Seguintes

- [Gerir atualizações na descrição geral do Azure Stack](azure-stack-updates.md)
- [O Azure Stack a política de manutenção](azure-stack-servicing-policy.md)
