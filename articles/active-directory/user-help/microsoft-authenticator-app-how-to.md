---
title: Comece com a aplicação Microsoft Authenticator - Azure Active Directory | Documentos da Microsoft
description: Saiba como atualizar para a versão mais recente do Microsoft Authenticator.
services: active-directory
author: eross-msft
manager: daveba
ms.assetid: 3065a1ee-f253-41f0-a68d-2bd84af5ffba
ms.service: active-directory
ms.workload: identity
ms.component: user-help
ms.topic: conceptual
ms.date: 07/30/2018
ms.author: lizross
ms.reviewer: librown
ms.openlocfilehash: 43c09bc01e0ed5bd9093242db9183883e402a6f1
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/23/2019
ms.locfileid: "54825583"
---
# <a name="get-started-with-the-microsoft-authenticator-app"></a>Comece com a aplicação Microsoft Authenticator

A aplicação Microsoft Authenticator ajudar a impedir o acesso não autorizado a contas e a parar transações fraudulentas, dando-lhe um nível adicional de segurança para o seu trabalho conta escolar ou profissional (por exemplo, alain@contoso.com) ou a sua conta Microsoft pessoal (para exemplo, alain@outlook.com).

Quando utilizar a aplicação para a verificação de dois passos, pode trabalhar em uma das seguintes formas:

- **Notificação.** A aplicação envia uma notificação para o seu dispositivo. Certifique-se a notificação é à direita e, em seguida, selecione **Verifique se**. Se não reconhecer a notificação, selecione **negar**. Depois de clicar em **negar**, também é possível marcar a solicitação como fraudulentas.

- **Código de verificação.** Depois de escrever o seu nome de utilizador e palavra-passe, pode abrir a aplicação e copie o código de verificação fornecido no **contas** ecrã no ecrã de início de sessão. O código de verificação atua como uma segunda forma de autenticação.

## <a name="opt-in-for-two-step-verification"></a>Optar por verificação de dois passos

Sua organização decide se utilizar a verificação de dois passos com a sua conta escolar ou profissional. O administrador permitirá que sabe quais métodos de verificação tem de ser configurado e utilizado. Para obter mais informações, consulte [o que o Azure multi-factor Authentication significa para mim](multi-factor-authentication-end-user.md).

Para a sua conta Microsoft pessoal, pode configurar a verificação de dois passos para si próprio. Para obter mais informações e instruções, consulte [sobre a verificação de dois passos](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification).

