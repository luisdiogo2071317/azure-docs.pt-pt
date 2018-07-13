---
title: Utilizar um MSI de VM do Windows para aceder ao armazenamento do Azure
description: Um tutorial que explica o processo de uso de um Windows VM Managed Service Identity (MSI) para aceder ao armazenamento do Azure.
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
ms.openlocfilehash: 6e1364db9ecba65b90be525141f03fb9b4a33d28
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38610841"
---
# <a name="use-a-windows-vm-managed-service-identity-to-access-azure-storage-via-access-key"></a>Utilizar uma identidade de serviço gerida do Windows VM para aceder ao armazenamento do Azure por meio da chave de acesso

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Este tutorial mostra como ativar a identidade de serviço gerida (MSI) para uma máquina de Virtual do Windows, em seguida, utilizar essa identidade para obter chaves de acesso da conta de armazenamento. Pode utilizar chaves de acesso de armazenamento como de costume, fazendo quando operações de armazenamento, por exemplo, ao utilizar o SDK de armazenamento. Para este tutorial, vamos carregar e transferir blobs através do PowerShell de armazenamento do Azure. Ficará a saber como:


> [!div class="checklist"]
> * Ativar o MSI numa máquina Virtual do Windows 
> * Conceder o acesso VM para chaves de acesso de conta de armazenamento no Gestor de recursos 
> * Obter um token de acesso com a identidade da VM e utilizá-lo para obter as chaves de acesso de armazenamento do Resource Manager 

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

