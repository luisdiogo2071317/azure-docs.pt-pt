---
title: Token, sessão e configuração de início de sessão única no Azure Active Directory B2C | Documentos da Microsoft
description: Token, sessão e configuração de início de sessão única no Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 55e17729894738d6b8691ce48a6934ffe46ac4ef
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/24/2019
ms.locfileid: "54845185"
---
# <a name="token-session-and-single-sign-on-configuration-in-azure-active-directory-b2c"></a>Token, sessão e configuração de início de sessão única no Azure Active Directory B2C

Esta funcionalidade dá-lhe um controlo detalhado, por um [base de fluxo de por utilizador](active-directory-b2c-reference-policies.md), de:

- Durações de tokens de segurança emitidos pelo Azure Active Directory (Azure AD) B2C.
- Durações das sessões do aplicativo web geridos pelo Azure AD B2C.
- Formatos de importantes declarações nos tokens de segurança emitidos pelo Azure AD B2C.
- Single sign-on (SSO) do comportamento em várias aplicações e fluxos de utilizador no seu inquilino do Azure AD B2C.

Pode utilizar esta funcionalidade em qualquer tipo de política, mas neste exemplo mostram como utilizar a funcionalidade com um fluxo de utilizador de inscrição ou início de sessão. Para fluxos de utilizador, pode utilizar esta funcionalidade no seu diretório do Azure AD B2C da seguinte forma:

1. Clique em **fluxos de utilizador**.
2. Abra um fluxo de utilizador ao clicar no mesmo. Por exemplo, clique em **B2C_1_SiUpIn**.
3. Clique em **Propriedades**.
4. Sob **definições de compatibilidade do Token**, faça as alterações pretendidas. Saiba mais sobre as propriedades disponíveis nas secções subsequentes.
5. Clique em **guardar** na parte superior do menu.

## <a name="token-lifetimes-configuration"></a>Configuração de durações de token

O Azure AD B2C suporta o [protocolo de autorização de OAuth 2.0](active-directory-b2c-reference-protocols.md) para ativar o acesso seguro a recursos protegidos. Para implementar esse suporte, o Azure AD B2C emite várias [tokens de segurança](active-directory-b2c-reference-tokens.md). 

As seguintes propriedades são utilizadas para gerir as durações de tokens de segurança emitidos pelo Azure AD B2C:

- **Duração (minutos) de token de acesso & ID** – o tempo de vida do token de portador OAuth 2.0 utilizado para obter acesso a um recurso protegido.
    - Predefinição = 60 minutos.
    - Mínimo (inclusive) = 5 minutos.
    - Máximo (inclusive) = 1440 minutos.
- **Duração do token atualizado (dias)** – o período de tempo máximo antes do qual um token de atualização pode ser utilizado para adquirir um novo acesso ou o token de ID (e, opcionalmente, um novo token de atualização, se seu aplicativo tivesse sido concedido a `offline_access` âmbito).
    - Predefinição = 14 dias.
    - Mínimo (inclusive) = 1 dia.
    - Máximo (inclusive) = 90 dias.
- **Duração janela deslizante do token atualizado (dias)** – após este tempo período decorrer o utilizador é forçado a autenticar novamente, independentemente do período de validade mais recente do token atualizado adquirido pela aplicação. Só pode ser fornecido se o parâmetro estiver definido como **limitado**. Tem de ser maior ou igual a **duração do token de atualização (dias)** valor. Se o parâmetro estiver definido como **Unbounded**, não é possível fornecer um valor específico.
    - Predefinição = 90 dias.
    - Mínimo (inclusive) = 1 dia.
    - Máximo (inclusive) = 365 dias.

Os seguintes casos de utilização estão ativados com estas propriedades:

- Permitir que um utilizador manter sessão iniciada para um aplicativo móvel indefinidamente, desde que o utilizador está continuamente ativo no aplicativo. Pode definir **atualização token duração da janela deslizante (dias)** ao **Unbounded** no seu fluxo de início de sessão do utilizador.
- Cumprir os requisitos de conformidade de segurança de seu setor e definindo as durações de token de acesso adequado.

Estas definições não estão disponíveis para fluxos de utilizador de reposição de palavra-passe. 

## <a name="token-compatibility-settings"></a>Definições de tokens de compatibilidade

As seguintes propriedades permitem aos clientes escolher conforme necessário:

