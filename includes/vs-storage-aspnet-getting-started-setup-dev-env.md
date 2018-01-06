## <a name="set-up-the-development-environment"></a>Configurar o ambiente de desenvolvimento

Esta secção explica como configurar o ambiente de desenvolvimento. Isto inclui criar uma aplicação ASP.NET MVC, adicionar uma ligação a serviços ligados, adicionar um controlador e especificar as diretivas de espaço de nomes necessários.

### <a name="create-an-aspnet-mvc-app-project"></a>Criar um projeto de aplicação de ASP.NET MVC

1. Abra o Visual Studio.

1. No menu principal, selecione **ficheiro** > **novo** > **projeto**.

1. No **novo projeto** caixa de diálogo, selecione **Web** > **uma aplicação Web ASP.NET (.NET Framework)**. No **nome** campo, especifique **StorageAspNet**. Selecione **OK**.

    ![Caixa de diálogo de captura de ecrã do novo projeto](./media/vs-storage-aspnet-getting-started-setup-dev-env/vs-storage-aspnet-getting-started-setup-dev-env-1.png)

1. No **nova aplicação Web do ASP.NET** caixa de diálogo, selecione **MVC**e, em seguida, selecione **OK**.

    ![Caixa de diálogo de captura de ecrã da nova aplicação Web ASP.NET](./media/vs-storage-aspnet-getting-started-setup-dev-env/vs-storage-aspnet-getting-started-setup-dev-env-2.png)

### <a name="use-connected-services-to-connect-to-an-azure-storage-account"></a>Utilizar serviços ligados para ligar a uma conta de armazenamento do Azure

1. No **Explorador de soluções**, clique com o botão direito no projeto.

2. No menu de contexto, selecione **adicionar** > **serviço ligado**.

1. No **serviços ligados** caixa de diálogo, selecione **armazenamento na nuvem com o Storage do Azure**e, em seguida, selecione **configurar**.

    ![Caixa de diálogo de captura de ecrã de serviços ligados](./media/vs-storage-aspnet-getting-started-setup-dev-env/vs-storage-aspnet-getting-started-setup-dev-env-3.png)

1. No **Storage do Azure** caixa de diálogo, selecione o armazenamento do Azure da conta a utilizar para este tutorial. Para criar uma nova conta de armazenamento do Azure, selecione **criar uma nova conta de armazenamento**e preencha o formulário. Depois de selecionar uma conta armazenamento existente ou criar uma nova, selecione **adicionar**. Visual Studio instala o pacote NuGet para armazenamento do Azure e uma cadeia de ligação de armazenamento para **Web. config**.

> [!TIP]
> Para saber como criar uma conta de armazenamento com o [portal do Azure](https://portal.azure.com), consulte [criar uma conta de armazenamento](../articles/storage/common/storage-create-storage-account.md#create-a-storage-account).
>
> Também pode criar uma conta de armazenamento utilizando [Azure PowerShell](../articles/storage/common/storage-powershell-guide-full.md), [CLI do Azure](../articles/storage/common/storage-azure-cli.md), ou [Shell de nuvem do Azure](../articles/cloud-shell/overview.md).

