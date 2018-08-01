---
title: Configurar informações de segurança para usar chamadas de telefone - Azure Active Directory | Documentos da Microsoft
description: Configure suas informações de segurança para verificar a sua identidade com um dispositivo móvel ou número de telefone de trabalho.
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
ms.openlocfilehash: 701a47a3907c77dbf5d51692e5d1ddc0c49ab985
ms.sourcegitcommit: e3d5de6d784eb6a8268bd6d51f10b265e0619e47
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/01/2018
ms.locfileid: "39390107"
---
# <a name="set-up-security-info-to-use-phone-calls-preview"></a>Configurar informações de segurança para usar chamadas de telefone (pré-visualização)

[!INCLUDE[preview-notice](../../../includes/active-directory-end-user-preview-notice-security-info.md)]

Configuração de suas informações de segurança requer que inicie sessão na sua conta escolar ou profissional e, em seguida, concluir o processo de registo. Se nunca configurou as informações de segurança, será solicitado a fazê-lo agora.

## <a name="set-up-phone-calls"></a>Configurar chamadas telefónicas

Dependendo das definições da sua organização, poderá ser-lhe pedido para adicionar um número de telefone para sua informação de segurança quando iniciar sessão. Caso contrário, para iniciar a configuração de chamadas telefónicas das informações de segurança, siga os passos em [gerir as suas informações de segurança](security-info-manage-settings.md).

>[!Note]
>Informações de segurança não suportam a utilização de extensões de telefone. Mesmo que adicione o formato correto, 4255551234 + 1 X 12345, as extensões são removidas antes da chamada é feita.<br>Se não vir a opção de telefone, é possível que a sua organização não permite a utilização de chamadas telefónicas para verificação. Se for este o caso, terá de escolher outro método ou contacte o administrador para obter mais ajuda.

### <a name="to-use-your-phone-number"></a>Para utilizar o seu número de telefone

1. Selecione o **Phone** opção.

    O **configurar o seu telemóvel** é apresentado o assistente.

    ![Configurar o seu país ou região de código e número de telefone](media/security-info/security-info-keep-secure-setup-phone.png)

2. Escolher seus **país ou região** na caixa de lista pendente, escreva o número de telefone (incluindo o código de área, se aplicável) para o **número de telefone** caixa, selecione o **telefonar-me** opção, e, em seguida, selecione **seguinte**.

    Receberá uma chamada telefónica para certificar-se de que escreveu o número de telefone correto. Nessa altura, será perguntado para enviar a chave de cardinal (#) para confirmar e concluir a configuração.

    ![Verifique se o ecrã de telefone, que mostra que a chamada foi respondida com êxito](media/security-info/security-info-keep-secure-verify-phone-call.png)

    As informações de segurança é atualizada para utilizar o seu número de telefone para verificar a sua identidade quando utilizar a reposição de palavra-passe de verificação ou Self-Service de dois passos.

    >[!Note]
    >Se pretender receber uma mensagem de texto em vez de uma chamada telefónica para o seu dispositivo móvel, siga os passos a [definir informações de segurança para utilizar mensagens de texto (SMS)](security-info-setup-text-msg.md) artigo.

## <a name="additional-security-info-options"></a>Opções de informações de segurança adicionais

Tem opções adicionais para como os contactos de organização para verificar a sua identidade, com base no que é que está tentando fazer. As opções incluem:

- **Aplicação de autenticador.** Transferir e utilizar uma aplicação de autenticação para obter uma notificação de aprovação ou um código de aprovação gerado aleatoriamente para a reposição de palavra-passe ou a verificação em dois passos. Para obter instruções passo a passo sobre como configurar e utilizar a aplicação Microsoft Authenticator, consulte [definir informações de segurança para utilizar uma aplicação de autenticação](security-info-setup-auth-app.md).

- **Texto de dispositivos móveis.** Introduza o seu número de dispositivos móveis e obtenha um código que irá utilizar para verificação de dois passos ou palavra-passe de uma mensagem de reposição. Para obter instruções passo a passo sobre como verificar a sua identidade com uma mensagem de texto (SMS), consulte [definir informações de segurança para utilizar mensagens de texto (SMS)](security-info-setup-text-msg.md).

- **Endereço de e-mail.** Introduza o seu e-mail profissional ou escolar para receber um e-mail para a reposição de palavra-passe. Esta opção não está disponível para verificação de dois passos. Para obter instruções passo a passo sobre como configurar o seu e-mail, consulte [definir informações de segurança para utilizar o e-mail](security-info-setup-email.md).

- **Perguntas de segurança.** Responder a algumas perguntas de segurança criadas pelo seu administrador para a sua organização. Esta opção só está disponível para a reposição de palavra-passe e não para verificação de dois passos. Para obter instruções passo a passo sobre como configurar a suas perguntas de segurança, consulte a [definir informações de segurança para utilizar perguntas de segurança](security-info-setup-questions.md) artigo.
    
    >[!Note]
    >Se algumas dessas opções estão em falta, é mais provável porque a sua organização não permite que esses métodos. Se for este o caso, terá de escolher um método disponível ou contacte o administrador para obter mais ajuda.

## <a name="next-steps"></a>Passos Seguintes

- Se precisar de atualizar as suas informações de segurança, siga as instruções no [gerir as suas informações de segurança](security-info-manage-settings.md) artigo.

- Repor a palavra-passe, se tiver perdido ou esquecidas, do [portal de reposição de palavra-passe](https://passwordreset.microsoftonline.com/) ou siga os passos a [repor a palavra-passe profissional ou escolar](user-help-reset-password.md) artigo.

- Obter solução de problemas dicas e ajuda para problemas de início de sessão no [não pode iniciar sessão na sua conta Microsoft](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant) artigo.