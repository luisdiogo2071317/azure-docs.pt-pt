---
title: 'Do Azure Active Directory B2C: Reposição de palavra-passe self-service | Microsoft Docs'
description: Um tópico demonstrar como configurar o self-service reposição palavra-passe para os consumidores no Azure Active Directory B2C
services: active-directory-b2c
documentationcenter: ''
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 12/06/2016
ms.author: davidmu
ms.openlocfilehash: f38473989f90bfe6d35bffb17a02a892ad08cf5e
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="azure-active-directory-b2c-set-up-self-service-password-reset-for-your-consumers"></a>O Azure Active Directory B2C: Configurar o self-service reposição palavra-passe para os consumidores
Com a funcionalidade de reposição de palavra-passe self-service, os consumidores (que tenham efetuado a inscrição para contas locais) podem repor as palavras-passe por si próprios. Esta opção reduz significativamente a carga sobre a sua equipa de suporte, especialmente se a sua aplicação tiver milhões de consumidores utilizá-lo regularmente. Atualmente, é apenas suportado utilizando um endereço de correio eletrónico verificado como um método de recuperação. Iremos adicionar métodos de recuperação adicionais (número de telefone verificado, perguntas de segurança, etc.) no futuro.

> [!NOTE]
> Este artigo aplica-se a palavra-passe self-service reposição utilizada no contexto de uma política de início de sessão. Se precisar de políticas de reposição de palavra-passe totalmente personalizável invocadas a partir da sua aplicação, consulte [neste artigo](active-directory-b2c-reference-policies.md#create-a-password-reset-policy).
> 
> 

Por predefinição, o diretório não tem palavra-passe self-service reposição ativada. Utilize os seguintes passos para ativá-la:

1. Iniciar sessão para o [portal do Azure](https://portal.azure.com/) como administrador da subscrição. Este é o mesmo ou conta profissional ou a mesma conta Microsoft que utilizou para criar o seu diretório.
2. Abrir o Active Directory (na barra de navegação no lado esquerdo).
3. Selecione **propriedades**.
4. Desloque para baixo até o **ativada de reposição de palavra-passe self-service** secção e ative-o para **todos os**. 
5. Clique em **guardar** na parte superior da página. Terminar!

Para testar, utilize a funcionalidade "Executar agora" em qualquer política de início de sessão que tenha contas locais como um fornecedor de identidade. No início de sessão-in de conta local página (onde introduziu um endereço de e-mail e a palavra-passe, ou um nome de utilizador e a palavra-passe), clique em **não é possível aceder à sua conta?** para verificar a experiência de consumidor.

> [!NOTE]
> As páginas de reposição de palavra-passe self-service podem ser personalizadas utilizando a [corporativa funcionalidade](../active-directory/customize-branding.md).
> 
> 

