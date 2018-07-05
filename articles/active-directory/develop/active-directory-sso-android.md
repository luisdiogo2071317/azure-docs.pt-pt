---
title: Como ativar o SSO em vária aplicações em dispositivos Android através da ADAL | Documentos da Microsoft
description: 'Como utilizar as funcionalidades do SDK da ADAL para ativar o início de sessão único em todas as suas aplicações. '
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 40710225-05ab-40a3-9aec-8b4e96b6b5e7
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: android
ms.devlang: java
ms.topic: article
ms.date: 06/13/2018
ms.author: celested
ms.reviewer: dadobali
ms.custom: aaddev
ms.openlocfilehash: 4abf6bd2d82753e22d4fde92e219109274ce36be
ms.sourcegitcommit: e0834ad0bad38f4fb007053a472bde918d69f6cb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37436145"
---
# <a name="how-to-enable-cross-app-sso-on-android-using-adal"></a>Como ativar o SSO em vária aplicações em dispositivos Android através da ADAL
Fornecer único início de sessão (SSO) para que os utilizadores só precisam de introduzir as respetivas credenciais de uma vez e ter essas credenciais funcionar automaticamente em todos os aplicativos agora é um padrão do setor. A dificuldade de introduzir o nome de utilizador e a palavra-passe num ecrã pequeno, muitas vezes vezes combinados com um fator adicional (2FA) como uma chamada telefónica ou um código SMS, os resultados no insatisfação se um utilizador tem de início de sessão em mais de uma vez.

Além disso, se aplicar uma plataforma de identidade que outros aplicativos podem utilizar como Accounts da Microsoft ou uma conta de trabalho do Microsoft365, os clientes esperar que as credenciais para ser disponibilizada para utilização em todas as suas aplicações, independentemente do publicador.

A plataforma do Microsoft Identity, juntamente com os SDKs de identidade da Microsoft, dá-lhe a capacidade de fascinarão seus usuários com o SSO no seu próprio conjunto de aplicativos ou, tal como acontece com a capacidade de Mediador Authenticator aplicativos e, em toda a dispositivo.

Este passo a passo informa como configurar o SDK na sua aplicação para fornecer SSO aos seus clientes.

