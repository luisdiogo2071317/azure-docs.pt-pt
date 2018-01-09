---
title: "Tutorial do Centro de segurança do Azure – proteger os seus recursos de centro de segurança do Azure | Microsoft Docs"
description: "Este tutorial mostra como configurar uma hora VM aceder a política e uma política de controlo de aplicação."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/08/2018
ms.author: terrylan
ms.openlocfilehash: f0a32f90e68101f805a52427fab2d5bb29b94939
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/08/2018
---
# <a name="tutorial-protect-your-resources-with-azure-security-center"></a>Tutorial: Proteger os seus recursos de centro de segurança do Azure
Centro de segurança limita a exposição a ameaças, ao utilizar controlos de acesso e a aplicação para bloquear atividade maliciosa. Apenas na máquina virtual de tempo (VM) acesso reduz a exposição a ataques, permitindo negar acesso persistente a VMs. Em vez disso, forneça acesso controlado e auditado para VMs apenas quando necessário. Os controlos de aplicação adaptável ajudar a proteger as VMs contra software maligno controlando as aplicações que podem ser executado nas suas VMs. O Centro de Segurança utiliza machine learning para analisar os processos que estão a ser executados na VM e ajuda-o a aplicar regras de inclusão nas listas de permissões com base nessas informações.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Configurar uma política de acesso no tempo de VM
> * Configurar uma política de controlo de aplicações

