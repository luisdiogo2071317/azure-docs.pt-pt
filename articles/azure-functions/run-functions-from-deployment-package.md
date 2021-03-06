---
title: Execute as suas funções do Azure a partir de um pacote | Documentos da Microsoft
description: Ter o tempo de execução de funções do Azure, executar as suas funções ao montar um ficheiro de pacote de implementação que contém os ficheiros de projeto de aplicação de função.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 09/10/2018
ms.author: glenga
ms.openlocfilehash: a0e643397372e5b132119a7c23f251ecec876916
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/11/2018
ms.locfileid: "44346582"
---
# <a name="run-your-azure-functions-from-a-package-file"></a>Execute as suas funções do Azure a partir de um ficheiro de pacote

> [!NOTE]
> A funcionalidade descrita neste artigo não está disponível para as funções no Linux.

No Azure, pode executar as suas funções diretamente a partir de um ficheiro de pacote de implementação na sua aplicação de função. A outra opção é implementar os ficheiros no `d:\home\site\wwwroot` diretório da sua aplicação de função.

Este artigo descreve os benefícios de executar as suas funções de um pacote. Ele também mostra como ativar esta funcionalidade na sua aplicação de função.

## <a name="benefits-of-running-from-a-package-file"></a>Benefícios da execução de um ficheiro de pacote
  
Existem várias vantagens para executar a partir de um ficheiro de pacote:

+ Reduz o risco de problemas de bloqueio de cópia de ficheiros.
+ Podem ser implementados para um aplicativo de produção (com reinício).
+ É possível ter certeza dos ficheiros que estão em execução na sua aplicação.
+ Melhora o desempenho das [implementações do Azure Resource Manager](functions-infrastructure-as-code.md).
+ Pode reduzir os tempos de arranque a frio, particularmente para funções de JavaScript com npm grandes árvores de pacote.

Para obter mais informações, consulte [este anúncio](https://github.com/Azure/app-service-announcements/issues/84).

## <a name="enabling-functions-to-run-from-a-package"></a>Ativar as funções para executar a partir de um pacote

Para ativar a sua aplicação de funções para execução a partir de um pacote, basta adicionar uma `WEBSITE_RUN_FROM_PACKAGE` na definição de suas definições de aplicação de função. O `WEBSITE_RUN_FROM_PACKAGE` definição pode ter um dos seguintes valores:

| Valor  | Descrição  |
|---------|---------|
|**`<url>`**  | Localização de um ficheiro de pacote específico que pretende executar. Ao utilizar o armazenamento de BLOBs, deve usar um contêiner privado com um [assinatura de acesso partilhado (SAS)](../vs-azure-tools-storage-manage-with-storage-explorer.md#attach-a-storage-account-by-using-a-shared-access-signature-sas) para ativar o runtime das funções aceder ao pacote. Pode utilizar o [Explorador de armazenamento do Azure](https://azure.microsoft.com/features/storage-explorer/) para carregar ficheiros de pacote para a conta de armazenamento de Blobs.         |
| **`1`**  | Execute a partir de um ficheiro de pacote no `d:\home\data\SitePackages` pasta da sua aplicação de função. Esta opção requer que a pasta para também tem um arquivo chamado `packagename.txt`. Este ficheiro contém apenas o nome do ficheiro de pacote na pasta, sem quaisquer espaços em branco. |

O código a seguir mostra uma aplicação de funções configurada para ser executado a partir de um ficheiro. zip alojado no armazenamento de Blobs do Azure:

![Definição de aplicação WEBSITE_RUN_FROM_ZIP](./media/run-functions-from-deployment-package/run-from-zip-app-setting-portal.png)

> [!NOTE]
> Atualmente, são suportados apenas ficheiros do pacote. zip.

## <a name="integration-with-zip-deployment"></a>Integração com a implementação de zip

[Zip implantação] [ Zip deployment for Azure Functions] é uma funcionalidade do serviço de aplicações do Azure que permite-lhe implementar o projeto de aplicação de função para o `wwwroot` diretório. O projeto é empacotado como um ficheiro de implementação. zip. As mesmas APIs que pode ser utilizadas para implementar o seu pacote para o `d:\home\data\SitePackages` pasta. Com o `WEBSITE_RUN_FROM_PACKAGE` valor de definição de aplicação do `1`, a implementação de zip APIs copiar o pacote para o `d:\home\data\SitePackages` pasta em vez de extrair os ficheiros para `d:\home\site\wwwroot`. Ele também cria o `packagename.txt` ficheiro. A aplicação de função for executada, em seguida, do pacote após um reinício, e `wwwroot` torna-se só de leitura. Para obter mais informações sobre a implantação do zip, consulte [Zip de implementação para as funções do Azure](deployment-zip-push.md).

## <a name="adding-the-websiterunfrompackage-setting"></a>Adicionar a definição de WEBSITE_RUN_FROM_PACKAGE

[!INCLUDE [Function app settings](../../includes/functions-app-settings.md)]

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Implementação contínua para Funções do Azure](functions-continuous-deployment.md)

[Zip deployment for Azure Functions]: deployment-zip-push.md
