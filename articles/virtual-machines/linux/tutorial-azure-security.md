---
title: Centro de Segurança do Azure e máquinas virtuais do Linux no Azure | Microsoft Docs
description: Saiba mais sobre a segurança da sua máquina virtual do Linux no Azure com o Centro de Segurança do Azure.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/07/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 6b791b2e1dbaffc90145c325dea7a85bd8abd98c
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/16/2018
---
# <a name="monitor-virtual-machine-security-by-using-azure-security-center"></a>Monitorizar a segurança da máquina virtual com o Centro de Segurança do Azure

O Centro de Segurança do Azure pode ajudá-lo a obter visibilidade para as práticas de segurança dos seus recursos do Azure. O Centro de Segurança oferece monitorização de segurança integrada. Pode detetar ameaças que, caso contrário, poderiam passar despercebidas. Neste tutorial, pode obter informações sobre o Centro de Segurança do Azure e como:
 
> [!div class="checklist"]
> * Configurar a recolha de dados
> * Configurar políticas de segurança
> * Ver e corrigir problemas de estado de funcionamento da configuração
> * Rever as ameaças detetadas  

## <a name="security-center-overview"></a>Descrição geral do Centro de Segurança

O Centro de Segurança identifica potenciais problemas de configuração de máquina virtual (VM) e ameaças de segurança direcionadas. Estes podem incluir as VMs em que faltam grupos de segurança de rede, discos não encriptados e ataques de protocolo RDP (Remote Desktop Protocol) de força bruta. As informações são mostradas no dashboard do Centro de Segurança, em gráficos fáceis de ler.

Para aceder ao dashboard do Centro de Segurança, no portal do Azure, no menu, selecione **Centro de Segurança**. No dashboard, pode ver o estado de funcionamento da segurança do seu ambiente do Azure, encontrar uma contagem de recomendações atuais e ver o estado atual de alertas de ameaça. Pode expandir cada gráfico de alto nível para ver mais detalhes.

![Dashboard Centro de Segurança](./media/tutorial-azure-security/asc-dash.png)

O Centro de Segurança vai mais além da deteção de dados para fornecer recomendações para problemas que deteta. Por exemplo, se uma VM tiver sido implementada sem um grupo de segurança de rede ligado à mesma, o Centro de Segurança apresenta uma recomendação, com passos de remediação que pode efetuar. Obtém a remediação automática sem sair do contexto do Centro de Segurança.  

![Recomendações](./media/tutorial-azure-security/recommendations.png)

## <a name="set-up-data-collection"></a>Configurar a recolha de dados

Antes de poder obter visibilidade das configurações de segurança de VM, terá de configurar a recolha de dados do Centro de Segurança. Isto envolve a ativação da recolha de dados e criar uma conta de armazenamento do Azure para armazenar os dados recolhidos. 

1. No dashboard Centro de Segurança, clique em **Política de Segurança** e, em seguida, selecione a sua subscrição. 
2. Em **Recolha de Dados**, selecione **Ligado**.
3. Para criar uma conta de armazenamento, selecione **Escolher uma conta de armazenamento**. Em seguida, selecione **OK**.
4. No painel **Política de Segurança**, selecione **Guardar**. 

O agente de recolha de dados do Centro de Segurança é então instalado em todas as VMs, e a recolha de dados começa. 

## <a name="set-up-a-security-policy"></a>Configurar uma política de segurança

As políticas de segurança são utilizadas para definir os itens para os quais o Centro de Segurança recolhe dados e faz recomendações. Pode aplicar políticas de segurança diferentes a diferentes conjuntos de recursos do Azure. Embora, por predefinição, os recursos do Azure sejam avaliados em relação a todos os itens de política, pode desativar os itens de política individuais para todos os recursos do Azure ou para um grupo de recursos. Para obter informações aprofundadas sobre as políticas de segurança do Centro de Segurança, veja [Definir políticas de segurança no Centro de Segurança do Azure](../../security-center/security-center-policies.md). 

Para configurar uma política de segurança para todos os recursos do Azure:

1. No dashboard Centro de Segurança, selecione **Política de Segurança** e, em seguida, selecione a sua subscrição.
2. Selecione **Política de prevenção**.
3. Ative ou desative os itens de política que pretende aplicar a todos os recursos do Azure.
4. Quando tiver terminado de selecionar as definições, selecione **OK**.
5. No painel **Política de segurança**, selecione **Guardar**. 

Para configurar uma política para um grupo de recursos específico:

1. No dashboard Centro de Segurança, selecione **Política de Segurança**e, em seguida, selecione um grupo de recursos.
2. Selecione **Política de prevenção**.
3. Ative ou desative os itens de política que pretende aplicar ao grupo de recursos.
4. Em **HERANÇA**, selecione **Única**.
5. Quando tiver terminado de selecionar as definições, selecione **OK**.
6. No painel **Política de segurança**, selecione **Guardar**.  

Também pode desativar a recolha de dados para um grupo de recursos específico nesta página.

No exemplo seguinte, uma política exclusiva foi criada para um grupo de recursos com o nome *myResourceGroup*. Nesta política, as recomendações de disco de encriptação e firewall de aplicações Web estão desativadas.

