## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão na sua subscrição do Azure com o comando `Connect-AzureRmAccount` e siga as instruções no ecrã.

```powershell
Connect-AzureRmAccount
```

Se não souber qual a localização que quer utilizar, pode listar as localizações disponíveis. Depois de a lista ser apresentada, localize a que quer utilizar. Este exemplo utiliza **eastus**. Armazene-a numa variável e utilize-a de forma a poder alterá-la num único local.

```powershell
Get-AzureRmLocation | select Location 
$location = "eastus"
```

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Crie um grupo de recursos do Azure com [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Um grupo de recursos é um contentor lógico no qual os recursos do Azure são implementados e geridos. 

```powershell
$resourceGroup = "myResourceGroup"
New-AzureRmResourceGroup -Name $resourceGroup -Location $location 
```

## <a name="create-a-storage-account"></a>Create a storage account

Crie uma conta de armazenamento standard para fins gerais com a replicação LRS, utilizando [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/New-AzureRmStorageAccount), e obtenha o contexto de conta de armazenamento que define a conta de armazenamento a utilizar. Ao efetuar ações em contas de armazenamento, referencia o contexto em vez de fornecer repetidamente as credenciais. Este exemplo cria uma conta de armazenamento denominada *mystorageaccount* com armazenamento localmente redundante (LRS) e encriptação de blobs (ativada por predefinição).

```powershell
$storageAccount = New-AzureRmStorageAccount -ResourceGroupName $resourceGroup `
  -Name "mystorageaccount" `
  -Location $location `
  -SkuName Standard_LRS `
  -Kind Storage

$ctx = $storageAccount.Context
```
