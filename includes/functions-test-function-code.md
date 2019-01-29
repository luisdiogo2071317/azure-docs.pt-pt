---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: 914c006daf49e22ebec870a549bfdbc63f882647
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/28/2019
ms.locfileid: "55148049"
---
## <a name="test"></a>Testar a função

Utilize o cURL para testar a função implementada num computador Mac ou Linux ou com o Powershell no Windows. Execute o comando cURL seguinte, substituindo o marcador de posição `<app_name>` pelo nome da sua aplicação Function App. Anexe a cadeia de consulta `&name=<yourname>` ao URL.

```powershell
Invoke-WebRequest -Uri "https://<app_name>.azurewebsites.net/api/MyHttpTrigger?name=<yourname>"
```

```bash
curl https://<app_name>.azurewebsites.net/api/MyHttpTrigger?name=<yourname>
```  

![Resposta da função mostrada num browser.](./media/functions-test-function-code/functions-azure-cli-function-test-curl.png)  

Se não tiver `cURL`ou `Invoke-WebRequest` disponíveis na sua linha de comandos, introduza o endereço do seu navegador da web, o mesmo URL. Mais uma vez, substitua o marcador de posição `<app_name>` pelo nome da aplicação de funções, anexe a cadeia de consulta `&name=<yourname>` ao URL e execute o pedido.

    https://<app_name>.azurewebsites.net/api/MyHttpTrigger?name=<yourname>
   
![Resposta da função mostrada num browser.](./media/functions-test-function-code/functions-azure-cli-function-test-browser.png)  
