---
title: Utilizar um MSI de VM do Windows para aceder ao Azure Resource Manager
description: Um tutorial que explica o processo de uso de um Windows VM Managed Service Identity (MSI) para aceder ao Azure Resource Manager.
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
ms.openlocfilehash: 089835d9c60780fb90f5c19c75c185ee0a908a25
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2018
ms.locfileid: "39007282"
---
# <a name="use-a-windows-vm-managed-service-identity-msi-to-access-resource-manager"></a>Utilizar um Windows VM Managed Service Identity (MSI) para aceder a Resource Manager

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Este tutorial mostra como ativar a identidade de serviço gerida (MSI) para uma máquina virtual de Windows (VM). Em seguida, pode utilizar essa identidade para aceder à API do Azure Resource Manager. Identidades de serviço geridas são geridas automaticamente pelo Azure e ativar a autenticação no serviços que suportam a autenticação do Azure AD sem a necessidade de inserir as credenciais no seu código. Saiba como:

> [!div class="checklist"]
> * Ativar o MSI num VM do Windows 
> * Conceder o acesso à sua VM a um grupo de recursos no Azure Resource Manager 
> * Obter um token de acesso com a identidade da VM e utilizá-la para chamar o Azure Resource Manager

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

[!INCLUDE [msi-tut-prereqs](~/includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure
Inicie sessão no Portal do Azure em [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-windows-virtual-machine-in-a-new-resource-group"></a>Criar uma máquina virtual do Windows num novo grupo de recursos

Para este tutorial, vamos criar uma nova VM do Windows.  Também pode ativar o MSI numa VM existente.

1.  Clique em **criar um recurso** no canto superior esquerdo do portal do Azure.
2.  Selecione **Computação** e, em seguida, selecione **Windows Server 2016 Datacenter**. 
3.  Introduza as informações da máquina virtual. O **nome de utilizador** e **palavra-passe** criado, Eis aqui as credenciais que utiliza para início de sessão para a máquina virtual.
4.  Selecione o elemento adequado **subscrição** para a máquina virtual na lista pendente.
5.  Para selecionar um novo **grupo de recursos** no qual criar a sua máquina virtual, escolha **criar nova**. Quando terminar, clique em **OK**.
6.  Selecione o tamanho da VM. Para ver mais tamanhos, selecione **Visualizar todos** ou altere o filtro **Tipo de disco suportado**. Na página Definições, mantenha as predefinições e clique em **OK**.

    ![Texto alternativo da imagem](../managed-service-identity/media/msi-tutorial-windows-vm-access-arm/msi-windows-vm.png)

## <a name="enable-msi-on-your-vm"></a>Ativar o MSI na sua VM 

Um MSI de VM permite-lhe obter os tokens de acesso do Azure AD sem a necessidade de colocar as credenciais em seu código. Ativar o MSI informa o Azure para criar uma identidade gerida para a sua VM. Nos bastidores, ativar o MSI faz duas coisas: instala a extensão de VM de MSI na sua VM e permite a MSI no Azure Resource Manager.

1.  Selecione o **Máquina Virtual** que pretende ativar o MSI em.  
2.  Na barra de navegação esquerdo, clique em **configuração**. 
3.  Verá **identidade do serviço gerido**. Para registar e ativar o MSI, selecione **Sim**, se desejar para desabilitá-lo, selecione não. 
4.  Certifique-se de que clica **guardar** para guardar a configuração.  
    ![Texto alternativo da imagem](../managed-service-identity/media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

5. Se desejar verificar e certifique-se de que extensões estão nesta VM, clique em **extensões**. Se estiver ativado MSI, em seguida, **ManagedIdentityExtensionforWindows** aparecerá na lista.

    ![Texto alternativo da imagem](../managed-service-identity/media/msi-tutorial-windows-vm-access-arm/msi-windows-extension.png)

## <a name="grant-your-vm-access-to-a-resource-group-in-resource-manager"></a>Conceder o acesso à sua VM a um grupo de recursos no Resource Manager
Com o MSI seu código pode obter tokens de acesso para se autenticarem nos recursos que suportam a autenticação do Azure AD.  O Azure Resource Manager suporta a autenticação do Azure AD.  Em primeiro lugar, precisamos de conceder acesso de identidade desta VM a um recurso no Resource Manager, neste caso, o grupo de recursos no qual a VM está contida.  

1.  Navegue até ao separador para **grupos de recursos**. 
2.  Selecione as específicas **grupo de recursos** que criou para o seu **Windows VM**. 
3.  Aceda a **controlo de acesso (IAM)** no painel esquerdo. 
4.  Em seguida, **Add** uma nova atribuição de função para sua **Windows VM**.  Escolher **função** como **leitor**. 
5.  Na seguinte lista pendente, **atribuir acesso aos** o recursos **Máquina Virtual**. 
6.  Em seguida, certifique-se a subscrição correta está listada na **subscrição** lista pendente. E para **grupo de recursos**, selecione **todos os grupos de recursos**. 
7.  Por fim, no **selecionar** escolha a sua VM do Windows no menu pendente e clique **guardar**.

    ![Texto alternativo da imagem](../managed-service-identity/media/msi-tutorial-windows-vm-access-arm/msi-windows-permissions.png)

## <a name="get-an-access-token-using-the-vm-identity-and-use-it-to-call-azure-resource-manager"></a>Obter um token de acesso com a identidade da VM e utilizá-la para chamar o Azure Resource Manager 

Precisará usar **PowerShell** nessa parte.  Se não tiver instalado, transfira- [aqui](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1). 

1.  No portal, navegue para **máquinas virtuais** e aceda à sua máquina virtual do Windows e na **descrição geral**, clique em **Connect**. 
2.  Introduza no seu **nome de utilizador** e **palavra-passe** para que adicionou ao criar a VM do Windows. 
3.  Agora que já criou um **conexão de área de trabalho remoto** com a máquina virtual, abra **PowerShell** na sessão remota. 
4.  Através Invoke-WebRequest do Powershell, fazer um pedido para o ponto de final de MSI local para obter um token de acesso para o Azure Resource Manager.

    ```powershell
       $response = Invoke-WebRequest -Uri http://localhost:50342/oauth2/token -Method GET -Body @{resource="https://management.azure.com/"} -Headers @{Metadata="true"}
    ```
    
    > [!NOTE]
    > O valor do parâmetro "recurso" tem de ser uma correspondência exata para o que é esperado pelo Azure AD. Ao utilizar o ID de recurso do Azure Resource Manager, tem de incluir a barra no URI.
    
    Em seguida, extrair a resposta completa, que é armazenada como uma cadeia de caracteres de JavaScript Object Notation (JSON) formatado no objeto $response. 
    
    ```powershell
    $content = $response.Content | ConvertFrom-Json
    ```
    Em seguida, extraia o token de acesso da resposta.
    
    ```powershell
    $ArmToken = $content.access_token
    ```
    
    Por fim, chame o Azure Resource Manager com o token de acesso. Neste exemplo, estamos também a utilizar Invoke-WebRequest do PowerShell para fazer a chamada para o Azure Resource Manager e incluem o token de acesso no cabeçalho de autorização.
    
    ```powershell
    (Invoke-WebRequest -Uri https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>?api-version=2016-06-01 -Method GET -ContentType "application/json" -Headers @{ Authorization ="Bearer $ArmToken"}).content
    ```
    > [!NOTE] 
    > O URL diferencia maiúsculas de minúsculas, por isso, certifique-se se estiver a utilizar as maiúsculas ou minúsculas mesmo que tenha utilizado anteriormente quando com o nome do grupo de recursos e em maiúsculas "G" em "resourceGroups."
        
    O comando seguinte devolve os detalhes do grupo de recursos:

    ```powershell
    {"id":"/subscriptions/98f51385-2edc-4b79-bed9-7718de4cb861/resourceGroups/DevTest","name":"DevTest","location":"westus","properties":{"provisioningState":"Succeeded"}}
    ```

## <a name="related-content"></a>Conteúdo relacionado

- Para uma descrição geral do MSI, consulte [descrição geral de identidade do serviço gerido](msi-overview.md).

Utilize a seguinte secção de comentários para fornecer comentários e ajude-na refinar e moldar o nosso conteúdo.

