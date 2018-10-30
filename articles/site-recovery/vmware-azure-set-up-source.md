---
title: Configurar o ambiente de origem para a recuperação após desastre de VMs de VMware para o Azure com o Azure Site Recovery | Documentos da Microsoft
description: Este artigo descreve como configurar o ambiente no local para a recuperação após desastre de VMs de VMware para o Azure com o Azure Site Recovery.
services: site-recovery
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 10/29/2018
ms.author: ramamill
ms.openlocfilehash: 44b1cac461fa49b4bea19b7cf98f6038eb264492
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/30/2018
ms.locfileid: "50230731"
---
# <a name="set-up-the-source-environment-for-disaster-recovery-of-vmware-vms-to-azure"></a>Configurar o ambiente de origem para recuperação após desastre de VMs de VMware para o Azure

Este artigo descreve como configurar o seu ambiente no local de origem, para replicar VMs de VMware para o Azure. Ele inclui os passos para selecionar o seu cenário de replicação, a configuração de uma máquina no local que o servidor de configuração do Site Recovery, e a detetar automaticamente VMs no local. 

## <a name="prerequisites"></a>Pré-requisitos

O artigo pressupõe que já tem:

- Planejado sua implementação com a ajuda do [do Azure Site Recovery Deployment Planner](site-recovery-deployment-planner.md). Isto ajuda a alocar a largura de banda suficiente com base na sua taxa de alteração de dados diária, de acordo com o objetivo de ponto de recuperação pretendido (RPO).
- [Configurar recursos](tutorial-prepare-azure.md) no [portal do Azure](http://portal.azure.com).
- [Configurar o VMware no local](vmware-azure-tutorial-prepare-on-premises.md), incluindo uma conta dedicada para a deteção automática.

## <a name="choose-your-protection-goals"></a>Escolha os seus objetivos de proteção

1. Em **Cofres dos Serviços de Recuperação**, selecione o nome do cofre. Neste cenário, estamos a utilizar **ContosoVMVault**.
2. Em **Introdução**, selecione Site Recovery. Em seguida, selecione **Preparar Infraestrutura**.
3. Em **Objetivo de proteção** > **Onde estão localizadas as máquinas virtuais**, selecione **No local**.
4. Em **Para onde pretende replicar as máquinas**, selecione **Para o Azure**.
5. Em **As suas máquinas estão virtualizadas**, selecione **Sim, com o VMware vSphere Hypervisor**. Em seguida, selecione **OK**.

## <a name="set-up-the-configuration-server"></a>Configurar o servidor de configuração

Pode configurar o servidor de configuração como um VM de VMware no local através de um modelo de aplicativo de Virtualização aberto (OVA). [Saiba mais](concepts-vmware-to-azure-architecture.md) sobre os componentes que serão instalados na VM do VMware.

1. Saiba mais sobre o [pré-requisitos](vmware-azure-deploy-configuration-server.md#prerequisites) para implementação de servidor de configuração.
2. [Verificar números de capacidade](vmware-azure-deploy-configuration-server.md#capacity-planning) para implementação.
3. [Baixe](vmware-azure-deploy-configuration-server.md#download-the-template) e [importar](vmware-azure-deploy-configuration-server.md#import-the-template-in-vmware) o modelo de OVA para configurar um VM que executa o servidor de configuração de VMware no local. A licença fornecida com o modelo é uma licença de avaliação e é válida por 180 dias. Publicar este período, cliente tem de ativar o windows com uma licença procured.
4. Ative a VM de VMware, e [registá-lo](vmware-azure-deploy-configuration-server.md#register-the-configuration-server-with-azure-site-recovery-services) no cofre dos serviços de recuperação.

## <a name="azure-site-recovery-folder-exclusions-from-antivirus-program"></a>Exclusões de pastas de recuperação de sites do Azure do programa antivírus

### <a name="if-antivirus-software-is-active-on-source-machine"></a>Se o software antivírus estiver ativo na máquina de origem

Se a máquina de origem tiver um software antivírus, Active Directory, a pasta de instalação deve ser excluída. Então, exclua a pasta *C:\ProgramData\ASR\agent* para replicação uniforme.

### <a name="if-antivirus-software-is-active-on-configuration-server"></a>Se o Software antivírus estiver ativo no servidor de configuração

Excluir seguintes pastas a partir do software antivírus para a replicação uniforme e para evitar problemas de conectividade

 1. C:\Program Files\Microsoft Azure serviços de recuperação.
 2. C:\Program Files\Microsoft Azure Site Recovery Provider
 3. C:\Programas\Microsoft Azure Site Recovery Configuration Manager 
 4. Ferramenta de coleção de erro do C:\Program Files\Microsoft Azure Site Recovery 
 5. C:\thirdparty
 6. C:\Temp
 7. C:\strawberry
 8. C:\ProgramData\MySQL
 9. C:\Program ficheiros (x86) \MySQL
 10. C:\ProgramData\ASR
 11. C:\ProgramData\Microsoft Azure Site Recovery
 12. C:\ProgramData\ASRLogs
 13. C:\ProgramData\ASRSetupLogs
 14. C:\ProgramData\LogUploadServiceLogs
 15. C:\Inetpub
 16. Diretório de instalação do servidor de ASR. Por exemplo: E:\Program ficheiros (x86) \Microsoft Azure Site Recovery

### <a name="if-antivirus-software-is-active-on-scale-out-process-servermaster-target"></a>Se o Software antivírus estiver ativo na horizontal processar servidor/principal de destino

Excluir seguintes pastas a partir do software antivírus

1. C:\Program Files\Microsoft Azure Recovery Services Agent
2. C:\ProgramData\ASR
3. C:\ProgramData\ASRLogs
4. C:\ProgramData\ASRSetupLogs
5. C:\ProgramData\LogUploadServiceLogs
6. C:\ProgramData\Microsoft Azure Site Recovery
7. Diretório de instalação de servidor de processo, exemplo com balanceamento de carga de ASR: C:\Program Files (x86) \Microsoft Azure Site Recovery

## <a name="common-issues"></a>Problemas comuns
[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]

## <a name="next-steps"></a>Passos Seguintes
[Configurar o ambiente de destino](./vmware-azure-set-up-target.md) no Azure.
