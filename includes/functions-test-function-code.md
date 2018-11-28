---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: ae88a6132c79d9d9f930f6445e53b0f0452c9f2a
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51262612"
---
## <a name="test"></a>Testar a função

Utilize cURL para testar a função implementada num computador Mac ou Linux ou utilize Bash no Windows. Execute o comando cURL seguinte, substituindo o marcador de posição `<app_name>` pelo nome da sua aplicação Function App. Anexe a cadeia de consulta `&name=<yourname>` ao URL.

```bash
curl https://<app_name>.azurewebsites.net/api/MyHttpTrigger?name=<yourname>
```  

![Resposta da função mostrada num browser.](./media/functions-test-function-code/functions-azure-cli-function-test-curl.png)  

Se não tiver cURL disponível na sua linha de comandos, introduza o mesmo URL na barra de endereço do browser. Mais uma vez, substitua o marcador de posição `<app_name>` pelo nome da aplicação de funções, anexe a cadeia de consulta `&name=<yourname>` ao URL e execute o pedido.

    https://<app_name>.azurewebsites.net/api/MyHttpTrigger?name=<yourname>
   
![Resposta da função mostrada num browser.](./media/functions-test-function-code/functions-azure-cli-function-test-browser.png)  
