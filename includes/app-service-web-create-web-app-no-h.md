No Cloud Shell, crie uma [aplicação Web](../articles/app-service/app-service-web-overview.md) no plano do Serviço de Aplicações `myAppServicePlan` com o comando [az webapp create](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create). 

No exemplo a seguir, substitua *\<app_name>* com um nome de aplicação globalmente exclusivo (os carateres válidos são `a-z`, `0-9` e `-`). 

```azurecli-interactive
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan --deployment-local-git
```

Quando a aplicação Web tiver sido criada, a CLI do Azure mostra informações semelhantes ao seguinte exemplo:

```json
Local git is configured with url of 'https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git'
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "deploymentLocalGitUrl": "https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

Criou uma aplicação Web vazia, com a implementação de git ativada.

> [!NOTE]
> O URL do Git remoto é apresentado na propriedade `deploymentLocalGitUrl`, com o formato `https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git`. Guarde este URL, uma vez que vai precisar dele mais tarde.
>

Navegue para a aplicação Web recentemente criada.

```bash
http://<app_name>.azurewebsites.net
```
