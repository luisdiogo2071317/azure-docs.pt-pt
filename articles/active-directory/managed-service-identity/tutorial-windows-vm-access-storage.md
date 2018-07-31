---
title: Utilizar uma Identidade de Serviço Gerida de VM do Windows para aceder ao Armazenamento do Azure
description: Um tutorial que explica o processo de utilização de uma Identidade de Serviço Gerida de VM do Windows para aceder ao Armazenamento do Azure.
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
ms.openlocfilehash: ca2a460658b0de4f91816342d2eabb78ceee89fb
ms.sourcegitcommit: 156364c3363f651509a17d1d61cf8480aaf72d1a
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/25/2018
ms.locfileid: "39247378"
---
# <a name="tutorial-use-a-windows-vm-managed-service-identity-to-access-azure-storage-via-access-key"></a>Tutorial: utilizar uma Identidade de Serviço Gerida de VM do Windows para aceder ao Armazenamento do Azure através de uma chave de acesso

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Este tutorial mostra como ativar a Identidade de Serviço Gerida para uma Máquina Virtual do Windows e, em seguida, utilizar essa identidade para obter as chaves de acesso à conta de armazenamento. Pode utilizar as chaves de acesso ao armazenamento como habitualmente ao fazer operações de armazenamento, por exemplo, ao utilizar o SDK de Armazenamento. Neste tutorial, vamos carregar e transferir blobs com o PowerShell do Armazenamento do Azure. Vai aprender a:


