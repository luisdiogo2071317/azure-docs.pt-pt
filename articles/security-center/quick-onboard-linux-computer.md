---
title: "Centro de segurança do Azure guia de introdução - carregar os seus computadores Linux ao centro de segurança | Microsoft Docs"
description: "Este guia de introdução mostra-lhe como integrar os seus computadores Linux ao centro de segurança."
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
ms.openlocfilehash: 365afd2199b1b8b2e70d882f6a729384edbdffbc
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/08/2018
---
# <a name="quickstart-onboard-linux-computers-to-azure-security-center"></a>Início rápido: Computadores com Linux carregar ao centro de segurança do Azure
Depois de carregar as subscrições do Azure, pode ativar o Centro de segurança para recursos de Linux em execução fora do Azure, de exemplo no local ou num outras nuvens, pelo agente Linux de aprovisionamento.

Este guia de introdução mostra como instalar o agente Linux num computador Linux.

## <a name="prerequisites"></a>Pré-requisitos
Para começar a utilizar o Centro de Segurança, tem de possuir uma subscrição do Microsoft Azure. Se não tiver uma subscrição, pode inscrever-se numa [conta gratuita](https://azure.microsoft.com/pricing/free-trial/).

Tem de ser no escalão de preço padrão de centro de segurança antes de iniciar este guia de introdução. Consulte [Onboard a subscrição do Azure Security Center Standard](security-center-get-started.md) para obter instruções de atualização. Pode tentar padrão de centro de segurança sem custos para os primeira 60 dias.

## <a name="add-new-linux-computer"></a>Adicionar novo computador com Linux

1. Inicie sessão no [Portal do Azure](https://azure.microsoft.com/features/azure-portal/).
2. No menu **Microsoft Azure**, selecione **Centro de Segurança**. **Centro de segurança - descrição geral** abre.

 ![Descrição geral do Centro de segurança][2]

3. No menu principal do Centro de segurança, selecione **integração de segurança avançada**.
4. Selecione **que pretende adicionar computadores não Azure**.
   ![Segurança carregar avançadas][3]

5. No **adicionar novos computadores não Azure**, é apresentada uma lista de áreas de trabalho as análise de registos. A lista inclui, se aplicável, a área de trabalho predefinido criada pelo centro de segurança quando o aprovisionamento automático foi ativado. Selecione esta área de trabalho ou outra área de trabalho que pretende utilizar.

    ![Adicionar o computador não do Azure][4]

6.  No **agente direta** página **transferir e carregar agente para LINUX**, selecione o **cópia** botão para copiar o *wget* comando.

7.  Abra o bloco de notas e cole este comando. Guarde este ficheiro para uma localização que pode ser acessível a partir do seu computador Linux.

## <a name="install-the-agent"></a>Instalar o agente

1.  No seu computador Linux, abra o ficheiro que foi guardado anteriormente. Selecionar a cópia de conteúdo, completa, abra uma consola de terminal e cole o comando.
2.  Depois da instalação estiver concluída, pode validar que o *omsagent* está instalado, executando o *pgrep* comando. O comando devolverá o *omsagent* PID (ID de processo) conforme mostrado abaixo:

  ![Instalar o agente][5]

Os registos para o agente do Centro de segurança para Linux podem ser encontrados em: */var/optar ativamente porparticipar/microsoft/omsagent/<workspace id>/log/*

  ![Registos para o agente][6]

Após algum tempo, pode demorar até 30 minutos, o novo computador Linux será apresentada no Centro de segurança.

Agora pode monitorizar as VMs do Azure e não do Azure computadores num único local. Em **computação**, tem uma descrição geral de todas as VMs e computadores, juntamente com recomendações. Cada coluna representa um conjunto de recomendações. A cor representa da VM ou do computador atual estado de segurança dessa recomendação. Centro de segurança também apresenta qualquer deteções para estes computadores nos alertas de segurança.

  ![Painel de computação][7] existem dois tipos de ícones representados no **computação** painel:

  ![icon1](./media/quick-onboard-linux-computer/security-center-monitoring-icon1.png) Computador não-Azure

  ![icon2](./media/quick-onboard-linux-computer/security-center-monitoring-icon2.png) VM do Azure

## <a name="clean-up-resources"></a>Limpar recursos
Quando já não é necessário, pode remover o agente do computador Linux.

Para remover o agente:

1. Transferir o agente Linux [universal script](https://github.com/Microsoft/OMS-Agent-for-Linux/releases) ao computador.
2. Execute o ficheiro de .sh do pacote com o *– remover* argumento no computador, o que remove completamente a respetiva configuração e o agente.

    `sudo sh ./omsagent-<version>.universal.x64.sh --purge`

## <a name="next-steps"></a>Passos Seguintes
Neste início rápido, que aprovisionou o agente num computador Linux. Para saber mais sobre como utilizar o Centro de segurança, avance para o tutorial para configurar uma política de segurança e avaliar a segurança dos seus recursos.

> [!div class="nextstepaction"]
> [Tutorial: Definir e avaliar as políticas de segurança](tutorial-security-policy.md)

<!--Image references-->
[1]: ./media/quick-onboard-linux-computer/portal.png
[2]: ./media/quick-onboard-linux-computer/overview.png
[3]: ./media/quick-onboard-linux-computer/onboard-windows-computer.png
[4]: ./media/quick-onboard-linux-computer/add-computer.png
[5]: ./media/quick-onboard-linux-computer/pgrep-command.png
[6]: ./media/quick-onboard-linux-computer/logs-for-agent.png
[7]: ./media/quick-onboard-linux-computer/compute.png
