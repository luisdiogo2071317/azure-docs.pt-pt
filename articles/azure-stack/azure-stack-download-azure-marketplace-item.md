---
title: Transferir itens do marketplace a partir do Azure | Microsoft Docs
description: O operador da nuvem pode transferir os itens do marketplace a partir do Azure para os meus implementação da pilha do Azure.
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
ms.date: 05/16/2018
ms.author: brenduns
ms.reviewer: jeffgo
ms.openlocfilehash: 69148a0ac9a5761eeee0ab47d83862724583619a
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/17/2018
---
# <a name="download-marketplace-items-from-azure-to-azure-stack"></a>Transferir os itens do marketplace a partir do Azure com a pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Como um operador da nuvem, transfira os itens no Azure Marketplace e disponibilizá-los na pilha do Azure. São os itens que pode escolher entre uma lista organizada de itens de Azure Marketplace, que são previamente testada e suportados para trabalhar com a pilha do Azure. Itens adicionais frequentemente são adicionados a esta lista, por isso, continue a verificar para o novo conteúdo. 

Existem dois cenários de ligação para o Azure Marketplace: 

- **Um cenário ligado** -requer que o ambiente de pilha do Azure para ser ligada à internet. Utilizar o portal do Azure pilha para localizar e transferir itens. 
- **Um cenário parcialmente ligado ou desligado** -que requer acesso à internet utilizando a ferramenta de sindicação do marketplace para transferir os itens do marketplace. Em seguida, transferir as transferências para a instalação de Azure pilha desligada. Este cenário utiliza do PowerShell.

Consulte [itens do Azure Marketplace para o Azure pilha](azure-stack-marketplace-azure-items.md) para obter uma lista dos itens do marketplace pode transferir.


## <a name="connected-scenario"></a>Cenário ligado
Pilha do Azure estabelece ligação à internet, pode utilizar o portal de administração para transferir os itens do marketplace.

### <a name="prerequisites"></a>Pré-requisitos
A implementação de pilha do Azure tem de ter conectividade à internet e ser [está registado no Azure](azure-stack-register.md).

### <a name="use-the-portal-to-download-marketplace-items"></a>Utilizar o portal para transferir os itens do marketplace  
1. Inicie sessão no portal de administrador a pilha do Azure.

