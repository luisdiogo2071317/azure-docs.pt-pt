---
title: Introdução ao MFA do Azure na cloud
description: Introdução ao Microsoft Azure multi-factor Authentication acesso condicional
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: michmcla
ms.openlocfilehash: c2d0f14bca2b9ab062c61407479ab45a0104ff0a
ms.sourcegitcommit: d16b7d22dddef6da8b6cfdf412b1a668ab436c1f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/08/2018
ms.locfileid: "39716268"
---
# <a name="deploy-cloud-based-azure-multi-factor-authentication"></a>Implementar com base na cloud do Azure multi-factor Authentication

Introdução ao multi-factor Authentication (MFA do Azure) é um processo simples.

Antes de começar, certifique-se de que tem os seguintes pré-requisitos:

* Uma conta de administrador global no inquilino do Azure AD. Se precisar de ajuda para concluir este passo, consulte nosso artigo [começar com o Azure AD](../get-started-azure-ad.md)
* Corretas licenças atribuídas a utilizadores. Se precisar de mais informações, consulte o tópico [como obter o Azure multi-factor Authentication](concept-mfa-licensing.md)

## <a name="choose-how-to-enable"></a>Escolha como pretende ativar

**Ativado pela política de acesso condicional** -este método é discutido neste artigo. É a maneira mais flexível para ativar a verificação de dois passos para os seus utilizadores. Ativar através da política de acesso condicional só funciona para o Azure MFA na cloud e é uma funcionalidade premium do Azure AD.

