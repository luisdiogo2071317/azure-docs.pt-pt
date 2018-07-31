---
title: Utilizar uma Identidade de Serviço Gerida de VM do Windows para aceder ao Azure Key Vault
description: Um tutorial que explica o processo de utilização de uma Identidade de Serviço Gerida de VM do Windows para aceder ao Azure Key Vault.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: daveba
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/20/2017
ms.author: daveba
ms.openlocfilehash: 81bab96b91bb71a91ea0b6046b16ef86c8d27061
ms.sourcegitcommit: 156364c3363f651509a17d1d61cf8480aaf72d1a
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/25/2018
ms.locfileid: "39248066"
---
# <a name="tutorial-use-a-windows-vm-managed-service-identity-to-access-azure-key-vault"></a>Tutorial: utilizar uma Identidade de Serviço Gerida de VM do Windows para aceder ao Azure Key Vault 

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Este tutorial mostra como ativar a Identidade de Serviço Gerida para uma Máquina Virtual do Windows e, em seguida, utilizar essa identidade para aceder ao Azure Key Vault. A servir de arranque, o Key Vault permite à sua aplicação cliente utilizar o segredo para aceder aos recursos não protegidos pelo Azure Active Directory (AD). As Identidades de Serviço Geridas são geridas automaticamente pelo Azure e permitem-lhe fazer a autenticação em serviços que suportam a autenticação do Azure AD, sem ser necessário inserir as credenciais no seu código. 

Saiba como:


