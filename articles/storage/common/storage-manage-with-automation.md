---
title: Gerir o armazenamento do Azure com a automatização do Azure
description: Saiba mais sobre como o serviço de automatização do Azure pode ser utilizado para gerir o armazenamento do Azure em escala.
services: storage, automation
author: tamram
ms.service: storage
ms.topic: article
ms.date: 01/10/2019
ms.author: tamram
ms.component: common
ms.openlocfilehash: 0cf8a1ee90757b9a89e7659282b2fd52fa436dea
ms.sourcegitcommit: f4b78e2c9962d3139a910a4d222d02cda1474440
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/12/2019
ms.locfileid: "54245077"
---
# <a name="managing-azure-storage-using-azure-automation"></a>Gerir o armazenamento do Azure com a automatização do Azure
Este guia apresenta-lhe para o serviço de automatização do Azure e como ele pode ser usado para simplificar a gestão de blobs, tabelas e filas de armazenamento do Azure.

## <a name="what-is-azure-automation"></a>O que é a Automatização do Azure?
[A automatização do Azure](https://azure.microsoft.com/services/automation/) é um serviço do Azure para simplificar a gestão da cloud através de automatização de processos. Utilizar a automatização do Azure, as tarefas de longa execução, manuais, propenso a erros e repetidas com frequência podem ser automatizadas para aumentar a confiabilidade e eficiência e reduzem o tempo de valor para a sua organização.

A automatização do Azure fornece um motor de execução do fluxo de trabalho fiáveis e de elevada disponibilidade que se dimensiona para atender às suas necessidades, à medida que aumenta a sua organização. Na automatização do Azure, processos podem ser iniciados manualmente, por sistemas de terceiros ou em intervalos agendados, de modo a que tarefas acontecem exatamente quando necessário.

Reduzir o overhead operacional e liberar IT / equipa do DevOps para se dedicar ao trabalho que adiciona business value movendo as tarefas de gestão na cloud a ser executada automaticamente pela automatização do Azure.

## <a name="how-can-azure-automation-help-manage-azure-storage"></a>Como pode automatização do Azure ajudar a gerir o armazenamento do Azure?
O armazenamento do Azure pode ser gerenciado na automatização do Azure, utilizando os cmdlets do PowerShell que estão disponíveis no [do Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx). A automatização do Azure tem estes cmdlets do PowerShell do armazenamento disponíveis de imediato, para que possa executar todos os seus BLOBs, tabela e tarefas de gestão de fila no serviço. Também pode emparelhar estes cmdlets na automatização do Azure com os cmdlets para outros serviços do Azure, para automatizar tarefas complexas em serviços do Azure e sistemas de terceiros.

O [Galeria de runbooks da automatização do Azure](https://azure.microsoft.com/blog/2014/10/07/introducing-the-azure-automation-runbook-gallery/) contém runbooks de equipe e da Comunidade de produto para começar a automatizar a gestão de armazenamento do Azure, outros serviços do Azure e sistemas de terceiros. Galeria runbooks incluem:

* [Remover os Blobs de armazenamento do Azure que são determinados dias antigo usando o serviço de automatização](https://gallery.technet.microsoft.com/scriptcenter/Remove-Storage-Blobs-that-aae4b761)
* [Transferir um Blob do armazenamento do Azure](https://gallery.technet.microsoft.com/scriptcenter/a-Blob-from-Azure-Storage-6bc13745)
* [Todos os discos de cópia de segurança para uma única VM do Azure ou para todas as VMs num serviço Cloud](https://gallery.technet.microsoft.com/scriptcenter/Backup-all-disks-for-a-ede940d5)

## <a name="next-steps"></a>Passos Seguintes
Agora que aprendeu as noções básicas da automatização do Azure e como ele pode ser usado para gerir blobs, tabelas e filas de armazenamento do Azure, siga estas ligações para saber mais sobre a automatização do Azure.

Veja o tutorial de automatização do Azure [criar ou importar um runbook na automatização do Azure](../../automation/automation-creating-importing-runbook.md).

