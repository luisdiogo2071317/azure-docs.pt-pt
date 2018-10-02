---
title: Ligar ao Azure Stack | Documentos da Microsoft
description: Saiba como ligar ao Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 3cebbfa6-819a-41e3-9f1b-14ca0a2aaba3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/28/2018
ms.author: mabrigg
ms.openlocfilehash: 1cdf013325afe4b217f5f56043e06f60a4933419
ms.sourcegitcommit: 5843352f71f756458ba84c31f4b66b6a082e53df
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/01/2018
ms.locfileid: "47585633"
---
# <a name="connect-to-azure-stack-development-kit"></a>Ligar ao Kit de desenvolvimento do Azure Stack

*Aplica-se a: Azure Stack Development Kit do*

Para gerir os recursos, primeiro tem de ligar para o Azure Stack Development Kit. Neste artigo, descrevemos os passos que efetuar para ligar para o kit de desenvolvimento. Pode utilizar uma das seguintes opções de ligação:

* [Ligação de ambiente de trabalho remota](#connect-with-remote-desktop). Quando se liga ao utilizar a ligação de ambiente de trabalho remota, um único utilizador pode ligar-se rapidamente para o kit de desenvolvimento.
* [Rede privada virtual (VPN)](#connect-with-vpn). Quando se liga através de uma VPN, vários utilizadores em simultâneo podem ligar a partir os clientes fora da infraestrutura do Azure Stack. Uma ligação VPN requer alguma configuração.

<a name="connect-to-azure-stack-with-remote-desktop"></a>
##  <a name="connect-to-azure-stack-by-using-remote-desktop-connection"></a>Ligar ao Azure Stack com a conexão de área de trabalho remoto

Um único utilizador em simultâneo pode gerir os recursos no portal do operador ou o portal de utilizador por meio da conexão de área de trabalho remoto.

1. Abrir a ligação de ambiente de trabalho remoto e ligue-se para o kit de desenvolvimento. Para o nome de utilizador, introduza **AzureStack\AzureStackAdmin**. Utilize o operador palavra-passe que especificou quando configurar o Azure Stack.  

2. No computador do kit de desenvolvimento, abra o Gestor de servidor. Selecione **servidor Local**, desmarque a **segurança avançada do Internet Explorer** caixa de verificação e, em seguida, feche o Gestor de servidor.

3. Para abrir o [portal de utilizador](azure-stack-key-features.md#portal), aceda a https://portal.local.azurestack.external/. Inicie sessão com credenciais de utilizador. Para abrir o Azure Stack [portal de operador](azure-stack-key-features.md#portal), aceda a https://adminportal.local.azurestack.external/. Inicie sessão com as credenciais do Azure Active Directory (Azure AD) que especificou durante a instalação.

<a name="connect-to-azure-stack-with-vpn"></a>
## <a name="connect-to-azure-stack-by-using-vpn"></a>Ligar ao Azure Stack com VPN

Pode estabelecer um ligação de VPN para um Kit de desenvolvimento do Azure Stack de túnel dividido. Pode utilizar uma ligação VPN para aceder ao portal de operador do Azure Stack, o portal de utilizador e as ferramentas instaladas localmente, como o Visual Studio e o PowerShell para gerir os recursos do Azure Stack. A conectividade VPN é suportada no Azure AD e implementações de serviços de Federação do Active Directory (AD FS). Ligações VPN tornam possível para vários clientes ligar ao Azure Stack, ao mesmo tempo.

> [!NOTE]
> Uma ligação VPN não fornecer conectividade a VMs de infraestrutura do Azure Stack.

### <a name="prerequisites"></a>Pré-requisitos

1. Instale [do Azure Stack compatível com o Azure PowerShell](azure-stack-powershell-install.md) no seu computador local.  
2. No desenvolvimento do [Azure Stack Kit, deve ser o blobEndpoint](azure-stack-powershell-download.md) .

### <a name="set-up-vpn-connectivity"></a>Configurar a conectividade VPN

Para criar uma ligação VPN para o kit de desenvolvimento, abra o Windows PowerShell como administrador no seu computador local com base em Windows. Em seguida, execute o seguinte script (o endereço IP e a palavra-passe os valores para o seu ambiente de atualização):

```PowerShell
# Configure Windows Remote Management (WinRM), if it's not already configured.
winrm quickconfig  

Set-ExecutionPolicy RemoteSigned

# Import the Connect module.
Import-Module .\Connect\AzureStack.Connect.psm1

# Add the development kit computer’s host IP address and certificate authority (CA) to the list of trusted hosts. Make sure you update the IP address and password values for your environment.

$hostIP = "<Azure Stack host IP address>"

$Password = ConvertTo-SecureString `
  "<operator's password provided when deploying Azure Stack>" `
  -AsPlainText `
  -Force

Set-Item wsman:\localhost\Client\TrustedHosts `
  -Value $hostIP `
  -Concatenate

# Create a VPN connection entry for the local user.
Add-AzsVpnConnection `
  -ServerAddress $hostIP `
  -Password $Password

```

Se o programa de configuração for concluída com êxito, **azurestack** aparece na sua lista de ligações VPN.

![Ligações de rede](media/azure-stack-connect-azure-stack/image3.png)  

### <a name="connect-to-azure-stack"></a>Ligar ao Azure Stack

Ligar à instância do Azure Stack através de um dos seguintes métodos:  

* Utilize o `Connect-AzsVpn ` comando:
    
  ```PowerShell
  Connect-AzsVpn `
    -Password $Password
  ```

  Quando lhe for pedido, o anfitrião do Azure Stack de confiança e instalar o certificado a partir **AzureStackCertificateAuthority** no arquivo de certificados do seu computador local. (Linha de comandos da botão poderá estar ocultada por janela do PowerShell.)

* No seu computador local, selecione **definições de rede** > **VPN** > **azurestack** > **ligar**. No prompt de início de sessão, introduza o nome de utilizador (**AzureStack\AzureStackAdmin**) e a palavra-passe.

### <a name="test-vpn-connectivity"></a>Testar a conectividade VPN

Para testar a ligação do portal, abra um browser e, em seguida, vá para o portal de utilizador (https://portal.local.azurestack.external/) ou no portal de operador (https://adminportal.local.azurestack.external/). Inicie sessão e criar recursos.  

## <a name="next-steps"></a>Passos Seguintes

[Tornar as máquinas virtuais disponíveis para os utilizadores do Azure Stack](azure-stack-tutorial-tenant-vm.md)
