---
title: Self-service reposição palavra-passe no Azure Active Directory B2C | Documentos da Microsoft
description: Demonstra como configurar o self-service reposição palavra-passe para os seus clientes no Azure Active Directory B2C
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 3612e10df12e2b18f32caae55bdd83b12a4e24a6
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37450075"
---
# <a name="set-up-self-service-password-reset-for-your-customers"></a>Configurar o self-service reposição palavra-passe para os seus clientes
Com a funcionalidade de reposição de palavra-passe self-service, os clientes que se inscreveram para contas locais podem repor as palavras-passe por conta própria. Esta opção reduz significativamente a carga sobre a equipe de suporte, especialmente se o seu aplicativo tiver milhões de clientes usá-lo regularmente. Atualmente, a utilizar um endereço de correio eletrónico verificado é o método de recuperação suportados apenas.

> [!NOTE]
> Este artigo aplica-se a palavra-passe self-service reposição utilizada no contexto de uma política de início de sessão. Se precisar de políticas de reposição de palavra-passe totalmente personalizável invocadas a partir da sua aplicação, veja [este artigo](active-directory-b2c-reference-policies.md#create-a-password-reset-policy).
> 
> 

Por predefinição, o diretório não ter a palavra-passe self-service reposição ativada. Utilize os seguintes passos para ativá-la:

1. Inicie sessão para o [portal do Azure](https://portal.azure.com/) como o administrador da subscrição. Este é o mesmo ou conta profissional ou a mesma conta Microsoft que utilizou para criar o seu diretório.
2. Open **do Azure Active Directory** (na barra de navegação no lado esquerdo).
4. Definir **auto-reposição de palavra-passe ativada** ao **todos os**. 
5. Clique em **guardar** na parte superior da página. Já está!

Para testar, utilize a funcionalidade de "Executar agora" em qualquer política de início de sessão com contas locais como fornecedor de identidade. Sobre a início de sessão na conta local página (onde introduzir um endereço de e-mail e palavra-passe, ou um nome de utilizador e palavra-passe), clique em **não é possível aceder à sua conta?** para verificar a experiência do cliente.

> [!NOTE]
> As páginas de reposição de palavra-passe self-service podem ser personalizadas utilizando a [funcionalidade de imagem corporativa](../active-directory/fundamentals/customize-branding.md).
> 
> 

