---
title: A verificar o sistema de ficheiros quando se reinicia uma VM do Azure | Documentos da Microsoft
description: Saiba como resolver o problema que VM mostram o sistema de ficheiros de bancária quando se reinicia | Documentos da Microsoft
services: virtual-machines-windows
documentationCenter: ''
authors: genlin
manager: cshepard
editor: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 08/31/2018
ms.author: genli
ms.openlocfilehash: 71619bed63d5d53e6d24eac434ce5a696a445165
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/31/2018
ms.locfileid: "43345199"
---
# <a name="windows-shows-checking-file-system-when-booting-an-azure-vm"></a>Mostra Windows "a verificar o sistema de ficheiros" quando se reinicia uma VM do Azure
Este artigo descreve o erro "A verificar o sistema de ficheiros", que poderá encontrar ao inicializar uma Máquina Virtual do Windows (VM) no Microsoft Azure.

> [!NOTE] 
> O Azure tem dois modelos de implementação diferentes para criar e trabalhar com recursos: [Resource Manager e clássica](../../azure-resource-manager/resource-manager-deployment-model.md). Este artigo descreve a utilizar o modelo de implementação do Resource Manager, que recomendamos que utilize para novas implementações em vez do modelo de implementação clássica.

## <a name="symptom"></a>Sintoma 

Uma VM do Windows não iniciar. Quando verifica as capturas de ecrã de arranque [diagnósticos de arranque](./boot-diagnostics.md), verá que o processo de verificação de disco (chkdsk.exe) está em execução com uma das seguintes mensagens:

- A analisar e reparar unidade (c)
- A verificar o sistema de arquivos em c:

## <a name="cause"></a>Causa

Se não for encontrado um erro NTFS no sistema de arquivos, o Windows irão verificar e reparar a consistência do disco na próxima reinicialização. Normalmente, isto acontece se a VM tiver qualquer reinício inesperado, ou se o processo de encerramento VM foi interrompido abruptamente.

## <a name="solution"></a>Solução 

Windows irão arrancar normalmente depois de concluído o processo de verificar o disco. Se a VM está bloqueada no processo de verificação de disco, tente executar o disco de verificação na VM offline:
1.  Tire um instantâneo do disco do SO da VM afetado como uma cópia de segurança. Para obter mais informações, consulte [instantâneo de um disco](snapshot-copy-managed-disk.md).
2.  [Anexar o disco do SO a uma VM de recuperação](.\troubleshoot-recovery-disks-portal.md).  
3.  Na VM de recuperação, execute o disco de verificação no disco do SO anexado. No exemplo seguinte, a letra de unidade de disco do SO anexado está e: 
        
        chkdsk E: /f
4.  Depois de concluir o disco de verificação, desanexe o disco da VM de recuperação e, em seguida, volte a anexar o disco à VM afetado como um disco de SO. Para obter mais informações, consulte [resolver problemas de uma VM do Windows ao anexar o disco do SO a uma VM de recuperação](.\troubleshoot-recovery-disks-portal.md).
