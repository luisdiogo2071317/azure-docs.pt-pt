---
title: Configurar o Microsoft Azure Data Box Disk | Microsoft Docs
description: Utilize este tutorial para aprender a configurar o Azure Data Box Disk
services: databox
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox
ms.devlang: NA
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/28/2018
ms.author: alkohli
Customer intent: As an IT admin, I need to be able to order Data Box Disk to upload on-premises data from my server onto Azure.
ms.openlocfilehash: 6fcc7823a7e2f2f1e280622a1fa05d4417a71546
ms.sourcegitcommit: a1140e6b839ad79e454186ee95b01376233a1d1f
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/28/2018
ms.locfileid: "43143487"
---
# <a name="tutorial-unpack-connect-and-unlock-azure-data-box-disk"></a>Tutorial: Descompactar, ligar e desbloquear o Disco do Azure Data Box

Este tutorial descreve como descompactar, ligar e desbloquear o seu Disco do Azure Data Box.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Descompactar o Disco do Data Box
> * Ligar e desbloquear o Disco do Data Box.

## <a name="prerequisites"></a>Pré-requisitos

Antes de começar, certifique-se de que:

1. Concluiu o [Tutorial: Encomendar o Disco do Azure Data Box](data-box-disk-deploy-ordered.md).
2. Recebeu os seus discos e o estado da tarefa no portal foi atualizado para **Entregue**.
3. Possui um computador anfitrião no qual pode instalar a ferramenta de desbloqueio do Disco do Data Box. O computador anfitrião tem de
    - Executar um [sistema operativo suportado](data-box-disk-system-requirements.md).
    - Ter o [Windows PowerShell 4 instalado](https://www.microsoft.com/download/details.aspx?id=40855).
    - Ter o [.NET Framework 4.5.1 instalado](https://www.microsoft.com/download/details.aspx?id=30653).
    - Ter o [BitLocker ativado](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-how-to-deploy-on-windows-server).
    - Ter o [Windows Management Framework 4 instalado](https://www.microsoft.com/en-us/download/details.aspx?id=40855). 

## <a name="unpack-your-disks"></a>Descompactar os seus discos

 Siga os seguintes passos para descompactar os discos.

1. Os Discos do Data Box são enviados por e-mail numa pequena Caixa de envio. Abra a caixa e remova os seus conteúdos. Verifique se a caixa tem 1 a 5 discos de estado sólido (SSDs) e um cabo de ligação USB por disco. Inspecione a caixa em busca de qualquer evidência de adulteração ou outros danos óbvios. 

    ![Pacote de envio do Disco do Data Box](media/data-box-disk-deploy-set-up/data-box-disk-ship-package1.png)

2. Se a caixa de envio estiver violada ou gravemente danificada, não abra a caixa. Entre em contacto com o Suporte da Microsoft para ajudar a avaliar se os discos estão a funcionar corretamente e se é preciso enviar uma substituição.
3. Certifique-se de que a caixa tem uma capa clara que contém uma etiqueta de envio (sob a etiqueta atual) para fazer a devolução. Se esta etiqueta estiver em falta ou danificada, pode transferir e imprimir uma nova no portal do Azure. 

    ![Etiqueta de envio do Disco do Data Box](media/data-box-disk-deploy-set-up/data-box-disk-package-ship-label.png)

4. Guarde a caixa e a espuma do pacote para devolver os discos.

## <a name="connect-and-unlock-your-disks"></a>Ligar e desbloquear os discos

Siga os seguintes passos para ligar e desbloquear os discos.

1. Utilize o cabo incluído para ligar o disco a um computador Windows com um SO suportado, conforme indicado nos pré-requisitos. 

    ![Ligação do Data Box Disk](media/data-box-disk-deploy-set-up/data-box-disk-connect-unlock.png)    
    
2. No portal do Azure, aceda a **Geral > Detalhes do dispositivo**. 
3. Clique em **Transferir ferramenta de desbloqueio do Data Box Disk**. 

    ![Transferência da ferramenta de desbloqueio do Data Box Disk](media/data-box-disk-deploy-set-up/data-box-disk-unload1.png)     

4. Extraia a ferramenta no mesmo computador que irá utilizar para copiar os dados.
5. Abra uma janela da Linha de Comandos ou execute o Windows PowerShell como administrador no mesmo computador.
6. (Opcional) Para verificar se o computador que está a utilizar para desbloquear o disco cumpre os requisitos do sistema operativo, execute o comando de verificação do sistema. É apresentada abaixo uma saída de exemplo. 

    ```powershell
    Windows PowerShell
    Copyright (C) Microsoft Corporation. All rights reserved.
    
    PS C:\DataBoxDiskUnlockTool\DiskUnlock> .\DataBoxDiskUnlock.exe /SystemCheck
    Successfully verified that the system can run the tool.
    PS C:\DataBoxDiskUnlockTool\DiskUnlock>
    ``` 

7. No portal do Azure, aceda a **Geral > Detalhes do dispositivo**. Utilize o ícone de cópia para copiar a chave de acesso.
8. Execute `DataBoxDiskUnlock.exe` e indique a chave de acesso. A letra de unidade atribuída ao disco é apresentada. É apresentada abaixo uma saída de exemplo.

    ```powershell
    PS C:\WINDOWS\system32> cd C:\DataBoxDiskUnlockTool\DiskUnlock
    PS C:\DataBoxDiskUnlockTool\DiskUnlock> .\DataBoxDiskUnlock.exe
    Enter the passkeys (format: passkey1;passkey2;passkey3):
    testpasskey1
    
    Following volumes are unlocked and verified.
    Volume drive letters: D:
    
    PS C:\DataBoxDiskUnlockTool\DiskUnlock>
    ```

9. Repita os passos 6 a 8 para quaisquer futuras reinserções de discos. Utilize o comando de ajuda se precisar de ajuda com a ferramenta de desbloqueio de Disco do Data Box.   

    ```powershell
    PS C:\DataBoxDiskUnlockTool\DiskUnlock> .\DataBoxDiskUnlock.exe /help
    USAGE:
    DataBoxUnlock /PassKeys:<passkey_list_separated_by_semicolon>
    
    Example: DataBoxUnlock /PassKeys:<your passkey>
    Example: DataBoxUnlock /SystemCheck
    Example: DataBoxUnlock /Help
    
    /PassKeys:       Get this passkey from Azure DataBox Disk order. The passkey unlocks your disks.
    /SystemCheck:    This option checks if your system meets the requirements to run the tool.
    /Help:           This option provides help on cmdlet usage and examples.
    
    PS C:\DataBoxDiskUnlockTool\DiskUnlock>
    ```  
10. Assim que o disco está desbloqueado, pode ver os conteúdos do disco.    

    ![Conteúdos do Disco do Data Box](media/data-box-disk-deploy-set-up/data-box-disk-content.png) 

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, ficou a conhecer tópicos do Azure Data Box Disk, como:

> [!div class="checklist"]
> * Descompactar o Disco do Data Box
> * Ligar e desbloquear Discos do Data Box


Avance para o próximo tutorial para saber como copiar dados no Disco do Data Box.

> [!div class="nextstepaction"]
> [Copiar dados no Disco do Data Box](./data-box-disk-deploy-copy-data.md)

