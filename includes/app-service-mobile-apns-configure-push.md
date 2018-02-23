

1. No Mac, inicie **acesso Keychain**. Na barra de navegação esquerdo, em **categoria**, abra **certificados My**. Localizar o certificado SSL que transferiu na secção anterior e, em seguida, informar o respetivo conteúdo. Selecione apenas o certificado (não selecione a chave privada). Em seguida, [exportá-lo](https://support.apple.com/kb/PH20122?locale=en_US).
2. No [portal do Azure](https://portal.azure.com/), selecione **Procurar tudo** > **serviços aplicacionais**. Em seguida, selecione as suas aplicações móveis back-end. 
3. Em **definições**, selecione **Push do serviço de aplicações**. Em seguida, selecione o nome de hub de notificação. 
3. Aceda a **notificações push da Apple** > **carregar certificado**. Carregue o ficheiro. p12, selecionando o correto **modo** (dependendo se o certificado de cliente SSL, do anteriormente é produção ou sandbox). Guarde as alterações.

O serviço está agora configurado para trabalhar com as notificações push no iOS.

[1]: ./media/app-service-mobile-apns-configure-push/mobile-push-notification-hub.png
