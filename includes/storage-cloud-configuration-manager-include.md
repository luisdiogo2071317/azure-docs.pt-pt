---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/26/2018
ms.author: tamram
ms.openlocfilehash: 3a065e5cd6e951544b3147d5833b4ad300ae5e30
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/26/2018
ms.locfileid: "50166502"
---
A [Biblioteca do Gestor de Configuração do Microsoft Azure para .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/) fornece uma classe para analisar uma cadeia de ligação a partir de um ficheiro de configuração. A classe [CloudConfigurationManager](https://msdn.microsoft.com/library/azure/mt634650.aspx) analisa as definições de configuração, independentemente de a aplicação cliente estar ou não em execução no ambiente de trabalho, num dispositivo móvel, numa máquina virtual do Azure ou num serviço cloud do Azure.

Para mencionar o pacote CloudConfigurationManager, adicione a seguinte diretiva `using`:

```csharp
using Microsoft.Azure; //Namespace for CloudConfigurationManager
```

Eis um exemplo que mostra como obter uma cadeia de ligação a partir de um ficheiro de configuração:

```csharp
// Parse the connection string and return a reference to the storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));
```

A utilização do Gestor de Configuração do Azure é opcional. De igual modo, pode utilizar uma API, tal como a [classe ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager.aspx) do .NET Framework.

