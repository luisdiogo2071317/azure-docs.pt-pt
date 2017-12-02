---
title: Transferir itens do marketplace a partir do Azure | Microsoft Docs
description: "Posso pode transferir itens do marketplace a partir do Azure para a minha implementação da pilha do Azure."
services: azure-stack
documentationcenter: 
author: ErikjeMS
manager: byronr
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/30/2017
ms.author: erikje
ms.openlocfilehash: 33b7be4a85723ab03e4c656a8dd28632ad854e29
ms.sourcegitcommit: 80eb8523913fc7c5f876ab9afde506f39d17b5a1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/02/2017
---
# <a name="download-marketplace-items-from-azure-to-azure-stack"></a>Transferir os itens do marketplace a partir do Azure com a pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Como decidir o conteúdo que pretende incluir na sua marketplace de pilha do Azure, deve considerar o conteúdo disponível no Azure Marketplace. Pode transferir a partir de uma lista organizada de itens do marketplace do Azure que tenham sido previamente testada para executar na pilha do Azure. Novos itens frequentemente são adicionados a esta lista, por isso, certifique-se para o novo conteúdo.

## <a name="download-marketplace-items-in-a-connected-scenario-with-internet-connectivity"></a>Transferir itens do marketplace num cenário ligado (com acesso à internet)

