## <a name="rest"></a>Implementar utilizando REST APIs 
 
Pode utilizar o [REST APIs do serviço de implementação](https://github.com/projectkudu/kudu/wiki/REST-API) para implementar o ficheiro. zip para a sua aplicação no Azure. Apenas envie um pedido POST para https://<app_name>.scm.azurewebsites.net/api/zipdeploy. O pedido POST tem de conter o ficheiro. zip no corpo da mensagem. Foram fornecidas credenciais de implementação para a sua aplicação no pedido, utilizando a autenticação básica de HTTP. Para obter mais informações, consulte o [referência de implementação de push. zip](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file). 

O exemplo seguinte utiliza a ferramenta de cURL para enviar um pedido que contém o ficheiro. zip. Pode executar cURL no terminal num computador Mac ou Linux ou através da utilização de Bash no Windows. Substitua o `<zip_file_path>` marcador de posição com o caminho para a localização do ficheiro. zip projeto. Substitua, também `<app_name>` com o nome exclusivo da sua aplicação.

Substitua o `<deployment_user>` marcador de posição com o nome de utilizador das suas credenciais de implementação. Quando lhe for pedido pelo cURL, escreva a palavra-passe. Para saber como definir as credenciais de implementação para a sua aplicação, consulte [definido e repor as credenciais de utilizador ao nível](../articles/app-service/app-service-deployment-credentials.md#userscope).   

```bash
curl POST -u <deployment_user> --data-binary @"<zip_file_path>" https://<app_name>.scm.azurewebsites.net/api/zipdeploy
```

Este pedido aciona uma implementação de push a partir de ficheiro. zip carregado. Pode rever as implementações atuais e anteriores, utilizando o ponto final https://<app_name>.scm.azurewebsites.net/api/deployments, conforme mostrado no exemplo seguinte cURL. Novamente, substitua `<app_name>` com o nome da sua aplicação e `<deployment_user>` com o nome de utilizador das suas credenciais de implementação.

```bash
curl -u <deployment_user> https://<app_name>.scm.azurewebsites.net/api/deployments
```