> [!div class="checklist"]
> * Ativar a Identidade de Serviço Gerida numa Máquina Virtual do Windows 
> * Conceder aceder à VM a chaves de acesso da conta de armazenamento no Resource Manager 
> * Obter um token de acesso com a identidade da VM e utilizá-lo para obter as chaves de acesso ao armazenamento a partir do Resource Manager 

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão no Portal do Azure em [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-windows-virtual-machine-in-a-new-resource-group"></a>Criar uma máquina virtual do Windows num novo grupo de recursos

Neste tutorial, vamos criar uma nova VM do Windows. Também pode ativar a Identidade de Serviço Gerida numa VM existente.

1.  Clique no botão **+/Criar novo serviço**, no canto superior esquerdo do portal do Azure.
2.  Selecione **Computação** e, em seguida, selecione **Windows Server 2016 Datacenter**. 
3.  Introduza as informações da máquina virtual. O **Nome de Utilizador** e a **Palavra-passe** aqui criados são as credenciais a utilizar para iniciar sessão na máquina virtual.
4.  Escolha uma **Subscrição** para a máquina virtual na lista pendente.
5.  Para selecionar um novo **Grupo de Recursos** no qual gostaria que a máquina virtual fosse criada, selecione **Criar Novo**. Quando terminar, clique em **OK**.
6.  Selecione o tamanho da VM. Para ver mais tamanhos, selecione **Visualizar todos** ou altere o filtro **Tipo de disco suportado**. No painel de definições, mantenha as predefinições e clique em **OK**.

    ![Texto alternativo da imagem](media/msi-tutorial-windows-vm-access-arm/msi-windows-vm.png)

## <a name="enable-managed-service-identity-on-your-vm"></a>Ativar a Identidade de Serviço Gerida na sua VM

Uma Identidade de Serviço Gerida de Máquina Virtual permite-lhe obter os tokens de acesso do Azure AD, sem ter de colocar as credenciais no código. Em segundo plano, a ativação da Identidade de Serviço Gerida faz duas coisas: regista a sua VM no Azure Active Directory para criar a respetiva identidade gerida e configura a identidade na VM.

1. Navegue para o grupo de recursos da sua nova máquina virtual e selecione a máquina virtual que criou no passo anterior.
2. Nas "Definições" da VM à esquerda, clique em **Configuração**.
3. Para registar e ativar a Identidade de Serviço Gerida, selecione **Sim**; se desejar desativá-la, selecione Não.
4. Certifique-se de que clica em **Guardar** para guardar a configuração.

    ![Texto alternativo da imagem](media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

## <a name="create-a-storage-account"></a>Criar uma conta de armazenamento 

Se ainda não tiver uma, irá agora criar uma conta de armazenamento. Também pode ignorar este passo e conceder o acesso da Identidade de Serviço Gerida de VM às chaves de uma conta de armazenamento existente. 

1. Clique no botão **+/Criar novo serviço**, no canto superior esquerdo do portal do Azure.
2. Clique em **Armazenamento**, em seguida, em **Conta de Armazenamento**, e um novo painel "Criar a conta de armazenamento" será apresentado.
3. Introduza um nome para a conta de armazenamento, que irá utilizar mais tarde.  
4. O **Modelo de implementação** e o **Tipo de conta** devem ser definidos como "Gestor de recursos" e "Fins gerais", respetivamente. 
5. Certifique-se de que a **Subscrição** e o **Grupo de Recursos** correspondem aos perfis que especificou quando criou a VM no passo anterior.
6. Clique em **Criar**.

    ![Criar nova conta de armazenamento](../managed-service-identity/media/msi-tutorial-linux-vm-access-storage/msi-storage-create.png)

## <a name="create-a-blob-container-in-the-storage-account"></a>Criar um contentor de blobs na conta de armazenamento

Mais tarde, iremos carregar e transferir um ficheiro para a nova conta de armazenamento. Uma vez que os ficheiros requerem armazenamento de blobs, é necessário criar um contentor de blobs para armazenar o ficheiro.

1. Navegue de volta para a sua conta de armazenamento recentemente criada.
2. Clique na ligação **Contentores** à esquerda, em "Serviço Blob".
3. Clique em **+ Contentor**, na parte superior da página, e surge um painel "Novo contentor".
4. Dê um nome ao contentor, selecione um nível de acesso e clique em **OK**. O nome que especificou será utilizado mais tarde no tutorial. 

    ![Criar contentor de armazenamento](../managed-service-identity/media/msi-tutorial-linux-vm-access-storage/create-blob-container.png)

## <a name="grant-your-vms-managed-service-identity-access-to-use-storage-account-access-keys"></a>Conceder acesso da Identidade de Serviço Gerida da VM para utilizar chaves de acesso da conta de armazenamento 

O Armazenamento do Azure não suporta nativamente a autenticação do Azure AD.  No entanto, pode utilizar uma Identidade de Serviço Gerida para obter as chaves de acesso da conta de armazenamento do Resource Manager e, em seguida, utilizar uma chave para aceder ao armazenamento.  Neste passo, pode conceder o acesso da Identidade de Serviço Gerida de VM às chaves da conta de armazenamento.   

1. Navegue de volta para a sua conta de armazenamento recentemente criada.  
2. Clique na ligação **Controlo de acesso (IAM)** no painel esquerdo.  
3. Clique em **+ Adicionar** na parte superior da página para adicionar uma nova atribuição de função à sua VM
4. Defina a **Função** como “Função do Serviço de Operador de Chave da Conta de Armazenamento”, no lado direito da página. 
5. Na lista pendente seguinte, defina **Atribuir acesso a** ao recurso "Máquina Virtual".  
6. Em seguida, certifique-se de que a subscrição adequada está listada na lista pendente **Subscrição** e, em seguida, defina **Grupo de Recursos** para "Todos os grupos de recursos".  
7. Por fim, em **Selecionar** escolha a sua Máquina Virtual do Windows na lista pendente e clique em **Guardar**. 

    ![Texto alternativo da imagem](../managed-service-identity/media/msi-tutorial-linux-vm-access-storage/msi-storage-role.png)

## <a name="get-an-access-token-using-the-vms-identity-and-use-it-to-call-azure-resource-manager"></a>Obter um token de acesso com a identidade da VM e utilizá-lo para chamar o Azure Resource Manager 

No resto do tutorial, iremos trabalhar a partir da VM que criámos anteriormente. 

Terá de utilizar os cmdlets PowerShell do Azure Resource Manager nesta parte.  Se não o tiver instalado, [transfira a versão mais recente](https://docs.microsoft.com/powershell/azure/overview) antes de continuar.

1. No portal do Azure, navegue para **Máquinas Virtuais**, aceda à sua máquina virtual do Windows e, em seguida, na página **Descrição Geral**, clique em **Ligar** na parte superior. 
2. Introduza o seu **Nome de Utilizador** e a **Palavra-passe** que adicionou quando criou a VM do Windows. 
3. Agora que já criou uma **Ligação ao Ambiente de Trabalho Remoto** com a máquina virtual, abra o PowerShell na sessão remota.
4. Através de Invoke-WebRequest do PowerShell, faça um pedido ao ponto final da Identidade de Serviço Gerida local para obter um token de acesso para o Azure Resource Manager.

    ```powershell
       $response = Invoke-WebRequest -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F' -Method GET -Headers @{Metadata="true"}
    ```
    
    > [!NOTE]
    > No pedido anterior, o valor do parâmetro "recurso" tem de ser uma correspondência exata para o que é esperado pelo Azure AD. Ao utilizar o ID de recurso do Azure Resource Manager, tem de incluir a barra à direita no URI.
    
    Em seguida, extraia o elemento "Content", que é armazenado como uma cadeia formatada do JavaScript Object Notation (JSON) no objeto $response. 
    
    ```powershell
    $content = $response.Content | ConvertFrom-Json
    ```
    Em seguida, extraia o token de acesso da resposta.
    
    ```powershell
    $ArmToken = $content.access_token
    ```
 
## <a name="get-storage-account-access-keys-from-azure-resource-manager-to-make-storage-calls"></a>Obter chaves de acesso da conta de armazenamento do Azure Resource Manager para fazer chamadas de armazenamento  

Agora, utilize o PowerShell para chamar o Resource Manager com o token de acesso que obtivemos na secção anterior para obter a chave de acesso à chave de acesso ao armazenamento. Assim que tivermos a chave de acesso ao armazenamento, podemos chamar operações de carregamento/transferência de armazenamento.

```powershell
$keysResponse = Invoke-WebRequest -Uri https://management.azure.com/subscriptions/<SUBSCRIPTION-ID>/resourceGroups/<RESOURCE-GROUP>/providers/Microsoft.Storage/storageAccounts/<STORAGE-ACCOUNT>/listKeys/?api-version=2016-12-01 -Method POST -Headers @{Authorization="Bearer $ARMToken"}
```
> [!NOTE] 
> O URL é sensível às maiúsculas de minúsculas; por isso, certifique-se de que utiliza as mesmas maiúsculas e minúsculas que utilizou anteriormente, quando atribuiu o nome ao Grupo de Recursos, incluindo a maiúscula “G” em “resourceGroups”. 

```powershell
$keysContent = $keysResponse.Content | ConvertFrom-Json
$key = $keysContent.keys[0].value
```

Em seguida, criamos um ficheiro chamado "test.txt". Em seguida, utilize a chave de acesso ao armazenamento para autenticar com o cmdlet `New-AzureStorageContent`, carregar o ficheiro para o nosso contentor de blobs e, em seguida, transfira o ficheiro.

```bash
echo "This is a test text file." > test.txt
```

Não se esqueça de instalar primeiro os cmdlets de Armazenamento do Azure, com `Install-Module Azure.Storage`. Em seguida, carregue o blob que acabou de criar, com o cmdlet `Set-AzureStorageBlobContent` do PowerShell:

```powershell
$ctx = New-AzureStorageContext -StorageAccountName <STORAGE-ACCOUNT> -StorageAccountKey $key
Set-AzureStorageBlobContent -File test.txt -Container <CONTAINER-NAME> -Blob testblob -Context $ctx
```

Resposta:

```powershell
ICloudBlob        : Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob
BlobType          : BlockBlob
Length            : 56
ContentType       : application/octet-stream
LastModified      : 9/13/2017 6:14:25 PM +00:00
SnapshotTime      :
ContinuationToken :
Context           : Microsoft.WindowsAzure.Commands.Storage.AzureStorageContext
Name              : testblob
```

Também pode transferir o blob que acabou de carregar, com o cmdlet `Get-AzureStorageBlobContent` do PowerShell:

```powershell
Get-AzureStorageBlobContent -Blob testblob -Container <CONTAINER-NAME> -Destination test2.txt -Context $ctx
```

Resposta:

```powershell
ICloudBlob        : Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob
BlobType          : BlockBlob
Length            : 56
ContentType       : application/octet-stream
LastModified      : 9/13/2017 6:14:25 PM +00:00
SnapshotTime      :
ContinuationToken :
Context           : Microsoft.WindowsAzure.Commands.Storage.AzureStorageContext
Name              : testblob
```

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, aprendeu a criar uma Identidade de Serviço Gerida para aceder ao Armazenamento do Azure com uma chave de acesso.  Para saber mais sobre as chaves de acesso ao Armazenamento do Azure, veja:

> [!div class="nextstepaction"]
>[Gerir as chaves de acesso ao armazenamento](/azure/storage/common/storage-create-storage-account#manage-your-storage-access-keys)

