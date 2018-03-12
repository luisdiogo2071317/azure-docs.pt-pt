---
title: "Implementar a aplicação no App Service do Azure com um ficheiro ZIP ou WAR | Microsoft Docs"
description: "Saiba como implementar a aplicação no App Service do Azure com um ficheiro ZIP (ou um ficheiro WAR para programadores do Java)."
services: app-service
documentationcenter: 
author: cephalin
manager: cfowler
editor: 
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/07/2018
ms.author: cephalin;sisirap
ms.openlocfilehash: 41fb529f6b4ae923f2920919306324c86a2baa45
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/09/2018
---
# <a name="deploy-your-app-to-azure-app-service-with-a-zip-or-war-file"></a>Implementar a aplicação no App Service do Azure com um ficheiro ZIP ou WAR

Este artigo mostra como utilizar um ficheiro ZIP ou o ficheiro WAR para implementar a sua aplicação web em [App Service do Azure](app-service-web-overview.md). 

Esta implementação de ficheiro ZIP utiliza o mesmo serviço Kudu esse powers contínua com base na integração implementações. O kudu suporta as seguintes funcionalidades para a implementação do ficheiro ZIP: 

- Eliminação de ficheiros à esquerda de uma implementação anterior.
- Opção para ativar o processo de compilação predefinido, que inclui o restauro do pacote.
- [Personalização da implementação](https://github.com/projectkudu/kudu/wiki/Configurable-settings#repository-and-deployment-related-settings), incluindo executar scripts de implementação.  
- Registos de implementação. 

Para obter mais informações, consulte [documentação do Kudu](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file).

A implementação de ficheiro WAR implementa o [WAR](https://wikipedia.org/wiki/WAR_(file_format)) ficheiro para o App Service para executar a sua aplicação web Java. Consulte [ficheiro WAR implementar](#deploy-war-file).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Pré-requisitos

Para concluir os passos neste artigo:

* [Crie uma aplicação do Serviço de Aplicações](/azure/app-service/) ou utilize uma aplicação que tenha criado para outro tutorial.

## <a name="create-a-project-zip-file"></a>Criar um ficheiro ZIP do projeto

>[!NOTE]
> Se tiver transferido os ficheiros de um ficheiro ZIP, extraia os ficheiros pela primeira vez. Por exemplo, se um ficheiro ZIP que transferiu a partir do GitHub, não é possível implementar esse ficheiro como-é. GitHub adiciona mais diretórios aninhados, que não funcionam com o serviço de aplicações. 
>

Numa janela de terminal local, navegue para o diretório de raiz do seu projeto de aplicação. 

Este diretório deve conter o ficheiro de entrada para a sua aplicação web, tal como _index.html_, _index.php_, e _app.js_. Também pode conter ficheiros do pacote de gestão, como _project.json_, _Composer_, _Package. JSON_, _bower.json_e _requirements.txt_.

Crie um arquivo ZIP de tudo no seu projeto. O comando seguinte utiliza a ferramenta predefinida no seu terminal:

```
# Bash
zip -r <file-name>.zip .

# PowerShell
Compress-Archive -Path * -DestinationPath <file-name>.zip
``` 

[!INCLUDE [Deploy ZIP file](../../includes/app-service-web-deploy-zip.md)]

## <a name="deploy-zip-file-with-azure-cli"></a>Implementar o ficheiro ZIP com a CLI do Azure

Certifique-se a versão da CLI do Azure 2.0.21 ou posterior. Para ver qual a versão, executou `az --version` comando na janela de terminal.

Implementar o ficheiro ZIP carregado para a sua aplicação web utilizando o [az webapp configuração origem de implementação de software-zip](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az_webapp_deployment_source_config_zip) comando.  

O exemplo seguinte implementa o ficheiro ZIP que carregou. Quando utilizar uma instalação local da CLI do Azure, especifique o caminho local no ficheiro ZIP para `--src`.   

```azurecli-interactive
az webapp deployment source config-zip --resource-group myResouceGroup --name <app_name> --src clouddrive/<filename>.zip
```

Este comando implementa os ficheiros e os diretórios do ficheiro ZIP na pasta de aplicações do Serviço de Aplicações predefinido (`\home\site\wwwroot`) e reinicia a aplicação. Se estiver configurado qualquer processo de compilação personalizado adicional, este é executado corretamente. Para obter mais informações, consulte [documentação do Kudu](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file).

[!INCLUDE [app-service-deploy-zip-push-rest](../../includes/app-service-deploy-zip-push-rest.md)]  

## <a name="deploy-war-file"></a>Implementar ficheiro WAR

Para implementar um ficheiro WAR no App Service, envie um pedido POST para https://<app_name>.scm.azurewebsites.net/api/wardeploy. O pedido POST tem de conter o ficheiro de .war no corpo da mensagem. Foram fornecidas credenciais de implementação para a sua aplicação no pedido, utilizando a autenticação básica de HTTP. 

Para a autenticação HTTP básica, terá das credenciais de implementação do serviço de aplicações. Para ver como definir as suas credenciais de implementação, consulte [definido e repor as credenciais de utilizador ao nível](app-service-deployment-credentials.md#userscope).

### <a name="with-curl"></a>Com o cURL

O exemplo seguinte utiliza a ferramenta de cURL para implementar um ficheiro de .war. Substitua os marcadores de posição `<username>`, `<war_file_path>`, e `<app_name>`. Quando lhe for pedido pelo cURL, escreva a palavra-passe.

```bash
curl -X POST -u <username> --data-binary @"<war_file_path>" https://<app_name>.scm.azurewebsites.net/api/wardeploy
```

### <a name="with-powershell"></a>Com o PowerShell

O exemplo seguinte utiliza [Invoke-RestMethod](/powershell/module/microsoft.powershell.utility/invoke-restmethod) para enviar um pedido que contém o ficheiro .war. Substitua os marcadores de posição `<deployment_user>`, `<deployment_password>`, `<zip_file_path>`, e `<app_name>`.

```PowerShell
$username = "<deployment_user>"
$password = "<deployment_password>"
$filePath = "<war_file_path>"
$apiUrl = "https://<app_name>.scm.azurewebsites.net/api/wardeploy"
$base64AuthInfo = [Convert]::ToBase64String([Text.Encoding]::ASCII.GetBytes(("{0}:{1}" -f $username, $password)))
Invoke-RestMethod -Uri $apiUrl -Headers @{Authorization=("Basic {0}" -f $base64AuthInfo)} -Method POST -InFile $filePath -ContentType "multipart/form-data"
```

## <a name="next-steps"></a>Passos Seguintes

Para cenários de implementação mais avançados, tente [implementar no Azure com o Git](app-service-deploy-local-git.md). Implementação baseada em Git para o Azure permite o controlo de versão, o restauro de pacote, MSBuild e muito mais.

## <a name="more-resources"></a>Mais recursos

* [O kudu: Implementação a partir de um ficheiro zip](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file)
* [Credenciais de implementação do App Service do Azure](app-service-deploy-ftp.md)