O documento anterior pressupõe que sabe como integrar a sua aplicação com o [SDK Android do Microsoft Identity](https://github.com/AzureAD/azure-activedirectory-library-for-android).

## <a name="sso-concepts-in-the-microsoft-identity-platform"></a>Conceitos SSO na plataforma de identidades da Microsoft
### <a name="microsoft-identity-brokers"></a>Mediadores de identidades da Microsoft
A Microsoft fornece aplicações para todas as plataformas móveis que permitem o bridging de credenciais em todos os aplicativos de diferentes fornecedores e permite funcionalidades avançadas especiais que exigem um único local seguro para a validação de credenciais. O SDK chama esses **mediadores**. No iOS e Android, os mediadores são fornecidos por meio de aplicativos que podem ser baixados que os clientes instalar de forma independente ou podem ser enviados para o dispositivo por uma empresa que gere alguns ou todos os dispositivos para os seus funcionários. Destes mediadores suportam a gestão da segurança apenas para alguns aplicativos ou todo o dispositivo com base no que os administradores de TI desejam. No Windows, esta funcionalidade é fornecida por um selecionador de conta incorporado no sistema operativo, conhecido em termos técnicos, como o Mediador de autenticação Web.

#### <a name="broker-assisted-logins"></a>Mediador assistidos inícios de sessão
Inícios de sessão assistida por mediador são experiências de início de sessão que ocorrem dentro da aplicação de Mediador e utilizam o armazenamento e a segurança de Mediador para partilhar as credenciais em todas as aplicações no dispositivo que se aplicam a plataforma do Microsoft Identity. A implicação a ser seus aplicativos dependerá o Mediador de sessão aos utilizadores. No iOS e Android, estes mediadores são fornecidos por meio de aplicativos que podem ser baixados que os clientes instalar de forma independente ou podem ser enviados para o dispositivo por uma empresa que gere o dispositivo para o utilizador. Um exemplo deste tipo de aplicativo é a aplicação Microsoft Authenticator em dispositivos iOS. No Windows, esta funcionalidade é fornecida por um selecionador de conta incorporado no sistema operativo, conhecido em termos técnicos, como o Mediador de autenticação Web.
A experiência varia consoante a plataforma e, às vezes, pode ser perturbador para os utilizadores se não for gerida corretamente. Provavelmente está mais familiarizado com esse padrão se tiver instalado o aplicativo para Facebook e utilizar o Facebook ligar a partir de outra aplicação. A plataforma do Microsoft Identity utiliza o mesmo padrão.

No Android, o selecionador de conta é apresentado na parte superior de seu aplicativo, que é menos nociva, para o usuário.

#### <a name="how-the-broker-gets-invoked"></a>Como o mediador é invocado
Se um mediador compatível estiver instalado no dispositivo, como a aplicação Microsoft Authenticator, os SDKs de identidade do Microsoft fará automaticamente o trabalho de invocar o mediador para quando um utilizador a indicar que desejam iniciar sessão com qualquer conta da Microsoft Plataforma de identidade. 
 
 #### <a name="how-microsoft-ensures-the-application-is-valid"></a>Como a Microsoft garante a aplicação é válido
 
 A necessidade de garantir a identidade de uma chamada de aplicativo o mediador é fundamental da segurança já fornecida nos inícios de sessão de Mediador assistido. iOS e Android não impõem identificadores exclusivos que só são válidos para um determinado aplicativo, de modo a que aplicativo mal-intencionados pode "falsificar" identificador de um aplicativo legítimo e receber tokens destinados-se o aplicativo legítimo. Para garantir que a Microsoft sempre está a comunicar com o aplicativo adequado no tempo de execução, o desenvolvedor é-lhe pedido para fornecer um redirectURI personalizado ao registar a sua aplicação com a Microsoft. **Como os desenvolvedores devem, crie o URI de redirecionamento é descrito detalhadamente abaixo.** Este redirectURI personalizada contém o thumbprint do certificado da aplicação e é garantida para que seja exclusivo para o aplicativo por meio de Store de Play Google. Quando um aplicativo chama o mediador, o mediador solicita o sistema operativo Android fornecê-lo com o thumbprint do certificado que o Mediador de chamada. O mediador fornece este thumbprint de certificado para a Microsoft na chamada para o sistema de identidade. Se o thumbprint do certificado da aplicação não coincide com o thumbprint do certificado fornecido para nós pelo desenvolvedor durante o registo, o acesso é negado para os tokens para o recurso que a aplicação está a solicitar. Esta verificação garante que apenas o aplicativo registrado pelo desenvolvedor recebe tokens.

Inícios de sessão de SSO mediada têm as seguintes vantagens:

* Experiências de usuário SSO em todas as suas aplicações, independentemente do fornecedor.
* A aplicação pode utilizar funcionalidades de negócios mais avançadas, como o acesso condicional e oferecer suporte a cenários do Intune.
* Seu aplicativo pode suportar a autenticação baseada em certificados para utilizadores empresariais.
* Experiência de início de sessão mais segura como a identidade da aplicação e o utilizador são verificadas pela aplicação de Mediador com encriptação e algoritmos de segurança adicional.

Esta é uma representação de como os SDKs de identidade da Microsoft trabalham com os aplicativos de Mediador para ativar o SSO:

```
+------------+ +------------+   +-------------+
|            | |            |   |             |
|   App 1    | |   App 2    |   |   Someone   |
|            | |            |   |    Else's   |
|            | |            |   |     App     |
+------------+ +------------+   +-------------+
|  ADAL SDK  | |  ADAL SDK  |   |  ADAL SDK   |
+-----+------+-+-----+------+-  +-------+-----+
      |              |                  |
      |       +------v------+           |
      |       |             |           |
      |       | Microsoft   |           |
      +-------> Broker      |^----------+
              | Application
              |             |
              +-------------+
              |             |
              |   Broker    |
              |   Storage   |
              |             |
              +-------------+

```

Armado com essas informações em segundo plano, que deve ser capaz de melhor compreender e implementar SSO no seu aplicativo usando a plataforma do Microsoft Identity e SDKs.

### <a name="turning-on-sso-for-broker-assisted-sso"></a>Ativar o SSO para broker assistido SSO
A capacidade para uma aplicação para utilizar qualquer Mediador de que está instalado no dispositivo é **desativada por predefinição**. Para utilizar a aplicação com o agente, tem de fazer algumas configurações adicionais e adicionar algum código ao seu aplicativo.

Os passos a seguir são:

1. Ativar o modo de Mediador chamar o código da aplicação para o SDK MS
2. Estabelecer um novo URI de redirecionamento e indique que a aplicação e o registo de aplicação
3. Como configurar as permissões corretas no manifesto do Android

#### <a name="step-1-enable-broker-mode-in-your-application"></a>Passo 1: Ativar o modo de mediador em seu aplicativo
A capacidade para a sua aplicação utilizar o mediador é ativada quando cria a "definições" ou a configuração inicial da sua instância de autenticação. Para fazê-lo na sua aplicação:

```
AuthenticationSettings.Instance.setUseBroker(true);
```

#### <a name="step-2-establish-a-new-redirect-uri-with-your-url-scheme"></a>Passo 2: Estabelecer um novo URI com o esquema de URL de redirecionamento
Para garantir que o aplicativo certo recevies retornado a credencial de tokens, existe uma necessidade de fazer-se de que a chamada para a sua aplicação de uma forma que pode verificar o sistema operativo Android. O sistema operativo Android utiliza o hash do certificado na Google Play store. Este hash do certificado não pode ser falsificada por um aplicativo de adesão. Juntamente com o URI da aplicação de Mediador, a Microsoft garante que os tokens são devolvidos para a aplicação correta. Um redirecionamento exclusivo URI é necessário a registar no aplicativo.

Seu URI de redirecionamento tem de ser no formato adequado:

`msauth://packagename/Base64UrlencodedSignature`

Por exemplo: *msauth://com.example.userapp/IcB5PxIyvbLkbFVtBI%2FitkW%2Fejk%3D*

Pode registrar esse URI de redirecionamento no seu registo de aplicação utilizando o [portal do Azure](https://portal.azure.com/). Para obter mais informações sobre o registo de aplicação do Azure AD, consulte [integração com o Azure Active Directory](active-directory-how-to-integrate.md).

#### <a name="step-3-set-up-the-correct-permissions-in-your-application"></a>Passo 3: Configurar as permissões corretas na sua aplicação
A aplicação de Mediador no Android utiliza a funcionalidade do Gestor de contas do sistema operativo Android para gerir as credenciais em todos os aplicativos. Para usar o mediador no Android o manifesto da aplicação tem de ter permissões para utilizar contas de AccountManager. Estas permissões são discutidas detalhadamente no [aqui Google documentação para o Gestor de conta](http://developer.android.com/reference/android/accounts/AccountManager.html)

Em particular, estas permissões são:

```
GET_ACCOUNTS
USE_CREDENTIALS
MANAGE_ACCOUNTS
```

### <a name="youve-configured-sso"></a>Configurar SSO!
Agora o SDK de identidade da Microsoft será automaticamente tanto partilhar as credenciais em seus aplicativos e invocar o mediador se estiver presente no dispositivo.

