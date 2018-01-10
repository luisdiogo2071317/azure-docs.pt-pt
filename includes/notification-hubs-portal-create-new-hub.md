

1. Inicie sessão no [portal do Azure](https://portal.azure.com).

2. Selecione **Novo** > **Web + Móvel** > **Hub de Notificação**.
   
      ![Portal do Azure – criar hubs de notificação](./media/notification-hubs-portal-create-new-hub/notification-hubs-azure-portal-create.png)
      
3. Na caixa **Hub de Notificação**, escreva um nome exclusivo. Selecione a **Região**, a **Subscrição** e o **Grupo de Recursos** (se já tiver um). 
   
      Se ainda não tiver um espaço de nomes do service bus, pode utilizar o nome predefinido, que é criado tendo por base o nome do hub (se esse nome do espaço de nomes estiver disponível).
    
      Se já tiver um espaço de nomes do service bus no qual quer criar o hub, siga estes passos

    a. Na área **Espaço de nomes**, selecione a ligação **Selecionar Existente**. 
   
    b. Selecione **Criar**.
   
      ![Portal do Azure – definir as propriedades do hub de notificação](./media/notification-hubs-portal-create-new-hub/notification-hubs-azure-portal-settings.png)

4. Após criar o espaço de nomes e o hub de notificação, abra-o ao selecionar **Todos os recursos** e, em seguida, selecionar o hub de notificação criado na lista. 
   
      ![Portal do Azure - página do portal do hub de notificação](./media/notification-hubs-portal-create-new-hub/notification-hubs-azure-portal-resources.png)

5. Selecione **Políticas de Acesso** na lista. Anote as duas cadeias de ligação que tem à sua disposição. Precisará destas para processar as notificações push mais tarde.

      >[!IMPORTANT]
      >**NÃO** utilize DefaultFullSharedAccessSignature na sua aplicação. Isto destina-se a ser utilizado apenas no seu back-end.
      >
   
      ![Portal do Azure – cadeias de ligação do hub de notificação](./media/notification-hubs-portal-create-new-hub/notification-hubs-connection-strings-portal.png)

