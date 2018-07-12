
1. Na [portal do Azure](https://portal.azure.com/), selecione **Procurar tudo** > **serviços aplicacionais**. Em seguida, selecione as suas aplicações de Mobile back-end. Sob **configurações**, selecione **Push do serviço de aplicações**. Em seguida, selecione o nome do hub de notificação.
2. Aceda a **Windows (WNS)**. Em seguida, introduza o **chave de segurança** (segredo do cliente) e **SID do pacote** que obteve a partir do site dos Serviços Live. Em seguida, selecione **guardar**.

    ![Defina a chave WNS no portal](./media/app-service-mobile-configure-wns/mobile-push-wns-credentials.png)

O back-end está agora configurado para utilizar o WNS para enviar notificações push.
