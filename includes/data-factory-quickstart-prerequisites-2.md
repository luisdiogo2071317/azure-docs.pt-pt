### <a name="azure-powershell"></a>Azure PowerShell

#### <a name="install-powershell"></a>Instalar o PowerShell
Instale o PowerShell mais recente, se não o tiver no seu computador. 

1. No browser, navegue para a página [Transferências do Azure](https://azure.microsoft.com/downloads/). 
2. Clique em **Instalação para Windows**, na secção **Ferramentas de linha de comandos** -> **PowerShell**. 
3. Para instalar o PowerShell, execute o ficheiro **MSI**. 

Para obter informações detalhadas, veja [Como instalar e configurar o Azure PowerShell](/powershell/azure/install-azurerm-ps). 

#### <a name="log-in-to-powershell"></a>Iniciar sessão no PowerShell

1. Inicie o **PowerShell** no seu computador. Mantenha o PowerShell aberto até ao fim deste início rápido. Se o fechar e reabrir, terá de executar os comandos novamente.

    ![Iniciar o PowerShell](media/data-factory-quickstart-prerequisites-2/search-powershell.png)
1. Execute o comando seguinte e introduza o mesmo nome de utilizador e a mesma palavra-passe do Azure que utiliza para iniciar sessão no portal do Azure:
       
    ```powershell
    Login-AzureRmAccount
    ```        
2. Execute o comando seguinte para ver todas as subscrições desta conta:

    ```powershell
    Get-AzureRmSubscription
    ```
3. Se vir várias subscrições associadas à sua conta, execute o comando seguinte selecionar aquela com que pretende trabalhar. Substitua **SubscriptionId** pelo ID da sua subscrição do Azure:

    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<SubscriptionId>"       
    ```
