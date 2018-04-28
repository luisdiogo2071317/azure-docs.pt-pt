---
title: Cópia de segurança e recuperar informações com o Microsoft Authenticator - Azure | Microsoft Docs
description: Saiba como criar cópias de segurança e recuperar as credenciais da conta, utilizando a aplicação Authenticator da Microsoft.
services: multi-factor-authentication
documentationcenter: ''
author: eross-msft
manager: mtillman
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/28/2018
ms.author: lizross
ms.reviewer: olhaun
ms.custom: end-user
ms.openlocfilehash: 0b76d2debb9814188e5551252e52bd0dae38eb50
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/23/2018
---
# <a name="backup-and-recover-account-credentials-with-the-microsoft-authenticator-app"></a>Cópia de segurança e recuperar as credenciais da conta com a aplicação Microsoft Authenticator
**Aplica-se a:**

- dispositivos iOS

A aplicação Microsoft Authenticator efetua a cópia de segurança de credenciais da conta e definições de aplicação relacionados, tais como a ordem das suas contas, para a nuvem. Após a cópia de segurança, também pode utilizar a aplicação para recuperar as informações num dispositivo novo, evitando potencialmente obter bloqueado fora ou ter de recriar as contas.

>[!IMPORTANT]
> Precisará de uma conta Microsoft pessoal e uma conta do iCloud para cada localização de armazenamento de cópia de segurança. Mas dentro dessa localização de armazenamento, pode criar cópias de segurança várias contas. Por exemplo, pode ter uma conta pessoal, uma conta profissional e uma conta de terceiros, como o Facebook, Google e assim sucessivamente.<br><br>Só as credenciais da conta são armazenadas, que inclui o nome de utilizador e o código de verificação de conta de 8-dígitos necessárias para provar a sua identidade com a aplicação Microsoft Authenticator. Não armazenamos como outras informações associadas com as suas contas, incluindo ficheiros ou mensagens de correio eletrónico. Podemos também não associar ou partilhar as contas de qualquer forma ou com qualquer produto ou serviço. E por fim, o administrador de TI não irá obter as informações sobre qualquer uma destas contas.

## <a name="back-up-your-account-credentials"></a>Criar cópias de segurança as credenciais da conta
Antes de fazer uma cópia de segurança as suas credenciais, tem de ter ambos:

