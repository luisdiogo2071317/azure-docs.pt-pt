---
title: Utilizar uma MSI de VM do Linux para aceder ao Azure Key Vault
description: Um tutorial que explica o processo de utilização de uma Identidade de Serviço Gerida (MSI) de VM do Linux para aceder ao Azure Resource Manager.
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
ms.openlocfilehash: 16b715261329544687fd78ed9c022d7392cc32d9
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/07/2018
ms.locfileid: "37901481"
---
# <a name="tutorial-use-a-linux-vm-managed-service-identity-msi-to-access-azure-key-vault"></a>Tutorial: Utilizar uma Identidade de Serviço Gerida (MSI) de VM do Linux para aceder ao Azure Key Vault 

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Este tutorial mostra como ativar a Identidade de Serviço Gerida (MSI) para uma Máquina Virtual do Linux e, em seguida, utilizar essa identidade para aceder ao Azure Key Vault. A servir de arranque, o Key Vault permite à sua aplicação cliente utilizar o segredo para aceder aos recursos não protegidos pelo Azure Active Directory (AD). As Identidades de Serviço Geridas são geridas automaticamente pelo Azure e permitem-lhe fazer a autenticação em serviços que suportam a autenticação do Azure AD, sem ser necessário inserir as credenciais no seu código. 

Saiba como:

> [!div class="checklist"]
> * Ativar a MSI numa Máquina Virtual do Linux 
> * Conceder o acesso da VM a um segredo armazenado num Key Vault 
> * Obter um token de acesso com a identidade da VM e utilizá-lo para obter o segredo a partir do Key Vault 
 
## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure
Inicie sessão no Portal do Azure em [https://portal.azure.com](https://portal.azure.com). 

## <a name="create-a-linux-virtual-machine-in-a-new-resource-group"></a>Criar uma Máquina Virtual do Linux num novo Grupo de Recursos

Neste tutorial, vamos criar uma nova VM do Linux. Também pode ativar o MSI numa VM existente.

1. Clique no botão **Criar um recurso**, no canto superior esquerdo do portal do Azure.
2. Selecione **Computação** e, em seguida, selecione **Ubuntu Server 16.04 LTS**.
3. Introduza as informações da máquina virtual. Em **Tipo de autenticação**, selecione **Chave SSH pública** ou **Palavra-passe**. As credenciais criadas permitem-lhe iniciar sessão na VM.

    ![Texto alternativo da imagem](../media/msi-tutorial-linux-vm-access-arm/msi-linux-vm.png)

4. Selecione uma **Subscrição** para a máquina virtual na lista pendente.
5. Para selecionar um novo **Grupo de Recursos** no qual gostaria que a máquina virtual fosse criada, escolha **Criar Novo**. Quando terminar, clique em **OK**.
6. Selecione o tamanho da VM. Para ver mais tamanhos, selecione **Visualizar todos** ou altere o filtro Tipo de disco suportado. Na página Definições, mantenha as predefinições e clique em **OK**.

## <a name="enable-msi-on-your-vm"></a>Ativar a MSI na sua VM

Uma MSI de Máquina Virtual permite-lhe obter os tokens de acesso do Azure AD, sem ter de colocar as credenciais no código. Ativar a Identidade de Serviço Gerida numa VM faz duas coisas: regista a sua VM no Azure Active Directory para criar a respetiva identidade gerida e configura a identidade na VM.

1. Selecione a **Máquina Virtual** na qual pretende ativar a MSI.
2. Na barra de navegação esquerda, clique em **Configuração**.
3. Vai ver a **Identidade de Serviço Gerida**. Para registar e ativar a MSI, selecione **Sim**; se desejar desativá-la, selecione Não.
4. Certifique-se de que clica em **Guardar** para guardar a configuração.

    ![Texto alternativo da imagem](../media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

## <a name="grant-your-vm-access-to-a-secret-stored-in-a-key-vault"></a>Conceder o acesso da VM a um segredo armazenado num Key Vault  

Com a MSI, o seu código pode obter tokens de acesso para autenticação em recursos que suportam a autenticação do Azure Active Directory. No entanto, nem todos os serviços do Azure suportam a autenticação do Azure AD. Para utilizar a MSI com esses serviços, armazene as credenciais do serviço no Azure Key Vault e utilize a MSI para aceder ao Key Vault para obter as credenciais. 

Primeiro, é necessário criar um Key Vault e conceder acesso de identidade à VM ao Key Vault.   

1. Na parte superior da barra de navegação esquerda, selecione **Criar um recurso** > **Segurança + Identidade** > **Key Vault**.  
2. Indique um **Nome** para o novo Key Vault. 
3. Localize o Key Vault na mesma subscrição e grupo de recursos da VM que criou anteriormente. 
4. Selecione **Políticas de acesso** e clique em **Adicionar novo**. 
5. Em Configurar a partir do modelo, selecione **Gestão de Segredos**. 
6. Selecione **Selecionar Principal** e, no campo de pesquisa, introduza o nome da VM que criou anteriormente.  Selecione a VM na lista de resultados e clique em **Selecionar**. 
7. Clique em **OK** para concluir a adição da nova política de acesso e em **OK** para concluir a seleção da política de acesso. 
8. Clique em **Criar** para concluir a criação do Key Vault. 

    ![Texto alternativo da imagem](../media/msi-tutorial-windows-vm-access-nonaad/msi-blade.png)

Em seguida, adicione um segredo ao Key Vault para que possa mais tarde obter o segredo com o código em execução na sua VM: 

1. Selecione **Todos os Recursos** e localize e selecione o Key Vault que criou. 
2. Selecione **Segredos** e clique em **Adicionar**. 
3. Selecione **Manual** em **Opções de carregamento**. 
4. Introduza o nome e o valor do segredo.  O valor pode ser o que quiser. 
5. Deixe as datas de ativação e expiração claras e mantenha **Ativado** como **Sim**. 
6. Clique em **Criar** para criar o segredo. 
 
## <a name="get-an-access-token-using-the-vms-identity-and-use-it-to-retrieve-the-secret-from-the-key-vault"></a>Obter um token de acesso com a identidade da VM e utilizá-lo para obter o segredo a partir do Key Vault  

Para concluir estes passos, precisa de um cliente SSH.  Se estiver a utilizar o Windows, pode utilizar o cliente SSH no [Subsistema Windows para Linux](https://msdn.microsoft.com/commandline/wsl/about). Se precisar de ajuda para configurar as chaves do seu cliente SSH, veja [Como utilizar chaves SSH com o Windows no Azure](../../virtual-machines/linux/ssh-from-windows.md) ou [Como criar e utilizar um par de chaves SSH públicas e privadas para VMs do Linux no Azure](../../virtual-machines/linux/mac-create-ssh-keys.md).
 
1. No portal, navegue para a VM do Linux e, em **Descrição Geral**, clique em **Ligar**. 
2. Clique em **Ligar** para ligar à VM com o cliente SSH que escolheu. 
3. Na janela de terminal, com o CURL, faça um pedido ao ponto final de MSI local para obter um token de acesso para o Azure Key Vault.  
 
    O pedido CURL para o token de acesso encontra-se abaixo.  
    
    ```bash
    curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.net' -H Metadata:true  
    ```
    A resposta inclui o token de acesso necessário para aceder ao Resource Manager. 
    
    Resposta:  
    
    ```bash
    {"access_token":"eyJ0eXAi...",
    "refresh_token":"",
    "expires_in":"3599",
    "expires_on":"1504130527",
    "not_before":"1504126627",
    "resource":"https://vault.azure.net",
    "token_type":"Bearer"} 
    ```
    
    Pode utilizar este token de acesso para autenticação no Azure Key Vault.  O pedido CURL seguinte mostra como ler um segredo do Key Vault com o CURL e a API REST do Key Vault.  Precisará do URL do Key Vault, que está na secção **Informações Básicas** da página **Descrição Geral** do Key Vault.  Também precisará do token de acesso que obteve na chamada anterior. 
        
    ```bash
    curl https://<YOUR-KEY-VAULT-URL>/secrets/<secret-name>?api-version=2016-10-01 -H "Authorization: Bearer <ACCESS TOKEN>" 
    ```
    
    A resposta terá o seguinte aspeto: 
    
    ```bash
    {"value":"p@ssw0rd!","id":"https://mytestkeyvault.vault.azure.net/secrets/MyTestSecret/7c2204c6093c4d859bc5b9eff8f29050","attributes":{"enabled":true,"created":1505088747,"updated":1505088747,"recoveryLevel":"Purgeable"}} 
    ```
    
Depois de recuperar o segredo do Key Vault, pode utilizá-lo para autenticação num serviço que requeira um nome e uma palavra-passe.

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, aprendeu a utilizar uma Identidade de Serviço Gerida numa máquina virtual do Linux para aceder ao Azure Key Vault.  Para saber mais sobre o Azure Key Vault, veja:

> [!div class="nextstepaction"]
>[Cofre de Chaves do Azure](/azure/key-vault/key-vault-whatis)




