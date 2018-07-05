---
title: Token, sessão e configuração de início de sessão única no Azure Active Directory B2C | Documentos da Microsoft
description: Token, sessão e configuração de início de sessão única no Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/16/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 35210a8e93b8437ea4d8c3b5f002c81c549d3afe
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37444836"
---
# <a name="azure-active-directory-b2c-token-session-and-single-sign-on-configuration"></a>O Azure Active Directory B2C: Token, sessão e configuração de início de sessão único

Esta funcionalidade dá-lhe um controlo detalhado, por um [base por política](active-directory-b2c-reference-policies.md), de:

1. Durações de tokens de segurança emitidos pelo Azure Active Directory (Azure AD) B2C.
2. Durações das sessões do aplicativo web geridos pelo Azure AD B2C.
3. Formatos de importantes declarações nos tokens de segurança emitidos pelo Azure AD B2C.
4. Início de sessão único (SSO) o comportamento em várias aplicações e políticas no seu inquilino do B2C.

Para as políticas incorporadas, pode utilizar esta funcionalidade no seu diretório do Azure AD B2C da seguinte forma:

1. Siga estes passos para [navegue até o menu de funcionalidades do B2C](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) no portal do Azure.
2. Clique em **políticas de inscrição ou início de sessão**. * Nota: Pode utilizar esta funcionalidade em qualquer tipo de política, não apenas nos * * as políticas de inscrição ou início de sessão ***.
3. Abra uma política ao clicar no mesmo. Por exemplo, clique em **B2C_1_SiUpIn**.
4. Clique em **editar** na parte superior do menu.
5. Clique em **Token, sessão e configuração de início de sessão única**.
6. Faça as alterações necessárias. Saiba mais sobre as propriedades disponíveis nas secções subsequentes.
7. Clique em **OK**.
8. Clique em **guardar** na parte superior do menu.

## <a name="token-lifetimes-configuration"></a>Configuração de durações de token

O Azure AD B2C suporta o [protocolo de autorização de OAuth 2.0](active-directory-b2c-reference-protocols.md) para ativar o acesso seguro a recursos protegidos. Para implementar esse suporte, o Azure AD B2C emite várias [tokens de segurança](active-directory-b2c-reference-tokens.md). Estas são as propriedades que pode usar para gerenciar as durações de tokens de segurança emitidos pelo Azure AD B2C:

* **Duração (minutos) de token de acesso & ID**: O tempo de vida do token de portador OAuth 2.0 utilizados para obter acesso a um recurso protegido.
  * Predefinição = 60 minutos.
  * Mínimo (inclusive) = 5 minutos.
  * Máximo (inclusive) = 1440 minutos.
* **Duração do token atualizado (dias)**: O período de tempo máximo antes do qual um token de atualização pode ser utilizado para adquirir um novo acesso ou o token de ID (e, opcionalmente, um novo token de atualização, se seu aplicativo tivesse sido concedido a `offline_access` âmbito).
  * Predefinição = 14 dias.
  * Mínimo (inclusive) = 1 dia.
  * Máximo (inclusive) = 90 dias.
* **Duração janela deslizante do token atualizado (dias)**: após este período de tempo decorrido o utilizador é forçado a autenticar novamente, independentemente do período de validade mais recente do token atualizado adquirido pela aplicação. Só pode ser fornecido se o parâmetro estiver definido como **limitado**. Tem de ser maior ou igual a **duração do token de atualização (dias)** valor. Se o parâmetro estiver definido como **Unbounded**, não é possível fornecer um valor específico.
  * Predefinição = 90 dias.
  * Mínimo (inclusive) = 1 dia.
  * Máximo (inclusive) = 365 dias.

Estes são alguns casos de utilização que pode habilitar o uso dessas propriedades:

* Permitir que um utilizador manter iniciada num aplicativo móvel indefinidamente, desde que ele esteja continuamente ativa na exceção da aplicação. Pode fazer isso definindo a **atualização token duração da janela deslizante (dias)** mudar para **Unbounded** na sua política de início de sessão.
* Cumprir os requisitos de conformidade de segurança de seu setor e definindo as durações de token de acesso adequado.

    > [!NOTE]
    > Estas definições não estão disponíveis para políticas de reposição de palavra-passe.
    > 
    > 

## <a name="token-compatibility-settings"></a>Definições de compatibilidade de token

Fizemos alterações de formatação para importantes afirmações nos tokens de segurança emitidos pelo Azure AD B2C. Isso foi feito para melhorar o nosso suporte do protocolo padrão e para melhor interoperabilidade com as bibliotecas de identidade de terceiros. No entanto, para evitar a quebra de aplicativos existentes, criamos as seguintes propriedades para permitir que os clientes participar conforme necessário:

