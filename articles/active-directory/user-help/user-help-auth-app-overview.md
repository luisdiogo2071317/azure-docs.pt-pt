---
title: Descrição geral da aplicação Microsoft Authenticator - Azure Active Directory | Microsoft Docs
description: Saiba mais sobre a aplicação Microsoft Authenticator, incluindo o que é, como ele funciona, e as informações que estão incluídas nesta secção do conteúdo.
services: active-directory
author: eross-msft
manager: daveba
ms.reviewer: sahenry
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.topic: overview
ms.date: 01/24/2019
ms.author: lizross
ms.openlocfilehash: 2379f1ff4fb4385015cc6077cb923cab998d1d11
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55175218"
---
# <a name="what-is-the-microsoft-authenticator-app"></a>O que é a aplicação Microsoft Authenticator?
A aplicação Microsoft Authenticator ajuda-o início de sessão para as suas contas se usar a verificação de dois fatores. Verificação de dois fatores ajuda-o a aceder às suas contas de forma mais segura, especialmente enquanto vê informações confidenciais. Uma vez que as palavras-passe podem ser esquecidas, roubado ou comprometido, dois fatores verificação é uma etapa adicional de segurança que ajuda a proteger a sua conta, tornando mais difícil para outras pessoas a entrar na.

Pode utilizar a aplicação Microsoft Authenticator de várias formas, incluindo:

- Fornecendo uma linha de comandos para um segundo método de verificação após entrar com seu nome de utilizador e palavra-passe.

- Fornecer início de sessão sem exigir uma palavra-passe, utilizando o seu nome de utilizador e dispositivo móvel com a impressão digital, face ou o PIN.

