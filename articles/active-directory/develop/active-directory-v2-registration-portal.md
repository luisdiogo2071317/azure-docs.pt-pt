---
title: Tópicos de ajuda de Portal de registo de aplicação | Microsoft Docs
description: Uma descrição das várias funcionalidades no portal de registo de aplicações da Microsoft.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: f0507c28-9464-4d3e-bd53-de9053fd5278
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/18/2018
ms.author: celested
ms.reviewer: lenalepa
ms.custom: aaddev
ms.openlocfilehash: 9d38f6e6d6b9fa47b1cd1497820f7ff887954ad5
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/14/2018
ms.locfileid: "34156192"
---
# <a name="app-registration-reference"></a>Referência de registo de aplicação
Este documento fornece contexto e descrições de diversas funcionalidades encontradas no Portal de registo de aplicações do Microsoft [ https://apps.dev.microsoft.com ](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/).

## <a name="my-applications"></a>As Minhas Aplicações
Esta lista contém todas as aplicações registadas para utilização com o ponto de final de v 2.0 do Azure AD. Estas aplicações tiverem a capacidade dos utilizadores com contas Microsoft pessoais e de trabalho/escola contas do Azure Active Directory a iniciar sessão. Para saber mais sobre o ponto final v 2.0 do Azure AD, consulte o [descrição geral de v 2.0](active-directory-appmodel-v2-overview.md). Estas aplicações também podem ser utilizadas para integrar com o Microsoft conta ponto final de autenticação, `https://login.live.com`.

## <a name="live-sdk-applications"></a>Aplicações do SDK em direto
Esta lista contém todas as aplicações registadas para utilização apenas com a conta Microsoft. Não estão ativadas para utilização com o Azure Active Directory. Este é onde encontrar todas as aplicações que foram anteriormente registadas com o portal de programador MSA em `https://account.live.com/developers/applications`. Todas as funções que efetuou anteriormente em `https://account.live.com/developers/applications` agora pode ser efetuada neste portal novo, `https://apps.dev.microsoft.com`. Se tiver alguma questão mais sobre as aplicações de conta Microsoft, contacte-nos.

## <a name="application-secrets"></a>Segredos da Aplicação
Os segredos de aplicação são credenciais que permitem que a aplicação efetuar fiável [autenticação de cliente](http://tools.ietf.org/html/rfc6749#section-2.3) com o Azure AD. OAuth & OpenID Connect, um segredo de aplicação é frequentemente referido como um `client_secret`. No protocolo v 2.0, qualquer aplicação que recebe um token de segurança numa localização endereçável web (através de um `https` esquema) tem de utilizar um segredo de aplicação para se identificar para o Azure AD após resgate esse de token de segurança. Além disso, qualquer cliente nativo que recebe tokens num dispositivo vai ser proibido de utilizar um segredo de aplicação para efetuar a autenticação de cliente. Isto não aconselha isto no armazenamento de segredos em ambientes inseguras.

Cada aplicação pode conter dois segredos de aplicação válida em qualquer momento. Mantendo dois segredos, tem a capacidade para efetuar o rollover da chave periódico em todo o ambiente da sua aplicação. Depois de ter migrado da totalidade da sua aplicação para um novo segredo, pode eliminar o segredo antigo e aprovisionar uma nova.

Neste momento, apenas dois tipos de segredos de aplicação são permitidos no portal de registo de aplicação. Escolher **gerar a nova palavra-passe** gera e armazena um segredo partilhado no arquivo de dados correspondentes, que pode utilizar na sua aplicação. Escolher **gerar novo par de chaves** cria um novo par de chaves públicas/privadas que pode ser transferido e utilizado para autenticação de cliente para o Azure AD. Escolher **carregar a chave pública** permite-lhe utilizar o seu próprio par de chaves públicas/privadas.
São necessários para carregar um certificado que contém uma chave pública.

## <a name="profile"></a>Perfil
A secção de perfil de portal de registo da aplicação pode ser utilizada para personalizar a página de início de sessão para a sua aplicação. Neste momento, pode alterar o logótipo da aplicação de início de sessão da página, os termos do URL do serviço e o URL da declaração de privacidade. O logótipo tem de ser um ficheiro de imagem GIF, PNG ou JPEG transparente com 48 x 48 ou 50 x 50 pixéis. Limite de tamanho: 15 KB. Tente alterando os valores e visualizar a página de início de sessão resultante!

## <a name="live-sdk-support"></a>Suporte do SDK em direto
Quando ativa "Live SDK suporte", os segredos de aplicações que cria serão aprovisionados para o Azure AD e arquivos de dados de Account Microsoft. Isto permite que a aplicação para integrar diretamente com o serviço Microsoft Account (login.live.com). Se pretender criar uma aplicação utilizando o Microsoft Account diretamente (por oposição a utilizar o ponto final v 2.0 do Azure AD), deve verificar se que o suporte de SDK em direto está ativado.

Desativar o suporte de Live SDK assegura que o segredo de aplicação só é escrito nos dados do Azure AD armazenar. Os dados do Azure AD arquivo incorpora regulamentos de nível empresarial que lhe permite cumprir determinadas normas, como a conformidade de FISMA. Se ativar o suporte de SDK em direto, a aplicação não pode obter a conformidade com algumas dessas normas.

Se planeia utilizar o ponto de final de v 2.0 do Azure AD apenas alguma vez, pode desativar em segurança o suporte da Live SDK.

