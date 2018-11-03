---
title: Utilizar o Emulator Express para executar e depurar um serviço cloud do Azure numa máquina local | Documentos da Microsoft
description: Utilizar o Emulator Express para executar e depurar um serviço em nuvem numa máquina local
services: visual-studio-online
documentationcenter: n/a
author: mikejo
manager: douge
editor: ''
ms.assetid: 73108f98-a552-4817-b7a1-551367b71906
ms.service: visual-studio-online
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/06/2017
ms.author: mikejo
ms.openlocfilehash: 3432e8778b2c5b5fd4db53a82ca23b55d75bcac5
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/02/2018
ms.locfileid: "50969539"
---
# <a name="using-emulator-express-to-run-and-debug-an-azure-cloud-service-on-a-local-machine"></a>Utilizar o Emulator Express para executar e depurar um serviço cloud do Azure numa máquina local
Ao utilizar o Emulator Express, pode testar e depurar um serviço em nuvem sem executar o Visual Studio como administrador. Pode definir suas configurações de projeto para utilizar o Emulator Express ou o emulador completo, dependendo dos requisitos do seu serviço cloud. Para obter mais informações sobre o emulador completo, consulte [executar uma aplicação do Azure no emulador de computação](storage/common/storage-use-emulator.md).

## <a name="using-emulator-express-in-visual-studio"></a>Utilizar o Emulator Express no Visual Studio
Quando cria um projeto do Azure no Azure SDK 2.3 ou posterior, Emulator Express é automaticamente utilizado. Para os projetos existentes que foram criados com uma versão anterior do SDK do Azure, utilize os seguintes passos para selecionar o Emulator Express:

1. Crie ou abra um projeto do serviço cloud do Azure no Visual Studio.

1. Na **Explorador de soluções**, clique com o botão direito no projeto e, no menu de contexto, selecione **propriedades**.

1. Nas páginas de propriedades de projetos, selecione o **Web** separador.

    ![Propriedades de um projeto do serviço cloud do Azure](./media/vs-azure-tools-emulator-express-debug-run/web-properties.png)

1. Sob **servidor de desenvolvimento Local**, selecione **opção Use IIS Express**.

1. Sob **emulador**, selecione **utilize o Emulator Express**.
   
1. Para iniciar o Emulator Express, execute o seguinte comando no prompt de comando: 

    ```
    csrun.exe /useemulatorexpress
    ```

## <a name="emulator-express-limitations"></a>Limitações de Express de emulador
Os seguintes problemas conhecidos limitações do emulador Express: 

- O emulator Express não é compatível com o servidor Web do IIS.
- Seu serviço cloud pode conter várias funções, mas cada função é limitada a uma instância.
- Não é possível aceder a números de porta abaixo 1000. Se utilizar um fornecedor de autenticação que normalmente utiliza uma porta abaixo 1000, poderá ter de alterar este valor para um número de porta que esteja acima de 1000.
- Quaisquer limitações que se aplicam ao emulador de computação do Azure também se aplicam ao Emulator Express. Por exemplo, não pode ter mais de 50 instâncias de função por implementação. Para obter mais informações sobre o emulador de computação do Azure, consulte [executar uma aplicação do Azure no emulador de computação](http://go.microsoft.com/fwlink/p/?LinkId=623050).

## <a name="next-steps"></a>Passos Seguintes
[Depuração de serviços cloud do Azure](https://msdn.microsoft.com/library/azure/ee405479.aspx)
