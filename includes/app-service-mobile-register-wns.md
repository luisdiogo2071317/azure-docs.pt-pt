
1. No Explorador de soluções do Visual Studio, clique no projeto de aplicação loja Windows. Em seguida, selecione **arquivo** > **associar aplicação à loja**.

    ![Associar aplicação à loja Windows](./media/app-service-mobile-register-wns/notification-hub-associate-win8-app.png)
2. No assistente, selecione **seguinte**. Em seguida, inicie sessão com a sua conta Microsoft. No **reservar um novo nome de aplicação**, escreva um nome para a sua aplicação e, em seguida, selecione **reserva**.
3. Depois do registo de aplicação é criado com êxito, selecione o novo nome de aplicação. Selecione **seguinte**e, em seguida, selecione **associar**. Este processo adiciona as informações de registo da loja Windows necessárias para o manifesto da aplicação.
4. Repita os passos 1 e 3 para o projeto de aplicação da loja Windows Phone utilizando o mesmo registo que criou anteriormente para a aplicação da loja Windows.  
5. Vá para o [Windows Dev Center](https://dev.windows.com/en-us/overview)e, em seguida, inicie sessão com a sua conta Microsoft. No **as minhas aplicações**, selecione o novo registo de aplicação. Em seguida, expanda **serviços** > **notificações Push**.
6. No **notificações Push** página **Push notificação serviços Windows (WNS) e as Mobile Apps do Microsoft Azure**, selecione **site dos Serviços Live**.  Tome nota dos valores do **SID do pacote** e *atual* valor **segredo da aplicação**. 

    ![Definição de aplicação no Centro de programadores](./media/app-service-mobile-register-wns/mobile-services-win8-app-push-auth.png)

   > [!IMPORTANT]
   > O segredo da aplicação e o SID do pacote são credenciais de segurança importantes. Não partilhe estes valores com ninguém e distribuí-los com a sua aplicação.
   >
   >