Também pode utilizar a aplicação Microsoft Authenticator com contas de não-Microsoft. Essas contas podem chamar a funcionalidade de algo diferente de verificação de dois passos, mas deve conseguir encontrá-la dentro da segurança ou as definições de início de sessão. Para obter mais informações, sobre como configurar estas contas não pertencentes à Microsoft, consulte a [vídeos de suporte técnico da Microsoft](https://www.youtube.com/playlist?list=PLyhj1WZ29G65QdD9NxTOAm8HwOS-OBUrX).

## <a name="install-the-app"></a>Instalar a aplicação

A aplicação Microsoft Authenticator está disponível para [Android](https://go.microsoft.com/fwlink/?linkid=866594), [iOS](https://go.microsoft.com/fwlink/?linkid=866594) e [Windows Phone](https://go.microsoft.com/fwlink/?Linkid=825071). Para obter a melhor experiência, deve permitir que a aplicação receber notificações quando lhe for pedido. 

## <a name="add-accounts-to-the-app"></a>Adicionar contas para a aplicação

Pode adicionar contas pessoais ou contas escolar ou profissional para a aplicação Microsoft Authenticator. 

### <a name="add-a-personal-microsoft-account"></a>Adicionar uma conta Microsoft pessoal

Para uma conta Microsoft pessoal (um que utiliza para iniciar sessão no Outlook.com, Xbox, Skype, etc.), tudo o que precisa fazer é iniciar sessão na sua conta na aplicação Microsoft Authenticator.

### <a name="add-a-work-or-school-account"></a>Adicionar uma conta escolar ou profissional

1. Se possível, vá para o [verificação adicional de segurança](https://aka.ms/mfasetup) ecrã noutro PC ou dispositivo. Para obter informações sobre como chegar neste ecrã, consulte [alterar as definições de segurança](multi-factor-authentication-end-user-manage-settings.md#where-to-find-the-settings-page) ou contacte o administrador.

    >[!Note]
    >Se o administrador tiver ativado a experiência de pré-visualização de informações de segurança, pode seguir as instruções a [definir informações de segurança para utilizar uma aplicação de autenticação](security-info-setup-auth-app.md) secção.

2. A caixa de verificação junto a **aplicação de autenticador**e, em seguida, selecione **configurar**.

    ![O botão Configurar no ecrã de definições de verificação de segurança](./media/microsoft-authenticator-app-how-to/auth-app-configure.png)

    O **aplicação móvel de configurar** ecrã é apresentado com um código QR para-lhe analisar com a aplicação de autenticador.

    ![Ecrã que fornece o código QR](./media/microsoft-authenticator-app-how-to/auth-app-barcode.png)

3. Abra a aplicação Microsoft Authenticator. Sobre o **contas** ecrã, selecione **adicionar conta**e, em seguida, selecione **trabalho conta escolar ou profissional**.

4. Utilizar a câmara do dispositivo para digitalizar o código QR e, em seguida, selecione **feito** para fechar o ecrã de código QR.

    >[!Note]
    >Se a câmara não está a funcionar corretamente, pode [introduzir manualmente o código QR e o URL](#add-an-account-to-the-app-manually).

    O **contas** ecrã da aplicação mostra-lhe o nome da conta e um código de verificação de seis dígitos. Para segurança adicional, as alterações de código de verificação a cada 30 segundos a impedir a utilização do mesmo código de duas vezes.  

    ![ecrã de contas](./media/microsoft-authenticator-app-how-to/auth-app-accounts.png)

### <a name="add-an-account-to-the-app-manually"></a>Adicionar manualmente uma conta para a aplicação

1. Vá para o **verificação adicional de segurança** ecrã. Para obter informações sobre como chegar neste ecrã, consulte [alterar as definições de segurança](multi-factor-authentication-end-user-manage-settings.md#where-to-find-the-settings-page).

2. A caixa de verificação junto a **aplicação de autenticador**e, em seguida, selecione **configurar**.

    O **aplicação móvel de configurar** ecrã é apresentado.

3. Copie o código e as informações de URL do **aplicação móvel de configurar** ecrã, para que pode digitá-los no scanner QR manualmente.

4. Abra a aplicação Microsoft Authenticator. Sobre o **contas** ecrã, selecione **adicionar conta**e, em seguida, selecione **trabalho conta escolar ou profissional**.

5. No ecrã de scanner QR, selecione **introduza o código manualmente**.

    ![Ecrã de analisar um código QR](./media/microsoft-authenticator-app-how-to/auth-app-manual-code.png)
   
6. Escreva o código e URL a partir do ecrã com o código QR para o **adicionar uma conta** ecrã e, em seguida, selecione **concluir**.

    ![Ecrã para a inserção de código e URL](./media/microsoft-authenticator-app-how-to/auth-app-code-url.png)

    O **contas** ecrã da aplicação mostra-lhe o nome da conta e um código de verificação de seis dígitos. Para segurança adicional, as alterações de código de verificação a cada 30 segundos a impedir a utilização do mesmo código de duas vezes.

### <a name="using-your-devices-fingerprint-or-facial-recognition-capabilities"></a>Usando a identificação digital ou recursos de reconhecimento facial do seu dispositivo

Sua organização poderá exigir um PIN para concluir a verificação de identidade. Pode configurar a aplicação Microsoft Authenticator para utilizar a impressão digital do seu dispositivo ou a capacidades de reconhecimento facial em vez de um PIN. Pode definir isso na primeira vez que utiliza a aplicação de autenticador para verificar a sua conta, ao selecionar a opção para utilizar as capacidades de Biometria do dispositivo como identificação em vez do PIN.

## <a name="use-the-app-when-you-sign-in"></a>Utilizar a aplicação ao iniciar sessão

Depois de adicionar as suas contas para a aplicação, pode utilizar a aplicação para iniciar sessão nas suas contas.

Se optar por utilizar códigos de verificação na aplicação, começará a vê-los sobre o **contas** página. Os códigos de alterar a cada 30 segundos para que sempre terá um novo código quando precisa de um. Mas não precisa de fazer nada com eles, depois de iniciar sessão e for pedido para introduzir um código de verificação.

## <a name="next-steps"></a>Passos Seguintes

- Se tiver dúvidas mais gerais sobre a aplicação, consulte o artigo [FAQs do Microsoft Authenticator](microsoft-authenticator-app-faq.md)

- Se pretender obter mais informações sobre a verificação de dois passos, veja [configurar a minha conta para verificação de dois passos](multi-factor-authentication-end-user-first-time.md)

- Se pretender obter mais informações sobre as informações de segurança, veja [gerir as suas informações de segurança](security-info-manage-settings.md)
