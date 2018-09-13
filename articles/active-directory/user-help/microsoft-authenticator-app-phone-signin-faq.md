---
title: Microsoft Authenticator sessão no telemóvel - contas do Azure e o Microsoft | Documentos da Microsoft
description: Utilize o seu telefone para iniciar sessão na sua conta Microsoft em vez de digitar a palavra-passe. Este artigo responde a FAQs sobre esta funcionalidade.
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directoary
ms.workload: identity
ms.component: user-help
ms.topic: conceptual
ms.date: 08/12/2017
ms.author: lizross
ms.reviewer: librown
ms.openlocfilehash: 3536c5da7833c32b583f1a510be43864c9107068
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/12/2018
ms.locfileid: "44717051"
---
# <a name="sign-in-with-your-phone-not-your-password"></a>Inicie sessão com o telefone, não com a palavra-passe.
A aplicação Microsoft Authenticator ajuda-o a proteger as suas contas ao realizar a verificação de dois passos depois de introduzir a palavra-passe. Mas sabia que pode substituir completamente a palavra-passe da sua conta Microsoft pessoal?

Esta funcionalidade está disponível em dispositivos iOS e Android e funciona com contas pessoais da Microsoft.
 
## <a name="how-it-works"></a>Como funciona
Muitos de vocês utilizarem a aplicação Microsoft Authenticator para verificação de dois passos quando iniciam sessão na sua conta Microsoft. Escreva a palavra-passe, em seguida, vá para a aplicação para aprovar uma notificação ou obter um código de verificação. Com o início de sessão no telemóvel, ignorar a palavra-passe e fazer tudo a verificação de identidade no seu telemóvel. Como o início de sessão no telefone é um tipo de verificação de dois passos, ainda assim será preciso fornecer algo que conhece e uma coisa que precisa verificar a sua identidade. O telefone ainda é a coisa que tem e do seu telemóvel PIN ou biométrico chave é a coisa de que sabe.

## <a name="how-to-get-started"></a>Como começar
Para iniciar sessão sua conta Microsoft pessoal com o seu telemóvel, siga estes passos:

1. Ative o início de sessão no telefone para a sua conta.

    - Se não tiver a aplicação Microsoft Authenticator, ainda, instalar e adicionar a sua conta Microsoft pessoal, de acordo com os passos sobre a [página do Microsoft Authenticator](microsoft-authenticator-app-how-to.md). As contas recentemente adicionadas são ativadas automaticamente, para que está pronto para começar.

    - Se já utiliza o Microsoft Authenticator para verificação de dois passos, selecione a conta a partir da home page da aplicação e selecione **ativar o início de sessão no telefone** no menu pendente.

    >[!NOTE]
    >Para proteger a sua conta, é necessário um PIN ou biométrico bloqueio no seu dispositivo. Se mantiver o seu telemóvel desbloqueado, a aplicação aparece um pedido que lhe pede para configurar um bloqueio antes de ativar o início de sessão no telefone.

2. A maioria das páginas onde normalmente deve introduzir a palavra-passe de conta do Microsoft têm uma ligação que diz **em alternativa, utilize uma aplicação**. Selecione esta ligação para iniciar sessão com o seu telemóvel.
 
3. A Microsoft envia uma notificação para o seu telefone. Aprove a notificação para iniciar sessão na sua conta.   
 
## <a name="faq"></a>FAQ

### <a name="how-is-signing-in-with-my-phone-more-secure-than-typing-a-password"></a>Como é iniciar sessão com o meu telefone mais segura do que escrever uma palavra-passe?  
Hoje em dia a maioria das pessoas sessão para web sites ou aplicações com um nome de utilizador e palavra-passe.  Infelizmente, as palavras-passe são, muitas vezes, perdidas, roubadas ou que adivinhado por hackers. Quando configurou a aplicação Microsoft Authenticator para iniciar sessão, geramos uma chave no seu telemóvel que pode desbloquear a sua conta. Podemos proteger esta chave com o PIN ou biométrico que já utiliza no seu telemóvel.  Quando inicia sessão com o seu telemóvel, esta chave é utilizada para provar a sua identidade com segurança com dois fatores – o próprio telefone e a capacidade para o desbloquear.
 
A chave usada é semelhante às chaves utilizadas em Hello do Windows e as especificações de FIDO Alliance UAF. Sua biografia dados só são utilizado para proteger a chave localmente e é nunca enviada para ou armazenada na cloud. 
 
