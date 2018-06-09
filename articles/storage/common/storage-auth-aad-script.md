---
title: Execute comandos do Azure CLI ou PowerShell com uma identidade do Azure AD para aceder ao armazenamento do Azure (pré-visualização) | Microsoft Docs
description: CLI do Azure e o PowerShell suportam iniciar sessão com uma identidade do Azure AD para executar comandos nos contentores de armazenamento do Azure e as filas e os respetivos dados. Um token de acesso é fornecido para a sessão e utilizado para autorizar chamadas operações. Dependem de permissões na função atribuída à identidade do Azure AD.
services: storage
author: tamram
manager: jeconnoc
ms.service: storage
ms.topic: article
ms.date: 05/30/2018
ms.author: tamram
ms.openlocfilehash: 98af46707485d1ab49e7d8c6fb1729e6edc6b2ff
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/08/2018
ms.locfileid: "35235870"
---
# <a name="use-an-azure-ad-identity-to-access-azure-storage-with-cli-or-powershell-preview"></a>Utilizar uma identidade do Azure AD para aceder ao armazenamento do Azure com o CLI ou PowerShell (pré-visualização)

Armazenamento do Azure fornece extensões de pré-visualização para a CLI do Azure e do PowerShell que permitem-lhe iniciar sessão e executar comandos de scripts com uma identidade do Azure Active Directory (Azure AD). A identidade do Azure AD pode ser um utilizador, grupo ou principal de serviço de aplicações, ou pode ser um [identidade do serviço gerido](../../active-directory/managed-service-identity/overview.md). Pode atribuir permissões para aceder a recursos de armazenamento para a identidade do Azure AD através do controlo de acesso baseado em funções (RBAC). Para obter mais informações sobre as funções de RBAC no armazenamento do Azure, consulte [gerir direitos de acesso aos dados de armazenamento do Azure com o RBAC (pré-visualização)](storage-auth-aad-rbac.md).

Quando iniciar sessão CLI do Azure ou o PowerShell com uma identidade do Azure AD, é devolvido um token de acesso para aceder ao armazenamento do Azure com essa identidade. Esse token é, em seguida, automaticamente utilizado pelo CLI ou o PowerShell para autorizar operações de armazenamento do Azure. Para operações suportadas, já não precisam de transmitir uma chave de conta ou um token SAS com o comando.

> [!IMPORTANT]
> Esta pré-visualização destina-se apenas a utilização de não produção. Contratos de nível de serviço de produção (SLAs) não estarão disponíveis até que a integração do Azure AD para o Storage do Azure está declarada geralmente disponível. Se a integração do Azure AD ainda não é suportada para o seu cenário, continue a utilizar a autorização de chave partilhada ou SAS tokens nas suas aplicações. Para obter informações adicionais sobre a pré-visualização, consulte [autenticar o acesso ao Storage do Azure utilizando o Azure Active Directory (pré-visualização)](storage-auth-aad.md).
>
> Durante a pré-visualização, atribuições de funções RBAC podem demorar até cinco minutos para propagar.
>
> Integração do AD do Azure com o Storage do Azure requer que utilize HTTPS para operações de armazenamento do Azure.

## <a name="supported-operations"></a>Operações suportadas

As extensões de pré-visualização são suportadas para operações de contentores e pelas filas. As operações podem chamar depende as permissões concedidas à identidade do Azure AD com a qual iniciar sessão Azure CLI ou PowerShell. Permissões para contentores de armazenamento do Azure ou filas são atribuídas através de controlo de acesso baseado em funções (RBAC). Por exemplo, se uma função de leitor de dados é atribuída para a identidade, em seguida, pode executar comandos de scripts que leiam os dados a partir de um contentor ou fila. Se uma função de contribuinte de dados é atribuída para a identidade, em seguida, pode executar comandos de scripts que ler, escrever ou eliminar um contentor ou fila ou os dados que contêm. 

