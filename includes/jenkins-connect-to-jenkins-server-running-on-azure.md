1. No seu browser, navegue até à máquina virtual Jenkins. Uma vez que o dashboard Jenkins acessível através de HTTP não segura, apresentada uma mensagem a indicar que tem de utilizar o SSH para o túnel para a máquina virtual.

    ![Jenkins fornece informações explicar como utilizar o SSH para ligar à máquina virtual Jenkins.](./media/jenkins-connect-to-jenkins-server-running-on-azure/jenkins-ssh-instructions.png)

1. Abra uma janela de bash ou terminal, com acesso a SSH.

1. Introduza o seguinte `ssh` comando, substituindo o  **&lt;username >** e  **&lt;domínio >** marcadores de posição com os valores especificados ao criar o Jenkins máquina virtual:

    ```bash
    ssh -L 127.0.0.1:8080:localhost:8080 <username>@<domain>.eastus.cloudapp.azure.com
    ```

1. Siga o `ssh` pedidos para iniciar sessão na máquina virtual Jenkins.

1. Introduza o seguinte comando para obter a palavra-passe inicial para Jenkins desbloqueio.

    ```bash
    sudo cat /var/lib/jenkins/secrets/initialAdminPassword
    ```

1. Copie para a área de transferência a palavra-passe que é apresentada na janela de bash ou terminal.

1. No seu browser, navegue para `http://localhost:8080`.

1. Cole a palavra-passe que obteve anteriormente para o **palavra-passe administrativa** campo e selecione **continuar**.

    ![Jenkins armazena uma palavra-passe inicial que tem de utilizar para desbloquear a máquina virtual de Jenkins na primeira vez que o se liga ao mesmo.](./media/jenkins-connect-to-jenkins-server-running-on-azure/jenkins-unlock.png)

1. Selecione **instalar plug-ins sugeridos**.

    ![Jenkins permite-lhe instalar facilmente uma coleção de plug-ins sugeridos após entrada inicial](./media/jenkins-connect-to-jenkins-server-running-on-azure/jenkins-customize.png)

1. No **criar primeiro utilizador de Admin** página, criar o utilizador administrativo e a palavra-passe para o dashboard Jenkins e selecione **guardar e concluir**.

1. No **Jenkins está pronto!** página, selecione **começar a utilizar Jenkins**.

    ![Depois de Jenkins está instalado e configurado para acesso, pode começar a utilizar para criar e tarefas de compilação.](./media/jenkins-connect-to-jenkins-server-running-on-azure/jenkins-ready.png)