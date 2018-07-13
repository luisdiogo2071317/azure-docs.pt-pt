---
title: Utilizar um MSI de VM do Windows para aceder ao armazenamento do Azure utilizando uma credencial SAS
description: Um tutorial que mostra-lhe como utilizar um Windows VM Managed Service Identity (MSI) para aceder ao armazenamento do Azure, utilizando uma credencial SAS em vez de uma chave de acesso da conta de armazenamento.
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
ms.openlocfilehash: a96e0948f652e49afb955eaf32f14cf14a10df34
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2018
ms.locfileid: "39007405"
---
# <a name="use-a-windows-vm-managed-service-identity-to-access-azure-storage-via-a-sas-credential"></a>Utilizar uma identidade de serviço gerida do Windows VM para aceder ao armazenamento do Azure através de uma credencial SAS

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Este tutorial mostra como ativar a identidade de serviço gerida (MSI) para uma máquina de Virtual do Windows, em seguida, utilizar o MSI para obter uma credencial de assinatura de acesso partilhado (SAS) de armazenamento. Especificamente, um [SAS de serviço de credenciais](~/articles/storage/common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#types-of-shared-access-signatures). 

Uma SAS de serviço fornece a capacidade de conceder acesso limitado aos objetos na conta de armazenamento, por tempo limitado e um serviço específico (no nosso caso, o serviço de BLOBs), sem expor uma chave de acesso da conta. Pode utilizar uma credencial SAS quando a fazer operações de armazenamento, por exemplo, ao utilizar o SDK de armazenamento como de costume. Para este tutorial, iremos demonstrar carregar e descarregar um blob através do PowerShell do armazenamento do Azure. Ficará a saber como:


> [!div class="checklist"]
> * Ativar o MSI numa máquina Virtual do Windows 
> * Conceder o acesso VM para uma conta de armazenamento SAS no Resource Manager 
> * Obter um token de acesso com a identidade da VM e utilizá-lo a obter a SAS do Resource Manager 

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

    ![Texto alternativo da imagem](../managed-service-identity/media/msi-tutorial-windows-vm-access-arm/msi-windows-vm.png)

## <a name="enable-msi-on-your-vm"></a>Ativar o MSI na sua VM

Um MSI de Máquina Virtual permite-lhe obter os tokens de acesso do Azure AD sem a necessidade de colocar as credenciais em seu código. Nos bastidores, ativar o MSI faz duas coisas: instala a extensão de VM de MSI na sua VM e permite a MSI para a Máquina Virtual.  

1. Navegue para o grupo de recursos de sua nova máquina virtual e selecione a máquina virtual que criou no passo anterior.
2. Na VM "Definições", no painel esquerdo, clique em **configuração**.
3. Para registar e ativar o MSI, selecione **Sim**, se desejar para desabilitá-lo, selecione não.
4. Certifique-se de que clica **guardar** para guardar a configuração.

    ![Texto alternativo da imagem](../managed-service-identity/media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

5. Se pretender verificar quais as extensões são na VM, clique em **extensões**. Se o MSI é ativado, o **ManagedIdentityExtensionforWindows** aparece na lista.

    ![Texto alternativo da imagem](../managed-service-identity/media/msi-tutorial-linux-vm-access-arm/msi-extension-value.png)

## <a name="create-a-storage-account"></a>Criar uma conta de armazenamento 

Se ainda não tiver uma, agora, irá criar uma conta de armazenamento. Também pode ignorar este passo e conceder o acesso MSI de VM para a credencial SAS de uma conta de armazenamento existente. 

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
2. Clique nas **contentores** ligação no painel do lado esquerdo, sob "Serviço de Blob".
3. Clique em **+ contentor** na parte superior da página e um "novo contentor" painel desliza.
4. Dê um nome ao contentor, selecione um nível de acesso, em seguida, clique em **OK**. O nome especificado será utilizado mais tarde no tutorial. 

    ! [Criar armazenamento container]~/articles/active-directory/(media/msi-tutorial-linux-vm-access-storage/create-blob-container.png)

## <a name="grant-your-vms-msi-access-to-use-a-storage-sas"></a>Conceder acesso MSI da VM para utilizar um armazenamento de SAS 

O armazenamento do Azure não suporta nativamente a autenticação do Azure AD.  No entanto, pode utilizar um MSI para obter um SAS de armazenamento do Resource Manager, em seguida, utilizar a SAS para aceder ao armazenamento.  Neste passo, pode conceder o acesso MSI de VM para a sua conta de armazenamento SAS.   

1. Navegue de volta para a sua conta de armazenamento recentemente criada.   
2. Clique nas **controlo de acesso (IAM)** ligação no painel do lado esquerdo.  
3. Clique em **+ adicionar** na parte superior da página para adicionar uma nova atribuição de função para a sua VM
4. Definir **função** para "Armazenamento contribuinte de conta", no lado direito da página.  
5. Na lista pendente seguinte, defina **atribuir acesso a** o recurso "Máquina Virtual".  
6. Em seguida, certifique-se a subscrição correta está listada na **subscrição** menu pendente, em seguida, defina **grupo de recursos** para "Todos os grupos de recursos".  
7. Por fim, em **selecionar** escolha sua máquina Virtual do Windows na lista pendente, em seguida, clique em **guardar**. 

    ![Texto alternativo da imagem](~/articles/active-directory/media/msi-tutorial-linux-vm-access-storage/msi-storage-role-sas.png)

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

## <a name="get-a-sas-credential-from-azure-resource-manager-to-make-storage-calls"></a>Obter uma credencial SAS do Azure Resource Manager para fazer chamadas de armazenamento 

Utilize o PowerShell para chamar o Resource Manager com o token de acesso que foi obtidos na secção anterior, para criar uma credencial SAS do armazenamento. Assim que tivermos a credencial SAS, podemos chamar operações de armazenamento.

Para este pedido, usaremos os parâmetros de pedido HTTP de seguir para criar a credencial SAS:

```JSON
{
    "canonicalizedResource":"/blob/<STORAGE ACCOUNT NAME>/<CONTAINER NAME>",
    "signedResource":"c",              // The kind of resource accessible with the SAS, in this case a container (c).
    "signedPermission":"rcw",          // Permissions for this SAS, in this case (r)ead, (c)reate, and (w)rite. Order is important.
    "signedProtocol":"https",          // Require the SAS be used on https protocol.
    "signedExpiry":"<EXPIRATION TIME>" // UTC expiration time for SAS in ISO 8601 format, for example 2017-09-22T00:06:00Z.
}
```

Estes parâmetros estão incluídos no corpo POST da solicitação para a credencial SAS. Para obter mais informações sobre os parâmetros para a criação de uma credencial SAS, consulte a [referência de REST de SAS de serviço de lista](/rest/api/storagerp/storageaccounts/listservicesas).

Em primeiro lugar, crie uma converter os parâmetros em JSON, em seguida, chamar o armazenamento `listServiceSas` ponto final para criar a SAS de credenciais:

```powershell
$params = @{canonicalizedResource="/blob/<STORAGE-ACCOUNT-NAME>/<CONTAINER-NAME>";signedResource="c";signedPermission="rcw";signedProtocol="https";signedExpiry="2017-09-23T00:00:00Z"}
$jsonParams = $params | ConvertTo-Json
```

```powershell
$sasResponse = Invoke-WebRequest -Uri https://management.azure.com/subscriptions/<SUBSCRIPTION-ID>/resourceGroups/<RESOURCE-GROUP>/providers/Microsoft.Storage/storageAccounts/<STORAGE-ACCOUNT-NAME>/listServiceSas/?api-version=2017-06-01 -Method POST -Body $jsonParams -Headers @{Authorization="Bearer $ArmToken"}
```
> [!NOTE] 
> O URL diferencia maiúsculas de minúsculas, por isso, certifique-se utilizar as maiúsculas exata utilizada anteriormente, quando com o nome do grupo de recursos, incluindo em maiúsculas "G" em "resourceGroups." 

Agora, pode extrair a credencial SAS da resposta:

```powershell
$sasContent = $sasResponse.Content | ConvertFrom-Json
$sasCred = $sasContent.serviceSasToken
```

Se inspecionar o cred SAS, verá algo parecido com isto:

```powershell
PS C:\> $sasCred
sv=2015-04-05&sr=c&spr=https&se=2017-09-23T00%3A00%3A00Z&sp=rcw&sig=JVhIWG48nmxqhTIuN0uiFBppdzhwHdehdYan1W%2F4O0E%3D
```

Em seguida, criamos um arquivo chamado "txt". Em seguida, utilizar a credencial SAS para autenticar com o `New-AzureStorageContent` cmdlet, carregar o ficheiro para o nosso contentor de BLOBs, em seguida, transfira o ficheiro.

```bash
echo "This is a test text file." > test.txt
```

Não se esqueça de instalar os cmdlets de armazenamento do Azure pela primeira vez, usando `Install-Module Azure.Storage`. Em seguida, carregue o blob que acabou de criar, utilizar o `Set-AzureStorageBlobContent` cmdlet do PowerShell:

```powershell
$ctx = New-AzureStorageContext -StorageAccountName <STORAGE-ACCOUNT-NAME> -SasToken $sasCred
Set-AzureStorageBlobContent -File test.txt -Container <CONTAINER-NAME> -Blob testblob -Context $ctx
```

Resposta:

```powershell
ICloudBlob        : Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob
BlobType          : BlockBlob
Length            : 56
ContentType       : application/octet-stream
LastModified      : 9/21/2017 6:14:25 PM +00:00
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
LastModified      : 9/21/2017 6:14:25 PM +00:00
SnapshotTime      :
ContinuationToken :
Context           : Microsoft.WindowsAzure.Commands.Storage.AzureStorageContext
Name              : testblob
```


## <a name="related-content"></a>Conteúdo relacionado

- Para uma descrição geral do MSI, consulte [descrição geral de identidade do serviço gerido](msi-overview.md).
- Para saber como realizar este tutorial mesmo com uma chave de conta de armazenamento, consulte [utilizar uma identidade de serviço gerida do Windows VM para aceder ao armazenamento do Azure](msi-tutorial-windows-vm-access-storage.md)
- Para obter mais informações sobre a funcionalidade SAS de conta de armazenamento do Azure, consulte:
  - [Utilizar assinaturas de acesso partilhado (SAS)](~/articles/storage/common/storage-dotnet-shared-access-signature-part-1.md)
  - [Construindo um serviço SAS](/rest/api/storageservices/Constructing-a-Service-SAS.md)

Utilize a seguinte secção de comentários para fornecer comentários e ajude-na refinar e moldar o nosso conteúdo.


