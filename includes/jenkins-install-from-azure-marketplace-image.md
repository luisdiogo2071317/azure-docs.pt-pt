1. No seu browser, abra o [imagem do Azure Marketplace para Jenkins](https://azuremarketplace.microsoft.com/marketplace/apps/azure-oss.jenkins?tab=Overview).

1. Selecione **obter IT agora**.

    ![Selecione o GIT TI agora para iniciar o processo de instalação para a imagem do Jenkins Marketplace.](./media/jenkins-install-from-azure-marketplace-image/jenkins-install-get-it-now.png)

1. Depois de rever as informações de termos e detalhes de preços, selecione **continuar**.

    ![Jenkins Marketplace termos de preços e as informações da imagem.](./media/jenkins-install-from-azure-marketplace-image/jenkins-install-pricing-and-terms.png)

1. Selecione **criar** para configurar o servidor de Jenkins no portal do Azure. 

    ![Instale a imagem do Jenkins Marketplace.](./media/jenkins-install-from-azure-marketplace-image/jenkins-install-create.png)

1. No **Noções básicas** separador, especifique os valores seguintes:

    - **Nome** -introduza `Jenkins`.
    - **Utilizador** -introduza o nome de utilizador a utilizar quando iniciar sessão na máquina virtual no qual Jenkins está em execução.
    - **Tipo de autenticação** - selecione **palavra-passe**.
    - **Palavra-passe** -introduza a palavra-passe a utilizar quando iniciar sessão na máquina virtual no qual Jenkins está em execução.
    - **Confirmar palavra-passe** -Reintroduza a palavra-passe a utilizar quando iniciar sessão na máquina virtual no qual Jenkins está em execução.
    - **Tipo de versão Jenkins** - selecione **LTS**.
    - **Subscrição** -selecionar a subscrição do Azure na qual pretende instalar Jenkins.
    - **Grupo de recursos** - selecione **criar nova**e introduza um nome para o grupo de recursos que funciona como um contentor para a coleção de recursos que compõem a sua instalação Jenkins lógico.
    - **Localização** - selecione **EUA Leste**.

    ![Introduza as informações do grupo de recursos e autenticação para Jenkins no separador básico.](./media/jenkins-install-from-azure-marketplace-image/jenkins-configure-basic.png)

1. Selecione **OK** prossiga para o **definições** separador. 

1. No **definições** separador, especifique os valores seguintes:

    - **Tamanho** -selecionar a opção de dimensionamento adequado para a máquina virtual de Jenkins.
    - **Tipo de disco da VM** - Especifique um HDD (unidade de disco rígido) ou SSD (unidade de estado sólido) para indicar o tipo de disco de armazenamento é permitido para a máquina virtual de Jenkins.
    - **Endereço IP público** -nome do endereço IP está predefinida para o nome de Jenkins que especificou na página anterior com o sufixo - IP. Pode selecionar a opção para alterar esse predefinido.
    - **Etiqueta de nome de domínio** -especifique o valor para o URL para a máquina virtual de Jenkins completamente qualificado.

    ![Introduza as definições de máquina virtual para Jenkins no separador Definições.](./media/jenkins-install-from-azure-marketplace-image/jenkins-configure-settings.png)

1. Selecione **OK** prossiga para o **resumo** separador.

1. Quando o **resumo** separador apresenta, as informações que introduziu são validadas. Quando vir o **validação transmitida** mensagem, selecione **OK**. 

    ![O separador Resumo mostra e valida as opções selecionadas.](./media/jenkins-install-from-azure-marketplace-image/jenkins-configure-summary.png)

1. Quando o **criar** separador apresenta, selecione **criar** para criar a máquina virtual de Jenkins. Quando o servidor estiver pronto, uma notificação é apresentada no portal do Azure.

    ![Jenkins é notificação pronta.](./media/jenkins-install-from-azure-marketplace-image/jenkins-install-notification.png)