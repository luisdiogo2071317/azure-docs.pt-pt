---
title: "Implementar a aplicação no App Service do Azure com um ficheiro ZIP | Microsoft Docs"
description: "Saiba como implementar a aplicação no App Service do Azure com um ficheiro ZIP."
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
ms.date: 12/05/2017
ms.author: cephalin;sisirap
ms.openlocfilehash: a0e4df0ef0a1c873f1efcac1d8dbfe3cada18218
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/14/2017
---
# <a name="deploy-your-app-to-azure-app-service-with-a-zip-file"></a>Implementar a aplicação no App Service do Azure com um ficheiro ZIP

Este artigo mostra como utilizar um ficheiro ZIP para implementar a sua aplicação web em [App Service do Azure](app-service-web-overview.md). 

Esta implementação de ficheiro ZIP utiliza o mesmo serviço Kudu esse powers contínua com base na integração implementações. O kudu suporta as seguintes funcionalidades para a implementação do ficheiro ZIP: 

- Eliminação de ficheiros à esquerda de uma implementação anterior.
- Opção para ativar o processo de compilação predefinido, que inclui o restauro do pacote.
- [Personalização da implementação](https://github.com/projectkudu/kudu/wiki/Configurable-settings#repository-and-deployment-related-settings), incluindo executar scripts de implementação.  
- Registos de implementação. 

Para obter mais informações, consulte [documentação do Kudu](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file).

## <a name="prerequisites"></a>Pré-requisitos

Para concluir os passos neste artigo:

* [Criar uma aplicação do app Service](/azure/app-service/), ou utilizar uma aplicação que criou para outro tutorial.

## <a name="create-a-project-zip-file"></a>Criar um ficheiro ZIP do projeto

>[!NOTE]
> Se tiver transferido os ficheiros de um ficheiro ZIP, extraia os ficheiros pela primeira vez. Por exemplo, se um ficheiro ZIP que transferiu a partir do GitHub, não é possível implementar esse ficheiro como-é. GitHub adiciona mais diretórios aninhados, que não funcionam com o serviço de aplicações. 
>

Numa janela de terminal local, navegue para o diretório de raiz do seu projeto de aplicação. 

Este diretório deve conter o ficheiro de entrada para a sua aplicação web, tal como _index.html_, _index.php_, e _app.js_. Também pode conter ficheiros do pacote de gestão, como _project.json_, _Composer_, _Package. JSON_, _bower.json_e _requirements.txt_.

Crie um arquivo ZIP de tudo no seu projeto. O comando seguinte utiliza a ferramenta de predefinição no seu terminal:

```
# Bash
zip -r <file-name>.zip .

# PowerShell
Compress-Archive -Path * -DestinationPath <file-name>.zip
``` 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="upload-zip-file-to-cloud-shell"></a>Carregar o ficheiro ZIP shell de nuvem

Se optar por executar a CLI do Azure a partir do seu terminal local em vez disso, ignore este passo.

Siga os passos aqui descritos para carregar o ficheiro ZIP para a Shell de nuvem. 

[!INCLUDE [app-service-web-upload-zip.md](../../includes/app-service-web-upload-zip-no-h.md)]

Para obter mais informações, consulte [manter os ficheiros na Shell de nuvem do Azure](../cloud-shell/persisting-shell-storage.md).

## <a name="deploy-zip-file"></a>Implementar o ficheiro ZIP

Implementar o ficheiro ZIP carregado para a sua aplicação web utilizando o [az webapp configuração origem de implementação de software-zip](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az_webapp_deployment_source_config_zip) comando. Se optar por não utilizar a Shell de nuvem, certifique-se a versão da CLI do Azure é 2.0.21 ou posterior. Para ver qual a versão, executou `az --version` comando na janela de terminal do local. 

O exemplo seguinte implementa o ficheiro ZIP que carregou. Quando utilizar uma instalação local da CLI do Azure, especifique o caminho local no ficheiro ZIP para `--src`.   

```azurecli-interactive
az webapp deployment source config-zip --resource-group myResouceGroup --name <app_name> --src clouddrive/<filename>.zip
```

Este comando implementa os ficheiros e diretórios do ZIP do ficheiro para a pasta de aplicação de serviço de aplicações predefinido (`\home\site\wwwroot`) e reinicia a aplicação. Se qualquer processo de compilação personalizadas adicionais estiver configurado, é executado bem. Para obter mais informações, consulte [documentação do Kudu](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file).

Para ver a lista de implementações para esta aplicação, tem de utilizar as APIs REST (consulte a secção seguinte). 

[!INCLUDE [app-service-deploy-zip-push-rest](../../includes/app-service-deploy-zip-push-rest.md)]  

## <a name="next-steps"></a>Passos seguintes

Para cenários de implementação mais avançados, tente [implementar no Azure com o Git](app-service-deploy-local-git.md). Implementação baseada em Git para o Azure permite o controlo de versão, o restauro de pacote, MSBuild e muito mais.

## <a name="more-resources"></a>Mais recursos

* [O kudu: Implementação a partir de um ficheiro zip](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file)
* [Credenciais de implementação do App Service do Azure](app-service-deploy-ftp.md)
