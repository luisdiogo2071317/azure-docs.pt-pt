---
title: Autenticar o acesso ao armazenamento do Azure com o Azure Active Directory (pré-visualização) | Documentos da Microsoft
description: Autenticar o acesso ao armazenamento do Azure com o Azure Active Directory (pré-visualização).
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 06/01/2018
ms.author: tamram
ms.component: common
ms.openlocfilehash: 90868961475c2e9d0ac7d28c5d9a50c8eb281675
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/06/2018
ms.locfileid: "39525210"
---
# <a name="authenticate-access-to-azure-storage-using-azure-active-directory-preview"></a>Autenticar o acesso ao armazenamento do Azure com o Azure Active Directory (pré-visualização)

O armazenamento do Azure suporta a autenticação e autorização com o Azure Active Directory (AD) para os serviços de BLOBs e filas. Com o Azure AD, pode utilizar o controlo de acesso baseado em funções (RBAC) para conceder acesso a utilizadores, grupos ou principais de serviço de aplicações. 

Autorizar aplicações aceder ao armazenamento do Azure com o Azure AD proporciona maior segurança e facilidade de utilização sobre outras opções de autorização. Enquanto pode continuar a utilizar a autorização de chave partilhada com as suas aplicações, utilizar o Azure AD evita a necessidade de armazenar a chave de acesso da conta com o seu código. Da mesma forma, pode continuar a utilizar assinaturas de acesso partilhado (SAS) para conceder um acesso otimizado aos recursos na sua conta de armazenamento, mas o Azure AD oferece recursos semelhantes sem a necessidade de gerir os tokens de SAS ou se preocupar sobre revogar uma SAS comprometida.

## <a name="about-the-preview"></a>Sobre a pré-visualização

Tenha em atenção os seguintes pontos relativamente a pré-visualização:

- Integração do Azure AD está disponível para os serviços de BLOBs e filas apenas na pré-visualização.
- Integração do Azure AD está disponível para contas de armazenamento de BLOBs, GPv1 e GPv2 em todas as regiões públicas. 
- São suportadas apenas as contas de armazenamento criadas com o modelo de implementação do Resource Manager. 
- Suporte para obter informações de identidade do autor da chamada no registo de análise de armazenamento do Azure estará disponível brevemente.
- O Azure AD autorização de acesso a recursos em contas de armazenamento standard é atualmente suportada. Autorização de acesso para blobs de páginas em contas de armazenamento premium será suportada em breve.
- O armazenamento do Azure suporta funções do RBAC incorporadas e personalizadas. Pode atribuir funções no âmbito da subscrição, o grupo de recursos, a conta de armazenamento, ou um contentor individual ou fila.
- As bibliotecas de cliente de armazenamento do Azure que suportam atualmente a integração do Azure AD incluem:
    - [.NET](https://www.nuget.org/packages/WindowsAzure.Storage/9.2.0)
    - [Java](http://mvnrepository.com/artifact/com.microsoft.azure/azure-storage) (utilizar 7.1.x-Preview)
    - Python
        - [Blob](https://github.com/Azure/azure-storage-python/releases/tag/v1.2.0rc1-blob)
        - [fila](https://github.com/Azure/azure-storage-python/releases/tag/v1.2.0rc1-queue)
    - [Node.js](https://www.npmjs.com/package/azure-storage)
    - [JavaScript](https://aka.ms/downloadazurestoragejs))

> [!IMPORTANT]
> Esta pré-visualização destina-se apenas a utilização de não produção. Contratos de nível de serviço de produção (SLAs) não estarão disponíveis até que a integração do Azure AD para o armazenamento do Azure é declarada em disponibilidade geral. Se a integração do Azure AD ainda não é suportada para o seu cenário, continue a usar a autorização de chave partilhada ou SAS tokens em seus aplicativos.
>
> Durante a pré-visualização, as atribuições de funções do RBAC podem demorar até cinco minutos para propagar.
>
> A integração do Azure AD com o armazenamento do Azure requer a utilização do HTTPS para operações de armazenamento do Azure.

## <a name="get-started-with-azure-ad-for-storage"></a>Introdução ao Azure AD para o armazenamento

É a primeira etapa no uso de integração do Azure AD com o armazenamento do Azure atribuir funções RBAC para dados de armazenamento para o seu principal de serviço (um utilizador, grupo ou principal de serviço de aplicação) ou a identidade de serviço gerida (MSI). Funções RBAC abrangem comuns conjuntos de permissões para contentores e filas. Para saber mais sobre as funções do RBAC do armazenamento do Azure, veja [gerir direitos de acesso aos dados de armazenamento com o RBAC (pré-visualização)](storage-auth-aad-rbac.md).

Para utilizar o Azure AD para autorizar o acesso aos recursos de armazenamento nas suas aplicações, terá de solicitar um token de acesso de OAuth 2.0 a partir do código. Para saber como solicitar um token de acesso e utilizá-lo para autorizar os pedidos ao armazenamento do Azure, veja [autenticar com o Azure AD a partir de uma aplicação de armazenamento do Azure (pré-visualização)](storage-auth-aad-app.md). Se estiver a utilizar um Azure Managed Service Identity (MSI), veja [autenticar com o Azure AD a partir de um Azure VM identidade do serviço gerido (pré-visualização)](storage-auth-aad-msi.md).

CLI do Azure e o PowerShell suportam agora iniciar sessão com uma identidade do Azure AD. Depois de iniciar sessão com uma identidade do Azure AD, a sessão é executado sob essa identidade. Para obter mais informações, consulte [utilizar uma identidade do Azure AD para aceder ao armazenamento do Azure com a CLI ou o PowerShell (pré-visualização)](storage-auth-aad-script.md).

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre a integração do Azure AD para os Blobs do Azure e filas de mensagens em fila, consulte a postagem no armazenamento do Azure team blog [a autenticação de pré-visualização do Azure AD para o armazenamento do Azure a anunciar](https://azure.microsoft.com/blog/announcing-the-preview-of-aad-authentication-for-storage/).
