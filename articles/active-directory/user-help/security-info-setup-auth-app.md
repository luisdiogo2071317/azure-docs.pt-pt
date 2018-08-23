---
title: Definir informações de segurança para utilizar uma aplicação de autenticação - Azure Active Directory | Documentos da Microsoft
description: Configure suas informações de segurança para verificar a sua identidade com a aplicação Microsoft Authenticator.
services: active-directory
author: eross-msft
manager: mtillman
ms.reviewer: sahenry
ms.service: active-directory
ms.workload: identity
ms.component: user-help
ms.topic: conceptual
ms.date: 07/30/2018
ms.author: lizross
ms.openlocfilehash: e7b07ba892f8f904b1b2127fa8e76649eb004388
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/18/2018
ms.locfileid: "42062061"
---
# <a name="set-up-security-info-to-use-an-authenticator-app-preview"></a>Definir informações de segurança para utilizar uma aplicação de autenticação (pré-visualização)

[!INCLUDE[preview-notice](../../../includes/active-directory-end-user-preview-notice-security-info.md)]

Configuração de suas informações de segurança requer que inicie sessão na sua conta escolar ou profissional e, em seguida, concluir o processo de registo. Se nunca configurou as informações de segurança, será solicitado a fazê-lo agora.

## <a name="set-up-the-microsoft-authenticator-app"></a>Configurar a aplicação Microsoft Authenticator

Dependendo das definições da sua organização, poderá ser-lhe pedido para configurar a aplicação Microsoft Authenticator ao iniciar sessão. Caso contrário, para iniciar a configuração da aplicação Microsoft Authenticator das informações de segurança, siga os passos em [gerir as suas informações de segurança](security-info-manage-settings.md).

Para transferir e obter mais informações sobre a aplicação Microsoft Authenticator, veja [começar com a aplicação Microsoft Authenticator](microsoft-authenticator-app-how-to.md).

>[!Note]
>Se não pretender utilizar a aplicação Microsoft Authenticator, pode escolher uma aplicação diferente durante a configurar. Este artigo utiliza a aplicação Microsoft Authenticator. Se não vir a opção de aplicação de autenticador, é possível que a sua organização não permite a utilização de uma aplicação de autenticação para a verificação. Se for este o caso, terá de escolher outro método ou contacte o administrador para obter mais ajuda.

### <a name="to-use-the-microsoft-authenticator-app"></a>Para utilizar a aplicação Microsoft Authenticator

1. Selecione o **aplicação de autenticador** opção.

    O **obter a aplicação** é apresentado o assistente.

    ![Obter o Assistente de aplicação, o ecrã inicial](media/security-info/security-info-auth-app-wizard.png)

    Se não pretender utilizar a aplicação Microsoft Authenticator, pode clicar a **desejo usar uma aplicação de autenticador diferente** uma ligação a **obter a aplicação** ecrã.

2. Depois de instalar a aplicação Microsoft Authenticator, selecione **seguinte**.

    Se lhe for pedido, permitir notificações, adicionar uma nova conta e, em seguida, selecione **trabalho conta escolar ou profissional**.

3. Selecione **Seguinte**.

    O **digitalizar o código QR** ecrã é apresentado.

    ![Verifique o código QR com a aplicação de autenticador](media/security-info/security-info-scan-qr.png)

4. Abra a aplicação Microsoft Authenticator, selecione **adicionar conta** partir a **personalizar e controle** ícone no canto superior direito e, em seguida, selecione **trabalho conta escolar ou profissional**. 

5. Se tiver uma aplicação de leitor de código QR, analise o código fornecido. Se não tiver uma aplicação de leitor de código, pode selecionar o **não é possível verificar a ligação do código de QR** e introduzir manualmente o código e o URL para a aplicação Microsoft Authenticator.

6. Utilize a aplicação Microsoft Authenticator para aprovar a notificação para ativar a aplicação.

    As informações de segurança é atualizada para utilizar a aplicação Microsoft Authenticator para verificar a sua identidade quando utilizar a reposição de palavra-passe de verificação ou Self-Service de dois passos.

    >[!Note]
    >Se sua organização permitir, também obterá um código de verificação junto com a notificação de aplicação Microsoft Authenticator. Se quiser tornar o código de seu método padrão, siga as instruções em [gerir as suas informações de segurança](security-info-setup-auth-app.md).

## <a name="additional-security-info-options"></a>Opções de informações de segurança adicionais

Tem opções adicionais para como os contactos de organização para verificar a sua identidade, com base no que é que está tentando fazer. As opções incluem:

- **Texto de dispositivos móveis.** Introduza o seu número de dispositivos móveis e obtenha um código que irá utilizar para verificação de dois passos ou palavra-passe de uma mensagem de reposição. Para obter instruções passo a passo sobre como verificar a sua identidade com uma mensagem de texto (SMS), consulte [definir informações de segurança para utilizar mensagens de texto (SMS)](security-info-setup-text-msg.md).

- **Dispositivo móvel ou de trabalho de chamadas telefónicas.** Introduza o seu número de dispositivos móveis e obtenha uma chamada telefónica para a reposição de palavra-passe ou a verificação em dois passos. Para obter instruções passo a passo sobre como verificar a sua identidade com um número de telefone, consulte [definir informações de segurança para utilizar chamadas telefónicas](security-info-setup-phone-number.md).

- **Endereço de e-mail.** Introduza o seu e-mail profissional ou escolar para receber um e-mail para a reposição de palavra-passe. Esta opção não está disponível para verificação de dois passos. Para obter instruções passo a passo sobre como configurar o seu e-mail, consulte [definir informações de segurança para utilizar o e-mail](security-info-setup-email.md).

- **Perguntas de segurança.** Responder a algumas perguntas de segurança criadas pelo seu administrador para a sua organização. Esta opção só está disponível para a reposição de palavra-passe e não para verificação de dois passos. Para obter instruções passo a passo sobre como configurar a suas perguntas de segurança, consulte a [definir informações de segurança para utilizar perguntas de segurança](security-info-setup-questions.md) artigo.
    
    >[!Note]
    >Se algumas dessas opções estão em falta, é mais provável porque a sua organização não permite que esses métodos. Se for este o caso, terá de escolher um método disponível ou contacte o administrador para obter mais ajuda.

## <a name="next-steps"></a>Passos Seguintes

- Se precisar de atualizar as suas informações de segurança, siga as instruções no [gerir as suas informações de segurança](security-info-manage-settings.md) artigo.

- Repor a palavra-passe, se tiver perdido ou esquecidas, do [portal de reposição de palavra-passe](https://passwordreset.microsoftonline.com/) ou siga os passos a [repor a palavra-passe profissional ou escolar](user-help-reset-password.md) artigo.

- Obter solução de problemas dicas e ajuda para problemas de início de sessão no [não pode iniciar sessão na sua conta Microsoft](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant) artigo.