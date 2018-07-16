---
title: Ajuda com a aplicação Microsoft Authenticator - Azure AD | Documentos da Microsoft
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
ms.openlocfilehash: d86bc84653e38a9b64a336b8ce9ed7e657129e8c
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39059831"
---
# <a name="microsoft-authenticator-app-faq"></a>Aplicação Microsoft Authenticator FAQ

Este artigo responde a perguntas comuns sobre a aplicação Microsoft Authenticator. Se não vir uma resposta à sua pergunta, vá para o [fórum de aplicação Microsoft Authenticator](https://social.technet.microsoft.com/Forums/en-US/home?forum=MicrosoftAuthenticatorApp). Além disso, outro FAQ sobre um recurso específico na aplicação, pode rever [iniciar sessão com o seu telemóvel FAQ](microsoft-authenticator-app-phone-signin-faq.md).

A aplicação Microsoft Authenticator substituído a aplicação Azure Authenticator e é a aplicação recomendada quando utiliza o Azure multi-factor Authentication. A aplicação Microsoft Authenticator está disponível para [Android](https://go.microsoft.com/fwlink/?linkid=866594), [iOS](https://go.microsoft.com/fwlink/?linkid=866594) e [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071).

## <a name="frequently-asked-questions"></a>Perguntas mais frequentes

### <a name="what-data-does-the-authenticator-store-on-my-behalf-and-how-can-i-delete-it"></a>Os dados que o autenticador armazena em meu nome e como pode eliminar?

Microsoft Authenticator armazena as informações de conta que criar quando adicionar uma conta. Quando utiliza o Authenticator, um registo de diagnóstico é criado para fins de depuração e armazena dados úteis nos ajudar a Microsoft a diagnosticar possíveis imprevistos. Pode acessar os dados de registo, abrindo **ajudar** > **enviar registos** > **ver registos**.

Pode eliminar os dados ao eliminar o mosaico de conta. Eliminar o mosaico de conta também elimina a todas as informações de conta a ser utilizadas pela aplicação, incluindo os registos. 

Para obter mais informações sobre como a Microsoft usa os dados, visite: https://servicetrust.microsoft.com/ViewPage/PrivacyGettingStarted

### <a name="what-are-the-codes-in-the-app-for-why-does-the-number-keep-counting-down"></a>Quais são os códigos na aplicação para? Por que motivo é que o número keep contagem para baixo?

Quando abre a aplicação Microsoft Authenticator, pode ver as contas que adicionou e um número de seis ou oito dígitos por cada um deles. Poderá ver um temporizador de 30 segundos para baixo de contagem.

Esses códigos são usados ao iniciar sessão sua conta. Depois de introduzir o seu nome de utilizador e palavra-passe, poderá ser-lhe pedido para introduzir um código de verificação. Abra a aplicação Microsoft Authenticator e copie o código que está atualmente a aparecer. Introduza o código na página de início de sessão para concluir.

A razão pela qual os códigos de alterar a cada 30 segundos é para que nunca usar o mesmo código duas vezes. Não é como uma palavra-passe que deverá se lembrar. A idéia é que somente alguém com acesso para o seu telefone sabe seu código de verificação.

Os códigos não necessitam de internet ou dados, para que não tenha de se preocupar com o serviço de telefone para iniciar sessão. Quando fecha a aplicação, não manter em execução em segundo plano e ele não drena a bateria. Pode fechar a aplicação e ignorá-lo até à próxima vez que iniciar sessão.  

### <a name="i-only-get-notifications-when-i-have-the-app-open-if-the-app-isnt-open-i-dont-get-any-notifications"></a>Eu apenas receber notificações quando tiver a aplicação abrir. Se a aplicação não estiver aberta, não tenha quaisquer notificações.

Se receber notificações, mas eles não fazem nenhum barulho ou Vibrar apesar de sua alteração do toque a ser no, primeiro verifique as definições de aplicação. Ative a aplicação para utilizar som ou Vibrar com suas notificações.

Se não receber notificações de todo, verifique os seguintes casos:

- É o seu telemóvel no modo silencioso ou de não incomodar? Esse modo pode manter as aplicações de envio de notificações.
- Pode receber notificações de outras aplicações? Caso contrário, poderá existir um problema com as ligações de rede no seu telefone ou o canal de notificações do Android ou Apple. Pode solucionar a primeira opção nas definições do telefone, mas poderá ter de se comunicar com o seu fornecedor de serviços para obter ajuda com a segunda opção.
- Pode receber notificações para algumas contas na aplicação, mas não para outros? Se Sim, remover a conta de problemática da sua aplicação e adicione-o novamente para ativar as notificações push.

Se tentasse estas sugestões de resolução de problemas, mas ainda estiver a ter problemas, pode enviar os registos de diagnóstico. Aceda às definições da aplicação, em seguida, selecione **ajuda e feedback** e **enviar registos**. Em seguida, vá para o [fórum de aplicação Microsoft Authenticator](https://social.technet.microsoft.com/Forums/en-US/home?forum=MicrosoftAuthenticatorApp) e informe-no que o problema que está a ver e que medidas já tentou até agora.

### <a name="im-already-using-the-microsoft-authenticator-application-for-verification-codes-how-do-i-switch-to-one-click-push-notifications"></a>Já estou usando a aplicação Microsoft Authenticator para códigos de verificação. Como posso mudar para enviar notificações push num único clique
Aprovar um início de sessão por meio de notificação push só está disponível para contas Microsoft pessoais ou profissionais e escolares contas Microsoft, não para contas de terceiros, como o Google ou Facebook. Se tiver uma empresa ou escola conta Microsoft, a sua organização pode optar por desativar esta opção.

Se utilizar uma conta Microsoft para a sua conta pessoal e quer mudar para notificações push, terá de adicionar a conta novamente. Voltar a registar o dispositivo com a sua conta e configure as notificações push.  

Se utilizar o Microsoft Authenticator para o seu trabalho ou escolar, sua organização decidir se pretende permitir que as notificações de um clique.

### <a name="do-one-click-push-notifications-work-for-non-microsoft-accounts"></a>Notificações push de um clique funcionam para contas de terceiros?
Não, notificações push só funcionam com contas Microsoft e contas do Azure Active Directory. Se a sua empresa ou escola utiliza contas do Azure AD, eles podem desativar esta funcionalidade.  

### <a name="i-got-a-new-device-or-restored-my-device-from-a-backup-how-do-i-set-up-my-accounts-in-the-microsoft-authenticator-app-again"></a>Eu tenho um novo dispositivo ou restaurado o meu dispositivo a partir de uma cópia de segurança. Como posso configurar minhas contas na aplicação Microsoft Authenticator novamente?
Se estiver executando um dispositivo iOS, ter ativado **cópia de segurança automática**e criou uma cópia de segurança das suas contas no seu dispositivo antigo; pode usar essa cópia de segurança para recuperar as credenciais da conta no seu dispositivo novo. Para obter mais informações, consulte a [cópia de segurança e recuperar credenciais de conta com a aplicação Microsoft Authenticator](microsoft-authenticator-app-backup-and-recovery.md) artigo. 

### <a name="i-lost-my-device-or-moved-on-to-a-new-device-how-do-i-make-sure-notifications-dont-continue-to-go-to-my-old-device"></a>Eu perder meu dispositivo ou movido para um novo dispositivo. Como me certifico de que não continuam para notificações para ir para o meu dispositivo antigo?  
Adicionar a aplicação Microsoft Authenticator para seu novo dispositivo iOS não remove automaticamente a aplicação do seu dispositivo antigo. Até mesmo eliminar a aplicação do seu dispositivo antigo não é suficiente. Tem de eliminar a aplicação a partir do seu dispositivo antigo e dizer ao Microsoft ou sua organização para se esqueça do dispositivo antigo e cancelar seu registro da sua conta.
- **Para remover a aplicação a partir de um dispositivo com uma conta Microsoft pessoal.** Aceda à área de verificação de dois passos da sua [segurança de conta](https://account.microsoft.com/security) página e optar por desativar a verificação para o seu dispositivo antigo.  
- **Para remover a aplicação a partir de um dispositivo através de uma conta escolar ou profissional Microsoft.** Aceda à área de verificação de dois passos da sua [MyApps](https://myapps.microsoft.com/) página ou para o portal personalizado da sua organização e optar por desativar a verificação para o seu dispositivo antigo. 



### <a name="how-do-i-remove-an-account-from-the-app"></a>Como remover uma conta a partir da aplicação?
* iOS: no ecrã principal, percorra para a esquerda num mosaico de conta. Selecione **Eliminar**.
* Windows Phone: A partir do ecrã principal, selecione o botão de menu, em seguida, **editar contas**. Toque no **X** junto ao nome da conta.
* Android: A partir do ecrã principal, selecione o botão de menu, em seguida, **editar contas**. Toque no **X** junto ao nome da conta.

Se tiver um dispositivo que está registado com a sua organização, terá de concluir uma etapa extra para remover a sua conta. Nestes dispositivos, a aplicação Microsoft Authenticator é automaticamente registada como um administrador do dispositivo. Se pretender desinstalar completamente o aplicativo, precisa primeiro anular o registo da aplicação nas definições da aplicação.

### <a name="why-does-the-app-request-so-many-permissions"></a>Por que a aplicação pedir tantas permissões?
Aqui está uma lista completa das permissões que pode ser-lhe pedido para, e como elas são usadas na aplicação. As permissões específicas que vir dependem do tipo de telefone que tem.

* **Câmara**: utilizado para ler códigos QR quando adiciona um trabalho, uma instituição de ensino ou uma conta não Microsoft.
* **Contatos e phone**: utilizado para simplificar o processo ao detetar contas existentes no seu telemóvel quando inicia sessão com a sua conta Microsoft pessoal.
* **SMS**: utilizado para se certificar de que o seu número de telefone corresponde ao número no registo. Quando inicia sessão com a sua conta Microsoft pessoal pela primeira vez.  Enviamos uma mensagem de texto para o telefone onde transferiu a aplicação que inclui um código de verificação de um dígito de 6 a 8. Em vez de solicitar que encontre esse código e introduza-o na aplicação, ele se baseia na mensagem de texto para.
* **Desenhar sobre outras aplicações**: A notificação obtém que verifica a sua identidade também é apresentada nas outras aplicações que podem estar em execução.
* **Receber dados a partir da internet**: Esta permissão é necessária para enviar notificações.
* **Impedir que o telefone em suspensão**: se registar o seu dispositivo com a sua organização, a sua organização pode alterar esta política no seu telemóvel.
* **Controlar vibração**: pode escolher se pretende uma vibração sempre que receber uma notificação para verificar a sua identidade.
* **Utilizar hardware de impressão digital**: algumas contas profissionais e escolares exigem um PIN adicional, sempre que verifique a sua identidade. PARA facilitar o processo, iremos permitem-lhe utilizar a impressão digital em vez de introduzir o PIN.
* **Ver ligações de rede**: ao adicionar uma conta Microsoft, a aplicação requer a ligação de rede/internet.
* **Ler o conteúdo do seu armazenamento**: Esta permissão só é utilizada quando reportar um problema técnico através das definições de aplicação. Algumas informações do seu armazenamento são recolhidas para diagnosticar o problema.
* **Acesso de rede total**: Esta permissão é necessária para enviar notificações para verificar a sua identidade.
* **São executados no arranque**: se reiniciar o seu telemóvel, esta permissão garante que continue recebe as notificações para verificar a sua identidade.

### <a name="why-does-the-microsoft-authenticator-app-allow-you-to-approve-a-request-without-unlocking-the-device"></a>Por que a aplicação Microsoft Authenticator permite-lhe aprovar um pedido sem desbloquear o dispositivo?

Não tem de desbloquear o dispositivo para aprovar pedidos de verificação porque só tem de provar que tem o seu telemóvel consigo. Verificação de dois passos requer provando duas coisas – uma coisa que sabe e uma coisa que precisa. O que sabe, é a palavra-passe. A coisa que tem é o seu telemóvel (configurar com a aplicação Microsoft Authenticator e registado como uma prova de MFA.) Portanto, ter o telefone e aprovar o pedido satisfaz os critérios de segundo fator de autenticação.

### <a name="what-does-the-lock-icon-in-the-account-list-mean"></a>O que significa o ícone de cadeado na lista conta?

O ícone de cadeado indica que o dispositivo é registado no Azure AD e registrado para a conta. Registo de dispositivos para iOS ocorre durante a inscrição do Microsoft Intune.

## <a name="next-steps"></a>Passos Seguintes

### <a name="contact-us"></a>Contacte-nos
Se a sua pergunta não foi respondida aqui, queremos a sua opinião. Vá para o [fórum de aplicação Microsoft Authenticator](https://social.technet.microsoft.com/Forums/en-US/home?forum=MicrosoftAuthenticatorApp) publique sua pergunta e obtenha ajuda da Comunidade ou deixe um comentário nesta página.


### <a name="related-topics"></a>Tópicos relacionados
* [Sobre a verificação de dois passos](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification) para contas Microsoft
* [A ter problemas com verificação de dois passos](multi-factor-authentication-end-user-troubleshoot.md) para a sua conta escolar ou profissional?
* [Utilizar o Microsoft Authenticator para iniciar sessão, a partir do telemóvel](microsoft-authenticator-app-phone-signin-faq.md)
