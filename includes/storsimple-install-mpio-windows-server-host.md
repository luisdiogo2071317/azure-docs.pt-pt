#### <a name="to-install-mpio-on-the-host"></a>Instalar a MPIO no anfitrião
1. Abra o Gestor de servidor no seu anfitrião do Windows Server. Por predefinição, o Gestor de servidor é iniciado quando um membro do grupo Administradores inicia sessão num computador que está a executar o Windows Server 2012 R2 ou Windows Server 2012. Se o Gestor de servidor não esteja ainda aberto, clique em **Iniciar > Gestor de servidor**.
   
    ![Gestor de servidores](./media/storsimple-install-mpio-windows-server/IC740997.png)
2. Clique em **Gestor de servidor > Dashboard > Adicionar funções e funcionalidades**. Esta ação inicia o **para adicionar funções e funcionalidades** assistente.
   
    ![Assistente para adicionar funções e funcionalidades 1](./media/storsimple-install-mpio-windows-server/IC740998.png)
3. Na **para adicionar funções e funcionalidades** assistente, efetue o seguinte procedimento:
   
   * Sobre o **antes de começar** página, clique em **próxima**.
   * Sobre o **selecionar tipo de instalação** página, aceite a predefinição de **baseada em funções ou baseada em recursos** instalação. Clique em **Seguinte**.
     
       ![Assistente para adicionar funções e funcionalidades 2](./media/storsimple-install-mpio-windows-server/IC740999.png)
   * Sobre o **selecionar servidor de destino** página, selecione **selecione um servidor no agrupamento de servidores**. O servidor de anfitrião deve ser detetado automaticamente. Clique em **Seguinte**.
   * Sobre o **selecionar funções de servidor** página, clique em **próxima**.
   * Sobre o **selecionar funcionalidades** , selecione **Multipath i /** e clique em **seguinte**.
     
       ![Assistente para adicionar funções e funcionalidades 5](./media/storsimple-install-mpio-windows-server/IC741000.png)
   * Sobre o **confirmar seleções de instalação** página, confirme a seleção e, em seguida, selecione **reiniciar automaticamente o servidor de destino quando necessário**, conforme mostrado abaixo. Clique em **Instalar**.
     
       ![Assistente para adicionar funções e funcionalidades 8](./media/storsimple-install-mpio-windows-server/IC741001.png)
   * Será notificado quando a instalação estiver concluída. Clique em **Fechar** para fechar o assistente.
     
       ![Assistente para adicionar funções e funcionalidades 9](./media/storsimple-install-mpio-windows-server/IC741002.png)