> [!div class="checklist"]
> * Ativar a Identidade de Serviço Gerida numa Máquina Virtual do Windows 
> * Conceder o acesso da VM a um segredo armazenado num Key Vault 
> * Obter um token de acesso com a identidade da VM e utilizá-lo para obter o segredo a partir do Key Vault 

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão no Portal do Azure em [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-windows-virtual-machine-in-a-new-resource-group"></a>Criar uma máquina virtual do Windows num novo grupo de recursos

Neste tutorial, vamos criar uma nova VM do Windows. Também pode ativar a Identidade de Serviço Gerida numa VM existente.

1.  Clique no botão **Criar um recurso**, no canto superior esquerdo do portal do Azure.
2.  Selecione **Computação** e, em seguida, selecione **Windows Server 2016 Datacenter**. 
3.  Introduza as informações da máquina virtual. O **Nome de Utilizador** e a **Palavra-passe** aqui criados são as credenciais a utilizar para iniciar sessão na máquina virtual.
4.  Escolha uma **Subscrição** para a máquina virtual na lista pendente.
5.  Para selecionar um novo **Grupo de Recursos** no qual gostaria que a máquina virtual fosse criada, selecione **Criar Novo**. Quando terminar, clique em **OK**.
6.  Selecione o tamanho da VM. Para ver mais tamanhos, selecione **Visualizar todos** ou altere o filtro **Tipo de disco suportado**. No painel de definições, mantenha as predefinições e clique em **OK**.

    ![Texto alternativo da imagem](media/msi-tutorial-windows-vm-access-arm/msi-windows-vm.png)

## <a name="enable-managed-service-identity-on-your-vm"></a>Ativar a Identidade de Serviço Gerida na sua VM 

Uma Identidade de Serviço Gerida de Máquina Virtual permite-lhe obter os tokens de acesso do Azure AD, sem ter de colocar as credenciais no código. A ativação da Identidade de Serviço Gerida informa o Azure para criar uma identidade gerida para a sua Máquina Virtual. Em segundo plano, a ativação da Identidade de Serviço Gerida faz duas coisas: regista a sua VM no Azure Active Directory para criar a respetiva identidade gerida e configura a identidade na VM.

1.  Selecione a **Máquina Virtual** na qual quer ativar a Identidade de Serviço Gerida.  
2.  Na barra de navegação esquerda, clique em **Configuração**. 
3.  Vai ver a **Identidade de Serviço Gerida**. Para registar e ativar a Identidade de Serviço Gerida, selecione **Sim**; se desejar desativá-la, selecione Não. 
4.  Certifique-se de que clica em **Guardar** para guardar a configuração.  

    ![Texto alternativo da imagem](media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

## <a name="grant-your-vm-access-to-a-secret-stored-in-a-key-vault"></a>Conceder o acesso da VM a um segredo armazenado num Key Vault 
 
Com a Identidade de Serviço Gerida, o seu código pode obter tokens de acesso para autenticação em recursos que suportam a autenticação do Azure AD.  No entanto, nem todos os serviços do Azure suportam a autenticação do Azure AD. Para utilizar a Identidade de Serviço Gerida com esses serviços, armazene as credenciais do serviço no Azure Key Vault e utilize a Identidade de Serviço Gerida para aceder ao Key Vault para obter as credenciais. 

Primeiro, é necessário criar um Key Vault e conceder o acesso de identidade da VM ao Key Vault.   

1. Na parte superior da barra de navegação esquerda, selecione **Criar um recurso** > **Segurança + Identidade** > **Key Vault**.  
2. Indique um **Nome** para o novo Key Vault. 
3. Localize o Key Vault na mesma subscrição e grupo de recursos da VM que criou anteriormente. 
4. Selecione **Políticas de acesso** e clique em **Adicionar novo**. 
5. Em Configurar a partir do modelo, selecione **Gestão de Segredos**. 
6. Selecione **Selecionar Principal** e, no campo de pesquisa, introduza o nome da VM que criou anteriormente.  Selecione a VM na lista de resultados e clique em **Selecionar**. 
7. Clique em **OK** para concluir a adição da nova política de acesso e em **OK** para concluir a seleção da política de acesso. 
8. Clique em **Criar** para concluir a criação do Key Vault. 

    ![Texto alternativo da imagem](../managed-service-identity/media/msi-tutorial-windows-vm-access-nonaad/msi-blade.png)


Em seguida, adicione um segredo ao Key Vault para que possa mais tarde obter o segredo com o código em execução na sua VM: 

1. Selecione **Todos os Recursos** e localize e selecione o Key Vault que criou. 
2. Selecione **Segredos** e clique em **Adicionar**. 
3. Selecione **Manual** em **Opções de carregamento**. 
4. Introduza o nome e o valor do segredo.  O valor pode ser o que quiser. 
5. Deixe as datas de ativação e expiração claras e mantenha **Ativado** como **Sim**. 
6. Clique em **Criar** para criar o segredo. 
 
## <a name="get-an-access-token-using-the-vm-identity-and-use-it-to-retrieve-the-secret-from-the-key-vault"></a>Obter um token de acesso com a identidade da VM e utilizá-lo para obter o segredo a partir do Key Vault  

Se não tiver o PowerShell 4.3.1 ou posterior instalado, terá de [transferir e instalar a versão mais recente](https://docs.microsoft.com/powershell/azure/overview).

Primeiro, utilizamos a Identidade de Serviço Gerida da VM para obter um token de acesso para autenticação no Key Vault:
 
1. No portal, navegue para **Máquinas Virtuais**, aceda à sua máquina virtual do Windows e, em **Descrição Geral**, clique em **Ligar**.
2. Introduza o seu **Nome de Utilizador** e a **Palavra-passe** que adicionou quando criou a **VM do Windows**.  
3. Agora que já criou uma **Ligação ao Ambiente de Trabalho Remoto** com a máquina virtual, abra o PowerShell na sessão remota.  
4. No PowerShell, invoca o pedido Web no inquilino para obter o token para o anfitrião local na porta específica para a VM.  

    O pedido do PowerShell:
    
    ```powershell
    $response = Invoke-WebRequest -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.net' -Method GET -Headers @{Metadata="true"} 
    ```
    
    Em seguida, extraia a resposta completa, que é armazenada como uma cadeia formatada do JavaScript Object Notation (JSON) no objeto $response.  
    
    ```powershell
    $content = $response.Content | ConvertFrom-Json 
    ```
    
    Em seguida, extraia o token de acesso da resposta.  
    
    ```powershell
    $KeyVaultToken = $content.access_token 
    ```
    
    Por fim, utilize o comando Invoke-WebRequest do PowerShell para obter o segredo que criou anteriormente no Key Vault ao transmitir o token de acesso no cabeçalho de Autorização.  Precisará do URL do Key Vault, que está na secção **Informações Básicas** da página **Descrição Geral** do Key Vault.  
    
    ```powershell
    (Invoke-WebRequest -Uri https://<your-key-vault-URL>/secrets/<secret-name>?api-version=2016-10-01 -Method GET -Headers @{Authorization="Bearer $KeyVaultToken"}).content 
    ```
    
    A resposta terá o seguinte aspeto: 
    
    ```powershell
    {"value":"p@ssw0rd!","id":"https://mytestkeyvault.vault.azure.net/secrets/MyTestSecret/7c2204c6093c4d859bc5b9eff8f29050","attributes":{"enabled":true,"created":1505088747,"updated":1505088747,"recoveryLevel":"Purgeable"}} 
    ```
    
Depois de recuperar o segredo do Key Vault, pode utilizá-lo para autenticação num serviço que requeira um nome e uma palavra-passe. 

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, aprendeu a criar uma Identidade de Serviço Gerida para aceder ao Azure Key Vault.  Para saber mais sobre o Azure Key Vault, veja:

> [!div class="nextstepaction"]
>[Cofre de Chaves do Azure](/azure/key-vault/key-vault-whatis)
