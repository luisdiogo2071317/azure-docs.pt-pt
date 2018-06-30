---
title: Ajudar a aplicação de autenticação da Microsoft - do Azure AD | Microsoft Docs
description: Fornece uma lista de perguntas mais frequentes e respostas relacionadas com a aplicação Microsoft Authentication e o Azure multi-factor Authentication.
services: multi-factor-authentication
documentationcenter: ''
author: eross-msft
manager: mtillman
ms.assetid: f04d5bce-e99e-4f75-82d1-ef6369be3402
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/08/2018
ms.author: lizross
ms.reviewer: librown
ms.custom: end-user
ms.openlocfilehash: 9027b09c186dfb7661fc63f200f4d2e5da96a70a
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37130106"
---
# <a name="microsoft-authenticator-app-faq"></a>Aplicação de autenticação do Microsoft FAQ

Este artigo responde a questões recorrentes sobre a aplicação Authenticator da Microsoft. Se não vir uma resposta à sua pergunta, vá para o [fórum de aplicação do Microsoft Authenticator](https://social.technet.microsoft.com/Forums/en-US/home?forum=MicrosoftAuthenticatorApp). Além disso, pode rever outro FAQ sobre uma funcionalidade específica da aplicação, [iniciar sessão com o seu telefone FAQ](microsoft-authenticator-app-phone-signin-faq.md).

A aplicação Microsoft Authenticator substituído a aplicação Azure Authenticator, não sendo a aplicação recomendada quando utiliza o Azure multi-factor Authentication. A aplicação Authenticator da Microsoft está disponível para [Android](https://go.microsoft.com/fwlink/?linkid=866594), [iOS](https://go.microsoft.com/fwlink/?linkid=866594), e [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071).

## <a name="frequently-asked-questions"></a>Perguntas mais frequentes

### <a name="what-data-does-the-authenticator-store-on-my-behalf-and-how-can-i-delete-it"></a>Os dados que o autenticador armazenar no meu nome e como pode eliminar?

Microsoft Authenticator armazena as informações de conta que criou quando adicionar uma conta. Quando utilizar a autenticação, um registo de diagnóstico é criado para fins de depuração e armazena dados útil em ajudar a Microsoft a diagnosticar problemas imprevistos. Pode aceder os dados de registo, abrindo **ajudar** > **enviar registos de** > **ver registos**.

Pode eliminar os dados, eliminando o mosaico de conta. Também eliminar o mosaico de conta elimina todas as informações de conta a ser utilizadas pela aplicação, incluindo os registos. 

Para obter mais informações sobre como a Microsoft utiliza os dados, visite: https://servicetrust.microsoft.com/ViewPage/PrivacyGettingStarted

### <a name="what-are-the-codes-in-the-app-for-why-does-the-number-keep-counting-down"></a>Quais são os códigos de na aplicação para? Por que motivo é o número keep contando?

Quando abre a aplicação Authenticator da Microsoft, pode ver as contas que adicionou e um número de dígitos de seis ou oito por cada um deles. Poderá ver um temporizador de 30 segundo contando para baixo.

Estes códigos são utilizados quando iniciam sessão na sua conta. Depois de introduzir o nome de utilizador e palavra-passe, poderá ser-lhe pedido para introduzir um código de verificação. Abra a aplicação Microsoft Authenticator e copie o código que está atualmente a ser mostrada. Introduza esse código na página de início de sessão para concluir.

É o motivo que os códigos de alterar a cada 30 segundos para que nunca utilize o mesmo código duas vezes. Não é como uma palavra-passe que está a ser deveria lembrar-se. A ideia é apenas um utilizador com acesso para o seu telefone a conhecer o seu código de verificação.

Os códigos não necessitam de internet ou de dados, pelo que não precisa de preocupar com o serviço de telefone para iniciar sessão. Quando fechar a aplicação, não continuar em execução em segundo plano e não drenar da bateria. Pode fechar a aplicação e ignorá-lo até a próxima vez que iniciar sessão.  

### <a name="i-only-get-notifications-when-i-have-the-app-open-if-the-app-isnt-open-i-dont-get-any-notifications"></a>Apenas receber notificações tenho a aplicação aberta. Se a aplicação não estiver aberta, posso não obter quaisquer notificações.

Se receber notificações, mas não fazer ruído ou Vibre, apesar da alteração do toque na, primeiro verifique as definições de aplicação. Ative a aplicação utilizar som ou Vibre com o respetivas notificações.

Se não obtiver as notificações de todo, verifique os seguintes casos:

- É o seu telefone no modo silenciosos ou não efetue Disturb? Desse modo pode manter as aplicações de envio de notificações.
- Pode receber notificações de outras aplicações? Caso contrário, poderão existir um problema com ligações de rede no seu telemóvel ou o canal de notificações do Android ou Apple. Pode resolver a primeira opção nas suas definições de telefone, mas poderá ser necessário para que comunique com o fornecedor de serviços para obter ajuda com a segunda opção.
- Pode receber notificações para algumas contas na aplicação, mas não a outros utilizadores? Se Sim, remover a conta problemática da sua aplicação e adicione-a novamente para ativar as notificações push.

Se tiver tentou estas sugestões de resolução de problemas, mas ainda está a ter problemas, pode enviar os registos de diagnóstico. Aceda às definições de aplicação, em seguida, selecione **ajuda e comentários** e **enviar registos de**. Em seguida, vá para o [fórum de aplicação do Microsoft Authenticator](https://social.technet.microsoft.com/Forums/en-US/home?forum=MicrosoftAuthenticatorApp) e informe-no que problema está a ver e que medidas já tentou até ao momento.

### <a name="im-already-using-the-microsoft-authenticator-application-for-verification-codes-how-do-i-switch-to-one-click-push-notifications"></a>A aplicação Microsoft Authenticator já estiver a utilizar para códigos de verificação. Como mudar para enviar notificações push de um clique?
Aprovar um início de sessão através de notificação push só está disponível para contas Microsoft pessoais ou de trabalho contas escolares ou profissionais Microsoft, não para contas de terceiros como o Google ou o Facebook. Se tiver uma empresa ou escola conta Microsoft, a sua organização pode optar por desativar esta opção.

Se utilizar uma conta Microsoft para a sua conta pessoal e pretende mudar a ativação pós-falha para enviar notificações push, terá de adicionar a sua conta novamente. Volte a registar o dispositivo com a sua conta e configurar as notificações push.  

Se utilizar o Microsoft Authenticator para o seu trabalho ou escola conta, em seguida, organização decide se pretende permitir que as notificações de um clique.

### <a name="do-one-click-push-notifications-work-for-non-microsoft-accounts"></a>Notificações push de um clique funcionam para contas de terceiros?
Não, as notificações push funcionam apenas com contas Microsoft e contas do Azure Active Directory. Se a sua empresa ou escola utiliza contas do Azure AD, estes podem desativar esta funcionalidade.  

### <a name="i-got-a-new-device-or-restored-my-device-from-a-backup-how-do-i-set-up-my-accounts-in-the-microsoft-authenticator-app-again"></a>Posso obteve um novo dispositivo ou restaurado o meu dispositivo a partir de uma cópia de segurança. Como configuro a minha contas na aplicação Microsoft Authenticator novamente?
Se estiver a executar um dispositivo iOS, ter ativado **cópia de segurança automática**e criou uma cópia de segurança das suas contas no seu dispositivo antigo; pode utilizar essa cópia de segurança para recuperar as credenciais da conta no seu novo dispositivo. Para obter mais informações, consulte o [cópia de segurança e recuperar as credenciais da conta com a aplicação Microsoft Authenticator](../../../../multi-factor-authentication/end-user/microsoft-authenticator-app-backup-and-recovery.md) artigo. 

### <a name="i-lost-my-device-or-moved-on-to-a-new-device-how-do-i-make-sure-notifications-dont-continue-to-go-to-my-old-device"></a>Posso perder o meu dispositivo ou movidos para um novo dispositivo. Como torno se as notificações não continuam a aceder ao meu dispositivo antigo?  
Adicionar a aplicação Microsoft Authenticator ao seu novo dispositivo iOS não conseguiu remover automaticamente a aplicação do seu dispositivo antigo. Mesmo eliminar a aplicação do seu dispositivo antigo não é suficiente. Tem de eliminar a aplicação do seu dispositivo antigo e diga a sua organização ou Microsoft se esqueça de dispositivo antigo e o eliminar do registo da sua conta.
- **Para remover a aplicação a partir de um dispositivo utilizando uma conta Microsoft pessoal.** Vá para a área de verificação de dois passos do seu [segurança da conta](https://account.microsoft.com/security) página e optar por desativar a verificação para o seu dispositivo antigo.  
- **Ao remover a aplicação de um dispositivo utilizando um trabalho ou escola conta Microsoft.** Vá para a área de verificação de dois passos do seu [MyApps](https://myapps.microsoft.com/) página ou para o portal personalizado da sua organização e optar por desativar a verificação para o seu dispositivo antigo. 



### <a name="how-do-i-remove-an-account-from-the-app"></a>Como remover uma conta a partir da aplicação?
* iOS: a partir do ecrã principal, percorra a esquerda num mosaico de conta. Selecione **Eliminar**.
* Windows Phone: A partir do ecrã principal, selecione o botão do menu, em seguida, **edita contas**. Toque no **X** junto ao nome da conta.
* Android: A partir do ecrã principal, selecione o botão do menu, em seguida, **edita contas**. Toque no **X** junto ao nome da conta.

Se tiver um dispositivo que está registado com a sua organização, poderá ter de concluir um passo adicional para remover a sua conta. Nestes dispositivos, a aplicação Microsoft Authenticator é automaticamente registada como um administrador do dispositivo. Se pretender desinstalar completamente a aplicação, terá de pela primeira vez anular o registo da aplicação nas definições da aplicação.

### <a name="why-does-the-app-request-so-many-permissions"></a>Por que motivo é que a aplicação solicitar permissões de muitas?
Eis uma lista completa das permissões que pode ser-lhe pedido para, e como são utilizados na aplicação. As permissões específicas que vir dependem do tipo de telefone que tem.

* **Câmara**: utilizado para analisar códigos QR quando adicionar uma conta de terceiros, escolar ou profissional.
* **Contactos e phone**: utilizado para simplificar o processo ao localizar as contas existentes no seu telemóvel quando iniciar sessão com a sua conta Microsoft pessoal.
* **SMS**: utilizado para certificar-se de que o número de telefone corresponde ao número no registo. Quando inicia sessão com a sua conta Microsoft pessoal pela primeira vez.  Enviar-lhe uma mensagem de texto para o telefone onde transferiu a aplicação que inclui um código de verificação 8 de 6 dígitos. Em vez de pedir-lhe para localizar este código e introduza-o na aplicação, é possível encontrá-lo na mensagem de texto para si.
* **Desenhar através de outras aplicações**: A notificação depara-se de que verifica a sua identidade também é apresentada em todas as aplicações que podem estar em execução.
* **Receber dados através da internet**: Esta permissão é necessária para enviar notificações.
* **Impedir que o telefone suspensão**: se registar o seu dispositivo com a sua organização, a sua organização pode alterar esta política no seu telemóvel.
* **Controlar vibration**: pode escolher se pretende que um vibration sempre que recebe uma notificação para verificar a sua identidade.
* **Utilize hardware de identificação digital**: algumas contas profissional e escolar exigem um PIN adicional, sempre que confirme a sua identidade. PARA facilitar o processo, iremos permitem-lhe utilizar a sua impressão digital em vez de introduzir o PIN.
* **Ver as ligações de rede**: ao adicionar uma conta Microsoft, a aplicação necessita de ligação de rede/à internet.
* **Ler o conteúdo do seu armazenamento**: Esta permissão só é utilizada quando comunicar um problema técnico através de definições da aplicação. Algumas informações do seu armazenamento são recolhidas para diagnosticar o problema.
* **Acesso de rede total**: Esta permissão é necessária para enviar notificações para verificar a sua identidade.
* **São executados no arranque**: se reiniciar o seu telemóvel, esta permissão garante que continuam a receber notificações para verificar a sua identidade.

### <a name="why-does-the-microsoft-authenticator-app-allow-you-to-approve-a-request-without-unlocking-the-device"></a>Por que razão a aplicação de autenticação da Microsoft permite-lhe aprovar um pedido sem desbloquear o dispositivo?

Não tem de desbloquear o dispositivo para aprovar pedidos de verificação porque só terá de provar que tem o seu telemóvel consigo. Verificação de dois passos requer comprovar duas coisas – uma coisa que conhece e uma coisa que precisa. A coisa que sabe que é a palavra-passe. Coisa que precisa é o seu telemóvel (configurar com a aplicação Microsoft Authenticator e registado como uma prova de MFA). Por conseguinte, tendo o telefone e aprovar o pedido satisfaz os critérios de segundo fator de autenticação.

### <a name="what-does-the-lock-icon-in-the-account-list-mean"></a>O que significa o ícone de cadeado na lista de conta?

O ícone de cadeado indica que o dispositivo é registado no Azure AD e registado para a conta. Registo de dispositivos para iOS ocorre durante a inscrição no Microsoft Intune.

## <a name="next-steps"></a>Passos Seguintes

### <a name="contact-us"></a>Contacte-nos
Se a sua pergunta não foi respondida aqui, Queremos ouvi do utilizador. Vá para o [fórum de aplicação do Microsoft Authenticator](https://social.technet.microsoft.com/Forums/en-US/home?forum=MicrosoftAuthenticatorApp) após a sua pergunta e obter ajuda da Comunidade ou deixe um comentário nesta página.


### <a name="related-topics"></a>Tópicos relacionados
* [Sobre a verificação de dois passos](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification) para contas Microsoft
* [Problemas com verificação de dois passos](multi-factor-authentication-end-user-troubleshoot.md) para a sua conta escolar ou profissional?
* [Utilizar o Microsoft Authenticator para iniciar sessão a partir do seu telemóvel](microsoft-authenticator-app-phone-signin-faq.md)
