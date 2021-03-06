---
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 11/03/2016
ms.author: cephalin
ms.openlocfilehash: 7771bbcab08fb84a941eedab402880fb7bc4232d
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/17/2018
ms.locfileid: "53551675"
---
## <a name="rest"></a>Implementar o ficheiro ZIP com as APIs REST 

Pode utilizar o [REST APIs do serviço de implementação](https://github.com/projectkudu/kudu/wiki/REST-API) para implementar o ficheiro. zip para a sua aplicação no Azure. Para implementar, envie um pedido POST para https://<app_name>.scm.azurewebsites.net/api/zipdeploy. O pedido POST tem de conter o ficheiro. zip no corpo da mensagem. As credenciais de implementação para a sua aplicação são fornecidas no pedido através da autenticação básica HTTP. Para obter mais informações, consulte a [referência de implementação de push. zip](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file). 

Para a autenticação básica HTTP, terá de suas credenciais de implementação do serviço de aplicações. Para ver como definir as suas credenciais de implementação, consulte [definido e repor as credenciais de nível de usuário](../articles/app-service/deploy-configure-credentials.md#userscope).

### <a name="with-curl"></a>Com cURL

O exemplo seguinte utiliza a ferramenta cURL para implementar um ficheiro. zip. Substitua os marcadores de posição `<username>`, `<password>`, `<zip_file_path>`, e `<app_name>`. Quando lhe for pedido curl, escreva a palavra-passe.

```bash
curl -X POST -u <deployment_user> --data-binary @"<zip_file_path>" https://<app_name>.scm.azurewebsites.net/api/zipdeploy
```

Este pedido aciona a implementação de push a partir do ficheiro. zip carregado. Pode rever as implementações atuais e anteriores, utilizando o `https://<app_name>.scm.azurewebsites.net/api/deployments` ponto de extremidade, conforme mostrado no exemplo cURL seguinte. Mais uma vez, substitua `<app_name>` com o nome da sua aplicação e `<deployment_user>` com o nome de utilizador de suas credenciais de implementação.

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

Este pedido aciona a implementação de push a partir do ficheiro. zip carregado. Para rever as implementações atuais e anteriores, execute os seguintes comandos. Mais uma vez, substitua o `<app_name>` marcador de posição.

```bash
$apiUrl = "https://<app_name>.scm.azurewebsites.net/api/deployments"
Invoke-RestMethod -Uri $apiUrl -Headers @{Authorization=("Basic {0}" -f $base64AuthInfo)} -UserAgent $userAgent -Method GET
```
