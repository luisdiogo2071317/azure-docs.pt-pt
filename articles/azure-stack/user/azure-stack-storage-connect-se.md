---
title: Ligar o Explorador de armazenamento para uma subscrição do Azure pilha ou uma conta de armazenamento | Microsoft Docs
description: Saiba como ligar o Explorador de armazenamento para uma subscrição de pilha do Azure
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/20/2018
ms.author: mattbriggs
ms.reviewer: xiaofmao
ms.openlocfilehash: 8b670ec7040aab7eca26d411c9e31a934052be19
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/23/2018
---
# <a name="connect-storage-explorer-to-an-azure-stack-subscription-or-a-storage-account"></a>Ligar o Explorador de armazenamento para uma subscrição do Azure pilha ou uma conta de armazenamento

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Explorador de armazenamento do Azure é uma aplicação autónoma que lhe permite trabalhar facilmente com dados de armazenamento de pilha do Azure no Windows, macOS e Linux. Existem várias ferramentas disponíveis para mover dados para e do armazenamento de pilha do Azure. Para obter mais informações, consulte [ferramentas para armazenamento de Azure pilha de transferência de dados](azure-stack-storage-transfer.md).

Neste artigo, irá aprender a estabelecer ligação às suas contas de armazenamento utilizando o Explorador de armazenamento e subscrições de pilha do Azure. 

