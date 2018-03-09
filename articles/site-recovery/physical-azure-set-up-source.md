---
title: "Configurar o ambiente de origem (servidores físicos para o Azure) | Microsoft Docs"
description: "Este artigo descreve como configurar o seu ambiente no local para iniciar a replicar servidores físicos que executem Windows ou Linux no Azure."
services: site-recovery
documentationcenter: 
author: AnoopVasudavan
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 03/05/2018
ms.author: anoopkv
ms.openlocfilehash: 96004a70547c4bfb3a1a3bfadecb1304e4910b52
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/08/2018
---
# <a name="set-up-the-source-environment-physical-server-to-azure"></a>Configurar o ambiente de origem (servidor físico para o Azure)

Este artigo descreve como configurar o seu ambiente no local para iniciar a replicar servidores físicos que executem Windows ou Linux no Azure.

## <a name="prerequisites"></a>Pré-requisitos

O artigo pressupõe que já tem:
1. Dos serviços de recuperação do cofre no [portal do Azure](http://portal.azure.com "portal do Azure").
3. Um computador físico em que pretende instalar o servidor de configuração.

### <a name="configuration-server-minimum-requirements"></a>Requisitos mínimos do servidor de configuração
A tabela seguinte lista o mínimos de hardware, software e requisitos de rede para um servidor de configuração.
[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

> [!NOTE]
> Servidores proxy baseado em HTTPS não são suportadas pelo servidor de configuração.

## <a name="choose-your-protection-goals"></a>Escolha os seus objetivos de proteção

1. No portal do Azure, visite o **dos serviços de recuperação** cofres dos painel e selecione o cofre.
2. No **recursos** menu do cofre, clique em **introdução** > **recuperação de Site** > **passo 1: preparar a infraestrutura** > **objetivo de proteção**.

    ![Selecione os objetivos](./media/physical-azure-set-up-source/choose-goals.png)
3. No **objetivo de proteção**, selecione **para o Azure** e **não virtualizados/outras**e, em seguida, clique em **OK**.

    ![Selecione os objetivos](./media/physical-azure-set-up-source/physical-protection-goal.png)

## <a name="set-up-the-source-environment"></a>Configurar o ambiente de origem

1. No **preparar a origem**, se não tiver um servidor de configuração, clique em **+ o servidor de configuração** para adicionar um.

  ![Configurar a origem](./media/physical-azure-set-up-source/plus-config-srv.png)
2. No **Adicionar servidor** painel, verifique se **servidor de configuração** aparece no **tipo de servidor**.
4. Transfira o ficheiro de instalação de configuração do Unified Site Recovery.
5. Transfira a chave de registo do cofre. Tem a chave de registo ao executar a configuração do Unified. A chave é válida durante cinco dias depois de gerá-la.

    ![Configurar a origem](./media/physical-azure-set-up-source/set-source2.png)
6. No computador que está a utilizar como o servidor de configuração, execute **configuração de unificada do Azure Site Recovery** para instalar o servidor de configuração, o servidor de processos e o servidor de destino principal.

#### <a name="run-azure-site-recovery-unified-setup"></a>Configuração de unificada de execução do Azure Site Recovery

> [!TIP]
> Registo do servidor de configuração falha se a hora no relógio do sistema do computador é mais de cinco minutos retire hora local. Sincronizar o relógio do seu sistema com um [hora server](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service) antes de iniciar a instalação.

[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> O servidor de configuração pode ser instalado através de uma linha de comandos. Para obter mais informações, consulte [instalar servidor de configuração com as ferramentas da linha de comandos](http://aka.ms/installconfigsrv).


## <a name="common-issues"></a>Problemas comuns

[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]


## <a name="next-steps"></a>Passos Seguintes

Passo seguinte envolve [como configurar o ambiente de destino](physical-azure-set-up-target.md) no Azure.
