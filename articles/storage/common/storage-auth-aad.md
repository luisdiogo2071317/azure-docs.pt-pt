---
title: Autenticar o acesso ao Storage do Azure utilizando o Azure Active Directory (pré-visualização) | Microsoft Docs
description: Autentica o acesso ao Storage do Azure utilizando o Azure Active Directory (pré-visualização).
services: storage
author: tamram
manager: jeconnoc
ms.service: storage
ms.topic: article
ms.date: 06/01/2018
ms.author: tamram
ms.openlocfilehash: 9a0782b96b45d27c9b7e603959ecadf5b2632064
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34737651"
---
# <a name="authenticate-access-to-azure-storage-using-azure-active-directory-preview"></a>Autenticar o acesso ao Storage do Azure utilizando o Azure Active Directory (pré-visualização)

Storage do Azure suporta a autenticação e autorização com o Azure Active Directory (AD) para os serviços de fila e Blob. Com o Azure AD, pode utilizar o controlo de acesso baseado em funções (RBAC) para conceder acesso a utilizadores, grupos ou principais de serviço de aplicações. 

Autorizar aplicações que acedem ao armazenamento do Azure com o Azure AD fornece segurança superior e facilidade de utilização por outras opções de autorização. Enquanto pode continuar a utilizar a autorização de chave partilhada com as aplicações, utilizar o Azure AD evita a necessidade de armazenar a chave de acesso da conta com o seu código. Da mesma forma, pode continuar a utilizar assinaturas de acesso partilhado (SAS) para conceder acesso detalhado para recursos na sua conta de armazenamento, mas do Azure AD oferece semelhantes funcionalidades sem a necessidade de gerir SAS tokens ou preocupar com a revogação de um SAS comprometido.

## <a name="about-the-preview"></a>Sobre a pré-visualização

Tenha em consideração os seguintes pontos sobre a pré-visualização:

- Integração do AD do Azure está disponível para os serviços de fila e Blob apenas na pré-visualização.
- Integração do AD do Azure está disponível para contas de armazenamento GPv1, GPv2 e BLOBs em todas as regiões públicas. 
- São suportadas apenas as contas de armazenamento criadas com o modelo de implementação Resource Manager. 
- Suporte para obter informações de identidade do autor da chamada no registo de análise de armazenamento do Azure está disponível em breve.
- Azure AD autorização do acesso a recursos em contas do standard storage é atualmente suportada. Autorização de acesso para blobs de páginas em contas do premium storage será suportada em breve.
- Storage do Azure suporta funções do RBAC incorporadas e personalizadas. Pode atribuir funções no âmbito da subscrição, o grupo de recursos, a conta de armazenamento ou um contentor individuais ou fila.
- As bibliotecas de cliente do Storage do Azure que suportam atualmente a integração do Azure AD incluem:
    - [.NET](https://www.nuget.org/packages/WindowsAzure.Storage/9.2.0)
    - [Java](http://mvnrepository.com/artifact/com.microsoft.azure/azure-storage) (utilize 7.1.x-Preview)
    - Python
        - [Blob](https://github.com/Azure/azure-storage-python/releases/tag/v1.2.0rc1-blob)
        - [fila](https://github.com/Azure/azure-storage-python/releases/tag/v1.2.0rc1-queue)
    - [Node.js](https://www.npmjs.com/package/azure-storage)
    - [JavaScript](https://aka.ms/downloadazurestoragejs))

> [!IMPORTANT]
> Esta pré-visualização destina-se apenas a utilização de não produção. Contratos de nível de serviço de produção (SLAs) não estarão disponíveis até que a integração do Azure AD para o Storage do Azure está declarada geralmente disponível. Se a integração do Azure AD ainda não é suportada para o seu cenário, continue a utilizar a autorização de chave partilhada ou SAS tokens nas suas aplicações.
>
> Durante a pré-visualização, atribuições de funções RBAC podem demorar até cinco minutos para propagar.
>
> Integração do AD do Azure com o Storage do Azure requer que utilize HTTPS para operações de armazenamento do Azure.

## <a name="get-started-with-azure-ad-for-storage"></a>Introdução ao Azure AD para armazenamento

É o primeiro passo através da integração do Azure AD com o Storage do Azure para atribuir funções RBAC para dados de armazenamento para o principal de serviço (um utilizador, grupo ou principal de serviço de aplicação) ou a identidade de serviço geridas (MSI). Funções do RBAC abranger comuns conjuntos de permissões para contentores e pelas filas. Para saber mais sobre as funções do RBAC para o Storage do Azure, consulte [gerir direitos de acesso aos dados de armazenamento com o RBAC (pré-visualização)](storage-auth-aad-rbac.md).

Para utilizar o Azure AD para autorizar o acesso aos recursos de armazenamento nas suas aplicações, terá de pedir um token de acesso de OAuth 2.0 a partir do código. Para saber como pedir um token de acesso e utilizá-la para autorizar pedidos para o Storage do Azure, consulte [autenticar com o Azure AD a partir de uma aplicação do Storage do Azure (pré-visualização)](storage-auth-aad-app.md). Se estiver a utilizar um Azure geridos serviço de identidade (MSI), consulte o artigo [autenticar com o Azure AD a partir de um Azure VM geridos identidade de serviço (pré-visualização)](storage-auth-aad-msi.md).

CLI do Azure e o PowerShell suportam agora iniciar sessão com uma identidade do Azure AD. Depois de iniciar sessão com uma identidade do Azure AD, executa a sua sessão com essa identidade. Para obter mais informações, consulte [utilizar uma identidade do Azure AD para aceder ao armazenamento do Azure com o CLI ou PowerShell (pré-visualização)](storage-auth-aad-script.md).

## <a name="next-steps"></a>Passos Seguintes

Para obter informações adicionais sobre a integração do Azure AD para filas e Blobs do Azure, consulte o blogue da equipa do Storage do Azure post, [anunciar a autenticação de pré-visualização do Azure AD para o Storage do Azure](https://azure.microsoft.com/blog/announcing-the-preview-of-aad-authentication-for-storage/).
