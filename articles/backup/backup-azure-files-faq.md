---
title: "FAQ sobre como Fazer Cópias de Segurança de Ficheiros do Azure"
description: "Este artigo dá detalhes sobre como proteger os Ficheiros do Azure no Azure. Esta síntese é apresentada no resultado da pesquisa."
services: backup
keywords: "Não adicione nem edite as palavras-chave sem consultar o perito em SEO."
author: markgalioto
ms.author: markgal
ms.date: 2/21/2018
ms.topic: tutorial
ms.service: backup
ms.workload: storage-backup-recovery
manager: carmonm
ms.openlocfilehash: d37e119709bc9d4643fcaa9512b5209d4139515e
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/22/2018
---
# <a name="questions-about-backing-up-azure-files"></a>Perguntas sobre a cópia de segurança de Ficheiros do Azure
Este artigo responde a questões comuns sobre a cópia de segurança de Ficheiros do Azure. Em algumas das respostas, existem ligações para os artigos que incluem informação abrangente. Também pode publicar perguntas sobre o serviço de Backup do Azure no [fórum de debate](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup).

Para analisar rapidamente as secções neste artigo, utilize as ligações à direita, em **Neste artigo**.

## <a name="configuring-the-backup-job-for-azure-files"></a>Configurar a tarefa de cópia de segurança dos Ficheiros do Azure

### <a name="why-cant-i-see-some-of-my-storage-accounts-i-want-to-protect-that-contain-valid-file-shares-br"></a>Por que motivo não vejo algumas das minhas Contas de Armazenamento que quero proteger, que contêm as Partilhas de ficheiros válidas? <br/>
A Cópia de Segurança dos Ficheiros do Azure está atualmente em Pré-visualização e apenas as Contas de Armazenamento suportadas podem ser configuradas para a Cópia de Segurança. Verifique se Conta de Armazenamento que procura é uma conta de armazenamento suportada.

### <a name="why-cant-i-see-some-of-my-file-shares-in-the-storage-account-when-im-trying-to-configure-backup-br"></a>Por que motivo não vejo algumas das minhas Partilhas de ficheiros na Conta de Armazenamento quando estou a tentar configurar a cópia de segurança? <br/>
Verifique se a partilha de Ficheiros já está protegida no mesmo cofre dos Serviços de Recuperação. Verifique se a partilha de Ficheiros que procura proteger não foi eliminada recentemente.

### <a name="why-cant-i-protect-file-shares-connected-to-a-sync-group-in-azure-files-sync-br"></a>Por que motivo não posso proteger as Partilhas de Ficheiros ligadas a um Grupo de Sincronização no Azure File Sync? <br/>
A proteção de Partilhas de Ficheiros do Azure ligadas aos Grupos de Sincronização está em pré-visualização limitada. Envie uma mensagem para [AskAzureBackupTeam@microsoft.com](email:askazurebackupteam@microsoft.com) com o ID da Subscrição para solicitar o acesso. 

### <a name="in-which-geos-can-i-back-up-azure-file-shares-br"></a>Em que área geográficas posso criar cópias de segurança de partilhas de Ficheiros do Azure? <br/>
A cópia de segurança das partilhas de Ficheiros do Azure encontra-se atualmente em pré-visualização e está disponível apenas nas seguintes áreas geográficas. 
-   Canadá Central (CNC)
-   Leste do Canadá (CE) 
-   EUA Central (CUS)
-   Ásia Oriental (EA)
-   Leste da Austrália (AE) 
-   Índia Central (INC) 
-   EUA Centro-Norte (NCUS) 
-   Sul do Reino Unido (UKS) 
-   Oeste do Reino Unido (UKW) 
-   EUA Centro-Oeste (WCUS)
-   EUA Oeste 2 (WUS 2)

A cópia de segurança de partilhas de Ficheiros do Azure estará disponível nas seguintes áreas geográficas a partir de *23 de fevereiro*.
-   Sudeste da Austrália (ASE) 
-   Sul do Brasil (BRS) 
-   EUA Leste (EUS) 
-   EUA Leste 2 (EUS2) 
-   Europa do Norte (NE) 
-   EUA Centro-Sul (SCUS) 
-   Sudeste Asiático (SEA)
-   Europa Ocidental (WE) 
-   EUA Oeste (WUS)  

