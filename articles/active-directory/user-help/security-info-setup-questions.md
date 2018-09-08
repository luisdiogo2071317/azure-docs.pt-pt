---
title: Definir informações de segurança para utilizar perguntas de segurança - Azure Active Directory | Documentos da Microsoft
description: Configure suas informações de segurança para verificar a sua identidade com perguntas de segurança predefinidas.
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
ms.openlocfilehash: b4913d55ee3d254f197512d9251ae750d8896f53
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/07/2018
ms.locfileid: "44160443"
---
# <a name="set-up-security-info-to-use-pre-defined-security-questions-preview"></a>Definir informações de segurança para utilizar perguntas de segurança predefinidas (pré-visualização)

[!INCLUDE [preview-notice](../../../includes/active-directory-end-user-preview-notice-security-info.md)]

Configuração de suas informações de segurança requer que inicie sessão na sua conta escolar ou profissional e, em seguida, concluir o processo de registo. Se nunca configurou as informações de segurança, será solicitado a fazê-lo agora.

## <a name="set-up-security-questions"></a>Configurar as perguntas de segurança

Dependendo das definições da sua organização, poderá ser-lhe pedido para adicionar perguntas de segurança às suas informações de segurança quando iniciar sessão. Caso contrário, para iniciar a configuração de perguntas de segurança das informações de segurança, siga os passos em [gerir as suas informações de segurança](security-info-manage-settings.md).

Se utilizar perguntas de segurança, recomendamos que utilize-os em conjunto com outro método. Perguntas de segurança podem ser menos seguras do que outros métodos, porque algumas pessoas podem saber as respostas a perguntas de outra pessoa.

>[!Note]
>Perguntas de segurança são armazenadas em privado e segura num objeto de utilizador no diretório e apenas podem ser respondidas por si durante o registo. Não é possível para o administrador para ler ou modificar as suas perguntas ou respostas.<br>Se não vir a opção de perguntas de segurança, é possível que a sua organização não permite-lhe utilizar perguntas de segurança para a verificação. Se for este o caso, terá de escolher outro método ou contacte o administrador para obter mais ajuda.

### <a name="to-choose-and-answer-your-security-questions"></a>Escolher e responder a perguntas de segurança

1. Selecione **perguntas de segurança**e, em seguida, escolha o que poderá útil responder às perguntas de segurança. 

    O número de perguntas de segurança, que deve escolher é determinado pelo seu administrador.

    ![Informações de segurança página, escolha as perguntas de segurança](media/security-info/security-info-keep-secure-setup-pick-questions.png)

2. Fornece respostas para suas perguntas selecionadas e, em seguida, selecione **feito**.

## <a name="additional-security-info-options"></a>Opções de informações de segurança adicionais

Tem opções para como os contactos de organização para verificar a sua identidade, com base no que é que está tentando fazer. As opções incluem:

- **Aplicação de autenticador.** Transferir e utilizar uma aplicação de autenticação para obter uma notificação de aprovação ou um código de aprovação gerado aleatoriamente para a reposição de palavra-passe ou a verificação em dois passos. Para obter instruções passo a passo sobre como configurar e utilizar a aplicação Microsoft Authenticator, consulte [definir informações de segurança para utilizar uma aplicação de autenticação](security-info-setup-auth-app.md).

- **Texto de dispositivos móveis.** Introduza o seu número de dispositivos móveis e obtenha um código que irá utilizar para verificação de dois passos ou palavra-passe de uma mensagem de reposição. Para obter instruções passo a passo sobre como verificar a sua identidade com uma mensagem de texto (SMS), consulte [definir informações de segurança para utilizar mensagens de texto (SMS)](security-info-setup-text-msg.md).

- **Dispositivo móvel ou de trabalho de chamadas telefónicas.** Introduza o seu número de dispositivos móveis e obtenha uma chamada telefónica para a reposição de palavra-passe ou a verificação em dois passos. Para obter instruções passo a passo sobre como verificar a sua identidade com um número de telefone, consulte [definir informações de segurança para utilizar chamadas telefónicas](security-info-setup-phone-number.md).

- **Endereço de e-mail.** Introduza o seu e-mail profissional ou escolar para receber um e-mail para a reposição de palavra-passe. Esta opção não está disponível para verificação de dois passos. Para obter instruções passo a passo sobre como configurar o seu e-mail, consulte [definir informações de segurança para utilizar o e-mail](security-info-setup-email.md).
   
    >[!Note]
    >Se algumas dessas opções estão em falta, é mais provável porque a sua organização não permite que esses métodos. Se for este o caso, terá de escolher um método disponível ou contacte o administrador para obter mais ajuda.

## <a name="next-steps"></a>Passos Seguintes

- Se precisar de atualizar as suas informações de segurança, siga as instruções no [gerir as suas informações de segurança](security-info-manage-settings.md) artigo.

- Repor a palavra-passe, se tiver perdido ou esquecidas, do [portal de reposição de palavra-passe](https://passwordreset.microsoftonline.com/) ou siga os passos a [repor a palavra-passe profissional ou escolar](user-help-reset-password.md) artigo.

- Obter solução de problemas dicas e ajuda para problemas de início de sessão no [não pode iniciar sessão na sua conta Microsoft](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant) artigo.