![Política exclusiva](./media/tutorial-azure-security/unique-policy.png)

## <a name="view-vm-configuration-health"></a>Ver o estado de funcionamento de configuração de VM

Depois de ter ativado a recolha de dados e definido uma política de segurança, o Centro de Segurança começa a disponibilizar recomendações e alertas. À medida que as VMs são implementadas, o agente de recolha de dados é instalado. O Centro de Segurança é então preenchido com os dados das novas VMs. Para obter informações aprofundadas sobre o estado de funcionamento de configuração de VM, veja [Proteger as suas VMs no Centro de Segurança](../../security-center/security-center-virtual-machine-recommendations.md). 

À medida que os dados são recolhidos, o estado de funcionamento de recursos de cada VM e os recursos do Azure relacionados são agregados. As informações são mostradas num gráfico de fácil leitura. 

Para ver o estados de funcionamento dos recursos:

1.  No dashboard do Centro de Segurança, em **Estado de funcionamento de segurança de recursos**, selecione **Computação**. 
2.  No painel **Computação**, selecione **Máquinas virtuais**. Esta vista fornece um resumo do estado de configuração de todas as suas VMs.

![Estado de funcionamento da computação](./media/tutorial-azure-security/compute-health.png)

Para ver todas as recomendações para uma VM, selecione a VM. As recomendações e a remediação são abordadas mais detalhadamente na secção seguinte deste tutorial.

## <a name="remediate-configuration-issues"></a>Remediar problemas de configuração

Depois de o Centro de Segurança começar a ser preenchido com dados de configuração, as recomendações são efetuadas com base na política de segurança que configurou. Por exemplo, se uma VM tiver sido configurada sem um grupo de segurança de rede associado, é feita uma recomendação para criar um. 

Para ver uma lista de todas as recomendações: 

1. No dashboard Centro de Segurança, selecione **Recomendações**.
2. Selecione uma recomendação específica. É apresentada uma lista de todos os recursos aos quais se aplica a recomendação.
3. Para aplicar uma recomendação, selecione um recurso específico. 
4. Siga as instruções dos passos de remediação. 

Em muitos casos, o Centro de Segurança fornece passos acionáveis que pode seguir para resolver uma recomendação sem sair do Centro de Segurança. No exemplo seguinte, o Centro de Segurança deteta um grupo de segurança de rede que tem uma regra de entrada sem restrições. Na página de recomendações, pode selecionar o botão **Editar regras de entrada**. A IU que é necessária para modificar a regra é apresentada. 

![Recomendações](./media/tutorial-azure-security/remediation.png)

À medida que as recomendações são resolvidas, são marcadas como resolvidas. 

## <a name="view-detected-threats"></a>Ver as ameaças detetadas

Além das recomendações de configuração de recursos, o Centro de Segurança apresenta os alertas de deteção de ameaças. A funcionalidade de alertas de segurança agrega os dados recolhidos de cada VM, os registos de rede do Azure e as soluções de parceiros ligadas para detetar ameaças de segurança relativamente aos recursos do Azure. Para obter informações aprofundadas sobre as capacidades de deteção do Centro de Segurança, veja [Capacidades de deteção do Centro de Segurança do Azure](../../security-center/security-center-detection-capabilities.md).

A funcionalidade de alertas de segurança requer que o escalão de preço do Centro de Segurança seja aumentado de *Gratuito* para *Standard*. Uma **avaliação gratuita** de 30 dias está disponível quando passar para este escalão de preço superior. 

Para alterar o escalão de preço:  

1. No dashboard Centro de Segurança, clique em **Política de Segurança** e, em seguida, selecione a sua subscrição.
2. Selecione **Escalão de preço**.
3. Selecione o novo escalão e, em seguida, selecione **Selecionar**.
4. No painel **Política de segurança**, selecione **Guardar**. 

Depois de alterar o escalão de preço, o gráfico de alertas de segurança começa a ser preenchido, à medida que são detetadas ameaças de segurança.

![Alertas de segurança](./media/tutorial-azure-security/security-alerts.png)

Selecione um alerta para ver as informações. Por exemplo, pode ver uma descrição da ameaça, a hora da deteção, todas as tentativas de ameaça e a remediação recomendada. No exemplo seguinte, foi detetado um ataque RDP de força bruta, com 294 tentativas falhadas de RDP. É fornecida uma resolução recomendada.

![Ataque RDP](./media/tutorial-azure-security/rdp-attack.png)

## <a name="next-steps"></a>Passos seguintes
Neste tutorial, configurou o Centro de Segurança do Azure e, em seguida, reviu VMs no Centro de Segurança. Aprendeu a:

> [!div class="checklist"]
> * Configurar a recolha de dados
> * Configurar políticas de segurança
> * Ver e corrigir problemas de estado de funcionamento da configuração
> * Rever as ameaças detetadas

Avance para o próximo tutorial para saber mais sobre como criar um pipeline de CI/CD com Jenkins, GitHub e Docker.

> [!div class="nextstepaction"]
> [Criar uma infraestrutura de CI/CD com Jenkins, GitHub, e Docker](tutorial-jenkins-github-docker-cicd.md)

