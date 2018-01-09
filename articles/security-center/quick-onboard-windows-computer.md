---
title: "Centro de segurança do Azure guia de introdução - carregar os computadores do Windows para o Centro de segurança | Microsoft Docs"
description: "Este guia de introdução mostra-lhe como aprovisionar o Microsoft Monitoring Agent num computador Windows."
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
ms.date: 01/07/2018
ms.author: terrylan
ms.openlocfilehash: 50cbbca9181d67bc41632a4650c76b9636a72356
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/08/2018
---
# <a name="quickstart-onboard-windows-computers-to-azure-security-center"></a>Início rápido: Computadores com Windows carregar ao centro de segurança do Azure
Depois de carregar as subscrições do Azure, pode ativar o Centro de segurança para os recursos a ser executados fora do Azure, de exemplo no local ou em outras nuvens, por o aprovisionamento do Microsoft Monitoring Agent.

Este guia de introdução mostra como instalar o Microsoft Monitoring Agent num computador Windows.

## <a name="prerequisites"></a>Pré-requisitos
Para começar a utilizar o Centro de Segurança, tem de possuir uma subscrição do Microsoft Azure. Se não tiver uma subscrição, pode inscrever-se numa [conta gratuita](https://azure.microsoft.com/pricing/free-trial/).

Tem de ser no escalão de preço padrão de centro de segurança antes de iniciar este guia de introdução. Consulte [Onboard a subscrição do Azure Security Center Standard](security-center-get-started.md) para obter instruções de atualização. Pode tentar padrão de centro de segurança sem custos para os primeira 60 dias.

## <a name="add-new-windows-computer"></a>Adicionar novo computador com o Windows

1. Inicie sessão no [Portal do Azure](https://azure.microsoft.com/features/azure-portal/).
2. No menu **Microsoft Azure**, selecione **Centro de Segurança**. **Centro de segurança - descrição geral** abre.

 ![Descrição geral do Centro de segurança][2]

3. No menu principal do Centro de segurança, selecione **integração de segurança avançada**.
4. Selecione **que pretende adicionar computadores não Azure**.

   ![Segurança carregar avançadas][3]

5. No **adicionar novos computadores não Azure**, é apresentada uma lista de áreas de trabalho as análise de registos. A lista inclui, se aplicável, a área de trabalho predefinido criada pelo centro de segurança quando o aprovisionamento automático foi ativado. Selecione esta área de trabalho ou outra área de trabalho que pretende utilizar.

    ![Adicionar o computador não do Azure][4]

  O **agente direta** painel abre-se com uma ligação para transferir o agente do Windows e chaves para o seu ID da área de trabalho utilizar na configuração do agente.

6.  Selecione o **transferir o agente do Windows** ligação aplicável ao seu tipo de processador do computador para transferir o ficheiro de configuração.

7.  À direita do **ID da área de trabalho**, selecione o ícone de copiar e colar o ID no bloco de notas.

8.  À direita do **chave primária**, selecione o ícone de copiar e colar a chave no bloco de notas.

## <a name="install-the-agent"></a>Instalar o agente
Agora tem de instalar o ficheiro transferido no computador de destino.

1. Copie o ficheiro para o computador de destino e execute a configuração.
2. No **boas-vindas** página, selecione **seguinte**.
3. No **termos de licenciamento** página, leia a licença e, em seguida, selecione **concordo**.
4. No **pasta de destino** página, altere ou mantenha a pasta de instalação predefinida e, em seguida, selecione **seguinte**.
5. No **opções de configuração do agente** página, escolha ligar o agente para análise de registos do Azure (OMS) e, em seguida, selecione **seguinte**.
6. No **Log Analytics do Azure** página, cole o **ID da área de trabalho** e **chave da área de trabalho (chave primária)** que copiou no bloco de notas no procedimento anterior.
7. Se o computador deve reportar uma área de trabalho de análise de registos na nuvem do Azure Government, selecione **Azure US Government** formulário a **nuvem do Azure** na lista pendente.  Se o computador tem de comunicar através de um servidor proxy para o serviço de análise de registos, selecione **avançadas** e forneça o URL e o número de porta do servidor proxy.
8. Selecione **seguinte** depois de concluir a fornecer as definições de configuração necessárias.

  ![Instalar o agente][5]

9. No **pronto para instalar** página, reveja as suas opções e, em seguida, selecione **instalar**.
10. No **configuração concluída com sucesso** página, selecione **concluir**

Quando terminar, o **Microsoft Monitoring Agent** aparece no **Painel de Controlo**. Pode rever a configuração não existe e certifique-se de que o agente está ligado.

Para obter mais informações sobre como instalar e configurar o agente, consulte [computadores Windows ligar](../log-analytics/log-analytics-agent-windows.md#install-the-agent-using-setup).

Agora pode monitorizar as VMs do Azure e não do Azure computadores num único local. Em **computação**, tem uma descrição geral de todas as VMs e computadores, juntamente com recomendações. Cada coluna representa um conjunto de recomendações. A cor representa da VM ou do computador atual estado de segurança dessa recomendação. Centro de segurança também apresenta qualquer deteções para estes computadores nos alertas de segurança.

  ![Painel de computação][6]

Existem dois tipos de ícones representados no **computação** painel:

![icon1](./media/quick-onboard-windows-computer/security-center-monitoring-icon1.png) Computador não-Azure

![icon2](./media/quick-onboard-windows-computer/security-center-monitoring-icon2.png) VM do Azure

## <a name="clean-up-resources"></a>Limpar recursos
Quando já não é necessário, pode remover o agente do computador Windows.

Para remover o agente:

1. Abra o **Painel de Controlo**.
2. Abra **Programas e Funcionalidades**.
3. Em **Programas e Funcionalidades**, selecione **Microsoft Monitoring Agent** e clique em **Desinstalar**.

## <a name="next-steps"></a>Passos Seguintes
Neste guia de introdução que aprovisionou o Microsoft Monitoring Agent num computador Windows. Para saber mais sobre como utilizar o Centro de segurança, avance para o tutorial para configurar uma política de segurança e avaliar a segurança dos seus recursos.

> [!div class="nextstepaction"]
> [Tutorial: Definir e avaliar as políticas de segurança](tutorial-security-policy.md)

<!--Image references-->
[2]: ./media/quick-onboard-windows-computer/overview.png
[3]: ./media/quick-onboard-windows-computer/onboard-windows-computer.png
[4]: ./media/quick-onboard-windows-computer/add-computer.png
[5]: ./media/quick-onboard-windows-computer/log-analytics-mma-setup-laworkspace.png
[6]: ./media/quick-onboard-windows-computer/compute.png
