

1. Inicie sessão na [consola Firebase](https://firebase.google.com/console/). Crie um novo projeto Firebase se ainda não tiver um.
2. Depois de criar o projeto, selecione **Adicionar Firebase à aplicação Android**. Em seguida, siga as instruções fornecidas.

    ![Adicionar Firebase à aplicação Android](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-add-firebase-to-android-app.png)
3. Na consola da Firebase, selecione o cog do seu projeto. Em seguida, selecione **Definições do projeto**.

    ![Selecione Definições do projeto](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-firebase-console-project-settings.png)
4. Selecione o separador **Geral** nas definições do projeto. Em seguida, transfira o ficheiro **google-services.json** que contém a chave de API do Servidor e o ID de Cliente.
5. Selecione o separador **Cloud Messaging** nas definições do projeto e, em seguida, copie o valor da **Chave do servidor do legado**. Utilize este valor para configurar a política de acesso do hub de notificação.
