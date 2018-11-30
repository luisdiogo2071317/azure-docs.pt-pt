---
author: cephalin
ms.service: app-service-web
ms.topic: include
ms.date: 10/24/2018
ms.author: cephalin
ms.openlocfilehash: e4bc749047bbf0d55fc60a699ac956421775d5b0
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/27/2018
ms.locfileid: "52423548"
---
Na [portal do Azure](https://portal.azure.com), clique em **grupos de recursos** > **cloud-shell-storage -\<your_region >**  >   **\<storage_account_name >**.

![Localizar a conta de armazenamento do Cloud Shell](../articles/app-service/media/app-service-deploy-zip/upload-choose-storage-account.png)

Na **descrição geral** página da conta de armazenamento, selecione **ficheiros**.

Selecione a partilha de ficheiros geradas automaticamente e selecione **carregar**. Esta partilha de ficheiros está montada no Cloud Shell como `clouddrive`.

![Encontrar o botão de carregamento](../articles/app-service/media/app-service-deploy-zip/upload-select-button.png)

Clique o Seletor de ficheiro e selecione o ficheiro ZIP, em seguida, clique em **carregar**. 

No Cloud Shell, utilize `ls` para verificar se pode ver o ficheiro ZIP carregado predefinida `clouddrive` partilhar.

```azurecli-interactive
ls clouddrive
```
