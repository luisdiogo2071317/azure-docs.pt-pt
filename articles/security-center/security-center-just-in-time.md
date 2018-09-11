---
title: Apenas na máquina de virtual de tempo de acesso no Centro de segurança do Azure | Documentos da Microsoft
description: Este documento demonstra como just-in-tempo de acesso à VM no Centro de segurança do Azure ajuda-o a controlar o acesso a máquinas virtuais do Azure.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: ''
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/05/2018
ms.author: rkarlin
ms.openlocfilehash: 2a079456813a67eb40d5cf42bcdd2c91fbc631d3
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/10/2018
ms.locfileid: "44297044"
---
# <a name="manage-virtual-machine-access-using-just-in-time"></a>Gerir o acesso de máquina virtual com just-in-time

Máquina virtual (VM) just-in acesso pode ser utilizado para bloquear o tráfego de entrada para as VMs do Azure, reduzindo a exposição a ataques, fornecendo acesso fácil para ligar a VMs quando necessário.

> [!NOTE]
> A apenas no tempo a funcionalidade está disponível no escalão Standard do Centro de segurança.  Veja [Preços](security-center-pricing.md) para saber mais sobre os escalões de preços do Centro de Segurança.
>
>

## <a name="attack-scenario"></a>Cenário de ataque

Força bruta normalmente ataques portas de gestão de destino como um meio para obter acesso a uma VM. Se tiver êxito, um invasor pode assumir o controlo de ativação pós-falha da VM e estabelecer sua posição no seu ambiente.

