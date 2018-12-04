---
title: Resolver problemas com o servidor de configuração durante a recuperação após desastre de VMs de VMware e servidores físicos para o Azure com o Azure Site Recovery | Documentos da Microsoft
description: Este artigo fornece informações de resolução de problemas para implementar o servidor de configuração para recuperação após desastre de VMs de VMware e servidores físicos para o Azure com o Azure Site Recovery.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 11/27/2018
ms.author: ramamill
ms.openlocfilehash: 4faadc27648b0d944e61a4d390313a35b4ba8bfa
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52837737"
---
# <a name="troubleshoot-configuration-server-issues"></a>Resolver problemas do servidor de configuração

Este artigo ajuda-o a resolver problemas quando implementar e gerir o [do Azure Site Recovery](site-recovery-overview.md) servidor de configuração. O servidor de configuração é utilizado quando configurar a recuperação após desastre para VMs de VMware no local e servidores físicos para o Azure com o Site Recovery. 

## <a name="installation-failures"></a>Falhas na instalação

| **Mensagem de erro** | **Ação recomendada** |
|--------------------------|------------------------|
|ERRO    Falha de erro ao carregar as Contas. Erro: System.IO.IOException: Não é possível ler dados a partir da ligação de transporte ao instalar e registar o servidor CS.| Certifique-se de que o TLS 1.0 está ativado no computador. |

## <a name="registration-failures"></a>Falhas de registo

Falhas de registo podem ser depuradas usando o l < br/ogs na pasta %ProgramData%\ASRLogs.

As falhas de registo podem ser depuradas ao consultar os registos na pasta **%ProgramData%\ASRLogs**.

| **Mensagem de erro** | **Ação recomendada** |
|--------------------------|------------------------|
|**09:20:06**:InnerException.Type: SrsRestApiClientLib.AcsException,InnerException.<br>Mensagem: ACS50008: O token SAML é inválido.<br>ID de rastreio: 1921ea5b-4723-4be7-8087-a75d3f9e1072<br>ID de correlação: 62fea7e6-2197-4be4-a2c0-71ceb7aa2d97 ><br>Carimbo de data/hora: **2016-12-12 14:50:08Z<br>** | Certifique-se de que a hora no relógio do seu sistema não excede mais de 15 minutos da hora local. Volte a executar o instalador para concluir o registo.|
|**09:35:27** :DRRegistrationException ao tentar obter todos os cofres da recuperação após desastre para o certificado selecionado: : Threw Exception.Type:Microsoft.DisasterRecovery.Registration.DRRegistrationException, Exception.Message: ACS50008: o token SAML é inválido.<br>ID de rastreio: e5ad1af1-2d39-4970-8eef-096e325c9950<br>ID de correlação: abe9deb8-3e64-464d-8375-36db9816427a<br>Carimbo de data/hora: **2016-05-19 01:35:39Z**<br> | Certifique-se de que a hora no relógio do seu sistema não excede mais de 15 minutos da hora local. Volte a executar o instalador para concluir o registo.|
|06:28:45: Falha ao criar certificado<br>06:28:45: Não é possível continuar a configuração. Não é possível criar um certificado necessário para autenticar no Site Recovery. Execute Novamente a Configuração | Certifique-se de que está a executar o programa de configuração como administrador local. |