* **Afirmação do emissor (iss)**: identifica o inquilino do Azure AD B2C que emitiu o token.
  * `https://login.microsoftonline.com/{B2C tenant GUID}/v2.0/`: Este é o valor predefinido.
  * `https://login.microsoftonline.com/tfp/{B2C tenant GUID}/{Policy ID}/v2.0/`: Este valor inclui o IDs de inquilino do B2C e a política utilizada no pedido de token. Se a sua aplicação ou a biblioteca precisa do Azure AD B2C para estar em conformidade com o [especificação do OpenID Connect 1.0 da deteção](http://openid.net/specs/openid-connect-discovery-1_0.html), utilize este valor.
* **Afirmação do requerente**: identifica a entidade, ou seja, o utilizador, para o qual o token declara informações.
  * **ObjectID**: Este é o valor predefinido. Preenche o ID de objeto do utilizador no diretório para o `sub` de afirmações no token.
  * **Não suportado**: Isto apenas é fornecido para compatibilidade com versões anteriores, e recomendamos que altere para **ObjectID** assim que possível.
* **Que representa a ID de política de afirmações**: identifica o tipo de afirmação na qual o ID de política utilizado no pedido de token é preenchido.
  * **tfp**: Este é o valor predefinido.
  * **Acr**: Isto apenas é fornecido para compatibilidade com versões anteriores, e recomendamos que altere para `tfp` assim que possível.

## <a name="session-behavior"></a>Comportamento da sessão

O Azure AD B2C suporta o [protocolo de autenticação OpenID Connect](active-directory-b2c-reference-oidc.md) para ativar a sessão seguro às aplicações web. Estas são as propriedades que pode utilizar para gerir sessões do aplicativo web:

* **Aplicação Web duração da sessão (minutos)**: A vida útil do cookie de sessão do Azure AD B2C armazenado no navegador do usuário após a autenticação com êxito.
  * Predefinição = 1440 minutos.
  * Mínimo (inclusive) = 15 minutos.
  * Máximo (inclusive) = 1440 minutos.
* **Tempo limite de sessão de aplicação Web**: se este parâmetro estiver definido como **absoluto**, o utilizador é forçado a autenticar novamente após o período de tempo especificado pelo **aplicação Web duração da sessão (minutos)** decorrida. Se este parâmetro estiver definido como **Rolling** (a predefinição), o usuário permanece com sessão iniciada, desde que o utilizador está continuamente ativo na sua aplicação web.

Estes são alguns casos de utilização que pode habilitar o uso dessas propriedades:

* Cumprir os requisitos de segurança e conformidade da sua indústria ao definir a sessão de aplicativo da web apropriados tempos de vida.
* Força a reautenticação após um período de tempo durante a interação do usuário com uma parte de alta segurança da sua aplicação web. 

    > [!NOTE]
    > Estas definições não estão disponíveis para políticas de reposição de palavra-passe.
    > 
    > 

## <a name="single-sign-on-sso-configuration"></a>Configuração de início de sessão único (SSO)
Se tiver várias aplicações e políticas no seu inquilino do B2C, pode gerir interações de utilizador por elas com o **configuração do início de sessão única** propriedade. Pode definir a propriedade a uma das seguintes definições:

* **Inquilino**: Esta é a predefinição. Utilizar esta definição permite que várias aplicações e políticas no seu inquilino de B2C para partilhar a mesma sessão do utilizador. Por exemplo, uma vez que um utilizador inicia sessão numa aplicação, compras de Contoso, ele ou ela pode diretamente inicie sessão no outro farmácia Contoso um, após a aceder ao mesmo.
* **Aplicação**: Isto permite-lhe manter uma sessão de utilizador exclusivamente para um aplicativo, independentemente de outras aplicações. Por exemplo, se pretender que o utilizador iniciar sessão na Contoso farmácia (com as mesmas credenciais), mesmo que ele já esteja assinada para compras de Contoso, outra aplicação no mesmo B2C inquilino. 
* **Política**: Isto permite-lhe manter uma sessão de utilizador exclusivamente para uma política, independente das aplicações que utilizam ele. Por exemplo, se o utilizador já iniciou sessão e concluir uma etapa de authentication (MFA) do fator de transmissões, ele pode ser dado acesso a partes de uma maior segurança de vários aplicativos, desde que a sessão associada à política que não expire.
* **Desativado**: isso força o usuário para percorrer o percurso do utilizador completa em cada execução da política. Por exemplo, isso permite que vários utilizadores para se inscrever para a sua aplicação (num cenário de área de trabalho partilhado), permanece até mesmo enquanto um único utilizador com sessão iniciada durante o tempo todo.

    > [!NOTE]
    > Estas definições não estão disponíveis para políticas de reposição de palavra-passe.
    > 
    > 