- **Afirmação do emissor (iss)** -esta propriedade identifica o inquilino do Azure AD B2C que emitiu o token.
    - `https://<domain>/{B2C tenant GUID}/v2.0/` -Este é o valor predefinido.
    - `https://<domain>/tfp/{B2C tenant GUID}/{Policy ID}/v2.0/` -Este valor inclui o IDs de inquilino do B2C e o fluxo de utilizador utilizado no pedido de token. Se a sua aplicação ou a biblioteca precisa do Azure AD B2C para estar em conformidade com o [especificação do OpenID Connect 1.0 da deteção](https://openid.net/specs/openid-connect-discovery-1_0.html), utilize este valor.
- **Afirmação do requerente** -esta propriedade identifica a entidade para o qual o token declara informações.
    - **ObjectID** -esta propriedade é o valor predefinido. Preenche o ID de objeto do utilizador no diretório para o `sub` de afirmações no token.
    - **Não suportado** - esta propriedade só é fornecida para compatibilidade com versões anteriores, e recomendamos que altere para **ObjectID** assim que possível.
- **Que representa a ID de política de afirmações** -esta propriedade identifica o tipo de afirmação na qual o ID de política utilizado no pedido de token é preenchido.
    - **tfp** -esta propriedade é o valor predefinido.
    - **Acr** -esta propriedade só é fornecida para compatibilidade com versões anteriores.

## <a name="session-behavior"></a>Comportamento da sessão

O Azure AD B2C suporta o [protocolo de autenticação OpenID Connect](active-directory-b2c-reference-oidc.md) para ativar a sessão seguro às aplicações web. Pode utilizar as seguintes propriedades para gerir sessões do aplicativo web:

- **Aplicação Web duração da sessão (minutos)** - a vida útil do cookie de sessão do Azure AD B2C armazenado no navegador do usuário após a autenticação com êxito.
    - Predefinição = 1440 minutos.
    - Mínimo (inclusive) = 15 minutos.
    - Máximo (inclusive) = 1440 minutos.
- **Tempo limite de sessão de aplicação Web** – se este parâmetro estiver definido como **absoluto**, o utilizador é forçado a autenticar novamente após o período de tempo especificado pelo **aplicação Web duração da sessão (minutos)** decorrida. Se este parâmetro estiver definido como **Rolling** (a predefinição), o usuário permanece com sessão iniciada, desde que o utilizador está continuamente ativo na sua aplicação web.

Os seguintes casos de utilização estão ativados com estas propriedades:

- Cumprir os requisitos de segurança e conformidade da sua indústria ao definir a sessão de aplicativo da web apropriados tempos de vida.
- Forçar autenticação após um período de tempo durante a interação do usuário com uma parte de alta segurança da sua aplicação web. 

Estas definições não estão disponíveis para fluxos de utilizador de reposição de palavra-passe.

## <a name="single-sign-on-sso-configuration"></a>Configuração de início de sessão único (SSO)

Se tiver várias aplicações e fluxos de utilizador no seu inquilino do B2C, pode gerir interações de utilizador por elas com o **configuração do início de sessão única** propriedade. Pode definir a propriedade a uma das seguintes definições:

- **Inquilino** -esta é a predefinição. Com esta definição permite que vários aplicativos e de utilizador fluxos no seu inquilino de B2C para partilhar a mesma sessão do utilizador. Por exemplo, uma vez que um utilizador inicia sessão numa aplicação, o utilizador pode iniciar diretamente em outro farmácia Contoso um, após a aceder ao mesmo.
- **Aplicação** -esta definição permite-lhe manter uma sessão de utilizador exclusivamente para um aplicativo, independentemente de outras aplicações. Por exemplo, se pretender que o utilizador iniciar sessão na Contoso farmácia (com as mesmas credenciais), mesmo que o usuário já está conectado a compras de Contoso, outra aplicação no mesmo B2C inquilino. 
- **Política** -esta definição permite-lhe manter uma sessão de utilizador exclusivamente para um fluxo de utilizador, independente das aplicações que utilizam ele. Por exemplo, se o utilizador já iniciou sessão e concluir uma etapa de authentication (MFA) do fator de transmissões, o utilizador pode ser dado acesso a partes de uma maior segurança de vários aplicativos, desde que a sessão associada ao fluxo de utilizador não expire.
- **Desativado** -esta definição força o usuário a executar o fluxo de utilizador completo em cada execução da política. Por exemplo, isso permite que vários utilizadores para se inscrever para a sua aplicação (num cenário de área de trabalho partilhado), permanece até mesmo enquanto um único utilizador com sessão iniciada durante o tempo todo.

Estas definições não estão disponíveis para fluxos de utilizador de reposição de palavra-passe. 