Para obter detalhes sobre as permissões necessárias para cada operação de armazenamento do Azure num contentor ou fila, consulte [permissões para chamar as operações REST](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-rest-operations).  

## <a name="call-cli-commands-with-an-azure-ad-identity"></a>Chamar comandos da CLI com uma identidade do Azure AD

Para instalar a extensão de pré-visualização para a CLI do Azure:

1. Certifique-se de que instalou a versão da CLI do Azure 2.0.32 ou posterior. Executar `az --version` para verificar a versão instalada.
2. Execute o seguinte comando para instalar a extensão de pré-visualização: 

    ```azurecli
    az extension add -n storage-preview
    ```

A extensão de pré-visualização adiciona uma nova `--auth-mode` parâmetro aos comandos suportados:

- Definir o `--auth-mode` parâmetro `login` para iniciar sessão com uma identidade do Azure AD.
- Definir o `--auth-mode` parâmetro para o legado `key` valor a tentativa de consulta para um caso de chave de conta, não existem parâmetros de autenticação para a conta são fornecidos. 

Por exemplo, para transferir um blob na CLI do Azure com uma identidade do Azure AD, primeiro execute `az login`, em seguida, chame o comando com `--auth-mode` definido como `login`:

```azurecli
az login
az storage blob download --account-name storagesamples --container sample-container --name myblob.txt --file myfile.txt --auth-mode login 
```

A variável de ambiente associada a `--auth-mode` parâmetro é `AZURE_STORAGE_AUTH_MODE`.

## <a name="call-powershell-commands-with-an-azure-ad-identity"></a>Chamar comandos do PowerShell com uma identidade do Azure AD

Para utilizar o Azure PowerShell para iniciar sessão com uma identidade do Azure AD:

1. Certifique-se de que tem a versão mais recente do PowerShellGet instalado. Execute o seguinte comando para instalar a versão mais recente:
 
    ```powershell
    Install-Module PowerShellGet –Repository PSGallery –Force
    ```

2. Desinstale quaisquer instalações anteriores do Azure PowerShell.
3. Instale AzureRM:

    ```powershell
    Install-Module AzureRM –Repository PSGallery –AllowClobber
    ```

4. Instale o módulo de pré-visualização:

    ```powershell
    Install-Module -Name Azure.Storage -AllowPrerelease –AllowClobber 
    ```

5. Chamar o [New-AzureStorageContext](https://docs.microsoft.com/powershell/module/azure.storage/new-azurestoragecontext) cmdlet para criar um contexto e incluir o `-UseConnectedAccount` parâmetro. 
6. Para chamar um cmdlet com uma identidade do Azure AD, passe o contexto ao cmdlet.

O exemplo seguinte mostra como listar os blobs num contentor do Azure PowerShell com uma identidade do Azure AD: 

```powershell
$ctx = New-AzureStorageContext -StorageAccountName $storageAccountName -UseConnectedAccount 
Get-AzureStorageBlob -Container $sample-container -Context $ctx 
```

## <a name="next-steps"></a>Passos Seguintes

- Para saber mais sobre as funções do RBAC para o armazenamento do Azure, consulte [gerir direitos de acesso aos dados de armazenamento com o RBAC (pré-visualização)](storage-auth-aad-rbac.md).
- Para saber mais sobre como utilizar identidade de serviço geridas com o Storage do Azure, consulte [autenticar com o Azure AD a partir de uma identidade de serviço gerida do Azure (pré-visualização)](storage-auth-aad-msi.md).
- Para saber como autorizar o acesso a contentores e filas de dentro das suas aplicações de armazenamento, consulte [utilizar o Azure AD com aplicações de armazenamento](storage-auth-aad-app.md).
- Para obter informações adicionais sobre a integração do Azure AD para filas e Blobs do Azure, consulte o blogue da equipa do Storage do Azure post, [anunciar a autenticação de pré-visualização do Azure AD para o Storage do Azure](https://azure.microsoft.com/blog/announcing-the-preview-of-aad-authentication-for-storage/).
