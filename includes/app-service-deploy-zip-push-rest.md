## <a name="rest"></a>Implementar o ficheiro ZIP com REST APIs 

Pode utilizar o [REST APIs do serviço de implementação](https://github.com/projectkudu/kudu/wiki/REST-API) para implementar o ficheiro. zip para a sua aplicação no Azure. Para implementar, envie um pedido POST para https://<app_name>.scm.azurewebsites.net/api/zipdeploy. O pedido POST tem de conter o ficheiro. zip no corpo da mensagem. Foram fornecidas credenciais de implementação para a sua aplicação no pedido, utilizando a autenticação básica de HTTP. Para obter mais informações, consulte o [referência de implementação de push. zip](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file). 

Para a autenticação HTTP básica, terá das credenciais de implementação do serviço de aplicações. Para ver como definir as suas credenciais de implementação, consulte [definido e repor as credenciais de utilizador ao nível](../articles/app-service/app-service-deployment-credentials.md#userscope).

### <a name="with-curl"></a>Com o cURL

O exemplo seguinte utiliza a ferramenta de cURL para implementar um ficheiro. zip. Substitua os marcadores de posição `<username>`, `<password>`, `<zip_file_path>`, e `<app_name>`. Quando lhe for pedido pelo cURL, escreva a palavra-passe.

```bash
curl -X POST -u <deployment_user> --data-binary @"<zip_file_path>" https://<app_name>.scm.azurewebsites.net/api/zipdeploy
```

Este pedido aciona uma implementação de push a partir de ficheiro. zip carregado. Pode rever as implementações atuais e anteriores, utilizando o ponto final https://<app_name>.scm.azurewebsites.net/api/deployments, conforme mostrado no exemplo seguinte cURL. Novamente, substitua `<app_name>` com o nome da sua aplicação e `<deployment_user>` com o nome de utilizador das suas credenciais de implementação.

```bash
curl -u <deployment_user> https://<app_name>.scm.azurewebsites.net/api/deployments
```

### <a name="with-powershell"></a>Com o PowerShell

O exemplo seguinte utiliza [Invoke-RestMethod](/powershell/module/microsoft.powershell.utility/invoke-restmethod) para enviar um pedido que contém o ficheiro. zip. Substitua os marcadores de posição `<deployment_user>`, `<deployment_password>`, `<zip_file_path>`, e `<app_name>`.

```PowerShell
#PowerShell
$username = "<deployment_user>"
$password = "<deployment_password>"
$filePath = "<zip_file_path>"
$apiUrl = "https://<app_name>.scm.azurewebsites.net/api/zipdeploy"
$base64AuthInfo = [Convert]::ToBase64String([Text.Encoding]::ASCII.GetBytes(("{0}:{1}" -f $username, $password)))
$userAgent = "powershell/1.0"
Invoke-RestMethod -Uri $apiUrl -Headers @{Authorization=("Basic {0}" -f $base64AuthInfo)} -UserAgent $userAgent -Method POST -InFile $filePath -ContentType "multipart/form-data"
```

Este pedido aciona uma implementação de push a partir de ficheiro. zip carregado. Para rever as implementações atuais e anteriores, execute os seguintes comandos. Novamente, substitua o `<app_name>` marcador de posição.

```bash
$apiUrl = "https://<app_name>.scm.azurewebsites.net/api/deployments"
Invoke-RestMethod -Uri $apiUrl -Headers @{Authorization=("Basic {0}" -f $base64AuthInfo)} -UserAgent $userAgent -Method GET
```
