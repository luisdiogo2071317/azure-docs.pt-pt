---
title: Autenticação Multifator no Azure Active Directory B2C | Documentos da Microsoft
description: Como ativar o multi-factor Authentication nas aplicações direcionadas para o consumidor protegidas pelo Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/01/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: eabae0f3575719c6cb93affefe0a393dd13d1439
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/05/2018
ms.locfileid: "51014011"
---
# <a name="enable-multi-factor-authentication-in-azure-active-directory-b2c"></a>Ativar a autenticação multifator no Azure Active Directory B2C

O Azure Active Directory (Azure AD) B2C se integra diretamente ao [multi-factor Authentication](../active-directory/authentication/multi-factor-authentication.md) para que pode adicionar uma segunda camada de segurança para experiências de inscrição e início de sessão nas suas aplicações. Ativar a autenticação multifator sem ter de escrever uma única linha de código. Se já criou o início de sessão a cópia de segurança e políticas de início de sessão, pode ainda ativar a autenticação multifator.

Esta funcionalidade ajuda a aplicativos manipular os cenários, como o seguinte:

- Não exigem a autenticação multifator para aceder a uma aplicação, mas necessitar de aceder a outro. Por exemplo, o cliente pode iniciar sessão num aplicativo de seguro de automática com uma conta local ou redes sociais, mas tem de verificar o número de telefone antes de aceder à aplicação de seguros a casa registados no mesmo diretório.
- Não exigem a autenticação multifator para aceder a uma aplicação em geral, mas necessitar de aceder as partes confidenciais dentro da mesma. Por exemplo, o cliente pode iniciar sessão aplicativo bancário com um social ou conta local e verifique a conta de saldo, mas tem de verificar o número de telefone antes de tentar uma transferência durante a transmissão.

## <a name="set-multi-factor-authentication"></a>Conjunto de multi-factor authentication

Quando cria uma política, tem a opção para ativar a autenticação multifator.

![Conjunto de multi-factor authentication](./media/active-directory-b2c-reference-mfa/add-policy.png)

Definir **estado** ao **no**.

Pode usar **executar agora** na política para verificar a experiência. Confirme o seguinte cenário:

É criada uma conta de cliente no seu inquilino antes de ocorre a etapa de autenticação multifator. Durante a etapa, o cliente é-lhe pedido para fornecer um número de telefone e verificá-lo. Se a verificação for bem sucedida, o número de telefone está ligado à conta para utilização posterior. Mesmo que o cliente cancela ou ignora, o cliente pode-lhe pedido para verificar um número de telefone novamente durante o próximo início de sessão com autenticação multifator ativada.

## <a name="add-multi-factor-authentication"></a>Adicionar o multi-factor authentication

É possível ativar a autenticação multifator numa política que criou anteriormente. 

Para ativar a autenticação multifator:

1. Abra a política e, em seguida, selecione **editar**. 
2. Selecione **multi-factor authentication**
3. Definir **estado** ao **no**.
4. Clique em **Guardar** no início da página.


