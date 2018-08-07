---
title: Execute comandos CLI do Azure ou o PowerShell sob uma identidade do Azure AD para aceder ao armazenamento do Azure (pré-visualização) | Documentos da Microsoft
description: CLI do Azure e o PowerShell suportam a iniciar sessão com uma identidade do Azure AD para executar comandos nos contentores de armazenamento do Azure e filas e os seus dados. Um token de acesso é fornecido para a sessão e utilizado para autorizar as operações de chamada. Permissões dependem a função atribuída para a identidade do Azure AD.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 05/30/2018
ms.author: tamram
ms.component: common
ms.openlocfilehash: 253edccef064e729e96bceac619458cf4c585ae4
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/06/2018
ms.locfileid: "39522490"
---
# <a name="use-an-azure-ad-identity-to-access-azure-storage-with-cli-or-powershell-preview"></a>Utilizar uma identidade do Azure AD para aceder ao armazenamento do Azure com a CLI ou o PowerShell (pré-visualização)

O armazenamento do Azure fornece extensões de pré-visualização para a CLI do Azure e do PowerShell que permitem-lhe iniciar sessão e executar comandos de scripts sob uma identidade do Azure Active Directory (Azure AD). A identidade do Azure AD pode ser um utilizador, grupo ou principal de serviço de aplicações, ou pode ser um [identidade do serviço gerido](../../active-directory/managed-service-identity/overview.md). Pode atribuir permissões para aceder aos recursos de armazenamento para a identidade do Azure AD através do controlo de acesso baseado em funções (RBAC). Para obter mais informações sobre as funções do RBAC no armazenamento do Azure, consulte [gerir direitos de acesso aos dados de armazenamento do Azure com o RBAC (pré-visualização)](storage-auth-aad-rbac.md).

Quando iniciar sessão CLI do Azure ou do PowerShell com uma identidade do Azure AD, é devolvido um token de acesso para aceder ao armazenamento do Azure sob essa identidade. Esse token é automaticamente usado pela CLI ou o PowerShell para autorizar as operações de armazenamento do Azure. Para operações suportadas, já não tem de transmitir uma chave de conta ou o token SAS com o comando.

> [!IMPORTANT]
> Esta pré-visualização destina-se apenas a utilização de não produção. Contratos de nível de serviço de produção (SLAs) não estarão disponíveis até que a integração do Azure AD para o armazenamento do Azure é declarada em disponibilidade geral. Se a integração do Azure AD ainda não é suportada para o seu cenário, continue a usar a autorização de chave partilhada ou SAS tokens em seus aplicativos. Para obter mais informações sobre a pré-visualização, veja [autenticar o acesso ao armazenamento do Azure com o Azure Active Directory (pré-visualização)](storage-auth-aad.md).
>
> Durante a pré-visualização, as atribuições de funções do RBAC podem demorar até cinco minutos para propagar.
>
> A integração do Azure AD com o armazenamento do Azure requer a utilização do HTTPS para operações de armazenamento do Azure.

## <a name="supported-operations"></a>Operações suportadas

As extensões de pré-visualização são suportadas para operações de contentores e filas. As operações que podem ser chamados depende das permissões concedidas à identidade do Azure AD com a qual inicia sessão na CLI do Azure ou o PowerShell. Permissões para os contentores de armazenamento do Azure ou filas são atribuídas através do controlo de acesso baseado em funções (RBAC). Por exemplo, se uma função de leitor de dados é atribuída à identidade, em seguida, pode executar comandos de scripts de leitura de dados de um contentor ou uma fila. Se uma função de contribuinte de dados é atribuída à identidade, em seguida, pode executar comandos de scripts que ler, escreverem ou eliminar um contentor ou fila ou os dados que contêm. 

Para obter detalhes sobre as permissões necessárias para cada operação de armazenamento do Azure num contentor ou fila, consulte [permissões para chamar as operações REST](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-rest-operations).  

