No Cloud Shell, crie um grupo de recursos com o comando [`az group create`](/cli/azure/group?view=azure-cli-latest#az_group_create).

[!INCLUDE [resource group intro text](resource-group.md)]

O exemplo seguinte cria um grupo de recursos com o nome *myResourceGroup*, na localização *Europa Ocidental*. Para ver todas as localizações suportadas para o Serviço de Aplicações, execute o comando `az appservice list-locations`.

```azurecli-interactive
az group create --name myResourceGroup --location "West Europe"
```

Geralmente, o grupo de recursos e os recursos são criados numa região perto de si. 
