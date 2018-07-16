---
title: Aplicação Microsoft Authenticator para celulares - Azure AD | Documentos da Microsoft
description: Saiba como atualizar para a versão mais recente do Azure Authenticator.
services: multi-factor-authentication
documentationcenter: ''
author: eross-msft
manager: mtillman
ms.assetid: 3065a1ee-f253-41f0-a68d-2bd84af5ffba
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/17/2017
ms.author: lizross
ms.reviewer: librown
ms.custom: H1Hack27Feb2017, end-user
ms.openlocfilehash: 8241dcaaf5623a22f4fc485f021766276472fb51
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39059824"
---
# <a name="get-started-with-the-microsoft-authenticator-app"></a>Comece com a aplicação Microsoft Authenticator
A aplicação Microsoft Authenticator oferece um nível adicional de segurança na sua conta escolar ou profissional (por exemplo, bsimon@contoso.com) ou a sua conta Microsoft (por exemplo, bsimon@outlook.com).

A aplicação funciona de uma de duas formas:

* **Notificação**. A aplicação pode ajudar a impedir o acesso não autorizado a contas e parar as transações fraudulentas ao enviar uma notificação para o seu smartphone ou tablet. Basta visualizar a notificação e se esta for legítima, selecionar **Verifique se**. Caso contrário, pode selecionar **negar**.
* **Código de verificação**. A aplicação pode ser utilizada como token de software para gerar um código de verificação de OAuth. Depois de introduzir o seu nome de utilizador e palavra-passe, é introduzir o código fornecido pela aplicação para o ecrã de início de sessão. O código de verificação fornece uma segunda forma de autenticação.

A aplicação Microsoft Authenticator substitui a aplicação Azure Authenticator. A aplicação Azure Authenticator ainda funciona, mas se decidir mudar para a nova aplicação Microsoft Authenticator, este artigo pode ajudá-lo.  

## <a name="opt-in-for-two-step-verification"></a>Optar por verificação de dois passos

A aplicação Microsoft Authenticator não funciona por si só. Configure cada uma das suas contas para solicitar-lhe um segundo método de verificação após entrar com seu nome de utilizador e palavra-passe.

Para uma conta escolar ou profissional, não obtém, normalmente, escolher esta funcionalidade para mesmo. Em vez disso, um administrador de segurança optar pela aceitação em seu nome e, em seguida, notifica-o para registar métodos de verificação para a sua conta. Se este cenário aplica-se a si, saiba mais na [o que o Azure multi-factor Authentication significa para mim](multi-factor-authentication-end-user.md).

Para uma conta pessoal, terá de configurar a verificação de dois passos para si próprio. Se tiver uma conta Microsoft, esses passos estão disponíveis no [sobre a verificação de dois passos](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification).

Também pode utilizar o Microsoft Authenticator com contas de não-Microsoft. Pode chamar a funcionalidade de algo diferente de verificação de dois passos, mas deve conseguir encontrá-lo em segurança ou as definições de início de sessão.