## <a name="call-cli-commands-with-an-azure-ad-identity"></a>Chamar comandos da CLI com uma identidade do Azure AD

Para instalar a extensão de pré-visualização do CLI do Azure:

1. Certifique-se de que instalou a versão da CLI do Azure 2.0.32 ou posterior. Executar `az --version` para verificar a versão instalada.
2. Execute o seguinte comando para instalar a extensão de pré-visualização: 

    ```azurecli
    az extension add -n storage-preview
    ```

A extensão de pré-visualização adiciona uma nova `--auth-mode` parâmetro para comandos com suporte:

- Definir o `--auth-mode` parâmetro `login` para iniciar sessão com uma identidade do Azure AD.
- Definir o `--auth-mode` parâmetro para o legado `key` valor para tentar consultar para um caso de chaves de conta sem parâmetros de autenticação da conta são fornecidos. 

Por exemplo, para transferir um blob na CLI do Azure com uma identidade do Azure AD, primeiro execute `az login`, em seguida, chame o comando com `--auth-mode` definido como `login`:

```azurecli
az login
az storage blob download --account-name storagesamples --container sample-container --name myblob.txt --file myfile.txt --auth-mode login 
```

A variável de ambiente associada com o `--auth-mode` parâmetro é `AZURE_STORAGE_AUTH_MODE`.

## <a name="call-powershell-commands-with-an-azure-ad-identity"></a>Chamar comandos do PowerShell com uma identidade do Azure AD

Para utilizar o Azure PowerShell para iniciar sessão com uma identidade do Azure AD:

1. Certifique-se de que tem a versão mais recente do PowerShellGet instalado. Execute o seguinte comando para instalar a versão mais recente:
 
    ```powershell
    Install-Module PowerShellGet –Repository PSGallery –Force
    ```

2. Desinstale quaisquer instalações anteriores do Azure PowerShell.
3. Instale o AzureRM:

    ```powershell
    Install-Module AzureRM –Repository PSGallery –AllowClobber
    ```

4. Instale o módulo de pré-visualização:

    ```powershell
    Install-Module -Name Azure.Storage -AllowPrerelease –AllowClobber 
    ```

5. Chamar o [New-AzureStorageContext](https://docs.microsoft.com/powershell/module/azure.storage/new-azurestoragecontext) cmdlet para criar um contexto e incluir o `-UseConnectedAccount` parâmetro. 
6. Para chamar um cmdlet com uma identidade do Azure AD, passe o contexto para o cmdlet.

O exemplo seguinte mostra como listar os blobs num contentor do Azure PowerShell com uma identidade do Azure AD: 

```powershell
$ctx = New-AzureStorageContext -StorageAccountName $storageAccountName -UseConnectedAccount 
Get-AzureStorageBlob -Container $sample-container -Context $ctx 
```

## <a name="next-steps"></a>Passos Seguintes

- Para saber mais sobre as funções do RBAC para o armazenamento do Azure, veja [gerir direitos de acesso aos dados de armazenamento com o RBAC (pré-visualização)](storage-auth-aad-rbac.md).
- Para saber mais sobre como utilizar a identidade do serviço gerido com o armazenamento do Azure, veja [autenticar com o Azure AD a partir de uma identidade do serviço gerido do Azure (pré-visualização)](storage-auth-aad-msi.md).
- Para saber como autorizar o acesso a contentores e filas de dentro dos seus aplicativos de armazenamento, veja [utilize o Azure AD com aplicações de armazenamento](storage-auth-aad-app.md).
- Para obter mais informações sobre a integração do Azure AD para os Blobs do Azure e filas de mensagens em fila, consulte a postagem no armazenamento do Azure team blog [a autenticação de pré-visualização do Azure AD para o armazenamento do Azure a anunciar](https://azure.microsoft.com/blog/announcing-the-preview-of-aad-authentication-for-storage/).