### <a name="where-can-i-use-my-phone-to-replace-my-password-and-where-would-i-still-need-the-password"></a>Onde posso usar meu telefone para substituir a minha palavra-passe e, em que eu ainda precisava a palavra-passe?  
Hoje em dia, a funcionalidade de início de sessão no telefone só funciona com aplicações web e serviços que têm a tecnologia contas Microsoft pessoais, iOS ou aplicações Android que utilizam uma conta Microsoft pessoal e aplicações no Windows 10 que utilizam uma conta Microsoft pessoal. Quando iniciar sessão a uma destes web sites ou aplicações, na página em que, normalmente, introduza a palavra-passe existe uma ligação que diz **em alternativa, utilize uma aplicação**. 

Início de sessão no telefone não pode ser utilizado para desbloquear um PC Windows, XBOX ou as versões de área de trabalho de aplicações da Microsoft, como as aplicações do Office neste momento.
 
### <a name="does-this-replace-two-step-verification-should-i-turn-it-off"></a>Isto substitui verificação em dois passos? Deve desativá-lo?   
Às vezes. Estamos a trabalhar em expandir o âmbito do início de sessão no telefone, mas por enquanto ainda existem lugares no ecossistema da Microsoft que não o suportam. Em desses lugares, ainda estamos usando a verificação de dois passos para o início de sessão seguro. Por esse motivo, não, não deve desativar a verificação de dois passos para a sua conta.
 
### <a name="okay-if-i-keep-two-step-verification-turned-on-for-my-account-do-i-have-to-approve-two-notifications"></a>Tudo bem, se eu manter a verificação de dois passos ativada para a minha conta, é necessário que aprovar dois notificações?
Não, não. Iniciar sessão na sua conta Microsoft com o seu telemóvel equivale a verificação de dois passos. Em vez de digitar a palavra-passe, em seguida, aprovar uma notificação de provar a sua identidade a saber como desbloquear o seu telemóvel e, em seguida, aprovar uma notificação. Não a enviar uma notificação de segundo para aprovar.

### <a name="what-if-i-lose-my-phone-or-dont-have-it-with-me-how-can-i-access-my-account"></a>E se eu perder o meu telefone ou não o tiver comigo, como posso acessar minha conta?  
Pode sempre clicar **em alternativa, utilize uma palavra-passe** na página de início de sessão para mudar para utilizar a sua palavra-passe. Tenha em atenção que se usar a verificação de dois passos, ainda tem um segundo método para verificar seu início de sessão. É por isso que recomendamos vivamente que certifique-se de que tem informações de segurança extra e atualizado na sua conta. Pode gerenciar suas informações de segurança em https://account.live.com/proofs/manage.
 
### <a name="how-do-i-stop-using-this-feature-and-go-back-to-entering-my-password"></a>Como parar de utilizar esta funcionalidade e volte a introduzir a palavra-passe?
Clique em **em alternativa, utilize uma palavra-passe** quando inicia sessão no. Podemos lembrar-se à sua escolha mais recente e oferecem que, por predefinição, a próxima vez que iniciar sessão. Se alguma vez pretender voltar a iniciar sessão com o seu telemóvel, clique em **em alternativa, utilize uma aplicação**. 
 
### <a name="can-i-use-the-app-to-sign-in-to-all-my-accounts-with-microsoft"></a>Pode utilizar a aplicação para iniciar sessão em todas as minhas contas com a Microsoft?   
Esta funcionalidade só está disponível para contas Microsoft pessoais neste momento. 
 
### <a name="can-i-sign-into-my-pc-with-my-phone"></a>Pode entrar no meu PC com o meu telefone?  
Para o seu PC, recomendamos que iniciar sessão com o Windows Hello no Windows 10 com o rosto, impressão digital ou um PIN.   
 
### <a name="can-i-sign-in-with-my-windows-phone"></a>Posso iniciar sessão com o meu telemóvel Windows?  
Neste momento, não podemos são desenvolvimento desta funcionalidade para o Microsoft Authenticator no Windows Phone. 

## <a name="next-steps"></a>Passos Seguintes
Se ainda não transferiu a aplicação Microsoft Authenticator, confira. A aplicação está disponível para [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), e início de sessão no telefone está disponível na aplicação Microsoft Authenticator para [Android](http://go.microsoft.com/fwlink/?Linkid=825072) e [iOS](http://go.microsoft.com/fwlink/?Linkid=825073).

Se tiver dúvidas sobre a aplicação em geral, dê uma olhada a [FAQs do Microsoft Authenticator](microsoft-authenticator-app-faq.md)