2.  Reveja o espaço de armazenamento disponível antes de transferir os itens do marketplace. Mais tarde, ao selecionar itens para transferência, pode comparar o tamanho de transferência para a capacidade de armazenamento disponível. Se a capacidade é limitada, considere as opções para [gerir o espaço disponível](azure-stack-manage-storage-shares.md#manage-available-space). 

    Para rever o espaço disponível, no **gestão região** selecione a região que pretende explorar e, em seguida, aceda a **fornecedores de recursos** > **armazenamento**.

    ![Reveja o espaço de armazenamento](media/azure-stack-download-azure-marketplace-item/storage.png)  

    
3. Abra a pilha do Azure Marketplace e ligar ao Azure. Para tal, selecione **gestão Marketplace**e, em seguida, selecione **adicionar a partir do Azure**.

    ![Adicionar a partir do Azure](media/azure-stack-download-azure-marketplace-item/marketplace.png)

    O portal apresenta a lista de itens disponíveis para transferência a partir do Azure Marketplace. Pode clicar em cada item para ver a descrição e informações adicionais sobre o assunto, incluindo o tamanho de transferência. 

    ![Lista de Marketplace](media/azure-stack-download-azure-marketplace-item/image03.png)

4. Selecione o item que pretende e, em seguida, selecione **transferir**. Tempo de transferência variar.

    ![Transferir a mensagem](media/azure-stack-download-azure-marketplace-item/image04.png)

    Depois de concluída a transferência, pode implementar o novo item do marketplace como um operador de pilha do Azure ou o utilizador.

5. Para implementar o item transferido, selecione **+ novo**e, em seguida, procure entre as categorias para o novo item do marketplace. Em seguida, selecione o item para iniciar o processo de implementação. O processo varia consoante o itens do marketplace diferentes. 

## <a name="disconnected-or-a-partially-connected-scenario"></a>Desligado ou um cenário parcialmente ligado

Se a pilha do Azure está num modo desligado e sem conectividade à internet, utilizar o PowerShell e o *ferramenta de sindicação do marketplace* para transferir os itens do marketplace para uma máquina com a conectividade à internet. Em seguida, transferir os itens para o seu ambiente de pilha do Azure. Num ambiente desligado, não é possível transferir os itens do marketplace utilizando o portal de pilha do Azure. 

A ferramenta de sindicação do marketplace também pode ser utilizada num cenário ligado. 

Existem duas partes para este cenário:
- **Parte 1:** transferir a partir do Azure Marketplace. No computador com acesso à internet configurar o PowerShell, transfira a ferramenta de sindicação e, em seguida, transferir o formulário de itens no Azure Marketplace.  
- **Parte 2:** carregar e publicar a pilha do Azure Marketplace. Mova os ficheiros transferidos para o ambiente de pilha do Azure, importe-as à pilha do Azure e, em seguida, publicá-los para a pilha do Azure Marketplace.  


### <a name="prerequisites"></a>Pré-requisitos
- A implementação de pilha do Azure tem de ser [está registado no Azure](azure-stack-register.md).  

- O computador que tem acesso à internet tem de ter **módulo do PowerShell do Azure pilha versão 1.2.11** ou superior. Se não estiver já presente, [instalar os módulos do PowerShell específicos do Azure pilha](azure-stack-powershell-install.md).  

- Para ativar a importação de um item do marketplace transferidos, o [ambiente de PowerShell para o operador de pilha do Azure](azure-stack-powershell-configure-admin.md) tem de ser configurado.  

- Tem de ter uma conta do storage na pilha do Azure que tem um contentor acessível publicamente (que é um blob de armazenamento). Utilizar o contentor como armazenamento temporário para os ficheiros de Galeria de itens do marketplace. Se não estiver familiarizado com as contas de armazenamento e de contentores, consulte [trabalhar com blobs - portal do Azure](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal) na documentação do Azure.

- A ferramenta de sindicação do marketplace é transferida durante o primeiro procedimento. 

### <a name="use-the-marketplace-syndication-tool-to-download-marketplace-items"></a>Utilize a ferramenta de sindicação do marketplace para transferir os itens do marketplace

1. Num computador com uma ligação à Internet, abra uma consola do PowerShell como administrador.

2. Adicione a conta do Azure que utilizou para registar a pilha do Azure. Para adicionar a conta, na execução do PowerShell `Add-AzureRmAccount` sem quaisquer parâmetros. É-lhe pedido que introduza as credenciais da conta do Azure e poderá ter de utilizar a autenticação de fator 2, com base na configuração da sua conta.

3. Se tiver várias subscrições, execute o seguinte comando para selecionar aquela que utilizou para o registo:  

   ```PowerShell  
   Get-AzureRmSubscription -SubscriptionID '<Your Azure Subscription GUID>' | Select-AzureRmSubscription
   $AzureContext = Get-AzureRmContext
   ```

4. Transferir a versão mais recente da ferramenta de sindicação marketplace utilizando o script seguinte:  

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
   cd .\AzureStack-Tools-master

   ```

5. Importe o módulo de sindicação e, em seguida, inicie a ferramenta, executando o seguinte script. Substitua o *caminho da pasta de destino* com uma localização para armazenar os ficheiros que transfere do Azure Marketplace.   

   ```PowerShell  
   Import-Module .\Syndication\AzureStack.MarketplaceSyndication.psm1

   Sync-AzSOfflineMarketplaceItem `
     -destination "Destination folder path" `
     -AzureTenantID $AzureContext.Tenant.TenantId `
     -AzureSubscriptionId $AzureContext.Subscription.Id  
   ```

6. Quando a ferramenta é executada, são-lhe pedido que insira as credenciais de conta do Azure. Iniciar sessão para a conta do Azure que utilizou para registar a pilha do Azure. Após o início de sessão for bem sucedida, verá um ecrã de como a imagem seguinte, com a lista de itens do marketplace disponíveis.  

   ![Sobreposição de itens do Azure Marketplace](media/azure-stack-download-azure-marketplace-item/image05.png)

7. Selecione o item que pretende transferir e anote o *versão*. (Pode selecionar várias imagens ao premir a *Ctrl* chave.) Irá referenciar o *versão* ao importar o item no procedimento seguinte. 
   
   Também pode filtrar a lista de imagens, utilizando o **adicionar critérios** opção.

8. Selecione **OK**e, em seguida, reveja e aceite os termos legais. 

9. O tempo que demora a transferência depende do tamanho do item. Depois de concluída a transferência, o item está disponível na pasta que especificou no script. A transferência inclui um ficheiro VHD (para máquinas virtuais) ou um. Ficheiro ZIP (para extensões de máquina virtual). Também inclui um pacote de galeria no *.azpkg* formato. (A *.azpkg* pacote é um *. zip* ficheiro.)
 

### <a name="import-the-download-and-publish-to-azure-stack-marketplace"></a>Importar a transferência e publicar no Azure Marketplace da pilha
1. Os ficheiros de imagens da máquina virtual ou modelos de solução que tiver [anteriormente transferidos](#use-the-marketplace-syndication-tool-to-download-marketplace-items) tem de ser disponibilizada localmente para o seu ambiente de pilha do Azure.  

2. Importe. Ficheiros VHD a pilha do Azure. Para importar com êxito uma imagem de máquina virtual (VM), tem de ter as seguintes informações sobre a VM:
   - O *versão*, conforme indicado no passo 7 do procedimento anterior.
   - Os valores para as VMs *publicador*, *oferecem*, e *sku*. Para obter estes valores, mudar o nome de uma cópia do **.azpkg** ficheiro para alterar a extensão de ficheiro para **. zip**. Em seguida, pode utilizar um editor de texto para abrir **DeploymentTemplates\CreateUiDefinition.json**. No ficheiro. JSON, localize o *imageReference* secção, que contém estes valores para o item do marketplace. O exemplo seguinte demonstra como estas informações aparecem:

     ```json  
     "imageReference": {  
        "publisher": "MicrosoftWindowsServer",  
        "offer": "WindowsServer",  
        "sku": "2016-Datacenter-Server-Core"  
      }
     ```  

   Importe a imagem à pilha do Azure utilizando o **adicionar AzsPlatformimage** cmdlet. Quando utilizar este cmdlet, certifique-se de que substitui o *publicador*, *oferecem*e outros valores de parâmetros com os valores da imagem que está a importar. Pode obter o *publicador*, *oferecem*, e *sku* valores da imagem do ficheiro de texto que é transferido, juntamente com o ficheiro AZPKG e armazenado na localização de destino . 

   O script de exemplo seguinte, são utilizados valores para o Windows Server 2016 Datacenter - máquina virtual de Server Core. 

   ```PowerShell  
   Add-AzsPlatformimage `
    -publisher "MicrosoftWindowsServer" `
    -offer "WindowsServer" `
    -sku "2016-Datacenter-Server-Core" `
    -osType Windows `
    -Version "2016.127.20171215" `
    -OsDiskLocalPath "C:\AzureStack-Tools-master\Syndication\Windows-Server-2016-DatacenterCore-20171215-en.us-127GB.vhd" `
   ```
   **Sobre os modelos de solução:** alguns modelos podem incluir um pequeno de 3 MB. Ficheiro VHD com o nome **fixed3.vhd**. Não é necessário importar esse ficheiro para a pilha do Azure. Fixed3.vhd.  Este ficheiro está incluído com alguns modelos de solução para satisfazer os requisitos de publicação para o Azure Marketplace.

   Reveja a descrição de modelos e transferir e, em seguida, importar requisitos adicionais, como os VHDs que são necessários para funcionar com o modelo de solução.

3. Utilize o portal de administração para carregar o pacote do item do marketplace (o ficheiro .azpkg) para pilha Blob storage do Azure. Carregamento do pacote torna disponível para a pilha do Azure para que mais tarde pode publicar o item a pilha do Azure Marketplace.

   Carregamento requer que tenha uma conta de armazenamento com um contentor acessível publicamente (consulte os pré-requisitos para este cenário)   
   1. No portal de administração de pilha do Azure, aceda a **mais serviços** > **contas do Storage**.  
   
   2. Selecione uma conta de armazenamento da sua subscrição e, em seguida, em **serviço BLOB**, selecione **contentores**.  
      ![Serviço blob](media/azure-stack-download-azure-marketplace-item/blob-service.png)  
   
   3. Selecione o contentor que pretende utilizar e, em seguida, selecione **carregar** para abrir o **carregar blob** painel.  
      ![contentor](media/azure-stack-download-azure-marketplace-item/container.png)  
   
   4. No painel de blob de carregamento, navegue para os ficheiros que pretende carregar para o armazenamento e, em seguida, selecione **carregar**.  
      ![upload](media/azure-stack-download-azure-marketplace-item/upload.png)  

   5. Os ficheiros que carregar aparecem no painel do contentor. Selecione um ficheiro e, em seguida, copie o URL do **Blob propriedades** painel. Irá utilizar este URL no próximo passo, quando importar o item do marketplace com pilha do Azure.  Na imagem seguinte, o contentor é *armazenamento de BLOBs teste* e o ficheiro é *Microsoft.WindowsServer2016DatacenterServerCore ARM.1.0.801.azpkg*.  O ficheiro de URL é *https://testblobstorage1.blob.local.azurestack.external/blob-test-storage/Microsoft.WindowsServer2016DatacenterServerCore-ARM.1.0.801.azpkg*.  
      ![Propriedades de blob](media/azure-stack-download-azure-marketplace-item/blob-storage.png)  

4.  Utilizar o PowerShell para publicar o item do marketplace com pilha do Azure utilizando o **adicionar AzsGalleryItem** cmdlet. Por exemplo:  
    ```PowerShell  
    Add-AzsGalleryItem `
     -GalleryItemUri "https://mystorageaccount.blob.local.azurestack.external/cont1/Microsoft.WindowsServer2016DatacenterServerCore-ARM.1.0.801.azpkg" `
     –Verbose
    ```
5. Depois de publicar um item da galeria, pode ver a **mais serviços** > **Marketplace**.  Se a transferência é um modelo de solução, certifique-se de que adicionar qualquer imagem VHD dependente para esse modelo de solução.  
  ![Marketplace de vista](media/azure-stack-download-azure-marketplace-item/view-marketplace.png)  

> [!NOTE]
> Com o lançamento da pilha do Azure PowerShell 1.3.0 agora pode adicionar extensões de Máquina Virtual.

Por exemplo:

````PowerShell
Add-AzsVMExtension -Publisher "Microsoft" -Type "MicroExtension" -Version "0.1.0" -ComputeRole "IaaS" -SourceBlob "https://github.com/Microsoft/PowerShell-DSC-for-Linux/archive/v1.1.1-294.zip" -SupportMultipleExtensions -VmOsType "Linux"
````

## <a name="next-steps"></a>Passos Seguintes
[Criar e publicar um item do Marketplace](azure-stack-create-and-publish-marketplace-item.md)