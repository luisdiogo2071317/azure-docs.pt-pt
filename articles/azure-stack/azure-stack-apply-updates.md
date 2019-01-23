---
title: Aplicar atualizações no Azure Stack | Documentos da Microsoft
description: Saiba como importar e instalar pacotes de atualização da Microsoft para um sistema integrado do Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 1/18/2019
ms.author: mabrigg
ms.reviewer: wfayed
ms.openlocfilehash: 190d81fc7811e4afdb32555407716f60f5b9a2d1
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/23/2019
ms.locfileid: "54476143"
---
# <a name="apply-updates-in-azure-stack"></a>Aplicar atualizações no Azure Stack

*Aplica-se a: Sistemas integrados do Azure Stack*

Pode utilizar o **atualizar** mosaico no portal de administração para aplicar os pacotes de atualização de OEM da Microsoft ou para o Azure Stack. Tem de transferir o pacote de atualização, importe os ficheiros de pacote para o Azure Stack e, em seguida, instale o pacote de atualização.

## <a name="download-the-update-package"></a>Transferir o pacote de atualização

Quando um pacote de atualização de OEM da Microsoft ou para o Azure Stack está disponível, transferir o pacote para uma localização que esteja acessível a partir do Azure Stack e rever o conteúdo do pacote. Um pacote de atualização consiste, normalmente, os seguintes ficheiros:

- Extração automática `<PackageName>.exe` ficheiro. Este ficheiro contém a carga para a atualização, por exemplo a atualização cumulativa mais recente para o Windows Server.

- Correspondente `<PackageName>.bin` ficheiros. Estes ficheiros fornecem compactação para o payload que está associada a *PackageName*.exe ficheiro.

- A `Metadata.xml` ficheiro. Esse arquivo contém informações essenciais sobre a atualização, por exemplo o publicador, nome, pré-requisito, tamanho e URL do caminho de suporte.

## <a name="import-and-install-updates"></a>Importar e instalar atualizações

O procedimento seguinte mostra como importar e instalar pacotes de atualização no portal do administrador.

> [!IMPORTANT]  
> Recomendamos vivamente que notifique os utilizadores de quaisquer operações de manutenção e agendar a janelas de manutenção normal durante o horário não comercial tanto quanto possível. Operações de manutenção podem afetar as cargas de trabalho de utilizador e de operações do portal.

1. No portal do administrador, selecione **todos os serviços**. Em seguida, no **dados + armazenamento** categoria, selecione **contas de armazenamento**. (Ou, na caixa de filtro, comece a escrever **contas de armazenamento**e selecioná-lo.)

    ![Mostra onde encontrar as contas de armazenamento no portal](media/azure-stack-apply-updates/ApplyUpdates1.png)

2. Na caixa de filtro, escreva **atualizar**e selecione o **updateadminaccount** conta de armazenamento.

3. No armazenamento de detalhes da conta, em **serviços**, selecione **Blobs**.
 
    ![Mostra como obter para Blobs para a conta de armazenamento](media/azure-stack-apply-updates/ApplyUpdates3.png) 

4. Sob **serviço Blob**, selecione **+ contentor** para criar um contentor. Introduza um nome (por exemplo *atualização 1811*) e, em seguida, selecione **OK**.
 
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

Pode eliminar manualmente as atualizações da conta de armazenamento depois de ter sido instaladas no Azure Stack. O Azure Stack periodicamente verifica a existência de pacotes de atualização de mais antigos e remove-los a partir do armazenamento. Poderá demorar Azure Stack duas semanas para remover os pacotes antigos.

## <a name="next-steps"></a>Passos Seguintes

- [Gerir atualizações na descrição geral do Azure Stack](azure-stack-updates.md)
- [O Azure Stack a política de manutenção](azure-stack-servicing-policy.md)