>[!Important]
>Este conteúdo destina-se aos utilizadores. Se for um administrador, pode encontrar mais informações sobre como configurar e gerir o ambiente do Azure Active Directory (Azure AD) na [Documentação do Azure Active Directory](https://docs.microsoft.com/azure/active-directory).<br><br>Se estiver a ter problemas ao iniciar sessão para a sua conta, veja [quando não pode iniciar sessão na sua conta Microsoft](https://support.microsoft.com/help/12429) para obter ajuda.  Obter mais informações sobre o que fazer quando receber o ["não existe essa conta Microsoft"](https://support.microsoft.com/help/13811) de mensagens ao tentar iniciar sessão na sua conta Microsoft.

## <a name="terminology"></a>Terminologia
|Termo|Descrição|
|----|-----------|
|Verificação de dois fatores |Um processo de verificação que requer a utilização especificamente apenas duas partes de informações de verificação, como uma palavra-passe e um PIN. A aplicação Microsoft Authenticator oferece suporte a verificação de dois fatores padrão e o início de sessão passe.|
|Multi-Factor Authentication (MFA)|Todos os verificação de dois fatores é autenticação multifator, que requerem que use *, pelo menos,* duas partes de informações de verificação, com base nos requisitos da sua organização.|
|Conta Microsoft (também denominada, MSA)|Criar suas próprias contas pessoais, para obter acesso aos seus produtos da Microsoft orientados ao consumidor e serviços em nuvem, como o Outlook, OneDrive, Xbox LIVE ou do Office 365. Sua conta Microsoft é criada e armazenada no sistema de contas de identidades de consumidor de Microsoft que é executado pela Microsoft.|
|Conta escolar ou profissional|Sua organização cria a sua conta escolar ou profissional (por exemplo, alain@contoso.com) para permitir que o acesso interno e recursos potencialmente restritos, como o Microsoft Azure, o Windows Intune e o Office 365.|

## <a name="how-two-factor-verification-works-with-the-app"></a>Como funciona a verificação de dois fatores com a aplicação
Dois fatorar verificação funciona com a aplicação Microsoft Authenticator das seguintes formas:

- **Notificação.** Digite seu nome de utilizador e palavra-passe para o dispositivo está fazendo logon em seu trabalho ou a conta escolar ou a sua conta Microsoft pessoal, e, em seguida, a aplicação Microsoft Authenticator envia uma notificação que lhe pede para **aprovar início de sessão**. Escolher **aprovar** se reconhecer a tentativa de início de sessão. Caso contrário, escolha **negar**. Se escolher **negar**, também é possível marcar a solicitação como fraudulentas.

- **Código de verificação.** Escreva o nome de utilizador e palavra-passe para o dispositivo está fazendo logon em seu trabalho ou a conta escolar ou a sua conta Microsoft pessoal e, em seguida, copie o código de verificação associados dos **contas** ecrã do Microsoft Aplicação de autenticador.

- **Passe início de sessão.** Digite seu nome de utilizador no dispositivo que fazemos logon no seu trabalho ou a conta escolar ou a sua conta Microsoft pessoal e, em seguida, utilize o seu dispositivo móvel para verificar que é o utilizador ao utilizar a sua impressão digital, face ou PIN. Para este método, não tem de introduzir a palavra-passe.

### <a name="whether-to-use-your-devices-biometric-capabilities"></a>Se pretende utilizar as capacidades de Biometria do seu dispositivo
Se utilizar um PIN para concluir o processo de autenticação, pode configurar a aplicação Microsoft Authenticator para utilizar em vez disso, identificação digital ou recursos de reconhecimento facial (biométrica) do seu dispositivo. Pode definir isso na primeira vez que utiliza a aplicação de autenticador para verificar a sua conta, ao selecionar a opção para utilizar as capacidades de Biometria do dispositivo como identificação em vez do PIN.

## <a name="who-decides-if-you-use-this-feature"></a>Quem decide se utilizar esta funcionalidade?
Dependendo do tipo de conta, a sua organização poderá decidir que tem de utilizar a verificação de dois fatores, ou poderá decidir sozinho.

- **Conta escolar ou profissional.** Se estiver a utilizar uma conta escolar ou profissional (por exemplo, alain@contoso.com), a escolha é sua organização se é necessário utilizar a verificação de dois fatores, juntamente com os métodos de verificação específica. Para obter mais informações sobre a validação multifator, consulte [o que o Azure multi-factor Authentication significa para mim](multi-factor-authentication-end-user.md). Para obter mais informações sobre como configurar as informações de segurança para utilizar a aplicação Microsoft Authentication, consulte [definir informações de segurança para utilizar uma aplicação de autenticação (pré-visualização)](security-info-setup-auth-app.md).

- **Conta Microsoft pessoal.** Pode optar por configurar a verificação de dois fatores para suas contas Microsoft pessoais (por exemplo, alain@outlook.com).

- **Conta pessoal não Microsoft.** Pode optar por configurar a verificação de dois fatores para suas contas pessoais não são da Microsoft (por exemplo, alain@gmail.com). As contas de não-Microsoft não poderão utilizar o termo, verificação de dois fatores, mas deve ser capaz de encontrar o recurso no **Security** ou o **início de sessão** definições.

## <a name="in-this-section"></a>Nesta secção

|Artigo |Descrição |
|------|------------|
|[Transferir e instalar a aplicação](microsoft-authenticator-app-how-to.md)|Descreve onde e como obter e instalar a aplicação Microsoft Authenticator para dispositivos Android, iOS e Windows Phone.|
|[Adicionar as suas contas escolares ou profissional](microsoft-authenticator-app-add-work-account.md)|Descreve como adicionar o seu trabalho ou escola e contas pessoais diversos para a aplicação Microsoft Authenticator.|
|[Adicionar as suas contas pessoais](microsoft-authenticator-app-add-personal-account.md)|Descreve como adicionar o seu Microsoft pessoa e contas de terceiros para a aplicação Microsoft Authenticator.|
|[Adicionar manualmente as suas contas](microsoft-authenticator-app-add-account-manual.md)|Descreve como adicionar manualmente as contas para a aplicação Microsoft Authenticator, se não for possível digitalizar o código de QR fornecido.|
|[Inicie sessão com a aplicação](microsoft-authenticator-app-phone-signin-faq.md)|Descreve como iniciar sessão nas suas várias contas, com a aplicação Microsoft Authenticator.|
|[Cópia de segurança e credenciais de recuperação da conta](microsoft-authenticator-app-backup-and-recovery.md)| Apresenta informações sobre como criar cópias de segurança e recuperar as suas credenciais da conta através da aplicação Microsoft Authenticator.|
|[FAQ da aplicação Microsoft Authenticator](microsoft-authenticator-app-faq.md)|Disponibiliza respostas a perguntas mais frequentes sobre a aplicação.|