[!INCLUDE [msi-tut-prereqs](~/includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão no Portal do Azure em [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-windows-virtual-machine-in-a-new-resource-group"></a>Criar uma máquina virtual do Windows num novo grupo de recursos

Para este tutorial, vamos criar uma nova VM do Windows. Também pode ativar o MSI numa VM existente.

1.  Clique nas **c++ /CLI criar novo serviço** botão encontrado no canto superior esquerdo do portal do Azure.
2.  Selecione **Computação** e, em seguida, selecione **Windows Server 2016 Datacenter**. 
3.  Introduza as informações da máquina virtual. O **nome de utilizador** e **palavra-passe** criado, Eis aqui as credenciais que utiliza para início de sessão para a máquina virtual.
4.  Selecione o elemento adequado **subscrição** para a máquina virtual na lista pendente.
5.  Para selecionar um novo **grupo de recursos** gostaria de máquina virtual para ser criado no, escolha **criar nova**. Quando terminar, clique em **OK**.
6.  Selecione o tamanho da VM. Para ver mais tamanhos, selecione **Visualizar todos** ou altere o filtro **Tipo de disco suportado**. No painel de definições, mantenha as predefinições e clique em **OK**.

    ![Texto alternativo da imagem](~/articles/active-directory/media/msi-tutorial-windows-vm-access-arm/msi-windows-vm.png)

## <a name="enable-msi-on-your-vm"></a>Ativar o MSI na sua VM

Um MSI de Máquina Virtual permite-lhe obter os tokens de acesso do Azure AD sem a necessidade de colocar as credenciais em seu código. Nos bastidores, ativar o MSI faz duas coisas: instala a extensão de VM de MSI na sua VM e permite a MSI para a Máquina Virtual.  

1. Navegue para o grupo de recursos de sua nova máquina virtual e selecione a máquina virtual que criou no passo anterior.
2. Na VM "Definições" à esquerda, clique em **configuração**.
3. Para registar e ativar o MSI, selecione **Sim**, se desejar para desabilitá-lo, selecione não.
4. Certifique-se de que clica **guardar** para guardar a configuração.

    ![Texto alternativo da imagem](~/articles/active-directory/media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

5. Se pretender verificar quais as extensões são na VM, clique em **extensões**. Se o MSI é ativado, o **ManagedIdentityExtensionforWindows** aparece na lista.

    ![Texto alternativo da imagem](~/articles/active-directory/media/msi-tutorial-linux-vm-access-arm/msi-extension-value.png)

## <a name="create-a-storage-account"></a>Criar uma conta de armazenamento 

Se ainda não tiver uma, agora, irá criar uma conta de armazenamento. Também pode ignorar este passo e conceder o acesso MSI de VM para as chaves de uma conta de armazenamento existente. 

1. Clique nas **c++ /CLI criar novo serviço** botão encontrado no canto superior esquerdo do portal do Azure.
2. Clique em **armazenamento**, em seguida, **conta de armazenamento**, e um novo painel "Criar a conta de armazenamento" será exibida.
3. Introduza um nome para a conta de armazenamento, que irá utilizar mais tarde.  
4. **Modelo de implementação** e **tipo de conta** deve ser definido como "Resource manager" e "Fins gerais", respectivamente. 
5. Certifique-se a **subscrição** e **grupo de recursos** corresponder aos perfis que especificou quando criou a VM no passo anterior.
6. Clique em **Criar**.

    ![Criar nova conta de armazenamento](~/articles/active-directory/media/msi-tutorial-linux-vm-access-storage/msi-storage-create.png)

## <a name="create-a-blob-container-in-the-storage-account"></a>Criar um contentor de BLOBs na conta de armazenamento

Iremos mais tarde carregar e transferir um ficheiro para a nova conta de armazenamento. Uma vez que ficheiros requerem armazenamento de BLOBs, é necessário criar um contentor de BLOBs para armazenar o ficheiro.

1. Navegue de volta para a sua conta de armazenamento recentemente criada.
2. Clique nas **contentores** ligação no lado esquerdo, sob "Serviço de Blob".
3. Clique em **+ contentor** na parte superior da página e um "novo contentor" painel desliza.
4. Dê um nome ao contentor, selecione um nível de acesso, em seguida, clique em **OK**. O nome especificado será utilizado mais tarde no tutorial. 

    ![Criar contentor de armazenamento](~/articles/active-directory/media/msi-tutorial-linux-vm-access-storage/create-blob-container.png)

## <a name="grant-your-vms-msi-access-to-use-storage-account-access-keys"></a>Conceder acesso MSI da VM para utilizar chaves de acesso da conta de armazenamento 

O armazenamento do Azure não suporta nativamente a autenticação do Azure AD.  No entanto, pode utilizar um MSI para obter chaves de acesso da conta de armazenamento do Resource Manager, em seguida, utilizar uma chave para aceder ao armazenamento.  Neste passo, pode conceder o acesso MSI de VM para as chaves para a sua conta de armazenamento.   

1. Navegue de volta para a sua conta de armazenamento recentemente criada.  
2. Clique nas **controlo de acesso (IAM)** ligação no painel do lado esquerdo.  
3. Clique em **+ adicionar** na parte superior da página para adicionar uma nova atribuição de função para a sua VM
4. Definir **função** à "Armazenamento de conta de chave de serviço de função de operador", no lado direito da página. 
5. Na lista pendente seguinte, defina **atribuir acesso a** o recurso "Máquina Virtual".  
6. Em seguida, certifique-se a subscrição correta está listada na **subscrição** menu pendente, em seguida, defina **grupo de recursos** para "Todos os grupos de recursos".  
7. Por fim, em **selecionar** escolha sua máquina Virtual do Windows na lista pendente, em seguida, clique em **guardar**. 

    ![Texto alternativo da imagem](~/articles/active-directory/media/msi-tutorial-linux-vm-access-storage/msi-storage-role.png)

## <a name="get-an-access-token-using-the-vms-identity-and-use-it-to-call-azure-resource-manager"></a>Obter um token de acesso com a identidade da VM e utilizá-la para chamar o Azure Resource Manager 

Para o resto do tutorial, iremos trabalhar da VM que criámos anteriormente. 

Terá de utilizar cmdlets do Azure Resource Manager PowerShell nessa parte.  Se não o tiver instalado, [transferir a versão mais recente](https://docs.microsoft.com/powershell/azure/overview) antes de continuar.

1. No portal do Azure, navegue para **máquinas virtuais**, aceda à sua máquina virtual Windows, em seguida, a partir do **descrição geral** página clique **Connect** na parte superior. 
2. Introduza no seu **nome de utilizador** e **palavra-passe** para que adicionou ao criar a VM do Windows. 
3. Agora que já criou um **conexão de área de trabalho remoto** com a máquina virtual, abra o PowerShell na sessão remota.
4. Através Invoke-WebRequest do Powershell, fazer um pedido para o ponto de final de MSI local para obter um token de acesso para o Azure Resource Manager.

    ```powershell
       $response = Invoke-WebRequest -Uri http://localhost:50342/oauth2/token -Method GET -Body @{resource="https://management.azure.com/"} -Headers @{Metadata="true"}
    ```
    
    > [!NOTE]
    > O valor do parâmetro "recurso" tem de ser uma correspondência exata para o que é esperado pelo Azure AD. Ao utilizar o ID de recurso do Azure Resource Manager, tem de incluir a barra no URI.
    
    Em seguida, extraia o elemento "Content", que é armazenado como uma cadeia de caracteres de JavaScript Object Notation (JSON) formatado no objeto $response. 
    
    ```powershell
    $content = $response.Content | ConvertFrom-Json
    ```
    Em seguida, extraia o token de acesso da resposta.
    
    ```powershell
    $ArmToken = $content.access_token
    ```
 
## <a name="get-storage-account-access-keys-from-azure-resource-manager-to-make-storage-calls"></a>Obter chaves de acesso da conta de armazenamento do Azure Resource Manager para fazer chamadas de armazenamento  

Agora utilizar o PowerShell para chamar o Resource Manager com o token de acesso foi obtidos na secção anterior, para obter a chave de acesso de armazenamento. Assim que tivermos a chave de acesso de armazenamento, podemos chamar operações de carregamento/transferência de armazenamento.

```powershell
$keysResponse = Invoke-WebRequest -Uri https://management.azure.com/subscriptions/<SUBSCRIPTION-ID>/resourceGroups/<RESOURCE-GROUP>/providers/Microsoft.Storage/storageAccounts/<STORAGE-ACCOUNT>/listKeys/?api-version=2016-12-01 -Method POST -Headers @{Authorization="Bearer $ARMToken"}
```
> [!NOTE] 
> O URL diferencia maiúsculas de minúsculas, por isso, certifique-se utilizar as maiúsculas exata utilizada anteriormente, quando com o nome do grupo de recursos, incluindo em maiúsculas "G" em "resourceGroups." 

```powershell
$keysContent = $keysResponse.Content | ConvertFrom-Json
$key = $keysContent.keys[0].value
```

Em seguida, criamos um arquivo chamado "txt". Em seguida, utilize a chave de acesso de armazenamento para se autenticar com o `New-AzureStorageContent` cmdlet, carregar o ficheiro para o nosso contentor de BLOBs, em seguida, transfira o ficheiro.

```bash
echo "This is a test text file." > test.txt
```

Não se esqueça de instalar os cmdlets de armazenamento do Azure pela primeira vez, usando `Install-Module Azure.Storage`. Em seguida, carregue o blob que acabou de criar, utilizar o `Set-AzureStorageBlobContent` cmdlet do PowerShell:

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

Também pode transferir o blob que acabou de carregar, usando o `Get-AzureStorageBlobContent` cmdlet do PowerShell:

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


## <a name="related-content"></a>Conteúdo relacionado

- Para uma descrição geral do MSI, consulte [descrição geral de identidade do serviço gerido](msi-overview.md).
- Para saber como fazer esse mesmo tutorial, utilizando um credencial SAS de armazenamento, veja [utilizar uma identidade de serviço gerida do Windows VM para aceder ao armazenamento do Azure através de uma credencial SAS](msi-tutorial-windows-vm-access-storage-sas.md)
- Para obter mais informações sobre a funcionalidade SAS de conta de armazenamento do Azure, consulte:
  - [Utilizar assinaturas de acesso partilhado (SAS)](~/articles/storage/common/storage-dotnet-shared-access-signature-part-1.md)
  - [Construindo um serviço SAS](/rest/api/storageservices/Constructing-a-Service-SAS.md)

Utilize a seguinte secção de comentários para fornecer comentários e ajude-na refinar e moldar o nosso conteúdo


