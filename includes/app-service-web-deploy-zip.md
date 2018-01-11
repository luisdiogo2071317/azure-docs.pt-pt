## <a name="deploy-uploaded-zip-file"></a>Implementar o ficheiro ZIP carregado

No Cloud Shell, implemente o ficheiro ZIP carregado na sua aplicação Web através do comando [az webapp deployment source config-zip](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az_webapp_deployment_source_config_zip). Não se esqueça de substituir *\<app_name>* pelo nome da sua aplicação Web.

```azurecli-interactive
az webapp deployment source config-zip --resource-group myResouceGroup --name <app_name> --src clouddrive/myAppFiles.zip
```

Este comando implementa os ficheiros e os diretórios do ficheiro ZIP na pasta de aplicações do Serviço de Aplicações predefinido (`\home\site\wwwroot`) e reinicia a aplicação. Se estiver configurado qualquer processo de compilação personalizado adicional, este é executado corretamente.
