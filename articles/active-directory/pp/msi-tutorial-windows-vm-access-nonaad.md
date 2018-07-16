---
title: Utilizar um MSI de VM do Windows para aceder ao Azure Key Vault
description: Um tutorial que explica o processo de uso de um Windows VM Managed Service Identity (MSI) para aceder ao Azure Key Vault.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: daveba
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/15/2017
ms.author: daveba
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 20f8ac8f301302f3121880752ea698425c194623
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39049440"
---
# <a name="use-a-windows-vm-managed-service-identity-msi-to-access-azure-key-vault"></a>Utilizar um Windows VM Managed Service Identity (MSI) para aceder ao Azure Key Vault 

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Este tutorial mostra como ativar a identidade de serviço gerida (MSI) para uma máquina de Virtual do Windows, em seguida, utilizar essa identidade para aceder ao Azure Key Vault. Atuando como fazer a inicialização, Key Vault torna possível para a sua aplicação de cliente, em seguida, utilizar o segredo para aceder aos recursos não protegidos pelo Azure Active Directory (AD). Identidades de serviço geridas são geridas automaticamente pelo Azure e ativar a autenticação no serviços que suportam a autenticação do Azure AD, sem a necessidade de inserir as credenciais no seu código. 

Saiba como:


> [!div class="checklist"]
> * Ativar a identidade de serviço gerida numa máquina Virtual Windows 
> * Conceder o acesso VM para um segredo armazenado num cofre de chave 
> * Obter um token de acesso com a identidade da VM e utilizá-lo a obter o segredo do Key Vault 

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

