Crie um grupo de recursos com o comando [az group create](/cli/azure/group?view=azure-cli-latest#az_group_create).

[!INCLUDE [resource group intro text](resource-group.md)]

O exemplo seguinte cria um grupo de recursos com o nome *myResourceGroup* na localização *westeurope*.

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```

Para ver as localizações disponíveis, execute o comando `az appservice list-locations`. Geralmente, os recursos são criados numa região perto de si.
