---
title: Zip a implementação de push para as funções do Azure | Documentos da Microsoft
description: Utilize os recursos de implantação de ficheiro. zip do serviço de implementação Kudu para publicar as suas funções do Azure.
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: ''
tags: ''
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/29/2018
ms.author: glenga
ms.openlocfilehash: 3ff02816cdd5641cdcd78a12206b80be6d518373
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39423713"
---
# <a name="zip-push-deployment-for-azure-functions"></a>Implementação de push zip para as funções do Azure 
Este artigo descreve como implementar os ficheiros de projeto de aplicação de função para o Azure a partir de um ficheiro (compactado). zip. Saiba como fazer uma implementação de push, ao utilizar a CLI do Azure e com as APIs REST. 

As funções do Azure tem uma gama completa de opções de implementação e integração contínuas que são fornecidos pelo App Service do Azure. Para obter mais informações, consulte [implementação contínua para funções do Azure](functions-continuous-deployment.md). 

Para mais rápida iteração durante o desenvolvimento, muitas vezes, é mais fácil de implantar seus arquivos de projeto de aplicação de função diretamente a partir de um ficheiro. zip comprimido. Esta implementação de ficheiro. zip utiliza o mesmo serviço de Kudu esse powers contínua com base na integração implementações, incluindo:

+ Eliminação de ficheiros que foram left de implementações anteriores.
+ Personalização da implementação, incluindo a execução de scripts de implementação.
+ Logs de implantação.
+ A sincronizar os acionadores de função numa [plano de consumo](functions-scale.md) aplicação de funções.

Para obter mais informações, consulte a [referência de implementação de push. zip](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file). 

## <a name="deployment-zip-file-requirements"></a>Requisitos de implementação do ficheiro. zip
O ficheiro. zip que utiliza para a implementação de push tem de conter todos os arquivos de projeto na sua aplicação de função, incluindo o código de função. 

>[!IMPORTANT]
> Quando usa a implementação de push. zip, são eliminados quaisquer ficheiros a partir de uma implementação existente que não forem encontrados no arquivo. zip da sua aplicação de função.  

[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

Uma aplicação de função inclui todos os ficheiros e pastas no `wwwroot` diretório. Uma implementação de ficheiro. zip inclui o conteúdo do `wwwroot` diretório, mas não o próprio diretório.  

## <a name="download-your-function-app-files"></a>Transferir os seus ficheiros de aplicação de função

Quando estiver a desenvolver num computador local, é fácil criar um ficheiro. zip da pasta de projeto de aplicação de função no seu computador de desenvolvimento. 

No entanto, pode ter criado as suas funções com o editor no portal do Azure. Pode baixar um projeto existente para a aplicação de função em uma das seguintes formas: 

+ **No portal do Azure:** 

    1. Inicie sessão para o [portal do Azure](https://portal.azure.com)e, em seguida, vá para a aplicação de funções.

    2. Sobre o **descrição geral** separador, selecione **transferir conteúdo da aplicação**. Selecione as opções de download e, em seguida, selecione **transferir**.     

        ![Transfira o projeto de aplicação de função](./media/deployment-zip-push/download-project.png)

    O ficheiro. zip transferido está no formato correto para ser publicada novamente para a sua aplicação de função utilizando a implementação de push. zip. O download do portal, também pode adicionar os arquivos necessários para abrir a aplicação de funções diretamente no Visual Studio.

+ **Utilizar REST APIs:** 

    Utilize a seguinte implementação obter API para transferir os ficheiros da sua `<function_app>` projeto: 

        https://<function_app>.scm.azurewebsites.net/api/zip/site/wwwroot/

    Incluindo `/site/wwwroot/` torna-se de que o ficheiro zip inclui apenas os ficheiros de projeto de aplicação função e não todo o site. Se não já têm sessão iniciada Azure, será solicitado a fazê-lo. Tenha em atenção que uma mensagem a enviar pedido para o `api/zip/` API é discoraged em favor do método de implantação de zip descrito neste tópico. 

Também pode transferir um ficheiro. zip a partir de um repositório do GitHub. Tenha em atenção que, ao baixar um repositório do GitHub como um ficheiro. zip, GitHub adiciona um nível de pasta extra para o ramo. Isso significa de nível de pasta extra que não é possível implementar o ficheiro. zip diretamente à medida que transferido-o partir do GitHub. Se estiver a utilizar um repositório do GitHub para manter a sua aplicação function app, deve usar [integração contínua](functions-continuous-deployment.md) para implementar a sua aplicação.  

## <a name="cli"></a>Implementar com a CLI do Azure

Pode utilizar a CLI do Azure para acionar uma implementação de push. Push implementar um ficheiro. zip para a sua aplicação de função utilizando o [az functionapp config origem de implementação de software-zip](/cli/azure/functionapp/deployment/source#az-functionapp-deployment-source-config-zip) comando. Para utilizar este comando, tem de utilizar a CLI do Azure da versão 2.0.21 ou posterior. Para ver qual estiver a utilizar a versão da CLI do Azure, utilize o `az --version` comando.

No comando seguinte, substitua o `<zip_file_path>` marcador de posição pelo caminho para a localização do ficheiro. zip. Substitua, também, `<app_name>` com o nome exclusivo da sua aplicação de função. 

```azurecli-interactive
az functionapp deployment source config-zip  -g myResourceGroup -n \
<app_name> --src <zip_file_path>
```
Este comando implementa os ficheiros de projeto do ficheiro. zip transferido para a aplicação de funções no Azure. Em seguida, reinicia a aplicação. Para ver a lista de implementações para esta aplicação de função, tem de utilizar as APIs REST.

Quando estiver a utilizar o CLI do Azure no seu computador local, `<zip_file_path>` é o caminho para o ficheiro. zip no seu computador. Também pode executar a CLI do Azure [Azure Cloud Shell](../cloud-shell/overview.md). Quando utilizar o Cloud Shell, primeiro tem de carregar o ficheiro. zip de implementação para a conta de ficheiros do Azure que está associada à sua Cloud Shell. Nesse caso, `<zip_file_path>` é a localização de armazenamento que utiliza a sua conta do Cloud Shell. Para obter mais informações, consulte [manter os ficheiros no Azure Cloud Shell](../cloud-shell/persisting-shell-storage.md).


[!INCLUDE [app-service-deploy-zip-push-rest](../../includes/app-service-deploy-zip-push-rest.md)]

[!INCLUDE [app-service-deploy-zip-push-custom](../../includes/app-service-deploy-zip-push-custom.md)]

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Implementação contínua para Funções do Azure](functions-continuous-deployment.md)

[.zip push deployment reference topic]: https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file
