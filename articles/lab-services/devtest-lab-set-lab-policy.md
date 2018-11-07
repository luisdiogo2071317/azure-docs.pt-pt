---
title: Gerir políticas de laboratório no Azure DevTest Labs | Documentos da Microsoft
description: Saiba como definir políticas de laboratório, como tamanhos de VM, VMs máximos por utilizador e automatização de encerramento.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.assetid: 7756aa64-49ca-45a0-9f90-0fd101c7be85
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: 378eb8c1f2070e8f4b28c221369938e2ff04e2f3
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51255186"
---
# <a name="manage-all-policies-for-a-lab-in-azure-devtest-labs"></a>Gerir todas as políticas para um laboratório no Azure DevTest Labs

Os laboratórios DevTest do Azure permite-lhe controlar o custo e minimize o desperdício em seus laboratórios através da gestão de políticas (definições) para cada um deles. Este artigo explica em detalhes passo a passo como definir cada política.  

## <a name="set-allowed-virtual-machine-sizes"></a>Conjunto de tamanhos de máquina virtual permitido
A política para definir os tamanhos VM permitidos ajuda a minimizar o desperdício de laboratório, permitindo-lhe especificar os tamanhos de VM são permitidos no laboratório. Se esta política estiver ativada, apenas os tamanhos de VM desta lista podem ser utilizados para criar VMs.