## <a name="install-the-app"></a>Instalar a aplicação
A aplicação Microsoft Authenticator está disponível para [Android](https://go.microsoft.com/fwlink/?linkid=866594), [iOS](https://go.microsoft.com/fwlink/?linkid=866594) e [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071).

## <a name="add-accounts-to-the-app"></a>Adicionar contas para a aplicação
Para cada conta que pretende adicionar à aplicação Microsoft Authenticator, utilize um dos seguintes procedimentos:

### <a name="add-a-personal-microsoft-account-to-the-app"></a>Adicionar uma conta Microsoft pessoal para a aplicação

Para uma conta Microsoft pessoal (um que utiliza para iniciar sessão no Outlook.com, Xbox, Skype, etc.), tudo o que precisa fazer é iniciar sessão na sua conta na aplicação Microsoft Authenticator.

### <a name="add-a-work-or-school-account-to-the-app-using-the-qr-code-scanner"></a>Adicionar uma conta profissional ou escolar na aplicação com o scanner de código QR
1. Vá para o ecrã de definições de verificação de segurança.  Para obter informações sobre como chegar neste ecrã, consulte [alterar as definições de segurança](multi-factor-authentication-end-user-manage-settings.md#where-to-find-the-settings-page).
2. A caixa de verificação junto a **aplicação de autenticador** , em seguida, selecione **configurar**.

    ![O botão Configurar no ecrã de definições de verificação de segurança](./media/microsoft-authenticator-app-how-to/azureauthe.png)

    Esta ação exibe uma tela com um código QR no mesmo.

    ![Ecrã que fornece o código QR](./media/microsoft-authenticator-app-how-to/barcode2.png)
3. Abra a aplicação Microsoft Authenticator. Sobre o **contas** ecrã, selecione **+** e, em seguida, especifique que pretende adicionar uma conta escolar ou profissional.
4. Utilizar a câmara para digitalizar o código QR e, em seguida, selecione **feito** para fechar o ecrã de código QR.

    Se a câmara não está a funcionar corretamente, pode [introduzir manualmente o código QR e o URL](#add-an-account-to-the-app-manually).

5. Quando a aplicação mostra o nome da sua conta com um código de seis dígitos abaixo dela, terá concluído.

    ![ecrã de contas](./media/microsoft-authenticator-app-how-to/accounts.png)

### <a name="add-an-account-to-the-app-manually"></a>Adicionar manualmente uma conta para a aplicação
1. Vá para o ecrã de definições de verificação de segurança.  Para obter informações sobre como chegar neste ecrã, consulte [alterar as definições de segurança](multi-factor-authentication-end-user-manage-settings.md).
2. Selecione **configurar**.

    ![O botão Configurar no ecrã de definições de verificação de segurança](./media/microsoft-authenticator-app-how-to/azureauthe.png)

    Esta ação exibe uma tela com um código QR no mesmo.  Tenha em atenção que o código e URL.

    ![Ecrã que fornece o código QR e o URL](./media/microsoft-authenticator-app-how-to/barcode2.png)
3. Abra a aplicação Microsoft Authenticator. Sobre o **contas** ecrã, selecione **+** e, em seguida, especifique que pretende adicionar uma conta escolar ou profissional.

4. No scanner, selecione **introduza o código manualmente**.

    ![Ecrã de analisar um código QR](./media/microsoft-authenticator-app-how-to/scan2.png)
5. Introduza o código e o URL nas caixas adequadas na aplicação, em seguida, selecione **concluir**.

    ![Ecrã para a inserção de código e URL](./media/microsoft-authenticator-app-how-to/manual.png)

6. Quando a aplicação mostra o nome da sua conta com um código de seis dígitos abaixo dela, terá concluído.

    ![ecrã de contas](./media/microsoft-authenticator-app-how-to/accounts.png)

### <a name="add-an-account-to-the-app-using-your-devices-fingerprint-or-facial-recognition-capabilities"></a>Adicionar uma conta na aplicação com a identificação digital ou recursos de reconhecimento facial do seu dispositivo
Sua organização pode exigir um PIN para concluir o desafio de verificação. A aplicação Microsoft Authenticator pode utilizar a impressão digital do seu dispositivo ou a capacidades de reconhecimento facial em vez de um PIN. Para configurar isso a sua primeira verificação na aplicação, verá uma opção para utilizar o Touch ID (para iOS) ou identificação de impressão digital em vez disso. 

Para configurar o Touch ID para o Microsoft Authenticator, terá de concluir um desafio de verificação normal com um PIN. Microsoft Authenticator automaticamente o definirá para dispositivos que suportam o Touch ID. 

![Verificação da configuração do Touch ID](./media/microsoft-authenticator-app-how-to/touchid1.png)

De que aponte para a frente, quando precisa verificar seu início de sessão, selecione a notificação push recebida e analisar a sua impressão digital em vez de introduzir o PIN.

![Notificações push](./media/microsoft-authenticator-app-how-to/touchid2.png)

## <a name="use-the-app-when-you-sign-in"></a>Utilizar a aplicação ao iniciar sessão

Assim que a sua conta é adicionada à aplicação, poderá ser solicitado a fazer uma verificação de teste para verificar se tudo o que foi configurado corretamente. Depois disso, está pronto! Não precisa de fazer mais nada até à próxima vez que iniciar sessão.

Se optar por utilizar códigos de verificação na aplicação, começar a vê-los na home page. Alterar a cada 30 segundos para que sempre terá um novo código quando precisa de um. Mas não precisa de fazer nada com eles, depois de iniciar sessão e for pedido para introduzir um código de verificação.  
