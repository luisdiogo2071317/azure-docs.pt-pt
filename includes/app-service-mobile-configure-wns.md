
1. No [portal do Azure](https://portal.azure.com/), selecione **Procurar tudo** > **serviços aplicacionais**. Em seguida, selecione as suas aplicações móveis back-end. Em **definições**, selecione **Push do serviço de aplicações**. Em seguida, selecione o nome de hub de notificação.
2. Aceda a **Windows (WNS)**. Em seguida, introduza o **a chave de segurança** (segredo do cliente) e **SID do pacote** que obtido a partir do site dos Serviços Live. Em seguida, selecione **guardar**.

    ![Defina a chave do WNS no portal](./media/app-service-mobile-configure-wns/mobile-push-wns-credentials.png)

O back-end está agora configurado para utilizar o WNS para enviar notificações push.