Ativada pelo Azure AD Identity Protection – este método utiliza a política de risco do Azure AD Identity Protection para exigir verificação de dois passos com base apenas em risco de início de sessão para todas as aplicações na cloud. Este método requer licenciamento do Azure Active Directory P2. Obter mais informações sobre este método podem ser encontradas na [do Azure Active Directory Identity Protection](../identity-protection/overview.md#risky-sign-ins).

Ativado por utilizador de alterar estado - este é o método tradicional para exigir a verificação de dois passos. Ele funciona com ambos os Azure MFA na cloud e o servidor MFA do Azure. Ao utilizar este método requer que os usuários realizem a verificação de dois passos **sempre que** que iniciar sessão e substitui as políticas de acesso condicional. Obter mais informações sobre este método podem ser encontradas no [como requerer verificação de dois passos para um utilizador](howto-mfa-userstates.md)

> [!Note]
> Podem encontrar mais informações sobre preços e licenças no [do Azure AD](https://azure.microsoft.com/pricing/details/active-directory/
) e [multi-factor Authentication](https://azure.microsoft.com/pricing/details/multi-factor-authentication/) páginas de preços.

## <a name="choose-authentication-methods"></a>Escolher métodos de autenticação

Ative, pelo menos, um método de autenticação para os seus utilizadores com base nos requisitos da sua organização. Achamos que, quando estiver ativado para os utilizadores da aplicação Microsoft Authenticator fornece a melhor experiência de utilizador. Se precisa entender quais métodos estão disponíveis e como configurá-los consulte o artigo [quais são os métodos de autenticação](concept-authentication-methods.md).

## <a name="get-users-to-enroll"></a>Introdução aos utilizadores a inscrição

Depois de ativar a política de acesso condicional, os utilizadores serão forçados a inscrição da próxima vez que utilizarem uma aplicação protegida com a política. Se ativar uma política que exija a MFA para todos os utilizadores em todas as aplicações de cloud isso poderá causar dores de cabeça para os seus utilizadores e o suporte técnico. Recomenda-se solicitar que os utilizadores registem os métodos de autenticação com antecedência com o portal de registo em [ https://aka.ms/mfasetup ](https://aka.ms/mfasetup). Muitas organizações consideram que criar cartazes, cartões de tabela e mensagens de e-mail ajuda a promover a adoção.

## <a name="enable-multi-factor-authentication-with-conditional-access"></a>Ativar a multi-factor Authentication com o acesso condicional

Inicie sessão para o [portal do Azure](https://portal.azure.com) através de uma conta de administrador global.

### <a name="choose-verification-options"></a>Escolher opções de verificação

Antes de ativar a multi-factor Authentication, sua organização tem de determinar quais opções de verificação permitem. Com o objetivo deste exercício, habilitar a chamada telefónica e mensagem de texto para telefone, pois são opções genéricas que a maioria é capaz de usar. Obter mais informações sobre métodos de autenticação e seu uso, podem ser encontradas no artigo, [quais são os métodos de autenticação?](concept-authentication-methods.md)

1. Navegue até **do Azure Active Directory**, **utilizadores**, **multi-factor Authentication**
   ![aceder a multi-factor Authentication Portal a partir do painel de utilizadores do Azure AD no portal do Azure](media/howto-mfa-getstarted/users-mfa.png) 
2. O novo separador que se abre, navegue até **definições do serviço**
3. Sob **opções de verificação**, verifique as seguintes caixas para os métodos disponíveis para os utilizadores
   * Ligar para telefone
   * Mensagem de texto para telefone

   ![Configurar métodos de verificação no separador de definições do serviço de multi-factor Authentication](media/howto-mfa-getstarted/mfa-servicesettings-verificationoptions.png)

4. Clique em **Guardar**
5. Fechar o **definições do serviço** separador

### <a name="create-conditional-access-policy"></a>Criar política de acesso condicional

1. Inicie sessão para o [portal do Azure](https://portal.azure.com) através de uma conta de administrador global.
1. Procure **Azure Active Directory**, **Acesso condicional**
1. Selecione **Nova política**
1. Dê um nome relevante para a sua política
1. Em **utilizadores e grupos**
   * Sobre o **inclusão** separador, selecione a **todos os utilizadores** botão de opção
   * RECOMENDADO: No **excluir** separador, marque a caixa **utilizadores e grupos** e escolha um grupo para ser utilizado para exclusões quando os utilizadores não têm acesso aos seus métodos de autenticação.
   * Clique em **Concluído**
1. Sob **aplicações na Cloud**, selecione a **todas as aplicações na cloud** botão de opção
   * Como opção: Na **excluir** separador, selecione aplicações na cloud que sua organização não exija a MFA para.
   * Clique em **Concluído**
1. Sob **condições** secção
   * : Se tiver ativado o Azure Identity Protection, pode optar por avaliar o risco de início de sessão como parte da política.
   * OPCIONALMENTE: Se tiver configurado locais confiáveis ou localizações com nome, pode especificar para incluir ou excluir esses locais da política.
1. Sob **Conceder**, certifique-se de que o botão de opção **Conceder acesso** está selecionado
    * Assinale a caixa **Pedir a autenticação multifator**
    * Clique em **Selecionar**
1. Ignore a secção **Sessão**
1. Defina o botão de alternar **Ativar política** para **Ativado**
1. Clique em **Criar**.

![Criar uma política de acesso condicional para ativar a MFA para os utilizadores do portal do Azure no grupo piloto](media/howto-mfa-getstarted/conditionalaccess-newpolicy.png)

### <a name="test-azure-multi-factor-authentication"></a>Testar Multi-Factor Authentication do Azure

Para confirmar que a sua política de acesso condicional funciona, teste a iniciar sessão a um recurso que não deve exigir a MFA e, em seguida, no portal do Azure que exija a MFA.

1. Abra uma nova janela de browser no modo InPrivate ou incógnito e navegue para [https://account.activedirectory.windowsazure.com](https://account.activedirectory.windowsazure.com).
   * Inicie sessão com o utilizador de teste criado como parte da secção de pré-requisitos deste artigo e tenha em atenção que ele não lhe deve pedir para concluir a MFA.
   * Feche a janela do browser
2. Abra uma nova janela de browser no modo InPrivate ou incógnito e navegue para [https://portal.azure.com](https://portal.azure.com).
   * Inicie sessão com o utilizador de teste criado como parte da secção de pré-requisitos deste artigo e tenha em atenção que agora deve ser preciso registar-se e utilizar o Multi-Factor Authentication do Azure.
   * Feche a janela do browser

## <a name="next-steps"></a>Passos Seguintes

Parabéns, tiver configurado o multi-factor Authentication na cloud.

Para configurar definições adicionais, como os IPs fidedignos, mensagens de voz personalizada e alertas de fraude, consulte o artigo [definições de configurar o Azure multi-factor Authentication](howto-mfa-mfasettings.md)

Informações sobre como gerir as definições de utilizador para o Azure multi-factor Authentication podem ser encontrado no artigo [gerir definições de utilizador com o Azure multi-factor Authentication na cloud](howto-mfa-userdevicesettings.md)

[Ativar o registo convergido para a reposição de palavra-passe self-service do Azure multi-factor Authentication e o Azure AD](concept-registration-mfa-sspr-converged.md)
