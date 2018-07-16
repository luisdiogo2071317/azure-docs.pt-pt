---
title: Resolver problemas de verificação de dois passos - Azure AD | Documentos da Microsoft
description: Este documento fornecerá aos utilizadores informações sobre o que fazer se eles se deparam um problema com o Azure multi-factor Authentication.
services: multi-factor-authentication
keywords: cliente de autenticação multifator, problema de autenticação, ID de correlação
documentationcenter: ''
author: eross-msft
manager: mtillman
ms.assetid: 8f3aef42-7f66-4656-a7cd-d25a971cb9eb
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/06/2017
ms.author: lizross
ms.reviewer: richagi
ms.custom: end-user
ms.openlocfilehash: deb75c2601fa55f7cdb1681d8f73e94d6b01310a
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39060076"
---
# <a name="get-help-with-two-step-verification"></a>Obter ajuda para a verificação de dois passos
Este artigo responde às perguntas mais comuns que as pessoas perguntam sobre a verificação de dois passos.

## <a name="why-do-i-have-to-perform-two-step-verification-can-i-turn-it-off"></a>Por que motivo tenho de executar a verificação de dois passos? Eu desativá-la?

Verificação de dois passos é uma funcionalidade de segurança que sua organização optar por utilizar para proteger as suas contas. É mais seguro do que apenas uma palavra-passe, porque depende de duas formas de autenticação: algo que conhece e algo que tenha com. É algo que conhece a palavra-passe. A algo que tem com é um telefone ou dispositivo que normalmente tem consigo. Quando a sua conta está protegida com verificação de dois passos, o que significa que um hacker malicioso não é possível iniciar sessão como utilizador se ela obtiver a palavra-passe alguma forma, porque eles não têm acesso para o seu telefone.

A Microsoft oferece verificação de dois passos, mas a sua organização opta por utilizar a funcionalidade. Não é possível optar por se o suporte da empresa exigir de, tal como não é possível optar por utilizar uma palavra-passe para proteger a sua conta.

Se tiver ativada para a sua conta Microsoft pessoal de verificação de dois passos e pretende alterar as definições, leia [sobre a verificação de dois passos](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification) em vez disso.

## <a name="i-dont-have-my-phone-with-me-today"></a>Não tenho meu telefone comigo hoje mesmo

Alguns dias, deixe o seu telemóvel em casa, mas ainda tem de iniciar sessão no trabalho. A primeira coisa que deve tentar está a iniciar sessão com um método de verificação diferente. Quando se registou para verificação de dois passos, configurar mais do que um número de telefone? Para tentar iniciar sessão com um método diferente, siga estes passos:

1. Inicie sessão como faria normalmente.
2. Quando abre a página de verificação de dois passos, escolha **utilizar outra opção de verificação**.

   ![Verificação diferente](./media/multi-factor-authentication-end-user-troubleshoot/diff_option.png)

3. Selecione a opção de verificação que pretende utilizar.
4. Continue com a verificação de dois passos.

Se não vir a **utilizar outra opção de verificação** ligar, em seguida, o que significa que não definir métodos alternativos quando registada pela primeira vez para a verificação de dois passos. Contacte o suporte da empresa para obter ajuda para iniciar sessão para a sua conta. Depois de iniciar sessão, certifique-se de que [gira as suas definições](multi-factor-authentication-end-user-manage-settings.md) adicionar métodos de verificação adicional para a próxima vez.

Se vir a **utilizar outra opção de verificação** ligação, mas não tem acesso para seus métodos alternativos, contacte o suporte da empresa para obter ajuda para iniciar sessão para a sua conta.

## <a name="i-lost-my-phone-or-got-a-new-number"></a>Eu perder meu telefone ou tem um novo número
Existem duas formas de aceder novamente à sua conta. A primeira é iniciar sessão com o seu número de telefone de autenticação alternativo, se tiver configurado um. A segunda é fazer o suporte da empresa para limpar as definições.

Se o seu telemóvel tiver sido perdido ou roubado, recomendamos também que informar a sua empresa memorizadas de suporte para que possam repor as palavras-passe de aplicação e limpe quaisquer dispositivos.

