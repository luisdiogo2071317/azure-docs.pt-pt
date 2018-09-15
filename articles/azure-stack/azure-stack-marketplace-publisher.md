---
title: Use o Kit de ferramentas do Marketplace para criar e publicar itens do marketplace | Documentos da Microsoft
description: Saiba como criar rapidamente os itens do marketplace com a Kit de ferramentas de publicação
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/14/2017
ms.author: sethm
ms.reviewer: jeffgo
ms.openlocfilehash: 0ade78dd992e8d1d2eda2cf27d44e52c4030563f
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/15/2018
ms.locfileid: "45630932"
---
#  <a name="add-marketplace-items-using-publishing-tool"></a>Adicionar itens do marketplace com a ferramenta de publicação
Adicionar o seu conteúdo para o [do Azure Stack Marketplace](azure-stack-marketplace.md) torna as suas soluções disponíveis para e seus inquilinos para a implementação.  O Kit de ferramentas do Marketplace cria ficheiros de pacotes de mercado do Azure (.azpkg) com base nos seus modelos de IaaS do Azure Resource Manager ou extensões de VM.  Também pode utilizar o Kit de ferramentas do Marketplace para publicar ficheiros .azpkg, criado com a ferramenta ou usando [manual](azure-stack-create-and-publish-marketplace-item.md) passos.  Este tópico orienta-a baixar a ferramenta de, criação de um item do marketplace com base num modelo de VM e, em seguida, publicar esse item no Marketplace de pilha do Azure.     


## <a name="prerequisites"></a>Pré-requisitos
 - Tem de executar o Kit de ferramentas no anfitrião do Azure Stack ou ter [VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) conectividade a partir do computador em que executar a ferramenta.

 - Transfira o [modelos de início rápido do Azure Stack](https://github.com/Azure/AzureStack-QuickStart-Templates/archive/master.zip) e extrair.

 - Transfira o [ferramenta de empacotamento de galeria do Azure](http://aka.ms/azurestackmarketplaceitem) (AzureGalleryPackage.exe). 

 - A publicação no Marketplace requer ícones e um ficheiro da miniatura.  Pode utilizar o seu próprio ou guardar o [exemplo](azure-stack-marketplace-publisher.md#support-files) ficheiros localmente para este exemplo.

## <a name="download-the-tool"></a>Transferir a ferramenta
Pode ser o Kit de ferramentas do Marketplace [transferido a partir do repositório de ferramentas do Azure Stack](azure-stack-powershell-download.md).


##  <a name="create-marketplace-items"></a>Criar itens do marketplace
Nesta secção, vai utilizar o Kit de ferramentas do Marketplace para criar um pacote de item do marketplace no formato de .azpkg.  

### <a name="provide-marketplace-information-with-wizard"></a>Fornecer informação do marketplace com o Assistente
1. Execute o Kit de ferramentas do Marketplace de uma sessão do PowerShell:
```PowerShell
    .\MarketplaceToolkit.ps1
```

2. Clique nas **solução** separador.  Este ecrã aceita informações sobre o item do marketplace. Introduza as informações sobre o item que quiser que apareça no marketplace.  Também pode especificar uma [ficheiro de parâmetros](azure-stack-marketplace-publisher.md#use-a-parameters-file) para pré-povoar o formulário.  
    
    ![captura de ecrã do ecrã principal do Kit de ferramentas do Marketplace](./media/azure-stack-marketplace-publisher/image7.png)
3. Clique em **procurar** e selecione um ficheiro de imagem para cada campo de ícone e a captura de ecrã.  Pode usar seus próprios ícones ou os ícones de exemplo no [ficheiros de suporte](azure-stack-marketplace-publisher.md#support-files) secção.
4. Depois de todos os campos são preenchidos, selecione "Solução de pré-visualização" para uma pré-visualização da solução no Marketplace.  Pode rever e editar o texto, imagens e captura de ecrã antes de clicar em **seguinte**.  

### <a name="import-template-and-create-package"></a>Importar o modelo e criar pacote
Nesta secção, importar o modelo e trabalhar com entradas para a sua solução.

1.  Clique em **navegue** e selecione o *azuredeploy. JSON* da pasta 101-simples-Windows-VM nos modelos de transferido.

    ![captura de ecrã do ecrã de segundo do Kit de ferramentas do Marketplace](./media/azure-stack-marketplace-publisher/image8.png)
2.  O Assistente de implementação é preenchido com um *básica* itens de entrada e passo para cada parâmetro especificado no modelo.  Pode adicionar mais passos e mover entradas entre passos.  Por exemplo, pode desejar passos "Configuração de front-end" e "Configuração de Back-End" para a sua solução.
3.  Especifique o caminho para AzureGalleryPackager.exe.  
4.  Clique em **criar** e o Kit de ferramentas do Marketplace empacota a sua solução num arquivo de .azpkg.  Depois de concluído, o assistente apresenta o caminho para o arquivo da solução e lhe dará a opção para continuar a publicar o seu pacote para o Azure Stack.


## <a name="publish-marketplace-items"></a>Publicar itens do marketplace
Nesta seção, publicar o item do marketplace para sua pilha do Azure Marketplace.

![captura de ecrã do ecrã principal do Kit de ferramentas do Marketplace](./media/azure-stack-marketplace-publisher/image9.png)

1.  O assistente requer informações para publicar a sua solução:
    
    |Campo|Descrição|
    |-----|-----|
    | Nome de administrador de serviço | Conta de administrador de serviço.  Exemplo:  ServiceAdmin@mydomain.onmicrosoft.com |
    | Palavra-passe | Palavra-passe da conta de administrador de serviço. |
    | Ponto final de API | Ponto final do Azure Stack Azure Resource Manager.  Exemplo: management.local.azurestack.external |
2.  Clique em **publicar** e o registo de publicação é apresentado.
3.  Agora é possível implementar o seu item publicado através do portal do Azure Stack.


## <a name="use-a-parameters-file"></a>Utilizar um ficheiro de parâmetros
Também pode utilizar um ficheiro de parâmetros para concluir as informações de item do marketplace.  

O Kit de ferramentas do Marketplace inclui um *solution.parameters.ps1* pode utilizar para criar seu próprio ficheiro de parâmetros.


## <a name="support-files"></a>Ficheiros de suporte
| Descrição | Sample |
| ----- | ----- |
| ícone do 40 x 40. png | ![](./media/azure-stack-marketplace-publisher/image1.png) |
| ícone do 90 x 90. png | ![](./media/azure-stack-marketplace-publisher/image2.png) |
| ícone do 115 x 115. png | ![](./media/azure-stack-marketplace-publisher/image3.png) |
| ícone do 255 x 115. png | ![](./media/azure-stack-marketplace-publisher/image4.png) |
| miniatura do 533 x 324. png | ![](./media/azure-stack-marketplace-publisher/image5.png) |


