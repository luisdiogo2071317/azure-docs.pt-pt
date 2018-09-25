---
title: Como as versões de tempo de execução de funções do Azure de destino
description: As funções do Azure suporta várias versões do tempo de execução. Saiba como especificar a versão de tempo de execução de uma aplicação de funções alojada no Azure.
services: functions
documentationcenter: ''
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: conceptual
ms.date: 01/24/2018
ms.author: glenga
ms.openlocfilehash: 889a5a40409238462ee81d3bbd51ac6b77d28173
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46947493"
---
# <a name="how-to-target-azure-functions-runtime-versions"></a>Como as versões de tempo de execução de funções do Azure de destino

Uma aplicação de funções é executado numa versão específica do runtime das funções do Azure. Existem duas versões principais: [1.x e 2.x](functions-versions.md). Este artigo explica como configurar uma aplicação de funções no Azure para executar a versão que escolher. Para obter informações sobre como configurar um ambiente de desenvolvimento local para uma versão específica, consulte [codificar e testar as funções do Azure localmente](functions-run-local.md).

## <a name="automatic-and-manual-version-updates"></a>Versão automática e manual de atualizações

As funções permitem uma versão específica do runtime de destino utilizando o `FUNCTIONS_EXTENSION_VERSION` definição da aplicação na aplicação de funções. A aplicação de funções é mantida na versão principal especificada até explicitamente optar por mover para uma nova versão.

Se especificar apenas a versão principal ("~ 2" para 2.x ou "~ 1" para a versão 1.x), a aplicação de funções é atualizada automaticamente para novas versões secundárias do runtime logo que ficarem disponíveis. Novas versões secundárias não introduz alterações significativas. Se especificar uma versão secundária (por exemplo, "2.0.12345"), a aplicação de funções encontram-se no que a versão até que altere explicitamente. 

Quando uma nova versão estiver disponível ao público, uma linha de comandos no portal do dá-lhe a oportunidade de se mover para cima para essa versão. Depois de mover para uma nova versão, pode sempre utilizar o `FUNCTIONS_EXTENSION_VERSION` definição de aplicação para mover de volta para uma versão anterior.

Uma alteração para a versão de tempo de execução faz com que uma aplicação de funções reiniciar.

Os valores podem ser definidos no `FUNCTIONS_EXTENSION_VERSION` definição para ativar as atualizações automáticas de aplicação atualmente é "~ 1" para o runtime 1.x e "~ 2" para 2.x.

## <a name="view-the-current-runtime-version"></a>Ver a versão atual do tempo de execução

Utilize o procedimento seguinte para ver a versão de runtime atualmente a ser utilizada por uma aplicação de funções. 

1. Na [portal do Azure](https://portal.azure.com), navegue para a aplicação de função e, em **recursos configurado**, escolha **definições de aplicação de função**. 

    ![Selecione as definições de aplicação de função](./media/functions-versions/add-update-app-setting.png)

2. Na **as definições da aplicação de função** separador, localize a **versão de Runtime**. Tenha em atenção que a versão de runtime específica e a versão principal pedida. No exemplo abaixo, as funções\_extensão\_definição de aplicação de versão é definida como `~1`.
 
   ![Selecione as definições de aplicação de função](./media/functions-versions/function-app-view-version.png)

## <a name="target-a-version-using-the-portal"></a>Selecionar uma versão com o portal

Quando precisar de uma versão que não seja a versão principal atual ou a versão 2.0 como destino, defina o `FUNCTIONS_EXTENSION_VERSION` definição da aplicação.

1. Na [portal do Azure](https://portal.azure.com), navegue para a sua aplicação de função e, em **recursos configurado**, escolha **as definições da aplicação**.

    ![Selecione as definições de aplicação de função](./media/functions-versions/add-update-app-setting1a.png)

2. Na **as configurações do aplicativo** separador, encontre o `FUNCTIONS_EXTENSION_VERSION` e altere o valor para uma versão válida do 1.x runtime ou `~2` para a versão 2.0. Um til com a versão principal significa utilizar a versão mais recente dessa versão principal (por exemplo, "~ 1"). 

    ![Definir a definição de aplicação de função](./media/functions-versions/add-update-app-setting2.png)

3. Clique em **guardar** para guardar a atualização da definição de aplicação. 

## <a name="target-a-version-using-azure-cli"></a>Selecionar uma versão com a CLI do Azure

 Também pode definir o `FUNCTIONS_EXTENSION_VERSION` partir da CLI do Azure. Com a CLI do Azure, atualizar a definição da aplicação na aplicação de funções com o [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings#set) comando.

```azurecli-interactive
az functionapp config appsettings set --name <function_app> \
--resource-group <my_resource_group> \
--settings FUNCTIONS_EXTENSION_VERSION=<version>
```
Nesse código, substitua `<function_app>` com o nome da sua aplicação de função. Substitua, também `<my_resource_group>` com o nome do grupo de recursos para a sua aplicação de função. Substitua `<version>` com uma versão válida do 1.x runtime ou `~2` para a versão 2.x. 

Pode executar este comando a partir do [Azure Cloud Shell](../cloud-shell/overview.md) escolhendo **experimente** no exemplo de código anterior. Também pode utilizar o [localmente a CLI do Azure](/cli/azure/install-azure-cli) para executar este comando após a execução [início de sessão az](/cli/azure/reference-index#az-login) para iniciar sessão.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [O tempo de execução 2.0 no seu ambiente de desenvolvimento local de destino](functions-run-local.md)

> [!div class="nextstepaction"]
> [Consulte as notas de versão para versões de tempo de execução](https://github.com/Azure/azure-webjobs-sdk-script/releases)
