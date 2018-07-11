---
title: O Azure Site Recovery de resolução de problemas do VMware para o Azure | Documentos da Microsoft
description: Resolva erros ao replicar máquinas virtuais do Azure.
services: site-recovery
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.author: ramamill
ms.date: 07/06/2018
ms.openlocfilehash: 8d5db03eeebb659414ea1f554e5b34c938fd2795
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/10/2018
ms.locfileid: "37952914"
---
# <a name="troubleshoot-mobility-service-push-installation-issues"></a>Resolver problemas de instalação de push do serviço de mobilidade

Este artigo descreve como resolver erros comuns que poderá se depara ao tentar instalar o serviço de mobilidade do Azure Site Recovery no servidor de origem para ativar a proteção.

## <a name="error-78007---the-requested-operation-could-not-be-completed"></a>Erro 78007 - não foi possível concluir a operação pedida
Este erro pode ser lançado pelo serviço por vários motivos. Escolha o erro do fornecedor correspondente para resolver problemas ainda mais.

* [Erro 95103](#error-95103---protection-could-not-be-enabled-ep0854) 
* [Erro 95105](#error-95105---protection-could-not-be-enabled-ep0856) 
* [Erro 95107](#error-95107---protection-could-not-be-enabled-ep0858) 
* [Erro 95108](#error-95108---protection-could-not-be-enabled-ep0859) 
* [Erro 95117](#error-95117---protection-could-not-be-enabled-ep0865) 
* [Erro 95213](#error-95213---protection-could-not-be-enabled-ep0874) 
* [Erro 95224](#error-95224---protection-could-not-be-enabled-ep0883) 
* [Erro 95265](#error-95265---protection-could-not-be-enabled-ep0902) 


## <a name="error-95105---protection-could-not-be-enabled-ep0856"></a>Erro 95105 - não foi possível proteção ativada (EP0856)

**Código de erro** | **Causas possíveis** | **Recomendações de erro específico**
--- | --- | ---
95105 </br>**Mensagem:** instalação Push do serviço de mobilidade na máquina de origem falhou com o código de erro **EP0856**. <br> Qualquer um dos **impressora partilha de ficheiros e** não é permitida na origem da máquina ou há são de rede problemas de conectividade entre o servidor de processos e a máquina de origem.| **Impressora partilha de ficheiros e** não está ativada. | Permitir **impressora partilha de ficheiros e** na máquina de origem na Firewall do Windows. No computador de origem, sob **Firewall do Windows** > **permitir uma aplicação ou funcionalidade através da Firewall**, selecione **de ficheiros e partilha de impressoras em todos os perfis**. </br> Além disso, verifique os pré-requisitos seguintes para concluir com êxito a instalação de push.<br> Leia mais sobre [problemas de resolução de problemas de WMI](#troubleshoot-wmi-issues).


## <a name="error-95107---protection-could-not-be-enabled-ep0858"></a>Erro 95107 - não foi possível proteção ativada (EP0858)

**Código de erro** | **Causas possíveis** | **Recomendações de erro específico**
--- | --- | ---
95107 </br>**Mensagem:** instalação Push do serviço de mobilidade na máquina de origem falhou com o código de erro **EP0858**. <br> Ou as credenciais fornecidas para instalar o serviço de mobilidade estão incorretas ou a conta de utilizador não tem privilégios suficientes. | Credenciais de utilizador fornecidas para instalar o serviço de mobilidade na máquina de origem estão incorretas. | Certifique-se de que as credenciais de utilizador fornecidas para a máquina de origem no servidor de configuração estão corretas. <br> Para adicionar ou editar credenciais de utilizador, vá para o servidor de configuração e selecione **Cspsconfigtool** > **gerir conta**. </br> Além disso, verifique o seguinte [pré-requisitos](vmware-azure-install-mobility-service.md#install-mobility-service-by-push-installation-from-azure-site-recovery) para concluir com êxito a instalação de push.


## <a name="error-95117---protection-could-not-be-enabled-ep0865"></a>Erro 95117 - não foi possível proteção ativada (EP0865)

**Código de erro** | **Causas possíveis** | **Recomendações de erro específico**
--- | --- | ---
95117 </br>**Mensagem:** instalação Push do serviço de mobilidade na máquina de origem falhou com o código de erro **EP0865**. <br> O computador de origem não está em execução ou existem problemas de conectividade de rede entre o servidor de processos e a máquina de origem. | Problemas de conectividade de rede entre o servidor de processos e o servidor de origem. | Verifique a conectividade entre o servidor de processos e o servidor de origem. </br> Além disso, verifique o seguinte [pré-requisitos](vmware-azure-install-mobility-service.md#install-mobility-service-by-push-installation-from-azure-site-recovery) para concluir com êxito a instalação de push.|

## <a name="error-95103---protection-could-not-be-enabled-ep0854"></a>Erro 95103 - não foi possível proteção ativada (EP0854)

**Código de erro** | **Causas possíveis** | **Recomendações de erro específico**
--- | --- | ---
95103 </br>**Mensagem:** instalação Push do serviço de mobilidade na máquina de origem falhou com o código de erro **EP0854**. <br> O Windows Management Instrumentation (WMI) não é permitida na máquina de origem ou existem problemas de conectividade de rede entre o servidor de processos e a máquina de origem.| WMI está bloqueado na Firewall do Windows. | Permitir WMI na Firewall do Windows. Sob **Firewall do Windows** > **permitir uma aplicação ou funcionalidade através da Firewall**, selecione **WMI em todos os perfis**. </br> Além disso, verifique o seguinte [pré-requisitos](vmware-azure-install-mobility-service.md#install-mobility-service-by-push-installation-from-azure-site-recovery) para concluir com êxito a instalação de push.|

## <a name="error-95213---protection-could-not-be-enabled-ep0874"></a>Erro 95213 - não foi possível proteção ativada (EP0874)

**Código de erro** | **Causas possíveis** | **Recomendações de erro específico**
--- | --- | ---
95213 </br>**Mensagem:** instalação do serviço de mobilidade para a origem de máquina % SourceIP; falhou com o código de erro **EP0874**. <br> | Não é suportada a versão do sistema operativo na máquina de origem. <br>| Certifique-se de que a máquina de origem é suportada a versão do SO. Leitura a [matriz de suporte](https://aka.ms/asr-os-support). </br> Além disso, verifique o seguinte [pré-requisitos](https://aka.ms/pushinstallerror) para concluir com êxito a instalação de push.| 


## <a name="error-95108---protection-could-not-be-enabled-ep0859"></a>Erro 95108 - não foi possível proteção ativada (EP0859)

**Código de erro** | **Causas possíveis** | **Recomendações de erro específico**
--- | --- | ---
95108 </br>**Mensagem:** instalação Push do serviço de mobilidade na máquina de origem falhou com o código de erro **EP0859**. <br>| Ou as credenciais fornecidas para instalar o serviço de mobilidade estão incorretas ou a conta de utilizador não tem privilégios suficientes. <br>| Certifique-se de que as credenciais fornecidas são a **raiz** credenciais da conta. Para adicionar ou editar credenciais de utilizador, vá para o servidor de configuração e selecione o **Cspsconfigtool** ícone de atalho no ambiente de trabalho. Selecione **gerir conta** para adicionar ou editar credenciais.|

## <a name="error-95265---protection-could-not-be-enabled-ep0902"></a>Erro 95265 - não foi possível proteção ativada (EP0902)

**Código de erro** | **Causas possíveis** | **Recomendações de erro específico**
--- | --- | ---
95265 </br>**Mensagem:** instalação Push do serviço de mobilidade para a máquina de origem foi efetuada com êxito, mas a máquina de origem requer um reinício para algumas alterações entrem em vigor do sistema. <br>| Uma versão mais antiga do serviço de mobilidade já foi instalada no servidor.| Replicação da máquina virtual continuará de forma totalmente integrada.<br> Reinicie o servidor durante a janela de manutenção seguinte para obter os benefícios dos novos melhoramentos no serviço de mobilidade.|


## <a name="error-95224---protection-could-not-be-enabled-ep0883"></a>Erro 95224 - não foi possível proteção ativada (EP0883)

**Código de erro** | **Causas possíveis** | **Recomendações de erro específico**
--- | --- | ---
95224 </br>**Mensagem:** Push instalação do serviço de mobilidade na máquina de origem % SourceIP; falhou com o código de erro **EP0883**. Está pendente um reinício de sistema de uma instalação anterior ou a atualização.| O sistema não foi reiniciado durante a desinstalação de uma versão mais antiga ou incompatível do serviço de mobilidade.| Certifique-se de que nenhuma versão do serviço de mobilidade existe no servidor. <br> Reinicie o servidor e execute novamente a tarefa de proteção de ativação.|

## <a name="resource-to-troubleshoot-push-installation-problems"></a>Recursos para resolver problemas de instalação de push

#### <a name="troubleshoot-file-and-print-sharing-issues"></a>Resolver problemas de ficheiros e partilha de problemas de impressão
* [Ativar ou desativar a partilha com diretiva de grupo de ficheiros](https://technet.microsoft.com/library/cc754359(v=ws.10).aspx)
* [Ativar a partilha de impressão através da Firewall do Windows e de ficheiros](https://technet.microsoft.com/library/ff633412(v=ws.10).aspx)

#### <a name="troubleshoot-wmi-issues"></a>Resolver problemas WMI
* [Teste básico de WMI](https://blogs.technet.microsoft.com/askperf/2007/06/22/basic-wmi-testing/)
* [Resolução de problemas de WMI](https://msdn.microsoft.com/library/aa394603(v=vs.85).aspx)
* [Resolução de problemas com scripts do WMI e serviços WMI](https://technet.microsoft.com/library/ff406382.aspx#H22)

## <a name="next-steps"></a>Passos Seguintes

[Saiba como](vmware-azure-tutorial.md) para configurar a recuperação após desastre para VMs de VMware.