[!INCLUDE [msi-tut-prereqs](~/includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão no Portal do Azure em [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-windows-virtual-machine-in-a-new-resource-group"></a>Criar uma máquina virtual do Windows num novo grupo de recursos

Para este tutorial, vamos criar uma nova VM do Windows. Também pode ativar o MSI numa VM existente.

1.  Clique em **criar um recurso** no canto superior esquerdo do portal do Azure.
2.  Selecione **Computação** e, em seguida, selecione **Windows Server 2016 Datacenter**. 
3.  Introduza as informações da máquina virtual. O **nome de utilizador** e **palavra-passe** criado, Eis aqui as credenciais que utiliza para início de sessão para a máquina virtual.
4.  Selecione o elemento adequado **subscrição** para a máquina virtual na lista pendente.
5.  Para selecionar um novo **grupo de recursos** gostaria de máquina virtual para ser criado no, escolha **criar nova**. Quando terminar, clique em **OK**.
6.  Selecione o tamanho da VM. Para ver mais tamanhos, selecione **Visualizar todos** ou altere o filtro **Tipo de disco suportado**. No painel de definições, mantenha as predefinições e clique em **OK**.

    ![Texto alternativo da imagem](../managed-service-identity/media/msi-tutorial-windows-vm-access-arm/msi-windows-vm.png)

## <a name="enable-msi-on-your-vm"></a>Ativar o MSI na sua VM 

Um MSI de Máquina Virtual permite-lhe obter os tokens de acesso do Azure AD sem a necessidade de colocar as credenciais em seu código. Ativar o MSI informa o Azure para criar uma identidade gerida para a Máquina Virtual. Nos bastidores, ativar o MSI faz duas coisas: instala a extensão de VM de MSI na sua VM e permite a MSI no Azure Resource Manager.

1.  Selecione o **Máquina Virtual** que pretende ativar o MSI em.  
2.  Na barra de navegação esquerdo, clique em **configuração**. 
3.  Verá **identidade do serviço gerido**. Para registar e ativar o MSI, selecione **Sim**, se desejar para desabilitá-lo, selecione não. 
4.  Certifique-se de que clica **guardar** para guardar a configuração.  

    ![Texto alternativo da imagem](../managed-service-identity/media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

5. Se desejar verificar e certifique-se de que extensões estão nesta VM, clique em **extensões**. Se estiver ativado MSI, em seguida, **ManagedIdentityExtensionforWindows** aparece na lista.

    ![Texto alternativo da imagem](../managed-service-identity/media/msi-tutorial-windows-vm-access-arm/msi-windows-extension.png)

## <a name="grant-your-vm-access-to-a-secret-stored-in-a-key-vault"></a>Conceder o acesso à sua VM a um segredo armazenado num cofre de chave 
 
Com o MSI seu código pode obter tokens de acesso para se autenticarem nos recursos que suportam a autenticação do Azure AD.  No entanto, nem todos os serviços do Azure suportam a autenticação do Azure AD. Para utilizar MSI com esses serviços, armazenar as credenciais de serviço no Azure Key Vault e utilizar o MSI para aceder ao Key Vault para obter as credenciais. 

Em primeiro lugar, é necessário criar um Key Vault e conceder acesso de identidade da nossa VM para o Key Vault.   

1. Na parte superior da barra de navegação esquerda, selecione **+ novo** , em seguida, **segurança + identidade** , em seguida, **Key Vault**.  
2. Fornecer um **nome** para o novo cofre de chaves. 
3. Localize o Cofre de chaves no mesmo grupo de recursos e subscrição da VM que criou anteriormente. 
4. Selecione **políticas de acesso** e clique em **adicionar novo**. 
5. Na configuração do modelo, selecione **gestão de segredo**. 
6. Escolher **selecionar Principal**e no campo de pesquisa, introduza o nome da VM que criou anteriormente.  Selecione a VM na lista de resultados e clique em **selecione**. 
7. Clique em **OK** para concluir a adição de nova política de acesso, e **OK** para concluir a seleção da política de acesso. 
8. Clique em **criar** para concluir a criação do Cofre de chaves. 

    ![Texto alternativo da imagem](../managed-service-identity/media/msi-tutorial-windows-vm-access-nonaad/msi-blade.png)


Em seguida, adicione um segredo ao Key Vault, para que mais tarde possa obter o segredo com código em execução na sua VM: 

1. Selecione **todos os recursos**e localize e selecione o Cofre de chaves que criou. 
2. Selecione **segredos**e clique em **Add**. 
3. Selecione **Manual**, do **opções de carregamento**. 
4. Introduza um nome e valor para o segredo.  O valor pode ser que quiser. 
5. Deixe a data de ativação e a data de expiração clara e deixe **Enabled** como **Sim**. 
6. Clique em **criar** para criar o segredo. 
 
## <a name="get-an-access-token-using-the-vm-identity-and-use-it-to-retrieve-the-secret-from-the-key-vault"></a>Obter um token de acesso com a identidade da VM e utilizá-lo a obter o segredo do Key Vault  

Se não tiver PowerShell 4.3.1 ou posterior instalado, precisará [transfira e instale a versão mais recente](https://docs.microsoft.com/powershell/azure/overview).

Em primeiro lugar, usamos MSI da VM para obter um token de acesso para autenticar para o Key Vault:
 
1. No portal, navegue para **máquinas virtuais** e aceda à sua máquina virtual do Windows e na **descrição geral**, clique em **Connect**.
2. Introduza no seu **nome de utilizador** e **palavra-passe** para que adicionou ao criar o **Windows VM**.  
3. Agora que já criou um **conexão de área de trabalho remoto** com a máquina virtual, abra o PowerShell na sessão remota.  
4. No PowerShell, invoca a solicitação da web no inquilino para obter o token para o anfitrião local na porta específica para a VM.  

    O pedido do PowerShell:
    
    ```powershell
    PS C:\> $response = Invoke-WebRequest -Uri http://localhost:50342/oauth2/token -Method GET -Body @{resource="https://vault.azure.net"} -Headers @{Metadata="true"} 
    ```
    
    Em seguida, extrair a resposta completa, que é armazenada como uma cadeia de caracteres de JavaScript Object Notation (JSON) formatado no objeto $response.  
    
    ```powershell
    PS C:\> $content = $response.Content | ConvertFrom-Json 
    ```
    
    Em seguida, extraia o token de acesso da resposta.  
    
    ```powershell
    PS C:\> $KeyVaultToken = $content.access_token 
    ```
    
    Por último, utilize o comando do PowerShell Invoke-WebRequest para recuperar o segredo que criou anteriormente no Key Vault, passando o token de acesso no cabeçalho de autorização.  Terá do URL do seu Cofre de chaves, que está no **Essentials** secção a **descrição geral** página do Cofre de chaves do.  
    
    ```powershell
    PS C:\> (Invoke-WebRequest -Uri https://<your-key-vault-URL>/secrets/<secret-name>?api-version=2016-10-01 -Method GET -Headers @{Authorization="Bearer $KeyVaultToken"}).content 
    ```
    
    A resposta será o seguinte aspeto: 
    
    ```powershell
    {"value":"p@ssw0rd!","id":"https://mytestkeyvault.vault.azure.net/secrets/MyTestSecret/7c2204c6093c4d859bc5b9eff8f29050","attributes":{"enabled":true,"created":1505088747,"updated":1505088747,"recoveryLevel":"Purgeable"}} 
    ```
    
Depois de recuperar o segredo do Key Vault, pode utilizá-lo para se autenticarem num serviço que requer um nome e uma palavra-passe. 

## <a name="related-content"></a>Conteúdo relacionado

- Para uma descrição geral do MSI, consulte [descrição geral de identidade do serviço gerido](msi-overview.md).

Utilize a seguinte secção de comentários para fornecer comentários e ajude-na refinar e moldar o nosso conteúdo.
