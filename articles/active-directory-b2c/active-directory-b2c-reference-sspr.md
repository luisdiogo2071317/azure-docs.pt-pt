---
title: Reposição de palavra-passe self-service | Microsoft Docs
description: Demonstra como configurar o self-service reposição palavra-passe para os seus clientes no Azure Active Directory B2C
services: active-directory-b2c
documentationcenter: ''
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 04/17/2018
ms.author: davidmu
ms.openlocfilehash: 5b75455ad604b594a5f85fea8299d35a7d02c848
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/18/2018
---
# <a name="set-up-self-service-password-reset-for-your-customers"></a>Configurar o self-service reposição palavra-passe para os seus clientes
Com a funcionalidade de reposição de palavra-passe self-service, os seus clientes que tenham sessão iniciada cópias de segurança para contas locais podem repor as palavras-passe por si próprios. Esta opção reduz significativamente a carga sobre a sua equipa de suporte, especialmente se a sua aplicação tiver milhões de clientes que utilizam-lo regularmente. Atualmente, utilizando um endereço de correio eletrónico verificado é o método de recuperação suportados apenas.

> [!NOTE]
> Este artigo aplica-se a palavra-passe self-service reposição utilizada no contexto de uma política de início de sessão. Se precisar de políticas de reposição de palavra-passe totalmente personalizável invocadas a partir da sua aplicação, consulte [neste artigo](active-directory-b2c-reference-policies.md#create-a-password-reset-policy).
> 
> 

Por predefinição, o seu diretório não tem palavra-passe self-service reposição ativada. Utilize os seguintes passos para ativá-la:

1. Iniciar sessão para o [portal do Azure](https://portal.azure.com/) como administrador da subscrição. Este é o mesmo ou conta profissional ou a mesma conta Microsoft que utilizou para criar o seu diretório.
2. Abra **do Azure Active Directory** (na barra de navegação no lado esquerdo).
4. Definir **Self-repor a palavra-passe serviço ativada** para **todos os**. 
5. Clique em **guardar** na parte superior da página. Terminar!

Para testar, utilize a funcionalidade "Executar agora" em qualquer política de início de sessão que tenha contas locais como um fornecedor de identidade. No início de sessão-in de conta local página (onde introduziu um endereço de e-mail e a palavra-passe, ou um nome de utilizador e a palavra-passe), clique em **não é possível aceder à sua conta?** para verificar a experiência do cliente.

> [!NOTE]
> As páginas de reposição de palavra-passe self-service podem ser personalizadas utilizando a [corporativa funcionalidade](../active-directory/customize-branding.md).
> 
> 

