---
title: Serviços cloud de opções para a depuração do Azure | Documentos da Microsoft
description: Depuração de serviços cloud do Azure
services: visual-studio-online
documentationcenter: n/a
author: mikejo
manager: douge
editor: ''
ms.assetid: 80755da7-8350-4f5c-97ce-2962beabb36d
ms.service: visual-studio-online
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/18/2017
ms.author: mikejo
ms.openlocfilehash: e4ca0f710257530a05acdec2b240ae2870d4fa80
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/02/2018
ms.locfileid: "50969308"
---
# <a name="learn-the-various-ways-to-debug-an-azure-cloud-service"></a>Aprender as várias formas para depurar um serviço cloud do Azure
Este artigo fornece ligações para as várias formas para depurar um serviço cloud do Azure. 

## <a name="debugging-an-azure-cloud-service-in-visual-studio"></a>Depurar um serviço cloud do Azure no Visual Studio
Pode poupar tempo e dinheiro ao utilizar o Azure computação emulador para depurar o seu serviço cloud numa máquina local. Ao depurar um serviço localmente antes de a implementar, pode melhorar a confiabilidade e desempenho sem ter de adquirir tempo de computação. No entanto, poderão ocorrer alguns erros apenas quando executa um serviço em nuvem no Azure. Erros que ocorrem apenas quando executa um serviço em nuvem no Azure podem ser depurados ao ativar a depuração remota, quando publica o seu serviço e, em seguida, anexar o depurador para uma instância de função. Para obter mais informações, consulte [depurar o seu serviço cloud no seu computador local](vs-azure-tools-debug-cloud-services-virtual-machines.md#debug-your-cloud-service-on-your-local-computer).

## <a name="using-intellitrace"></a>Usando o IntelliTrace 
Se estiver a utilizar o Visual Studio Enterprise para escrever funções alvo o .NET Framework 4.5, pode ativar o IntelliTrace no momento que implementar um serviço cloud do Azure a partir do Visual Studio. IntelliTrace fornece um registo que pode utilizar com o Visual Studio para depurar seu aplicativo, como se estivesse em execução no Azure. Para obter mais informações, consulte [depurar um serviço cloud publicado com o IntelliTrace e o Visual Studio](http://go.microsoft.com/fwlink/p/?LinkId=623016).

## <a name="remote-debugging"></a>Depuração remota 
Pode ativar a depuração remota em seus serviços cloud no momento ao implementar o serviço em nuvem do Visual Studio. Se optar por ativar a depuração remota para uma implementação, a depuração remotos de serviços são instalados nas máquinas virtuais que são executados a cada instância de função. Estes serviços - como `msvsmon.exe` - não afetam o desempenho ou resultar em custos adicionais. Para obter mais informações, consulte [depurar um serviço cloud no Azure](vs-azure-tools-debug-cloud-services-virtual-machines.md#debug-a-cloud-service-in-azure).

## <a name="next-steps"></a>Passos Seguintes
- [Depurar um serviço cloud do Azure ou a VM no Visual Studio](./vs-azure-tools-debug-cloud-services-virtual-machines.md) -saiba os detalhes de como depurar serviços cloud do Azure.