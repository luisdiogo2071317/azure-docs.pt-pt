1. Inicie sessão no [portal do Azure].
2. Selecione **+ novo** > **Web + móvel** > **aplicação móvel**e, em seguida, forneça um nome para o seu Mobile Apps back-end.
3. Para **grupo de recursos**, selecione um grupo de recursos existente ou crie um novo (utilizando o mesmo nome que a sua aplicação). 
4. Para **plano do App Service**, o plano predefinido (no [escalão Standard](https://azure.microsoft.com/pricing/details/app-service/)) está selecionado. Também pode selecionar um plano diferente ou [criar um novo](../articles/app-service/app-service-plan-manage.md#create-an-app-service-plan). 

   Definições do plano de serviço de aplicações determinam o [localização, funcionalidades, custos e recursos de computação](https://azure.microsoft.com/pricing/details/app-service/) associados à aplicação. Para obter mais informações sobre o App Service planos e como criar um novo plano num preço diferente camada e na localização pretendida, consulte [descrição geral dos planos do App Service do Azure](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
   
5. Selecione **Criar**. Este passo cria o back-end do Mobile Apps. 
6. No **definições** painel para as aplicações móveis novo back-end, selecione **início rápido** > sua plataforma de aplicação de cliente > **ligar uma base de dados**. 
   
   ![Seleções para ligar uma base de dados](./media/app-service-mobile-dotnet-backend-create-new-service/dotnet-backend-create-data-connection.png)
7. No **adicionar ligação de dados** painel, selecione **base de dados SQL** > **criar uma nova base de dados**. Introduza o nome de base de dados, escolha um escalão de preço e, em seguida, selecione **servidor**. Pode reutilizar esta nova base de dados. Se já tiver uma base de dados na mesma localização, pode escolher **Utilizar uma base de dados existente**. Não recomendamos a utilização de uma base de dados numa localização diferente, devido a custos de largura de banda e latência superior.
   
   ![Selecionar uma base de dados](./media/app-service-mobile-dotnet-backend-create-new-service/dotnet-backend-create-db.png)
8. No **novo servidor** painel, introduza um nome de servidor exclusivo no **nome do servidor** caixa, forneça um início de sessão e palavra-passe, selecione **serviços do Azure permite ao servidor de acesso**e selecione  **OK**. Este passo cria uma nova base de dados.
9. Volta a **adicionar ligação de dados** painel, selecione **cadeia de ligação**, introduza os valores de início de sessão e palavra-passe para a base de dados e selecione **OK**. 

   Aguarde alguns minutos para que a base de dados ser implementada com êxito antes de continuar.

<!-- URLs. -->
[portal do Azure]: https://portal.azure.com/