Envie uma mensagem para [AskAzureBackupTeam@microsoft.com](email:askazurebackupteam@microsoft.com) se precisar de a utilizar numa área geográfica específica que não esteja listada acima.

### <a name="how-many-file-shares-can-i-protect-in-a-vaultbr"></a>Quantas partilhas de ficheiros posso proteger num Cofre?<br/>
Durante a pré-visualização, pode proteger Partilhas de ficheiros de um máximo de 25 Contas de Armazenamento por Cofre. Também pode proteger até 200 Partilhas de ficheiros num único cofre. 

## <a name="backup"></a>Cópia de segurança

### <a name="how-many-on-demand-backups-can-i-take-per-file-share-br"></a>Quantas cópias de segurança A Pedido posso fazer por partilha de ficheiros? <br/>
Em qualquer altura, pode ter até 200 Instantâneos de uma partilha de ficheiros, incluindo os captados pelo Azure Backup, conforme definido pela sua política. Se as cópias de segurança começam a falhar por estar a atingir este limite, elimine os pontos de restauro A Pedido em conformidade.

### <a name="after-enabling-virtual-networks-on-my-storage-account-the-backup-of-file-shares-in-the-account-started-failing-why"></a>Depois de ativar as Redes Virtuais na minha Conta de Armazenamento, a Cópia de Segurança das partilhas de ficheiros na conta começou a falhar. Porquê?
Atualmente, a cópia de segurança de Ficheiros do Azure não é suportada para as Contas de Armazenamento que têm Redes Virtuais ativadas. Desative as Redes Virtuais nas Contas de Armazenamento que quer criar cópias de segurança. 

## <a name="restore"></a>Restauro

### <a name="can-i-recover-from-a-deleted-file-share-br"></a>Posso recuperar a partir de uma partilha de ficheiros eliminada? <br/>
Ao tentar eliminar uma partilha de ficheiros, será apresentada a lista de cópias de segurança que também serão eliminadas se continuar e será pedida uma confirmação. Não pode restaurar a partir de uma partilha de ficheiros eliminada.

### <a name="can-i-restore-from-backups-if-i-stopped-protection-on-a-file-share-br"></a>Posso restaurar a partir de cópias de segurança se tiver parado a proteção numa partilha de ficheiros? <br/>
Sim. Se tiver escolhido **Reter Dados de Cópia de Segurança** quando parou a proteção, poderá restaurar a partir de todos os pontos de restauro existentes.

## <a name="manage-backup"></a>Gerir a Cópia de Segurança

### <a name="can-i-access-the-snapshots-taken-by-azure-backups-and-mount-it-br"></a>Posso aceder aos instantâneos realizados pelas Cópias de Segurança do Azure e montá-los? <br/>
Todos os Instantâneos realizados pelo Azure Backup podem ser acedidos ao utilizar a opção Ver Instantâneos no portal, no PowerShell ou na CLI. Pode montá-los através do procedimento indicado aqui.

### <a name="what-is-the-maximum-retention-i-can-configure-for-backups-br"></a>Qual é a retenção máxima que posso configurar para as Cópias de Segurança? <br/>
A cópia de segurança das partilhas de Ficheiros do Azure permite manter as cópias de segurança diárias até 120 dias.

### <a name="what-happens-when-i-change-the-backup-policy-for-a-file-share-br"></a>O que acontece quando altero a Política de cópias de segurança de uma partilha de ficheiros? <br/>
Quando é aplicada uma política nova a uma ou mais partilhas de ficheiros, a agenda e a retenção da política nova são seguidas. Se a retenção for estendida, os pontos de recuperação existentes serão marcados para que estejam em conformidade com a política nova. Se a retenção for reduzida, serão marcados para eliminação na tarefa de limpeza seguinte e, posteriormente, eliminados.


## <a name="see-also"></a>Consulte também
Esta informação é apenas sobre a cópia de segurança de Ficheiros do Azure. Para obter mais informações sobre outras áreas do Azure Backup, veja algumas destas outras FAQs sobre Cópias de Segurança:
-  [FAQ sobre o cofre dos Serviços de Recuperação](backup-azure-backup-faq.md)
-  [FAQ sobre as cópias de segurança de VMs do Azure](backup-azure-vm-backup-faq.md)
-  [FAQ sobre o agente do Azure Backup](backup-azure-file-folder-backup-faq.md)
