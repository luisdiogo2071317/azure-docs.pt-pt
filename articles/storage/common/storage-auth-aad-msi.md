---
title: Autenticar com o Azure AD a partir de uma VM do Azure Managed Service Identity (pré-visualização) | Documentos da Microsoft
description: Autenticar com o Azure AD a partir de uma VM do Azure Managed Service Identity (pré-visualização).
services: storage
author: tamram
manager: jeconnoc
ms.service: storage
ms.topic: article
ms.date: 05/18/2018
ms.author: tamram
ms.openlocfilehash: 080cb3ee536227e5ddce3fac856de79b2b061dcf
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/12/2018
ms.locfileid: "38970773"
---
# <a name="authenticate-with-azure-ad-from-an-azure-managed-service-identity-preview"></a>Autenticar com o Azure AD a partir de uma identidade de serviço gerido do Azure (pré-visualização)

O armazenamento do Azure suporta a autenticação do Azure Active Directory (Azure AD) com [identidade do serviço gerido](../../active-directory/managed-service-identity/overview.md). Identidade de serviço gerida (MSI) fornece uma identidade gerida automaticamente no Azure Active Directory (Azure AD). Pode utilizar o MSI para autenticar-se ao armazenamento do Azure a partir de aplicações em execução em máquinas virtuais do Azure, aplicações de funções, os conjuntos de dimensionamento de máquinas virtuais e outros. Ao utilizar o MSI e tirar partido do poder de autenticação do Azure AD, é possível evitar o armazenamento de credenciais com as aplicações que são executados na cloud.  

Para conceder permissões para uma identidade de serviço gerido para os contentores de armazenamento ou filas, atribuir uma função RBAC que abrange as permissões de armazenamento para o MSI. Para obter mais informações sobre as funções do RBAC no armazenamento, consulte [gerir direitos de acesso aos dados de armazenamento com o RBAC (pré-visualização)](storage-auth-aad-rbac.md). 

> [!IMPORTANT]
> Esta pré-visualização destina-se apenas a utilização de não produção. Contratos de nível de serviço de produção (SLAs) não estarão disponíveis até que a integração do Azure AD para o armazenamento do Azure é declarada em disponibilidade geral. Se a integração do Azure AD ainda não é suportada para o seu cenário, continue a usar a autorização de chave partilhada ou SAS tokens em seus aplicativos. Para obter mais informações sobre a pré-visualização, veja [autenticar o acesso ao armazenamento do Azure com o Azure Active Directory (pré-visualização)](storage-auth-aad.md).
>
> Durante a pré-visualização, as atribuições de funções do RBAC podem demorar até cinco minutos para propagar.

Este artigo mostra como autenticar para o armazenamento do Azure com o MSI a partir de uma VM do Azure.  

## <a name="enable-msi-on-the-vm"></a>Ativar o MSI na VM

Antes de poder utilizar o MSI para autenticar para o armazenamento do Azure, a partir da sua VM, primeiro tem de ativar MSI na VM. Para saber como ativar o MSI, consulte um dos seguintes artigos:

- [Portal do Azure](https://docs.microsoft.com/azure/active-directory/managed-service-identity/qs-configure-portal-windows-vm)
- [Azure PowerShell](../../active-directory/managed-service-identity/qs-configure-powershell-windows-vm.md)
- [CLI do Azure](../../active-directory/managed-service-identity/qs-configure-cli-windows-vm.md)
- [Modelo Azure Resource Manager](../../active-directory/managed-service-identity/qs-configure-template-windows-vm.md)
- [SDKs do Azure](../../active-directory/managed-service-identity/qs-configure-sdk-windows-vm.md)

## <a name="get-an-msi-access-token"></a>Obtenha o token de acesso MSI

Para autenticar com o MSI, a sua aplicação ou script tem de adquirir um token de acesso MSI. Para saber mais sobre como adquirir um token de acesso, veja [como utilizar um Azure VM Managed Service Identity (MSI) para a aquisição do token](../../active-directory/managed-service-identity/how-to-use-vm-token.md).

## <a name="net-code-example-create-a-block-blob"></a>Exemplo de código do .NET: criar um blob de blocos

O exemplo de código parte do princípio de que tem um token de acesso MSI. O token de acesso é utilizado para autorizar a identidade de serviço gerido para criar um blob de blocos.

### <a name="add-references-and-using-statements"></a>Adicionar referências e instruções "using"  

No Visual Studio, instale a versão de pré-visualização da biblioteca de clientes de armazenamento do Azure. Do **ferramentas** menu, selecione **Gestor de pacotes Nuget**, em seguida, **Package Manager Console**. Escreva o seguinte comando na consola:

```
Install-Package https://www.nuget.org/packages/WindowsAzure.Storage/9.2.0  
```

Adicione o seguinte através de instruções para o seu código:

```dotnet
using Microsoft.WindowsAzure.Storage.Auth;
```

### <a name="create-credentials-from-the-msi-access-token"></a>Criar as credenciais do token de acesso do MSI

Para criar o blob de blocos, utilize o **TokenCredentials** classe fornecida pelo pacote de pré-visualização. Construir uma nova instância do **TokenCredentials**, passando o token de acesso MSI que obteve anteriormente:

```dotnet
// Create storage credentials from your MSI access token.
TokenCredential tokenCredential = new TokenCredential(msiAccessToken);
StorageCredentials storageCredentials = new StorageCredentials(tokenCredential);

// Create a block blob using the credentials.
CloudBlockBlob blob = new CloudBlockBlob(new Uri("https://storagesamples.blob.core.windows.net/sample-container/Blob1.txt"), storageCredentials);
``` 

> [!NOTE]
> A integração do Azure AD com o armazenamento do Azure requer a utilização do HTTPS para operações de armazenamento do Azure.

## <a name="next-steps"></a>Passos Seguintes

- Para saber mais sobre as funções do RBAC para o armazenamento do Azure, veja [gerir direitos de acesso aos dados de armazenamento com o RBAC (pré-visualização)](storage-auth-aad-rbac.md).
- Para saber como autorizar o acesso a contentores e filas de dentro dos seus aplicativos de armazenamento, veja [utilize o Azure AD com aplicações de armazenamento](storage-auth-aad-app.md).
- Para saber como iniciar sessão no CLI do Azure e o PowerShell com uma identidade do Azure AD, veja [utilizar uma identidade do Azure AD para aceder ao armazenamento do Azure com a CLI ou o PowerShell (pré-visualização)](storage-auth-aad-script.md).
- Para obter mais informações sobre a integração do Azure AD para os Blobs do Azure e filas de mensagens em fila, consulte a postagem no armazenamento do Azure team blog [a autenticação de pré-visualização do Azure AD para o armazenamento do Azure a anunciar](https://azure.microsoft.com/blog/announcing-the-preview-of-aad-authentication-for-storage/).
