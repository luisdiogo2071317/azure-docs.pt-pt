---
title: Autenticação Multifator no Azure Active Directory B2C | Documentos da Microsoft
description: Como ativar o multi-factor Authentication nas aplicações direcionadas para o consumidor protegidas pelo Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/06/2016
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 3d18e1b2e45aba4e83989e29c533cfc7bf5033fc
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37442713"
---
# <a name="azure-active-directory-b2c-enable-multi-factor-authentication-in-your-consumer-facing-applications"></a>O Azure Active Directory B2C: Ativar a multi-factor Authentication nas suas aplicações voltadas para consumidores
O Azure Active Directory (Azure AD) B2C se integra diretamente ao [multi-factor Authentication](../active-directory/authentication/multi-factor-authentication.md) para que pode adicionar uma segunda camada de segurança para experiências de inscrição e início de sessão nas suas aplicações voltadas para consumidores. E pode fazer isso sem escrever uma única linha de código. Atualmente suportamos a verificação de mensagens de chamadas telefónicas e de texto. Se já criou as políticas de inscrição e início de sessão, ainda pode ativar a multi-factor Authentication.

> [!NOTE]
> Também pode ser ativada a multi-factor Authentication quando criar políticas de inscrição e início de sessão, não apenas ao editar as políticas existentes.
> 
> 

Esta funcionalidade ajuda a aplicativos manipular os cenários, como o seguinte:

* Não necessitam de multi-factor Authentication aceder a uma aplicação, mas necessitar de aceder a outro. Por exemplo, o consumidor pode iniciar sessão num aplicativo de seguro de automática com uma conta local ou redes sociais, mas tem de verificar o número de telefone antes de aceder à aplicação de seguros a casa registados no mesmo diretório.
* Não necessitam de multi-factor Authentication aceder a uma aplicação em geral, mas necessitar de aceder as partes confidenciais dentro da mesma. Por exemplo, o consumidor pode iniciar sessão para um aplicativo bancário com uma conta local ou redes sociais e o saldo da conta de verificação, mas tem de verificar o número de telefone antes de tentar uma transferência durante a transmissão.

## <a name="modify-your-sign-up-policy-to-enable-multi-factor-authentication"></a>Modificar a política de inscrição para ativar a multi-factor Authentication
1. [Siga estes passos para navegar para o painel de funcionalidades do B2C no portal do Azure](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).
2. Clique em **Políticas de inscrição**.
3. Clique em política de inscrição (por exemplo, "B2C_1_SiUp") para abri-lo.
4. Clique em **multi-factor authentication** e ative os **estado** para **ON**. Clique em **OK**.
5. Clique em **guardar** na parte superior do painel.

Pode utilizar a funcionalidade de "Executar agora" na política para verificar a experiência de consumidor. Confirme se:

Uma conta de consumidor é criada no seu diretório antes de ocorre o passo de multi-factor Authentication. Durante a etapa, o consumidor é-lhe pedido para fornecer o respetivo número de telefone e verificá-lo. Se a verificação for bem sucedida, o número de telefone está ligado à conta de consumidor para utilização posterior. Mesmo que o consumidor cancela ou ignora, ele pode ser-lhe pedido para verificar um número de telefone novamente durante o próximo início de sessão (com o multi-factor Authentication ativada).

## <a name="modify-your-sign-in-policy-to-enable-multi-factor-authentication"></a>Modificar a política de início de sessão para ativar a multi-factor Authentication
1. [Siga estes passos para navegar para o painel de funcionalidades do B2C no portal do Azure](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).
2. Clique em **políticas de início de sessão**.
3. Clique em seu início de sessão política (por exemplo, "B2C_1_SiIn") para abri-lo. Clique em **editar** na parte superior do painel.
4. Clique em **multi-factor authentication** e ative os **estado** para **ON**. Clique em **OK**.
5. Clique em **guardar** na parte superior do painel.

Pode utilizar a funcionalidade de "Executar agora" na política para verificar a experiência de consumidor. Confirme se:

Quando o consumidor inicia sessão (com uma conta de redes sociais ou local), se um número de telefone verificado é anexado à conta de consumidor, ele é-lhe pedido para o confirmar. Se nenhum número de telefone é anexado, o consumidor é-lhe pedido para fornecer uma e certifique-se ele. Verificação bem sucedida, o número de telefone está ligado à conta de consumidor para utilização posterior.

## <a name="multi-factor-authentication-on-other-policies"></a>Multi-factor Authentication em outras políticas
Conforme descrito para políticas de inscrição e início de sessão acima, também é possível ativar a autenticação multifator na inscrição ou políticas de reposição de políticas de início de sessão e palavra-passe. Estará disponível em breve no perfil de políticas de edição.

