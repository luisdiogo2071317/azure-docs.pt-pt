---
title: Transferir e extrair o Kit de desenvolvimento de pilha do Azure (ASDK) | Microsoft Docs
description: Descreve como a transferência e extraiu o Kit de desenvolvimento de pilha do Azure (ASDK).
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/16/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 0cf389c9443a9cff477b884c277d72de27769afc
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/17/2018
ms.locfileid: "29975882"
---
# <a name="download-and-extract-the-azure-stack-development-kit-asdk"></a>Transferir e extrair o Kit de desenvolvimento de pilha do Azure (ASDK)
Depois de se certificar de que o computador de anfitrião de kit de desenvolvimento cumpre os requisitos básicos para instalar o ASDK, o passo seguinte é a mesma e extraiu o pacote de implementação para obter o Cloudbuilder.vhdx ASDK.

## <a name="download-the-asdk"></a>Transferir o ASDK
1. Antes de iniciar a transferência, certifique-se de que o computador cumpre os seguintes pré-requisitos:

  - O computador tem de ter, pelo menos, 60 GB de espaço livre em disco em quatro separadas, idênticas unidades lógicas de disco rígidas além para o disco de sistema operativo.
  - [.NET framework 4.6 (ou uma versão posterior)](https://aka.ms/r6mkiy) tem de estar instalado.

2. [Aceda à página Introdução ao](https://azure.microsoft.com/overview/azure-stack/try/?v=try) onde pode transferir o Kit de desenvolvimento de pilha do Azure, forneça os detalhes e, em seguida, clique em **submeter**.
3. Transfira e execute o [Verificador de implementação para a Azure pilha Development Kit](https://go.microsoft.com/fwlink/?LinkId=828735&clcid=0x409) script Verificador de pré-requisitos. Este script autónomo realiza as verificações de pré-requisitos efetuadas pelo programa de configuração para Kit de desenvolvimento de pilha do Azure. Fornece uma forma para confirmar que está a cumprir os requisitos de hardware e software, antes de transferir o pacote maior para Kit de desenvolvimento de pilha do Azure.
4. Em **transferir o software**, clique em **Kit de desenvolvimento de pilha do Azure**.

  > [!NOTE]
  > A transferência ASDK (AzureStackDevelopmentKit.exe) é cerca de 10GB.

## <a name="extract-the-asdk"></a>Extrair o ASDK
1. Depois de concluída a transferência, clique em **executar** para iniciar o ASDK Self-extractor (AzureStackDevelopmentKit.exe).
2. Rever e aceitar o contrato de licença apresentadas do **contrato de licença** página do Assistente de Self-Extractor e, em seguida, clique em **seguinte**.
3. Reveja as informações de declaração de privacidade apresentadas no **aviso importante** página do Assistente de Self-Extractor e, em seguida, clique em **seguinte**.
4. Selecione a localização dos ficheiros de configuração de pilha do Azure a ser extraído no **selecionar localização de destino** página do Assistente de Self-Extractor e, em seguida, clique em **seguinte**. A localização predefinida é *pasta atual*\Azure Kit de desenvolvimento de pilha. 
5. Reveja a resumo na localização de destino a **pronto para extrair** página do Assistente de Self-Extractor e, em seguida, clique em **extrair** para extrair o CloudBuilder.vhdx (aproximadamente 25 GB) e ThirdPartyLicenses.rtf ficheiros. Este processo demora algum tempo a concluir.
6. Copiar ou mover o ficheiro de CloudBuilder.vhdx na raiz da unidade C:\ (C:\CloudBuilder.vhdx) no computador anfitrião ASDK.

> [!NOTE]
> Depois de extrair os ficheiros, pode eliminar o. EXE e. Ficheiros de reciclagem para recuperar espaço no disco rígido. Em alternativa, pode criar cópias de segurança se estes ficheiros para que não precisa de transferir os ficheiros novamente se tiver de Reimplementar o ASDK.


## <a name="next-steps"></a>Passos Seguintes
[Preparar o computador de anfitrião ASDK](asdk-prepare-host.md)