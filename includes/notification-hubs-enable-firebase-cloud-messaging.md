

1. Inicie sessão na [consola Firebase](https://firebase.google.com/console/). Crie um novo projeto Firebase se ainda não tiver um.
2. Depois de o projeto ser criado, clique em **Adicionar Firebase à sua aplicação Android** e siga as instruções fornecidas.

    ![](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-add-firebase-to-android-app.png)
3. Na consola Firebase, clique na roda dentada do seu projeto e, em seguida, clique em **Definições do Projeto**.

    ![](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-firebase-console-project-settings.png)
4. Clique em de **geral** separador nas definições do projeto e, em seguida, transferir o **google services.json** ficheiro que contém a chave de API do servidor e o ID de cliente.
5. Clique em de **Cloud Messaging** separador nas definições do projeto e copie o valor do **chave do servidor legado**. Este valor será utilizado para configurar a política de acesso do hub de notificação.
