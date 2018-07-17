## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Crie um grupo de recursos com o comando [az group create](/cli/azure/group#az_group_create). Um grupo de recursos do Azure é um contentor lógico no qual os recursos do Azure, como aplicações Function App, bases de dados e contas de armazenamento, são implementados e geridos.

O exemplo seguinte cria um grupo de recursos com o nome `myResourceGroup`.  
Se não estiver a utilizar o Cloud Shell, inicie sessão primeiro com `az login`.

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```
Geralmente, o grupo de recursos e os recursos são criados numa região perto de si. Para ver todas as localizações suportadas para os planos do Serviço de Aplicações, execute o comando [az appservice list-locations](/cli/azure/appservice#az_appservice_list_locations).