Se ainda não instalou o Explorador de armazenamento ainda, [transferir](http://www.storageexplorer.com/) e instalá-la.

Depois de ligar a uma subscrição do Azure pilha ou uma conta de armazenamento, pode utilizar o [artigos do Explorador de armazenamento do Azure](../../vs-azure-tools-storage-manage-with-storage-explorer.md) para trabalhar com os seus dados de pilha do Azure. 

## <a name="prepare-for-connecting-to-azure-stack"></a>Preparar para ligar à pilha do Azure

Precisa de acesso direto à pilha do Azure ou uma ligação VPN para o Explorador de armazenamento para aceder à subscrição do Azure pilha. Para saber como configurar uma ligação VPN ao Azure Stack, veja [Connect to Azure Stack with VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) (Ligar ao Azure Stack com VPN).

Para o Kit de desenvolvimento de pilha do Azure, terá de exportar o certificado de raiz da autoridade de pilha do Azure.

### <a name="export-and-then-import-the-azure-stack-certificate"></a>Exportar e, em seguida, importar o certificado de pilha do Azure

1. Abra `mmc.exe` uma máquina de anfitrião de pilha do Azure ou uma máquina local com uma ligação VPN à pilha do Azure. 

2. No **ficheiro**, selecione **Adicionar/Remover Snap-in**e, em seguida, adicione **certificados** para gerir **minha conta de utilizador**.

3. Em **consola Root\Certificated (computador Local) \Trusted Root Certification Authorities\Certificates** localizar **AzureStackSelfSignedRootCert**.

    ![Carregue o certificado de raiz do Azure stack através de mmc.exe](./media/azure-stack-storage-connect-se/add-certificate-azure-stack.png)

4. O certificado com o botão direito, selecione **todas as tarefas** > **exportar**e, em seguida, siga as instruções para exportar o certificado com **x. 509 com codificação Base 64 (. CER)**.  

    O certificado exportado será utilizado no próximo passo.

5. Iniciar o Explorador de armazenamento, e se vir o **ligar ao armazenamento de Azure** diálogo caixa, cancelá-lo.

6. No **editar** menu, aponte para **certificados SSL**e, em seguida, selecione **importar certificados**. Utilize a caixa de diálogo do selecionador de ficheiros para encontrar e abrir o certificado que foi exportado no passo anterior.

    Depois de importar, é-lhe pedido para reiniciar o Explorador de Armazenamento.

    ![Importar o certificado para o Explorador de armazenamento](./media/azure-stack-storage-connect-se/import-azure-stack-cert-storage-explorer.png)

7. Depois de reiniciar o Explorador de armazenamento, selecione o **editar** menu e, em seguida, certifique-se de que **pilha do Azure de destino** está selecionada. Se não estiver, selecione-o e reinicie o Explorador de Armazenamento para que a alteração entre em vigor. Esta configuração é necessária para ser compatível com o seu ambiente do Azure Stack.

    ![Certifique-se de que o Azure Stack de destino está selecionado](./media/azure-stack-storage-connect-se/target-azure-stack.png)

## <a name="connect-to-an-azure-stack-subscription"></a>Ligar a uma subscrição do Azure Stack

Agora, está pronto para estabelecer a ligação Explorador de armazenamento para uma subscrição de pilha do Azure.

1. No painel esquerdo do Explorador de armazenamento, selecione **gerir contas**.  
    São apresentados todos os a subscrição da Microsoft que iniciou sessão.

2. Para ligar à subscrição do Azure pilha, selecione **adicionar uma conta**.

    ![Adicionar uma conta do Azure Stack](./media/azure-stack-storage-connect-se/add-azure-stack-account.png)

3. Na ligar à caixa de diálogo de armazenamento do Azure, em **ambiente do Azure**, selecione **Azure** ou **Azure China**, que depende da conta de pilha do Azure que está a ser utilizada, e em seguida, selecione **sessão**. Para iniciar sessão com a conta de pilha do Azure que está associada a pelo menos uma subscrição de pilha do Azure Active Directory.

    ![Ligar ao armazenamento do Azure](./media/azure-stack-storage-connect-se/azure-stack-connect-to-storage.png)

4. Depois de iniciar sessão com êxito numa conta do Azure Stack, o painel esquerdo é preenchido com as subscrições do Azure Stack associadas a essa conta. Selecione as subscrições de pilha do Azure pretende trabalhar e, em seguida, selecione a aplicar. (Selecionar ou desmarcar os todas as subscrições caixa de verificação botões de alternar selecionar todas ou nenhuma das subscrições Azure pilha listadas.)

    ![Selecione as subscrições do Azure Stack depois de preencher a caixa de diálogo de Ambiente de Cloud Personalizado](./media/azure-stack-storage-connect-se/select-accounts-azure-stack.png)  

    O painel esquerdo apresenta as contas de armazenamento associadas às subscrições do Azure Stack selecionadas.

    ![Lista das contas de armazenamento, incluindo as contas de subscrição do Azure Stack](./media/azure-stack-storage-connect-se/azure-stack-storage-account-list.png)

## <a name="connect-to-an-azure-stack-storage-account"></a>Ligar a uma conta de armazenamento de pilha do Azure

Também pode ligar a uma conta de armazenamento de pilha do Azure utilizando o nome da conta de armazenamento e o par de chaves.

1.  No painel esquerdo do Explorador de armazenamento, selecione gerir contas. São apresentadas todas as contas do Microsoft que tem sessão iniciada.

    ![Adicionar uma conta](./media/azure-stack-storage-connect-se/azure-stack-sub-add-an-account.png)

2.  Para ligar à subscrição do Azure pilha, selecione **adicionar uma conta**.
 
    ![Adicionar uma conta](./media/azure-stack-storage-connect-se/azure-stack-use-a-storage-and-key.png)

3.  Ligar à caixa de diálogo de armazenamento do Azure, selecionar **utilizar um nome de conta de armazenamento e a chave**.

4. Introduza o nome da sua conta na **nome da conta**, cole a chave de conta para o **chave da conta** caixa de texto, selecione **outros (introduza abaixo)** no **pontos finais de armazenamento domínio** e o ponto final de pilha do Azure de entrada.  

    Um ponto final de pilha do Azure inclui duas partes: o nome de uma região e o domínio de pilha do Azure. O Kit de desenvolvimento de pilha do Azure, o ponto final predefinido é **local.azurestack.external**. Se não tiver a certeza sobre o ponto final, contacte o administrador da nuvem.

    ![Anexar o nome e a chave](./media/azure-stack-storage-connect-se/azure-stack-attach-name-and-key.png)

5.  Selecione **Ligar**.
6.  Depois da conta de armazenamento foi anexada com êxito, a conta de armazenamento é apresentada com (**externo, outros**) acrescentado ao respetivo nome.

    ![VMWINDISK](./media/azure-stack-storage-connect-se/azure-stack-vmwindisk.png)

## <a name="next-steps"></a>Passos Seguintes
* [Introdução ao Explorador de armazenamento](../../vs-azure-tools-storage-manage-with-storage-explorer.md)
* [Armazenamento do Azure da pilha: diferenças e as considerações](azure-stack-acs-differences.md)
* Para saber mais sobre o Storage do Azure, consulte [introdução ao Storage do Microsoft Azure](../../storage/common/storage-introduction.md)