1. Para transferir os itens do marketplace, deve primeiro [registar pilha do Azure com o Azure](azure-stack-register.md). 
2. Inicie sessão no portal de administrador do Azure pilha (https://portal.local.azurestack.external).
3. Alguns itens do marketplace podem ser elevados. Certifique-se de que tem espaço suficiente no seu sistema clicando **fornecedores de recursos** > **armazenamento**.

    ![](media/azure-stack-download-azure-marketplace-item/image01.png)

4. Clique em **mais serviços** > **Marketplace gestão**.

    ![](media/azure-stack-download-azure-marketplace-item/image02.png)

4. Clique em **adicionar a partir do Azure** para ver uma lista de itens disponíveis para transferência. Pode clicar em cada item na lista para visualizar a descrição e tamanho de transferência.

    ![](media/azure-stack-download-azure-marketplace-item/image03.png)

5. Selecione o item que pretende na lista e, em seguida, clique em **transferir**. Esta ação inicia a transferência da imagem VM para o item selecionado. Tempo de transferência variar.

    ![](media/azure-stack-download-azure-marketplace-item/image04.png)

6. Depois de concluída a transferência, pode implementar o novo item do marketplace como um operador de pilha do Azure ou o utilizador. Clique em **+ novo**, pesquisar entre as categorias para o novo item do marketplace e, em seguida, selecione o item.
7. Clique em **criar** para abrir a experiência de criação para o item recentemente transferido. Siga as instruções passo a passo para implementar o item.

## <a name="download-marketplace-items-in-a-disconnected-or-a-partially-connected-scenario-with-limited-internet-connectivity"></a>Transferir itens do marketplace num desligado ou um cenário parcialmente ligado (com acesso à internet limitada)

Quando implementa a pilha do Azure num modo desligado (sem qualquer acesso à internet), não é possível transferir os itens do marketplace utilizando o portal de pilha do Azure. No entanto, pode utilizar a ferramenta de sindicação do marketplace para transferir os itens do marketplace para uma máquina que tenha acesso à internet e, em seguida, transfere-os para o seu ambiente de pilha do Azure. 

### <a name="prerequisites"></a>Pré-requisitos
Antes de poder utilizar a ferramenta de sindicação do marketplace, certifique-se de que tem [registado pilha do Azure com a sua subscrição do Azure](azure-stack-register.md).  

Partir do computador que tenha acesso à internet, utilize os seguintes passos para transferir os itens do marketplace necessário:

1. Abra uma consola do PowerShell como administrador e [instalar os módulos do PowerShell específicos do Azure pilha](azure-stack-powershell-install.md). Certifique-se de que instala **PowerShell versão 1.2.11 ou superior**.  

2. Adicione a conta do Azure que utilizou para registar a pilha do Azure. Para tal, execute o **Add-AzureRmAccount** cmdlet sem quaisquer parâmetros. É-lhe pedido que introduza as credenciais da conta do Azure e poderá ter de utilizar a autenticação de fator 2, com base na configuração da sua conta.  

3. Se tiver várias subscrições, execute o seguinte comando para selecionar aquela que utilizou para o registo:  

   ```powershell
   Get-AzureRmSubscription -SubscriptionID '<Your Azure Subscription GUID>' | Select-AzureRmSubscription
   $AzureContext = Get-AzureRmContext
   ```

4. Transferir a versão mais recente da ferramenta de sindicação do marketplace utilizando o script seguinte:  

   ```PowerShell
   # Download the tools archive.
   invoke-webrequest https://github.com/Azure/AzureStack-Tools/archive/vnext.zip `
     -OutFile vnext.zip

   # Expand the downloaded files.
   expand-archive vnext.zip `
     -DestinationPath . `
     -Force

   # Change to the tools directory.
   cd \AzureStack-Tools-vnext

   ```

5. Importe o módulo de sindicação e iniciar a ferramenta, executando os seguintes comandos:  

   ```powershell
   Import-Module .\ Syndication\AzureStack.MarketplaceSyndication.psm1

   Sync-AzSOfflineMarketplaceItem `
     -destination “<Destination folder path>” `
     -AzureTenantID $AzureContext.Tenant.TenantId `
     -AzureSubscriptionId $AzureContext.Subscription.Id  
   ```

6. Quando a ferramenta é executada, são-lhe pedido que insira as credenciais de conta do Azure. Iniciar sessão para a conta do Azure que utilizou para registar a pilha do Azure. Depois do início de sessão for bem sucedida, verá o ecrã seguinte, com a lista de itens do marketplace disponíveis.  

   ![Sobreposição de itens do Azure Marketplace](./media/azure-stack-download-azure-marketplace-item/image05.png)

7. Selecione a imagem que pretende transferir (pode selecionar várias imagens ao premir a tecla Ctrl) e tome nota da versão de imagem, irá utilizar esta versão para importar a imagem na secção seguinte > clique **Ok** > aceite os termos legais ao clicar no **Sim**. Também pode filtrar a lista de imagens, utilizando o **adicionar critérios** opção. A transferência demora algum tempo, consoante o tamanho da imagem. Uma vez as transferências de imagem, está disponível no caminho de destino que forneceu anteriormente. A transferência contém os itens de galeria e ficheiros VHD no formato Azpkg.  

### <a name="import-the-image-and-publish-it-to-azure-stack-marketplace"></a>Importe a imagem e publicá-lo no mercado de pilha do Azure

1. Depois de transferir o pacote de imagem & Galeria, guarde-los e o conteúdo na pasta Ferramentas-AzureStack-vnext para uma unidade de disco amovível e copie-a para o ambiente de pilha do Azure (pode copiar para localmente em qualquer localização, tais como: "C:\MarketplaceImages".)   

2. Antes de importar a imagem, tem de ligar ao ambiente do operador de pilha do Azure, utilizando os passos descritos no [configurar o ambiente de PowerShell do operador de pilha do Azure](azure-stack-powershell-configure-admin.md).  

3. Importe a imagem à pilha do Azure utilizando o cmdlet Add-AzsVMImage. Quando utilizar este cmdlet, certifique-se de que substitui o publicador, oferta e outros valores de parâmetros com os valores da imagem que está a importar. Pode obter "publisher", "oferta" e "sku" valores da imagem a partir do objeto imageReference do ficheiro Azpkg que transferiu anteriormente e o valor de "versão" do passo 6 na secção anterior.

   ```json
   "imageReference": {
      "publisher": "MicrosoftWindowsServer",
      "offer": "WindowsServer",
      "sku": "2016-Datacenter-Server-Core"
    }
   ```

   Substitua os valores de parâmetros e execute o seguinte comando:

   ```powershell
   Import-Module .\ComputeAdmin\AzureStack.ComputeAdmin.psm1

   Add-AzsVMImage `
    -publisher "MicrosoftWindowsServer" `
    -offer "WindowsServer" `
    -sku "2016-Datacenter-Server-Core" `
    -osType Windows `
    -Version "2017.09.25" `
    -OsDiskLocalPath "C:\AzureStack-Tools-master\Syndication\Microsoft.WindowsServer2016DatacenterServerCore-ARM-Eval.2017.09.25.vhd" `
    -CreateGalleryItem $False `
    -Location Local 
   ```

4. Portal de utilização para carregar o item do Marketplace (. Azpkg) para pilha Blob storage do Azure. Pode carregar para o armazenamento de Azure pilha local ou carregar para o Storage do Azure. (Esta é uma localização temporária para o pacote.) Certifique-se de que o blob acessível publicamente e tenha em atenção o URI.  

5. Publicar o item do marketplace com pilha do Azure utilizando o **adicionar AzureRMGalleryItem**. Por exemplo:

   ```powershell
   Add-AzsGalleryItem `
     -GalleryItemUri "https://mystorageaccount.blob.local.azurestack.external/cont1/Microsoft.WindowsServer2016DatacenterServerCore-ARM.1.0.2.azpkg" `
     –Verbose
   ```

6. Depois do item da galeria é publicado, pode ver ao **novo** > **Marketplace** painel.  

   ![Marketplace](./media/azure-stack-download-azure-marketplace-item/image06.png)

## <a name="next-steps"></a>Passos seguintes

[Criar e publicar um item do Marketplace](azure-stack-create-and-publish-marketplace-item.md)