- Um pessoal [conta Microsoft](https://account.microsoft.com/account) para atuar como a sua conta de recuperação.

- Um [conta do iCloud](https://www.icloud.com/) para a localização de armazenamento real. 

Exigir que inicie sessão para ambas as contas em conjunto proporciona uma segurança reforçada para sua informação de cópia de segurança.

**Para ativar na nuvem de cópia de segurança**
-   No seu dispositivo iOS, selecione **definições**, selecione **cópia de segurança**e, em seguida, ative **cópia de segurança automática**.

    As credenciais da conta são cópias de segurança à sua conta do iCloud.

    ![ecrã de definições do iOS, que mostra a localização de auto definições de cópia de segurança](./media/authenticator-app-backup-and-recovery/backup-and-recovery-turn-on.png)

## <a name="recover-your-account-credentials-on-your-new-device"></a>Recuperar as credenciais da conta no seu novo dispositivo
Pode recuperar as credenciais da conta da sua conta do iCloud, com a mesma conta de recuperação do Microsoft que configurou quando criou cópias de segurança as suas informações.

**Para recuperar as informações**
1.  No seu dispositivo iOS, abra a aplicação Microsoft Authenticator e selecione **iniciar a recuperação** na parte inferior do ecrã.

    ![Aplicação Microsoft Authenticator, que mostra onde clique Begin recuperação](./media/authenticator-app-backup-and-recovery/backup-and-recovery-begin-recovery.png)

2.  Inicie sessão na sua conta de recuperação, com a mesma conta Microsoft pessoal utilizado durante o processo de cópia de segurança.

    As credenciais da conta são recuperadas para o novo dispositivo.

Depois de concluir a recuperação, poderá reparar que os códigos de verificação de conta Microsoft pessoais na aplicação Microsoft Authenticator são diferentes entre os telemóveis antigas e novas. Os códigos são diferentes porque cada dispositivo tem as suas próprias credenciais exclusivo, mas são ambos válido e de trabalho ao iniciar sessão utilizando o telemóvel associado.

## <a name="recover-additional-accounts-requiring-more-verification"></a>Recuperar contas adicionais que requerem mais verificação
Se utilizar notificações push com o seu pessoal, profissional, contas escolares ou profissionais, receberá um alerta no ecrã que diz tem de fornecer verificação adicional para poder recuperar as suas informações. Porque necessitam de notificações push utilizando uma credencial que tenha associado para o seu dispositivo específico e nunca são enviadas através da rede, tem de provar a identidade antes da credencial é criada no seu dispositivo.

Para contas Microsoft pessoais, pode provar a sua identidade ao introduzir a palavra-passe, juntamente com um número de e-mail ou telefone alternativo. Para contas profissionais ou escolares, tem de analisar um código QR atribuído pelo seu fornecedor de conta.

**Para fornecer uma verificação adicional para contas pessoais**
1.  No **contas** ecrã da aplicação Microsoft Authenticator, selecione na lista pendente de seta junto a conta que pretende recuperar.

    ![Aplicação Microsoft Authenticator, que mostra as contas disponíveis com os respetivos setas pendente associadas](./media/authenticator-app-backup-and-recovery/backup-and-recovery-arrow.png)

2.  Selecione **iniciar sessão para recuperar**, escreva a palavra-passe e, em seguida, confirme o seu e-mail endereço ou número de telefone como verificação adicional.

    ![Aplicação Microsoft Authenticator, permitindo-lhe introduzir as suas informações de início de sessão](./media/authenticator-app-backup-and-recovery/backup-and-recovery-sign-in.png)

**Para fornecer uma verificação adicional para contas profissionais ou escolares**
1.  No **contas** ecrã da aplicação Microsoft Authenticator, selecione na lista pendente de seta junto a conta que pretende recuperar.

    ![Aplicação Microsoft Authenticator, que mostra as contas disponíveis com os respetivos setas pendente associadas](./media/authenticator-app-backup-and-recovery/backup-and-recovery-additonal-accts.png)

2.  Selecione **código QR analisar recuperar**e, em seguida, analisar o código QR fornecido pelo seu administrador.

    ![Aplicação Microsoft Authenticator, permitindo-lhe o código QR](./media/authenticator-app-backup-and-recovery/backup-and-recovery-scan-qr-code.png)

## <a name="troubleshooting-backup-and-recovery-problems"></a>Resolução de problemas de cópia de segurança e recuperação
Existem alguns motivos por que razão a cópia de segurança poderão não estar disponível:

-   **Alterar a sistemas operativos.** A cópia de segurança é armazenada na opção de armazenamento na nuvem fornecida pelo sistema operativo do seu telemóvel, que significa que a cópia de segurança não está disponível se mudar entre Android e iOS. Nesta situação, tem de recriar manualmente a conta na aplicação.

-   **Problemas de rede ou a palavra-passe.** Certifique-se de que está ligado a uma rede e iniciou a sessão na sua conta do iCloud utilizando o mesmo ID de Apple utilizadas no último iPhone.

-   **Eliminação acidental.** É possível que tenha eliminado da sua conta de cópia de segurança do seu dispositivo anterior ou ao gerir a conta de armazenamento na nuvem. Nesta situação, tem de recriar manualmente a conta na aplicação.

-   **Contas Microsoft Authenticator existentes.** Se já configurou contas na aplicação Microsoft Authenticator, a aplicação não será possível recuperar as contas de cópia de segurança. Recuperação impedir que ajuda a garantir que os detalhes da sua conta não são substituídos por informações Desatualizadas. Nesta situação, tem de remover quaisquer informações de conta existente das contas existentes configuradas na sua aplicação de autenticação para poder recuperar a cópia de segurança.

## <a name="next-steps"></a>Passos Seguintes
Agora que já tiver uma cópia de segurança e recuperação as credenciais da conta para o novo dispositivo, pode continuar a utilizar a aplicação Microsoft Authenticator para verificar a sua identidade.

## <a name="related-topics"></a>Tópicos relacionados
- [Começar a utilizar a aplicação Authenticator da Microsoft](microsoft-authenticator-app-how-to.md)  

- [Iniciar sessão com o telemóvel](microsoft-authenticator-app-phone-signin-faq.md)

- [Aplicação de autenticação do Microsoft FAQ](microsoft-authenticator-app-faq.md)

- [Multi-factor Authentication](https://docs.microsoft.com/en-us/azure/multi-factor-authentication/)