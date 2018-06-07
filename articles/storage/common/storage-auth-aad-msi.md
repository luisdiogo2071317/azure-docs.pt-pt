---
title: Autenticar com o Azure AD a partir de uma VM do Azure gerida identidade de serviço (pré-visualização) | Microsoft Docs
description: Autenticar com o Azure AD a partir de uma VM do Azure gerida identidade de serviço (pré-visualização).
services: storage
author: tamram
manager: jeconnoc
ms.service: storage
ms.topic: article
ms.date: 05/18/2018
ms.author: tamram
ms.openlocfilehash: 83d3a2d973604e3b8a709b24cabcb3abba1e304c
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34660997"
---
# <a name="authenticate-with-azure-ad-from-an-azure-managed-service-identity-preview"></a>Autenticar com o Azure AD a partir de uma identidade de serviço gerida do Azure (pré-visualização)

Armazenamento do Azure suporta a autenticação do Azure Active Directory (Azure AD) com [identidade de serviço geridas](../../active-directory/managed-service-identity/overview.md). Identidade de serviço geridas (MSI) fornece uma identidade gerida automaticamente no Azure Active Directory (Azure AD). Pode utilizar o MSI para autenticar ao Storage do Azure a partir de aplicações em execução em máquinas virtuais do Azure, aplicações de função, conjuntos de dimensionamento de máquina virtual e outros. Ao utilizar o MSI e tirar partido do poder de autenticação do Azure AD, pode evitar armazenar credenciais com as aplicações que são executados na nuvem.  

Para conceder permissões para uma identidade de serviço geridas para contentores de armazenamento ou filas, atribua uma função do RBAC que abrange permissões de armazenamento para o MSI. Para obter mais informações sobre as funções de RBAC no armazenamento, consulte [gerir direitos de acesso aos dados de armazenamento com o RBAC (pré-visualização)](storage-auth-aad-rbac.md). 

> [!IMPORTANT]
> Esta pré-visualização destina-se apenas a utilização de não produção. Contratos de nível de serviço de produção (SLAs) não estarão disponíveis até que a integração do Azure AD para o Storage do Azure está declarada geralmente disponível. Se a integração do Azure AD ainda não é suportada para o seu cenário, continue a utilizar a autorização de chave partilhada ou SAS tokens nas suas aplicações. Para obter informações adicionais sobre a pré-visualização, consulte [autenticar o acesso ao Storage do Azure utilizando o Azure Active Directory (pré-visualização)](storage-auth-aad.md).
>
> Durante a pré-visualização, atribuições de funções RBAC podem demorar até cinco minutos para propagar.

Este artigo mostra como autenticar para o armazenamento do Azure com o MSI da VM do Azure.  

## <a name="enable-msi-on-the-vm"></a>Ativar o MSI da VM

Antes de poder utilizar MSI para autenticar para o armazenamento do Azure da sua VM, tem de ativar primeiro MSI da VM. Para saber como permitir MSI, consulte um dos seguintes artigos:

- [Portal do Azure](https://docs.microsoft.com/en-us/azure/active-directory/managed-service-identity/qs-configure-portal-windows-vm)
- [Azure PowerShell](../../active-directory/managed-service-identity/qs-configure-powershell-windows-vm.md)
- [CLI do Azure](../../active-directory/managed-service-identity/qs-configure-cli-windows-vm.md)
- [Modelo Azure Resource Manager](../../active-directory/managed-service-identity/qs-configure-template-windows-vm.md)
- [SDKs do Azure](../../active-directory/managed-service-identity/qs-configure-sdk-windows-vm.md)

## <a name="get-an-msi-access-token"></a>Obtenha o token de acesso MSI

Para autenticar com o MSI, a aplicação ou script tem de adquirir um token de acesso MSI. Para saber mais sobre como adquirir um token de acesso, consulte [como utilizar um Azure VM geridos serviço de identidade (MSI) para a aquisição do token](../../active-directory/managed-service-identity/how-to-use-vm-token.md).

## <a name="net-code-example-create-a-block-blob"></a>Exemplo de código do .NET: criar um blob de bloco

O exemplo de código parte do princípio de que tem um token de acesso MSI. O token de acesso é utilizado para autorizar a identidade de serviço geridas para criar um blob de blocos.

### <a name="add-references-and-using-statements"></a>Adicione referências e instruções de utilização  

No Visual Studio, instale a versão de pré-visualização da biblioteca de clientes de armazenamento do Azure. Do **ferramentas** menu, selecione **Gestor de pacotes Nuget**, em seguida, **consola do Gestor de pacotes**. Escreva o seguinte comando para a consola:

```
Install-Package https://www.nuget.org/packages/WindowsAzure.Storage/9.2.0  
```

Adicione as seguintes instruções para o código de utilização:

```dotnet
using Microsoft.WindowsAzure.Storage.Auth;
```

### <a name="create-credentials-from-the-msi-access-token"></a>Criar as credenciais a partir do token de acesso de MSI

Para criar o blob de bloco, utilize o **TokenCredentials** classe fornecida pelo pacote de pré-visualização. Construir uma nova instância do **TokenCredentials**, passando no token de acesso MSI que obteve anteriormente:

```dotnet
// Create storage credentials from your MSI access token.
TokenCredential tokenCredential = new TokenCredential(msiAccessToken);
StorageCredentials storageCredentials = new StorageCredentials(tokenCredential);

// Create a block blob using the credentials.
CloudBlockBlob blob = new CloudBlockBlob(new Uri("https://storagesamples.blob.core.windows.net/sample-container/Blob1.txt"), storageCredentials);
``` 

> [!NOTE]
> Integração do AD do Azure com o Storage do Azure requer que utilize HTTPS para operações de armazenamento do Azure.

## <a name="next-steps"></a>Passos Seguintes

- Para saber mais sobre as funções do RBAC para o armazenamento do Azure, consulte [gerir direitos de acesso aos dados de armazenamento com o RBAC (pré-visualização)](storage-auth-aad-rbac.md).
- Para saber como autorizar o acesso a contentores e filas de dentro das suas aplicações de armazenamento, consulte [utilizar o Azure AD com aplicações de armazenamento](storage-auth-aad-app.md).
- Para saber como iniciar sessão na CLI do Azure e do PowerShell com uma identidade do Azure AD, consulte [utilizar uma identidade do Azure AD para aceder ao armazenamento do Azure com o CLI ou PowerShell (pré-visualização)](storage-auth-aad-script.md).
- Para obter informações adicionais sobre a integração do Azure AD para filas e Blobs do Azure, consulte o blogue da equipa do Storage do Azure post, [anunciar a autenticação de pré-visualização do Azure AD para o Storage do Azure](https://azure.microsoft.com/blog/announcing-the-preview-of-aad-authentication-for-storage/).
