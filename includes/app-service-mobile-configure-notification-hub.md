A funcionalidade Mobile Apps do App Service do Azure utiliza [Notification Hubs do Azure] enviar pushes, pelo que irá configurar um hub de notificação para a sua aplicação móvel.

1. No [portal do Azure], aceda a **serviços aplicacionais**e, em seguida, selecione a aplicação de back-end. Em **definições**, selecione **Push**.
2. Para adicionar um recurso de hub de notificação para a aplicação, selecione **Connect**. Pode criar um hub ou ligar a um existente.

    ![Configurar um hub](./media/app-service-mobile-create-notification-hub/configure-hub-flow.png)

Agora ligou-se um hub de notificação para o projeto de back-end das Mobile Apps. Mais tarde configurar este notification hub para se ligar a um sistema de notificação de plataforma (PNS) para fazer o push para dispositivos.

[portal do Azure]: https://portal.azure.com/
[Notification Hubs do Azure]: https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-push-notification-overview/
