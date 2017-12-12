## <a name="set-up-the-development-environment"></a>Configurar o ambiente de desenvolvimento

Esta secção explica como configurar o ambiente de desenvolvimento, incluindo a criação de uma aplicação ASP.NET MVC, adicionar uma ligação a serviços ligados, adicionar um controlador e especificar as diretivas de espaço de nomes necessários.

### <a name="create-an-aspnet-mvc-app-project"></a>Criar um projeto de aplicação de ASP.NET MVC

1. Abra o Visual Studio.

1. Selecione **ficheiro -> novo -> projeto** no menu principal

1. No **novo projeto** caixa de diálogo, especifique as opções como realçados na figura seguinte:

    ![Criar projeto ASP.NET](./media/vs-storage-aspnet-getting-started-setup-dev-env/vs-storage-aspnet-getting-started-setup-dev-env-1.png)

1. Selecione **OK**.

1. No **novo projeto ASP.NET** caixa de diálogo, especifique as opções como realçados na figura seguinte:

    ![Especifique o MVC](./media/vs-storage-aspnet-getting-started-setup-dev-env/vs-storage-aspnet-getting-started-setup-dev-env-2.png)

1. Selecione **OK**.

### <a name="use-connected-services-to-connect-to-an-azure-storage-account"></a>Utilizar os serviços ligados para ligar a uma conta de armazenamento do Azure

1. No **Explorador de soluções**, clique com o botão direito no projeto e, no menu de contexto, selecione **adicionar -> serviço ligado**.

1. No **adicionar o serviço ligado** caixa de diálogo, selecione **armazenamento na nuvem com o Storage do Azure**e, em seguida, selecione **configurar**.

    ![Caixa de diálogo de serviço ligada](./media/vs-storage-aspnet-getting-started-setup-dev-env/vs-storage-aspnet-getting-started-setup-dev-env-3.png)

1. No **Storage do Azure** caixa de diálogo, selecione o armazenamento do Azure da conta a utilizar para este tutorial.  Para criar uma nova conta de armazenamento do Azure, clique em **criar uma nova conta de armazenamento** e preencha o formulário.  Depois de selecionar uma conta armazenamento existente ou criar um novo, clique em **adicionar**.  Visual Studio irá instalar o pacote NuGet para o Storage do Azure e uma cadeia de ligação de armazenamento para **Web. config**.

> [!TIP]
> Para saber como criar uma conta de armazenamento com o [portal do Azure](https://portal.azure.com), consulte [criar uma conta de armazenamento](../articles/storage/common/storage-create-storage-account.md#create-a-storage-account).
>
> Também é possível criar uma conta de armazenamento do Azure utilizando [Azure PowerShell](../articles/storage/common/storage-powershell-guide-full.md), [CLI do Azure](../articles/storage/common/storage-azure-cli.md), ou o [Shell de nuvem do Azure](../articles/cloud-shell/overview.md).

