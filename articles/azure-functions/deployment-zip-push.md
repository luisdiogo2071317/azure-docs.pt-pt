---
title: "Zip push de implementação para as funções do Azure | Microsoft Docs"
description: "Utilize as instalações de implementação de ficheiro. zip do serviço de implementação do Kudu para publicar as suas funções do Azure."
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 12/06/2017
ms.author: glenga
ms.openlocfilehash: faddb73522200f60f18294dc43e8d235943f8bbb
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/20/2017
---
# <a name="zip-push-deployment-for-azure-functions"></a>Implementação de push zip para as funções do Azure 
Este artigo descreve como implementar os ficheiros de projeto de aplicação de função para o Azure a partir de um ficheiro (comprimido). zip. Saiba como efetuar uma implementação de push, utilizando a CLI do Azure e ao utilizar as APIs REST. 

As funções do Azure tem o intervalo completo de opções de implementação e a integração contínuas que são fornecidos pelo App Service do Azure. Para obter mais informações, consulte [a implementação contínua para as funções do Azure](functions-continuous-deployment.md). 

Para mais rápida iteração durante o desenvolvimento, muitas vezes, é mais fácil de implementar os ficheiros de projeto de aplicação de função diretamente a partir de um ficheiro. zip comprimido. Esta implementação de ficheiro. zip utiliza o mesmo serviço Kudu esse powers contínua com base na integração implementações, incluindo:

+ Eliminação de ficheiros que foram deixado de implementações anteriores.
+ Personalização da implementação, incluindo a execução de scripts de implementação.
+ Registos de implementação.
+ A sincronizar os acionadores de função num [plano de consumo](functions-scale.md) aplicação de função.

Para obter mais informações, consulte o [referência de implementação de push. zip](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file). 

## <a name="deployment-zip-file-requirements"></a>Requisitos de implementação do ficheiro. zip
O ficheiro. zip que utiliza para implementação de push tem de conter todos os ficheiros de projeto na sua aplicação de função, incluindo o código de função. 

>[!IMPORTANT]
> Quando utilizar a implementação de push. zip, são eliminados quaisquer ficheiros de uma implementação existente que não foi encontrados no ficheiro. zip da sua aplicação de função.  

### <a name="function-app-folder-structure"></a>Estrutura de pastas de aplicação de função

[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

### <a name="download-your-function-app-project"></a>Transfira o projeto de aplicação de função

Quando estiver a desenvolver num computador local, é fácil criar um ficheiro. zip da pasta do projeto de aplicação de função no seu computador de desenvolvimento. 

No entanto, pode ter criado as suas funções utilizando o editor no portal do Azure. Para transferir o projeto de aplicação de função a partir do portal: 

1. Iniciar sessão para o [portal do Azure](https://portal.azure.com)e, em seguida, aceda a sua aplicação de função.

2. No **descrição geral** separador, selecione **transferir conteúdo de aplicação**. Selecione as opções de transferência e, em seguida, selecione **transferir**.     

    ![Transferir o projeto de aplicação de função](./media/deployment-zip-push/download-project.png)

O ficheiro. zip transferido está no formato correto para a sua aplicação de função ser republicadas utilizando a implementação de push. zip.

Também pode transferir um ficheiro. zip de um repositório do GitHub. Tenha em atenção que ao transferir um repositório do GitHub como um ficheiro. zip, o GitHub adiciona um nível de pasta adicional para o ramo. Isto significa nível de pasta adicionais que não é possível implementar o ficheiro. zip diretamente como transferido-lo a partir do GitHub. Se estiver a utilizar um repositório do GitHub para manter a sua aplicação de função, deve utilizar [integração contínua](functions-continuous-deployment.md) para implementar a sua aplicação.  

## <a name="cli"></a>Implementar utilizando a CLI do Azure

Pode utilizar a CLI do Azure para acionar uma implementação de push. Push um ficheiro. zip implementar a sua aplicação de função utilizando o [az functionapp configuração origem de implementação de software-zip](/cli/azure/functionapp/deployment/source#az_functionapp_deployment_source_config_zip) comando. Para utilizar este comando, tem de utilizar a CLI do Azure versão 2.0.21 ou posterior. Para ver qual estiver a utilizar a CLI do Azure versão, utilize o `az --version` comando.

O seguinte comando, substitua o `<zip_file_path>` marcador de posição com o caminho para a localização do ficheiro. zip. Substitua, também, `<app_name>` com o nome exclusivo da sua aplicação de função. 

```azurecli-interactive
az functionapp deployment source config-zip  -g myResourceGroup -n \
<app_name> --src <zip_file_path>
```
Este comando implementa ficheiros de projeto a partir do ficheiro. zip transferido para a sua aplicação de função no Azure. Esta, em seguida, reiniciar a aplicação. Para ver a lista de implementações para esta aplicação de função, tem de utilizar as APIs REST.

Quando estiver a utilizar a CLI do Azure no seu computador local, `<zip_file_path>` é o caminho para o ficheiro. zip no seu computador. Também pode executar a CLI do Azure [Shell de nuvem do Azure](../cloud-shell/overview.md). Quando utilizar a Shell de nuvem, primeiro tem de carregar o ficheiro. zip de implementação para a conta de ficheiros do Azure associado à sua Shell de nuvem. Nesse caso, `<zip_file_path>` é a localização de armazenamento que utiliza a sua conta da Shell de nuvem. Para obter mais informações, consulte [manter os ficheiros na Shell de nuvem do Azure](../cloud-shell/persisting-shell-storage.md).


[!INCLUDE [app-service-deploy-zip-push-rest](../../includes/app-service-deploy-zip-push-rest.md)]

[!INCLUDE [app-service-deploy-zip-push-custom](../../includes/app-service-deploy-zip-push-custom.md)]

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Implementação contínua para Funções do Azure](functions-continuous-deployment.md)

[.zip push deployment reference topic]: https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file