1. Na [portal do Azure](https://go.microsoft.com/fwlink/p/?LinkID=525040), selecione um laboratório e, em seguida, selecione **Konfigurace a zásady**.

    ![O laboratório configuração e políticas de acesso](./media/devtest-lab-set-lab-policy/policies-menu.png)

1. O laboratório **Konfigurace a zásady** painel, selecione **tamanhos de máquinas virtuais permitidos**.
   
    ![Tamanhos de máquinas de virtuais permitidas](./media/devtest-lab-set-lab-policy/allowed-vm-sizes.png)

1. Selecione **nos** para ativar esta política, e **desativar** desabilitá-lo.

1. Se ativar esta política, selecione um ou mais tamanhos de VM que podem ser criados no seu laboratório.

1. Selecione **Guardar**.

## <a name="set-virtual-machines-per-user"></a>Máquinas virtuais do conjunto por utilizador
A política para **máquinas virtuais por utilizador** permite-lhe especificar o número máximo de VMs que podem ser criadas por um utilizador individual. Se um utilizador tenta criar ou uma VM de afirmação quando o limite de utilizador tiver sido cumprido, uma mensagem de erro indica que a VM não pode ser criado/solicitadas. 

1. O laboratório **Konfigurace a zásady** painel, selecione **máquinas virtuais por utilizador**.
   
    ![Máquinas virtuais por utilizador](./media/devtest-lab-set-lab-policy/max-vms-per-user.png)

1. Selecione **Sim** para limitar o número de VMs por utilizador. Se não pretender limitar o número de VMs por utilizador, selecione **não**. Se selecionou **Sim**, introduza um valor numérico que indica o número máximo de VMs que podem ser criadas ou solicitadas por um utilizador. 

1. Selecione **Sim** para limitar o número de VMs que pode utilizar o SSD (disco de estado sólido). Se não pretender limitar o número de VMs que pode utilizar o SSD, selecione **não**. Se selecionou **Sim**, introduza um valor que indica o número máximo de VMs que podem ser criadas usando SSD. 

1. Selecione **Guardar**.

## <a name="set-virtual-machines-per-lab"></a>Máquinas virtuais do conjunto por laboratório
A política para **máquinas virtuais por laboratório** permite-lhe especificar o número máximo de VMs que podem ser criadas para o laboratório atual. Se um utilizador tenta criar uma VM, quando foi alcançado o limite de laboratório, uma mensagem de erro indica que não é possível criar a VM. 

1. O laboratório **Konfigurace a zásady** painel, selecione **máquinas virtuais por laboratório**.
   
    ![Máquinas virtuais por laboratório](./media/devtest-lab-set-lab-policy/max-vms-per-lab.png)

1. Selecione **Sim** para limitar o número de VMs por laboratório. Se não pretender limitar o número de VMs por laboratório, selecione **não**. Se selecionou **Sim**, introduza um valor numérico que indica o número máximo de VMs que podem ser criadas ou solicitadas por um utilizador. 

1. Selecione **Sim** para limitar o número de VMs que pode utilizar o SSD (disco de estado sólido). Se não pretender limitar o número de VMs que pode utilizar o SSD, selecione **não**. Se selecionou **Sim**, introduza um valor que indica o número máximo de VMs que podem ser criadas usando SSD. 

1. Selecione **Guardar**.

## <a name="set-auto-shutdown"></a>Encerramento do conjunto automático
A política de encerramento automático ajuda a minimizar o desperdício de laboratório, permitindo que especifique o tempo que VMs este laboratório são encerradas.

1. O laboratório **Konfigurace a zásady** painel, selecione **encerramento automático**.
   
    ![Automatické vypnutí](./media/devtest-lab-set-lab-policy/auto-shutdown.png)

1. Selecione **nos** para ativar esta política, e **desativar** desabilitá-lo.

1. Se ativar esta política, especifique o tempo (e o fuso horário) para encerrar a todas as VMs do laboratório atual.

1. Especifique **Sim** ou **não** para a opção Enviar uma notificação a 15 minutos antes do tempo de encerramento automático especificado. Se escolher **Sim**, introduza um ponto de final do URL do webhook ou um endereço de e-mail que especificar onde pretende que a notificação para ser publicado ou enviados. O utilizador recebe a notificação e é dada a opção para atrasar o encerramento.

   Para obter mais informações sobre os webhooks, consulte [criar um webhook ou uma função de Azure API](../azure-functions/functions-create-a-web-hook-or-api-function.md). 

1. Selecione **Guardar**.

Por predefinição, uma vez ativada, esta política aplica-se a todas as VMs do laboratório atual. Para remover esta definição de uma VM específica, abra o painel de gestão da VM e altere seu **encerramento automático** definição.

## <a name="set-auto-start"></a>Início do conjunto automático
A política de início automático permite-lhe especificar quando as VMs do laboratório atual devem ser iniciadas.  

1. O laboratório **Konfigurace a zásady** painel, selecione **início automático**.
   
    ![Início automático](./media/devtest-lab-set-lab-policy/auto-start.png)

2. Selecione **nos** para ativar esta política, e **desativar** desabilitá-lo.

3. Se ativar esta política, especifique a hora de início agendada, fuso horário e os dias da semana para os quais se aplica a hora. 

4. Selecione **Guardar**.

Uma vez ativada, esta política não é automaticamente aplicada a todas as VMs do laboratório atual. Para aplicar esta definição para uma VM específica, abra o painel de gestão da VM e altere seu **início automático** definição.

## <a name="set-expiration-date"></a>Definir data de expiração
Pode definir uma expiração data quando [criar a VM](devtest-lab-add-vm.md). Na **definições avançadas**, escolha o ícone de calendário para especificar uma data em que a VM é eliminada automaticamente. Por predefinição, a VM nunca expira ao fim.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Passos Seguintes
Depois de definidos e aplicados as várias configurações de diretiva VM para o laboratório, seguem-se algumas coisas a experimentar a seguir:

* [Compreender endereços IP partilhados](devtest-lab-shared-ip.md) -explica como partilhada IP endereços são utilizados no DevTest Labs para minimizar o número de endereços IP públicos, necessárias para ligar ao seu laboratório VMs.
* [Configurar gestão de custos](devtest-lab-configure-cost-management.md) -ilustra como usar o **tendência de custo estimado mensal** gráfico  
  Para ver o mês atual do estimado custo até à data e o custo previsto do mês final.
* [Criar uma imagem personalizada](devtest-lab-create-template.md) - quando cria uma VM, especifique uma base, que pode ser uma imagem personalizada ou uma imagem do Marketplace. Este artigo ilustra como criar uma imagem personalizada de um ficheiro VHD.
* [Configurar imagens do Marketplace](devtest-lab-configure-marketplace-images.md) - Azure DevTest Labs suporta a criação de VMs com base nas imagens do Azure Marketplace. Este artigo ilustra como especificar que, se houver, imagens do Azure Marketplace podem ser utilizadas durante a criação de VMs num laboratório.
* [Criar uma VM num laboratório](devtest-lab-add-vm.md) -ilustra como criar uma VM a partir de uma imagem base (qualquer um dos personalizado ou Marketplace) e como trabalhar com artefactos em sua VM.