Se não tiver uma subscrição do Azure, crie um [conta gratuita](https://azure.microsoft.com/pricing/free-trial/) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos
Para seguir as funcionalidades abrangidas neste tutorial, tem de ser no escalão de preço padrão de centro de segurança. Pode tentar padrão de centro de segurança sem custos para os primeira 60 dias. O guia de introdução [Onboard a subscrição do Azure Security Center Standard](security-center-get-started.md) explica-lhe como atualizar para Standard.

## <a name="manage-vm-access"></a>Gerir o acesso VM
Apenas no tempo de acesso VM pode ser utilizado para bloquear o tráfego de entrada para as VMs do Azure, reduzir a exposição a ataques, fornecendo acesso fácil para ligar a VMs, quando necessário.

Apenas no tempo de acesso VM está em pré-visualização.

As portas de gestão não precisam de estar aberta permanente. Apenas têm de estar aberta enquanto estiverem ligados à VM, por exemplo, para efetuar tarefas de gestão ou manutenção. Quando apenas na hora estiver ativada, o Centro de segurança utiliza regras de grupo de segurança de rede (NSG), que restringem o acesso às portas de gestão, pelo que não pode ser visados pelos atacantes.

1. No menu principal do Centro de segurança, selecione **apenas acesso de VM de tempo** em **avançadas defesa de NUVEM**.

  ![Acesso à VM just-in-time][1]

  **Apenas no acesso VM de tempo** fornece informações sobre o estado das suas VMs:

  - **Configurado** -VMs que tenham sido configuradas para suportar apenas de acesso VM de tempo.
  - **Recomendado** -VMs que podem suportar apenas no acesso VM de tempo, mas não foram configuradas para.
  - **Nenhuma recomendação** -razões que podem causar uma VM para não ser recomendado são:

    - Falta o NSG - o apenas no tempo a solução requer um NSG a ser implementada.
    - VMS clássicas - o Centro de segurança apenas do acesso VM de tempo atualmente suporta apenas as VMs implementadas através do Azure Resource Manager.
    - Outros - uma VM está nesta categoria se a apenas no tempo solução está desativada na política de segurança da subscrição ou o grupo de recursos ou se a VM está em falta um IP público e não tem um NSG no local.

2. Selecione uma VM recomendada e clique em **ativar JIT 1 VM** para configurar uma política de tempo para essa VM:

  Pode guardar a predefinição portas que recomenda do Centro de segurança ou pode adicionar e configurar uma porta de novo no qual pretende ativar a apenas na solução de tempo. Neste tutorial, vamos adicionar uma porta selecionando **adicionar**.

  ![Adicionar configuração de porta][2]

3. Em **configuração da porta adicionar**, que identifica a:

  - A porta
  - O tipo de protocolo
  - Intervalos de IP permitido origem IPs - permitido. para obter acesso após um pedido aprovado
  - Tempo máximo de pedido - janela de tempo máximo que pode ser aberta uma porta específica

4. Selecione **OK** para guardar.

## <a name="harden-vms-against-malware"></a>Proteger VMs contra software maligno
Controlos de aplicação adaptável ajudam-na definir um conjunto de aplicações que podem ser executados nos grupos de recursos configurados, que, entre outras vantagens, ajuda a proteger as suas VMs contra software maligno. O Centro de Segurança utiliza machine learning para analisar os processos que estão a ser executados na VM e ajuda-o a aplicar regras de inclusão nas listas de permissões com base nessas informações.

Controlos de aplicação adaptável está em pré-visualização. Esta funcionalidade só está disponível para máquinas do Windows.

1. Regressar ao menu principal do Centro de segurança. Em **avançadas defesa de NUVEM**, selecione **controlos de aplicação adaptável**.

   ![Controlos de aplicações adaptáveis][3]

  O **grupos de recursos** secção contém três separadores:

  - **Configurado**: lista de recursos de grupos que contém as VMs que foram configuradas com controlo de aplicação.
  - **Recomendado**: lista de grupos de recursos para que aplicação controlo é recomendado.
  - **Nenhuma recomendação**: lista de recursos de grupos que contenha VMs sem qualquer recomendações de controlo de aplicação. Por exemplo, VMs em que as aplicações estão sempre a ser alteradas e que não atingiram um estado estável.

2. Selecione o **recomendado** separador para obter uma lista de grupos de recursos com recomendações de controlo de aplicação.

  ![Recomendações de controlo de aplicação][4]

3. Selecione um grupo de recursos para abrir o **criar regras de controlo de aplicação** opção. Em **Selecionar VMs**, reveja a lista de VMs recomendadas e desmarque aquelas às quais não pretende aplicar o controlo de aplicações. Em **Selecionar processos para regras de inclusão em listas de permissões**, reveja a lista de aplicações recomendadas e desmarque aquelas às quais não pretende aplicar. A lista inclui:

  - **NOME**: O caminho completo da aplicação
  - **PROCESSOS**: quantas aplicações residem no cada caminho
  - **COMUNS**: "Sim" indica que estes processos foram executados na maioria das VMs deste grupo de recursos
  - **EXPLOITABLE**: um ícone de aviso indica se as aplicações podem ser utilizadas por um atacante para ignorar a listas brancas de aplicação. Recomenda-se que reveja estas aplicações antes da respetiva aprovação.

4. Depois de concluir as suas seleções, selecione **criar**.

## <a name="clean-up-resources"></a>Limpar recursos
Outras inícios rápidos e tutoriais nesta coleção tirar partido deste guia de introdução. Se pretender continuar com a trabalhar com inícios rápidos subsequentes e tutoriais, continuar a executar o escalão Standard e manter o aprovisionamento automático ativada. Se não pretender continuar ou pretende voltar para o escalão gratuito:

1. Regressar ao menu principal do Centro de segurança e selecionar **política de segurança**.
2. Selecione a subscrição ou a política que pretende voltar a gratuito. **Política de segurança** abre.
3. Em **componentes da política**, selecione **escalão de preço**.
4. Selecione **livres** para alterar a subscrição do padrão camada para o escalão gratuito.
5. Selecione **Guardar**.

Se pretender desativar aprovisionamento automático:

1. Regressar ao menu principal do Centro de segurança e selecionar **política de segurança**.
2. Selecione a subscrição que pretende desativar aprovisionamento automático.
3. Em **política de segurança – recolha de dados**, selecione **desativar** em **integração** para desativar aprovisionamento automático.
4. Selecione **Guardar**.

>[!NOTE]
> Desativar aprovisionamento automático não remover o Microsoft Monitoring Agent VMs do Azure onde o agente tiver sido aprovisionado. Desativar automática aprovisionamento limites monitorização de segurança para os seus recursos.
>

## <a name="next-steps"></a>Passos Seguintes
Neste tutorial, aprendeu a limitar a exposição a ameaças por:

> [!div class="checklist"]
> * Configurar um apenas acesso de VM de tempo de política para fornecer controladas e auditar o acesso a VMs apenas quando necessário
> * Configurar uma política de controlos de aplicação adaptável para controlar que aplicações podem ser executado nas suas VMs

Avançar para o próximo tutorial para saber mais sobre a responder a incidentes de segurança.

> [!div class="nextstepaction"]
> [Tutorial: Responder a incidentes de segurança](tutorial-security-incident.md)

<!--Image references-->
[1]: ./media/tutorial-protect-resources/just-in-time-vm-access.png
[2]: ./media/tutorial-protect-resources/add-port.png
[3]: ./media/tutorial-protect-resources/adaptive-application-control-options.png
[4]: ./media/tutorial-protect-resources/recommended-resource-groups.png
