---
title: Transferir itens do marketplace a partir do Azure | Microsoft Docs
description: Posso pode transferir itens do marketplace a partir do Azure para a minha implementação da pilha do Azure.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/08/2018
ms.author: brenduns
ms.reviewer: jeffgo
ms.openlocfilehash: 2e92dc96a69400f689e49b70d1b855c955084362
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/12/2018
---
# <a name="download-marketplace-items-from-azure-to-azure-stack"></a>Transferir os itens do marketplace a partir do Azure com a pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*


Como decidir o conteúdo que pretende incluir na sua marketplace de pilha do Azure, deve considerar o conteúdo disponível no Azure Marketplace. Pode transferir a partir de uma lista organizada de itens do marketplace do Azure que tenham sido previamente testada para executar na pilha do Azure. Novos itens frequentemente são adicionados a esta lista, por isso, certifique-se para o novo conteúdo.

## <a name="download-marketplace-items-in-a-connected-scenario-with-internet-connectivity"></a>Transferir itens do marketplace num cenário ligado (com acesso à Internet)

1. Para transferir os itens do marketplace, deve primeiro [registar pilha do Azure com o Azure](azure-stack-register.md).
2. Inicie sessão no portal de administrador a pilha do Azure (para ASDK, utilize https://portal.local.azurestack.external).
3. Alguns itens do marketplace podem ser elevados. Certifique-se de que tem espaço suficiente no seu sistema clicando **fornecedores de recursos** > **armazenamento**.

    ![](media/azure-stack-download-azure-marketplace-item/image01.png)

4. Clique em **mais serviços** > **Marketplace gestão**.

    ![](media/azure-stack-download-azure-marketplace-item/image02.png)

4. Clique em **adicionar a partir do Azure** para ver uma lista de itens disponíveis para transferência. Pode clicar em cada item na lista para visualizar a descrição e tamanho de transferência.

    ![](media/azure-stack-download-azure-marketplace-item/image03.png)

5. Selecione o item que pretende na lista e, em seguida, clique em **transferir**. A imagem VM para o item selecionado começa a transferir. Tempo de transferência variar.

    ![](media/azure-stack-download-azure-marketplace-item/image04.png)

6. Depois de concluída a transferência, pode implementar o novo item do marketplace como um operador de pilha do Azure ou o utilizador. Clique em **+ novo**, pesquisar entre as categorias para o novo item do marketplace e, em seguida, selecione o item.
7. Clique em **criar** para abrir a experiência de criação para o item recentemente transferido. Siga as instruções passo a passo para implementar o item.

## <a name="download-marketplace-items-in-a-disconnected-or-a-partially-connected-scenario-with-limited-internet-connectivity"></a>Transferir itens do marketplace num desligado ou um cenário parcialmente ligado (com acesso à internet limitada)

Quando implementa a pilha do Azure num modo desligado (sem qualquer acesso à internet), não é possível transferir os itens do marketplace utilizando o portal de pilha do Azure. No entanto, pode utilizar a ferramenta de sindicação do marketplace para transferir os itens do marketplace para uma máquina que tenha acesso à internet e, em seguida, transfere-os para o seu ambiente de pilha do Azure.

### <a name="prerequisites"></a>Pré-requisitos
Antes de poder utilizar a ferramenta de sindicação do marketplace, certifique-se de que tem [registado pilha do Azure com a sua subscrição do Azure](azure-stack-register.md).  

Partir do computador que tenha acesso à internet, utilize os seguintes passos para transferir os itens do marketplace necessário:

1. Abra uma consola do PowerShell como administrador e [instalar os módulos do PowerShell específicos do Azure pilha](azure-stack-powershell-install.md). Certifique-se de que instala **módulo do PowerShell do Azure pilha versão 1.2.11 ou superior**.  

2. Adicione a conta do Azure que utilizou para registar a pilha do Azure. Para adicionar a conta, execute o **Add-AzureRmAccount** cmdlet sem quaisquer parâmetros. É-lhe pedido que introduza as credenciais da conta do Azure e poderá ter de utilizar a autenticação de fator 2, com base na configuração da sua conta.  

3. Se tiver várias subscrições, execute o seguinte comando para selecionar aquela que utilizou para o registo:  

   ```powershell
   Get-AzureRmSubscription -SubscriptionID '<Your Azure Subscription GUID>' | Select-AzureRmSubscription
   $AzureContext = Get-AzureRmContext
   ```

4. Transferir a versão mais recente da ferramenta de sindicação do marketplace utilizando o script seguinte:  

   ```PowerShell
   # Download the tools archive.
   [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12 
   invoke-webrequest https://github.com/Azure/AzureStack-Tools/archive/master.zip `
     -OutFile master.zip

   # Expand the downloaded files.
   expand-archive master.zip `
     -DestinationPath . `
     -Force

   # Change to the tools directory.
   cd \AzureStack-Tools-master

   ```

5. Importe o módulo de sindicação e iniciar a ferramenta, executando os seguintes comandos:  

   ```powershell
   Import-Module .\Syndication\AzureStack.MarketplaceSyndication.psm1

   Sync-AzSOfflineMarketplaceItem `
     -destination "<Destination folder path>" `
     -AzureTenantID $AzureContext.Tenant.TenantId `
     -AzureSubscriptionId $AzureContext.Subscription.Id  
   ```

6. Quando a ferramenta é executada, são-lhe pedido que insira as credenciais de conta do Azure. Iniciar sessão para a conta do Azure que utilizou para registar a pilha do Azure. Após o início de sessão for bem sucedida, verá o ecrã seguinte, com a lista de itens do marketplace disponíveis.  

   ![Sobreposição de itens do Azure Marketplace](./media/azure-stack-download-azure-marketplace-item/image05.png)

7. Selecione a imagem que pretende transferir e tome nota da versão de imagem. Pode selecionar várias imagens ao premir a tecla Ctrl. Utilize a versão da imagem para importar a imagem na secção seguinte.  Em seguida, clique em **Ok**e, em seguida, aceite os termos legais, clicando no **Sim**. Também pode filtrar a lista de imagens, utilizando o **adicionar critérios** opção. 

   A transferência demora algum tempo, consoante o tamanho da imagem. Uma vez as transferências de imagem, está disponível no caminho de destino que forneceu anteriormente. A transferência contém um ficheiro VHD (para máquinas virtuais) ou um. Ficheiro ZIP (para extensões de máquina virtual) e um item da galeria no formato Azpkg.

### <a name="import-the-image-and-publish-it-to-azure-stack-marketplace"></a>Importe a imagem e publicá-lo no mercado de pilha do Azure
Existem três tipos diferentes de itens no marketplace: máquinas virtuais, extensões de Máquina Virtual e modelos de solução. Modelos de solução são abordados abaixo.
> [!NOTE]
> Extensões de máquina virtual não é possível adicionar a pilha de Azure neste momento.

1. Depois de transferir o pacote de imagem e galeria, guarde-los e o conteúdo na pasta master de ferramentas de AzureStack para uma unidade de disco amovível e copie-a para o ambiente de pilha do Azure (pode copiá-la localmente em qualquer localização, tais como: "C:\MarketplaceImages").     

2. Antes de importar a imagem, tem de ligar ao ambiente do operador de pilha do Azure, utilizando os passos descritos no [configurar o ambiente de PowerShell do operador de pilha do Azure](azure-stack-powershell-configure-admin.md).  

3. Se a transferência incluído um pequeno ficheiro VHD de 3MB fixed3.vhd com o nome, é um modelo de solução. Este ficheiro não é necessária; avance para o passo 5. Certifique-se que transferir quaisquer itens dependentes, conforme indicado na descrição para a transferência.

4. Importe a imagem à pilha do Azure utilizando o cmdlet Add-AzsVMImage. Quando utilizar este cmdlet, certifique-se de que substitui o *publicador*, *oferecem*e outros valores de parâmetros com os valores da imagem que está a importar. Pode obter o *publicador*, *oferecem*, e *sku* valores da imagem a partir do objeto imageReference do ficheiro Azpkg que transferiu anteriormente e a  *versão* valor no passo 6 na secção anterior.

Para localizar o imageReference, terá de mudar o nome do ficheiro AZPKG com o. ZIP extensão, extraia-o para uma localização temporária e abra o ficheiro DeploymentTemplates\CreateUiDefinition.json com um editor de texto. Localize nesta secção:

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
    -Version "2016.127.20171215" `
    -OsDiskLocalPath "C:\AzureStack-Tools-master\Syndication\Windows-Server-2016-DatacenterCore-20171215-en.us-127GB.vhd" `
    -CreateGalleryItem $False `
    -Location Local
   ```

5. Portal de utilização para carregar o item do Marketplace (. Azpkg) para pilha Blob storage do Azure. Pode carregar para o armazenamento de Azure pilha local ou carregar para o Storage do Azure. (Esta é uma localização temporária para o pacote.) Certifique-se de que o blob acessível publicamente e tenha em atenção o URI.  

6. Publicar o item do marketplace com pilha do Azure utilizando o **adicionar AzsGalleryItem**. Por exemplo:

   ```powershell
   Add-AzsGalleryItem `
     -GalleryItemUri "https://mystorageaccount.blob.local.azurestack.external/cont1/Microsoft.WindowsServer2016DatacenterServerCore-ARM.1.0.2.azpkg" `
     –Verbose
   ```

7. Depois do item da galeria é publicado, pode ver ao **novo** > **Marketplace** painel. Se a transferência estava um modelo de solução, certifique-se de que transferiu também a imagem VHD dependente.

   ![Marketplace](./media/azure-stack-download-azure-marketplace-item/image06.png)

## <a name="next-steps"></a>Passos Seguintes

[Criar e publicar um item do Marketplace](azure-stack-create-and-publish-marketplace-item.md)