É uma forma de reduzir a exposição a um ataque de força bruta limitar a quantidade de tempo que uma porta está aberta. As portas de gestão não precisam de estar abertas permanentemente. Apenas têm de estar abertas enquanto estiver ligado à VM, por exemplo, para realizar tarefas de gestão ou manutenção. Quando o just-in-time estiver ativado, o Centro de segurança utiliza [grupo de segurança de rede](../virtual-network/security-overview.md#security-rules) regras (NSG), que restringem o acesso às portas de gestão para que não pode ser visadas pelos atacantes.

![Apenas no cenário de tempo][1]

## <a name="how-does-just-in-time-access-work"></a>Como o acesso de tempo just-in funciona?

Quando o just in time estiver ativado, o Centro de Segurança bloqueia o tráfego de entrada para as suas VMs do Azure através da criação de uma regra NSG. Selecione as portas na VM para o qual será bloqueado o tráfego de entrada. Estas portas são controladas mediante a apenas na solução de tempo.

Quando um utilizador solicita acesso a uma VM, o Centro de segurança verifica se o utilizador possui [controlo de acesso baseado em funções (RBAC)](../role-based-access-control/role-assignments-portal.md) permissões que fornecem acesso de escrita para a VM. Se eles têm permissões de escrita, o pedido for aprovado e Centro de segurança configura automaticamente os grupos de segurança de rede (NSGs) para permitir tráfego de entrada para portas selecionadas para a quantidade de tempo especificado. Após a expiração do tempo, o Centro de segurança restaurar os NSGs para o respetivo estado anterior. Essas ligações estabelecidas já não sejam a ser interrompidas, no entanto.

> [!NOTE]
> Centro de segurança no acesso VM de tempo atualmente suporta apenas VMs implementadas através do Gestor de recursos do Azure. Para saber mais sobre o clássico e modelos de implementação do Resource Manager, veja [vs. de implementação clássica do Azure Resource Manager](../azure-resource-manager/resource-manager-deployment-model.md).
>
>

## <a name="using-just-in-time-access"></a>Utilizar apenas no momento exato o acesso

1. Abra o dashboard **Centro de Segurança**.

2. No painel esquerdo, selecione **Just-in de acesso à VM de tempo**.

![Tempo de acesso à VM just-in mosaico][2]

O **Just-in de acesso à VM de tempo** é aberta a janela.

![Tempo de acesso à VM just-in mosaico][10]

O **Acesso à VM just-in-time** fornece informações sobre o estado das suas VMs:

- **Configurado** - VMs que foram configuradas para suportar o acesso à VM just-in-time. Os dados apresentados destina-se a última semana e incluem, para cada VM, o número de pedidos aprovados, o data do último acesso e o tempo e o último utilizador.
- **Recomendado** - VMs que conseguem suportar o acesso à VM just-in-time, mas não foram configuradas para tal. Recomendamos que ative apenas no controlo de acesso VM de tempo para essas VMs. Ver [configurar uma política de acesso de tempo](#configuring-a-just-in-time-access-policy).
- **Nenhuma recomendação** - As razões que podem fazer com que uma VM não seja recomendada são:
  - NSG em falta - A solução just-in-time requer a existência de um NSG implementado.
  - VM clássica - Atualmente, o acesso à VM just-in-time do Centro de Segurança suporta apenas VMs implementadas através do Azure Resource Manager. Implementação clássica não é suportada pela apenas na solução de tempo.
  - Outro - Uma VM está nesta categoria se a solução just-in-time estiver desativada na política de segurança da subscrição ou do grupo de recursos, ou se existir um IP público em falta na VM e não tiver um NSG implementado.

## <a name="configuring-a-just-in-time-access-policy"></a>Configurar uma política de acesso de tempo

Para selecionar as VMs que pretende ativar:

1. Sob **Just-in de acesso à VM de tempo**, selecione a **recomendado** separador.

  ![Ativar o acesso de tempo just-in][3]

2. Sob **máquina VIRTUAL**, selecione as VMs que pretende ativar. Isso coloca uma marca de verificação junto a uma VM.
3. Selecione **ativar JIT em VMs**.
4. Selecione **Guardar**.

### <a name="default-ports"></a>Portas predefinidas

Pode ver as portas predefinidas que o Centro de segurança recomenda a ativação just-in-time.

1. Sob **Just-in de acesso à VM de tempo**, selecione a **recomendado** separador.

  ![Apresentar as portas predefinidas][6]

2. Sob **VMs**, selecione uma VM. Isso coloca uma marca de verificação junto a VM e abre **configuração do acesso JIT da VM**. Este painel apresenta as portas predefinidas.

### <a name="add-ports"></a>Adicionar portas

Sob **configuração do acesso JIT da VM**, também pode adicionar e configurar uma nova porta na qual pretende ativar o apenas na solução de tempo.

1. Sob **configuração do acesso JIT da VM**, selecione **Add**. Esta ação abre **Adicionar configuração da porta**.

  ![Configuração da porta][7]

2. Sob **Adicionar configuração da porta**, identificar a porta, o tipo de protocolo, a IPs de origem permitidos e o tempo máximo do pedido.

  IPs de origem permitidos são os intervalos IP podem obter acesso após um pedido aprovado.

  Tempo máximo do pedido é a janela de tempo máximo que pode ser aberta uma porta específica.

3. Selecione **OK**.

## <a name="requesting-access-to-a-vm"></a>Pedir acesso a uma VM

Para pedir acesso a uma VM:

1. Sob **Just-in de acesso à VM de tempo**, selecione a **configurado** separador.
2. Sob **VMs**, selecione as VMs que pretende ativar o acesso. Isso coloca uma marca de verificação junto a uma VM.
3. Selecione **pedir acesso**. Esta ação abre **pedir acesso**.

  ![Pedir acesso a uma VM][4]

4. Sob **pedir acesso**, configurar para cada VM as portas para abrir, juntamente com o IP de origem que a porta está aberta para e a janela de tempo para que a porta está aberta. Pode pedir o acesso apenas para as portas que são configuradas no apenas na política de tempo. Cada porta tem um tempo máximo permitido derivado a apenas na política de tempo.
5. Selecione **abrir portas**.

> [!NOTE]
> Quando um utilizador solicita acesso a uma VM, o Centro de segurança verifica se o utilizador possui [controlo de acesso baseado em funções (RBAC)](../role-based-access-control/role-assignments-portal.md) permissões que fornecem acesso de escrita para a VM. Se tiver permissões de escrita, o pedido for aprovado.
>
>

> [!NOTE]
> Se um utilizador que está a pedir acesso estiver atrás de um proxy, a opção "O meu IP" poderá não funcionar. Pode haver a necessidade de definir o intervalo completo da organização.
>
>

## <a name="editing-a-just-in-time-access-policy"></a>Editar uma política de acesso de tempo

Pode alterar uma VM existente just-in Política de tempo ao adicionar e configurar uma nova porta para abrir para essa VM ou ao alterar qualquer outro parâmetro relacionado a uma porta já protegida.

Para editar uma existente just-in Política de tempo de uma VM, o **configurado** da guia:

1. Sob **VMs**, selecione uma VM para adicionar uma porta ao clicar nos três pontos na linha a essa VM. Esta ação abre um menu.
2. Selecione **editar** no menu. Esta ação abre **configuração do acesso JIT da VM**.

  ![Editar política][8]

3. Sob **configuração do acesso JIT da VM**, pode optar por editar as definições existentes de uma porta já protegida ao clicar em sua porta, ou pode selecionar **Add**. Esta ação abre **Adicionar configuração da porta**.

  ![Adicionar uma porta][7]

4. Sob **Adicionar configuração da porta**, identificar a porta, protocolo de tipo, tempo de pedido de IPs de origem e o máximo permitido.
5. Selecione **OK**.
6. Selecione **Guardar**.

## <a name="auditing-just-in-time-access-activity"></a>Auditoria just-in a atividade de acesso do tempo

Pode obter informações sobre atividades VM utilizando a pesquisa de registos. Para ver os registos:

1. Sob **Just-in de acesso à VM de tempo**, selecione a **configurado** separador.
2. Sob **VMs**, selecione uma VM para ver informações sobre ao clicar nos três pontos na linha a essa VM. Esta ação abre um menu.
3. Selecione **registo de atividades** no menu. Esta ação abre **registo de atividades**.

  ![Selecione o registo de atividades][9]

  **Registo de atividades** fornece uma vista filtrada de operações anteriores para essa VM, juntamente com o tempo, a data e a subscrição.

  ![Ver o registo de atividade][5]

Pode baixar as informações de registo, selecionando **clique aqui para transferir todos os itens como CSV**.

Modificar os filtros e selecione **aplicar** para criar um registo e de pesquisa.

## <a name="using-just-in-time-vm-access-via-rest-apis"></a>Através do just in time acesso à VM através de REST APIs

A apenas no acesso à VM de tempo a funcionalidade pode ser utilizada através da API de centro de segurança do Azure. Pode obter informações sobre VMs configuradas, adicionar novos, pedir acesso a uma VM e mais, por meio desta API. Ver [políticas de acesso de rede Jit](https://docs.microsoft.com/rest/api/securitycenter/jitnetworkaccesspolicies), para saber mais sobre o apenas no tempo de REST API.

### <a name="configuring-a-just-in-time-policy-for-a-vm"></a>Configurar uma política de tempo para uma VM

Para configurar uma política de tempo numa VM específica, tem de executar este comando na sua sessão do PowerShell: ASCJITAccessPolicy do conjunto.
Siga a documentação do cmdlet para obter mais informações.

### <a name="requesting-access-to-a-vm"></a>Pedir acesso a uma VM

Para aceder a uma VM específica que está protegida com a solução de tempo, tem de executar este comando na sua sessão do PowerShell: ASCJITAccess invocar.
Siga a documentação do cmdlet para obter mais informações.

## <a name="next-steps"></a>Passos Seguintes
Neste artigo, aprendeu como just-in-time acesso VM no Centro de segurança ajuda a que controlar o acesso às suas máquinas virtuais do Azure.

Para saber mais acerca do Centro de Segurança, consulte o seguinte:

- [Definir políticas de segurança](security-center-policies.md) – Saiba como configurar políticas de segurança para as suas subscrições do Azure e grupos de recursos.
- [Gerir recomendações de segurança](security-center-recommendations.md) – Saiba como o recomendações o ajudam a proteger os seus recursos do Azure.
- [Monitorização de estado de funcionamento de segurança](security-center-monitoring.md) – Saiba como monitorizar o estado de funcionamento dos seus recursos do Azure.
- [Gerir e responder a alertas de segurança](security-center-managing-and-responding-alerts.md) – Saiba como gerir e responder a alertas de segurança.
- [Monitorizar soluções de parceiros](security-center-partner-solutions.md) – Saiba como monitorizar o estado de funcionamento das suas soluções de parceiros.
- [FAQ do Centro de segurança](security-center-faq.md) – encontre as perguntas mais frequentes sobre como utilizar o serviço.
- [Blogue de Segurança do Azure](https://blogs.msdn.microsoft.com/azuresecurity/) – Encontre mensagens do blogue acerca da segurança e conformidade do Azure.


<!--Image references-->
[1]: ./media/security-center-just-in-time/just-in-time-scenario.png
[2]: ./media/security-center-just-in-time/just-in-time.png
[10]: ./media/security-center-just-in-time/just-in-time-access.png
[3]: ./media/security-center-just-in-time/enable-just-in-time-access.png
[4]: ./media/security-center-just-in-time/request-access-to-a-vm.png
[5]: ./media/security-center-just-in-time/activity-log.png
[6]: ./media/security-center-just-in-time/default-ports.png
[7]: ./media/security-center-just-in-time/add-a-port.png
[8]: ./media/security-center-just-in-time/edit-policy.png
[9]: ./media/security-center-just-in-time/select-activity-log.png
