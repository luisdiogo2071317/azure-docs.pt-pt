

1. Inicie sessão na [consola Firebase](https://firebase.google.com/console/). Crie um novo projeto Firebase se ainda não tiver um.
2. Depois de criar o projeto, selecione **adicionar Firebase à sua aplicação Android**. Em seguida, siga as instruções fornecidas.

    ![Adicionar Firebase à sua aplicação Android](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-add-firebase-to-android-app.png)
3. Na consola Firebase, selecione na roda dentada do seu projeto. Em seguida, selecione **definições do projeto**.

    ![Selecione as definições do projeto](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-firebase-console-project-settings.png)
4. Selecione o **geral** separador nas definições do projeto. Em seguida, transferir o **google services.json** ficheiro que contém a chave de API do servidor e o ID de cliente.
5. Selecione o **Cloud Messaging** separador nas definições do projeto e, em seguida, copie o valor da **chave do servidor legado**. Utilize este valor para configurar a política de acesso do hub de notificação.
