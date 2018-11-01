---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: f7e023bcfeaa07a4ee9a80ccf4ec17120605c1ba
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/25/2018
ms.locfileid: "50133652"
---
## <a name="test"></a>Testar a função

Utilize cURL para testar a função implementada num computador Mac ou Linux ou utilize Bash no Windows. Execute o comando cURL seguinte, substituindo o marcador de posição `<app_name>` pelo nome da sua aplicação Function App. Anexe a cadeia de consulta `&name=<yourname>` ao URL.

```bash
curl https://<app_name>.azurewebsites.net/api/HttpTrigger?name=<yourname>
```  

![Resposta da função mostrada num browser.](./media/functions-test-function-code/functions-azure-cli-function-test-curl.png)  

Se não tiver cURL disponível na sua linha de comandos, introduza o mesmo URL na barra de endereço do browser. Mais uma vez, substitua o marcador de posição `<app_name>` pelo nome da aplicação de funções, anexe a cadeia de consulta `&name=<yourname>` ao URL e execute o pedido.

    https://<app_name>.azurewebsites.net/api/HttpTrigger?name=<yourname>
   
![Resposta da função mostrada num browser.](./media/functions-test-function-code/functions-azure-cli-function-test-browser.png)  
