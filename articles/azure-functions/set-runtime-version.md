---
title: "Como destino a versões de tempo de execução das funções do Azure"
description: "As funções do Azure suporta várias versões do tempo de execução. Saiba como especificar a versão de tempo de execução de uma aplicação de função alojada no Azure."
services: functions
documentationcenter: 
author: ggailey777
manager: cfowler
editor: 
ms.service: functions
ms.workload: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2018
ms.author: glenga
ms.openlocfilehash: 6fc84642050f4b7acfa2e3c5b4518135d6a97171
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/09/2018
---
# <a name="how-to-target-azure-functions-runtime-versions"></a>Como destino a versões de tempo de execução das funções do Azure

Uma aplicação de função é executada numa versão específica do tempo de execução das funções do Azure. Existem duas versões principais: [1. x e 2](functions-versions.md). Este artigo explica como configurar uma aplicação de função no Azure com a versão que selecionar. Para obter informações sobre como configurar um ambiente de desenvolvimento local para uma versão específica, consulte [código e teste das funções do Azure localmente](functions-run-local.md).

>[!IMPORTANT]   
> Tempo de execução funções do Azure 2.0 está em pré-visualização e são suportadas atualmente nem todas as funcionalidades das funções do Azure. Para obter mais informações, consulte [descrição geral de versões de tempo de execução das funções do Azure](functions-versions.md).

## <a name="automatic-and-manual-version-updates"></a>Versão automática e manual de atualizações

As funções permite-lhe uma versão específica do tempo de execução de destino utilizando o `FUNCTIONS_EXTENSION_VERSION` definição da aplicação na aplicação de função. A aplicação de função é mantida na versão principal especificado, até decidir explicitamente mover para uma nova versão.

Se especificar apenas a versão principal ("~ 1" para 1. x) ou "beta", para 2. x, a aplicação de função é atualizada automaticamente para novas versões de secundárias do tempo de execução que ficarem disponíveis. Novas versões de secundárias introduz alterações. Se especificar uma versão secundária (por exemplo, "1.0.11360"), a aplicação de função é mantida nessa versão até que altere explicitamente. 

Quando uma nova versão é publicamente disponível, numa linha no portal do dá-lhe a oportunidade para mover para cima para essa versão. Depois de mover para uma nova versão, é sempre possível utilizar o `FUNCTIONS_EXTENSION_VERSION` definição da aplicação para mover novamente para uma versão anterior.

Uma alteração para a versão de tempo de execução faz com que uma aplicação de função reiniciar.

Os valores que pode definir no `FUNCTIONS_EXTENSION_VERSION` aplicação definição para ativar as atualizações automáticas está atualmente "~ 1" para o tempo de execução de 1. x e "beta", para 2. x.

## <a name="view-the-current-runtime-version"></a>Ver a versão atual do tempo de execução

Utilize o procedimento seguinte para ver a versão de runtime atualmente utilizada por uma aplicação de função. 

1. No [portal do Azure](https://portal.azure.com), navegue para a aplicação de função e, em **configurado funcionalidades**, escolha **as definições de aplicação de função**. 

    ![Selecione as definições de aplicação de função](./media/functions-versions/add-update-app-setting.png)

2. No **as definições de aplicação de função** separador, localize o **versão Runtime**. Tenha em atenção que a versão de runtime específico e a versão principal pedida. No exemplo abaixo, as funções\_extensão\_definição de aplicação de versão é definida como `~1`.
 
   ![Selecione as definições de aplicação de função](./media/functions-versions/function-app-view-version.png)

## <a name="target-a-version-using-the-portal"></a>Uma versão através do portal de destino

Quando precisar de uma versão diferente da versão principal atual ou a versão 2.0 de destino, defina o `FUNCTIONS_EXTENSION_VERSION` definição da aplicação.

1. No [portal do Azure](https://portal.azure.com), navegue para a sua aplicação de função e, em **configurado funcionalidades**, escolha **definições da aplicação**.

    ![Selecione as definições de aplicação de função](./media/functions-versions/add-update-app-setting1a.png)

2. No **definições da aplicação** separador, localizar o `FUNCTIONS_EXTENSION_VERSION` definição e altere o valor para uma versão válida do tempo de execução de 1. x ou `beta` para a versão 2.0. Um til com a versão principal significa utilizar a versão mais recente do que a versão principal (por exemplo, "~ 1"). 

    ![Configure a definição de aplicação de função](./media/functions-versions/add-update-app-setting2.png)

3. Clique em **guardar** para guardar a atualização da definição de aplicação. 

## <a name="target-a-version-using-azure-cli"></a>Destino de uma versão utilizando a CLI do Azure

 Também pode definir o `FUNCTIONS_EXTENSION_VERSION` partir da CLI do Azure. Utilizar a CLI do Azure, atualizar a definição da aplicação na aplicação de função com o [az functionapp configuração appsettings conjunto](/cli/azure/functionapp/config/appsettings#set) comando.

```azurecli-interactive
az functionapp config appsettings set --name <function_app> \
--resource-group <my_resource_group> \
--settings FUNCTIONS_EXTENSION_VERSION=<version>
```
Neste código, substitua `<function_app>` com o nome da sua aplicação de função. Substitua, também `<my_resource_group>` com o nome do grupo de recursos para a sua aplicação de função. Substitua `<version>` com uma versão válida do tempo de execução de 1. x ou `beta` para a versão 2.0. 

Pode executar este comando a partir de [Shell de nuvem do Azure](../cloud-shell/overview.md) escolhendo **experimente** no exemplo de código anterior. Também pode utilizar o [localmente a CLI do Azure](/cli/azure/install-azure-cli) para executar este comando depois de executar [início de sessão az](/cli/azure/reference-index#az_login) para iniciar sessão.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [O tempo de execução 2.0 no seu ambiente de desenvolvimento local de destino](functions-run-local.md)

> [!div class="nextstepaction"]
> [Consulte as notas de versão para versões de tempo de execução](https://github.com/Azure/azure-webjobs-sdk-script/releases)