### <a name="use-an-alternate-phone-number"></a>Utilize um número de telefone alternativo
Se tiver definido várias opções de verificação, incluindo um número de telefone secundário ou uma aplicação de autenticação num dispositivo diferente, pode utilizar um para iniciar sessão.

Para iniciar sessão com o número de telefone alternativo, siga estes passos:

1. Inicie sessão como faria normalmente.
2. Quando lhe for pedido para verificar a sua conta, escolha **utilizar outra opção de verificação**.

   ![Verificação diferente](./media/multi-factor-authentication-end-user-troubleshoot/diff_option.png)

3. Selecione o número de telefone ou o dispositivo tem acesso.
4. Depois que estiver na sua conta [gira as suas definições](multi-factor-authentication-end-user-manage-settings.md) para alterar o número de telefone de autenticação.

### <a name="clear-your-settings"></a>Limpar as definições
Se não tiver configurado um número de telefone de autenticação secundária, terá de contactar o suporte da empresa para obter ajuda. Tê-los para limpar as definições para que o da próxima vez que iniciar sessão, ser-lhe-á pedido para [registar para a verificação de](multi-factor-authentication-end-user-first-time.md) novamente.

## <a name="i-am-not-receiving-a-text-or-call-on-my-phone"></a>Eu não estou a receber uma mensagem de texto ou chamada em meu telefone
Há vários motivos por que pode tentar iniciar sessão, mas não receber o texto ou chamada telefónica. Se tiver recebido com êxito textos ou chamadas telefónicas para o seu telefone no passado, em seguida, isto é provavelmente um problema com o fornecedor de telefone, não a sua conta. Certifique-se de que tem o sinal de célula bom e, se estiver a tentar receber uma mensagem de texto certifique-se de que é capaz de receber mensagens de texto. Peça um amigo para chamar lhe ou enviar-lhe como um teste.

Se pouquinho alguns minutos até que um texto ou chamada, a forma mais rápida para a sua conta é tentar uma opção diferente.

1. Selecione **utilizar outra opção de verificação** na página que está a aguardar a verificação.

    ![Verificação diferente](./media/multi-factor-authentication-end-user-troubleshoot/diff_option.png)
2. Selecione o método de número ou entrega de telefone que pretende utilizar.

    Se tiver recebido vários códigos de verificação, utilize o mais recente.

Se não tiver outro método configurado, contacte o suporte da empresa e peça-lhe para limpar as definições. Da próxima vez que iniciar sessão, será solicitado a [configurar a autenticação multifator](multi-factor-authentication-end-user-first-time.md) novamente.

Se tiver, muitas vezes, os atrasos devido a sinal de célula ruim, recomendamos que utilize o [aplicação Microsoft Authenticator](microsoft-authenticator-app-how-to.md) no seu smartphone. A aplicação pode gerar códigos de segurança aleatória que utiliza para iniciar sessão, e esses códigos não requerem qualquer ligação de sinal ou de internet da célula.

## <a name="app-passwords-are-not-working"></a>As palavras-passe de aplicação não estão a funcionar
Em primeiro lugar, certifique-se de que introduziu a palavra-passe de aplicação corretamente. A palavra-passe de aplicação gerada substitui a sua palavra-passe normal, mas apenas para aplicativos de área de trabalho mais antigos que não suportam a verificação de dois passos. Se ainda não está funcionando, tente iniciar sessão e [criar uma nova palavra-passe de aplicação](multi-factor-authentication-end-user-app-passwords.md).  Se continuar sem funcionar, contacte o suporte da empresa e tê-los [eliminar suas palavras-passe de aplicação existentes](../authentication/howto-mfa-userdevicesettings.md) e, em seguida, pode criar um novo.

## <a name="i-didnt-find-an-answer-to-my-problem"></a>Não encontrei uma resposta para meu problema.
Se já tentou estes passos de resolução de problemas, mas é ainda em execução em problemas, contacte o suporte da empresa. Eles devem ser capazes de ajudá-lo.

## <a name="related-topics"></a>Tópicos relacionados
* [Gerir as definições de verificação de dois passos](multi-factor-authentication-end-user-manage-settings.md)  
* [FAQ de aplicação Microsoft Authenticator](microsoft-authenticator-app-faq.md)
