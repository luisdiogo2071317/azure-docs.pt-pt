---
title: Remover Funções de Trabalho de Runbook Híbridas da Automatização do Azure
description: Este artigo fornece informações sobre gerir implementado Azure automatização de Runbook híbridos que permite a execução de runbooks em computadores no seu ambiente de nuvem ou centro de dados local.
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 03/19/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: 0b8f951bbd9712e3d20c3286ef3571e287499c68
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="remove-azure-automation-hybrid-runbook-workers"></a>Remover Funções de Trabalho de Runbook Híbridas da Automatização do Azure

A funcionalidade de trabalho de Runbook híbrida da automatização do Azure permite-lhe executar runbooks diretamente no computador que aloja a função e relativamente aos recursos no ambiente para gerir os recursos locais. Este artigo orienta-os passos para remover uma função de trabalho híbrida a partir de um computador local.

## <a name="removing-hybrid-runbook-worker"></a>Remover o Runbook Worker híbrido

Pode remover uma ou mais os Runbook Workers híbridos de um grupo ou pode remover o grupo dependendo dos seus requisitos.  Para remover um Runbook Worker híbrido a partir de um computador no local, execute os seguintes passos.

1. No portal do Azure, navegue até à sua conta de automatização.  
2. Do **definições** painel, selecione **chaves** e anote os valores de campo **URL** e **chave de acesso primária**.  Estas informações são necessárias para o passo seguinte.
3. Abra uma sessão do PowerShell no modo de administrador e execute o seguinte comando - `Remove-HybridRunbookWorker -url <URL> -key <PrimaryAccessKey>`.  Utilize o **-Verbose** mudar para um registo detalhado do processo de remoção.

> [!NOTE]
> Isto não remove o Microsoft Monitoring Agent no computador, apenas a funcionalidade e a configuração da função de trabalho de Runbook híbrida.  

## <a name="remove-hybrid-worker-groups"></a>Remova os grupos de trabalho híbrida

Para remover um grupo, terá primeiro de remover o Runbook Worker híbrido cada computador que seja membro do grupo utilizando o procedimento apresentado anteriormente e, em seguida, efetuar os seguintes passos para remover o grupo.  

1. Abra a conta de automatização no portal do Azure.
1. Em **automatização de processos** selecione **grupos de trabalho híbrida**. Selecione o grupo que pretende eliminar.  Depois de selecionar o grupo específico, o **grupo de trabalho híbridas** é apresentado o painel de propriedades.<br> ![Painel do grupo de trabalho de Runbook híbrida](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-group-properties.png)   
2. No painel de propriedades para o grupo selecionado, clique em **eliminar**.  É apresentada uma mensagem a pedir-lhe para confirmar esta ação, selecione **Sim** se tiver a certeza de que pretende prosseguir.<br> ![Caixa de diálogo de confirmação de eliminação de grupo](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-confirm-delete.png)<br> Este processo pode demorar alguns segundos até ser concluído e pode acompanhar o progresso em **Notificações**, no menu. 

## <a name="next-steps"></a>Próximos Passos

* Reveja [executar runbooks num Runbook Worker híbrido](automation-hrw-run-runbooks.md) para saber como configurar os runbooks para automatizar processos no seu centro de dados no local ou outro ambiente de nuvem.
* Para obter informações sobre como instalar os Runbook Workers híbridos Windows, consulte [Azure Automation Windows Runbook Worker híbrido](automation-windows-hrw-install.md).
* Para obter informações sobre como instalar os Runbook Workers híbridos Linux, consulte [Azure Automation Linux Runbook Worker híbrido](automation-linux-hrw-install.md).