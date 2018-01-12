---
title: "Resolver problemas relacionados com alterações numa máquina virtual do Azure | Microsoft Docs"
description: "Utilize o Controlo de Alterações para resolver problemas relacionados com alterações numa máquina virtual do Azure."
services: automation
keywords: "alterações, controlo, automatização"
author: jennyhunter-msft
ms.author: jehunte
ms.date: 12/14/2017
ms.topic: tutorial
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: 0aefa175d676bd7e98841d3a1e9ff5a8c90b7deb
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/10/2018
---
# <a name="troubleshoot-changes-in-your-environment"></a>Resolver problemas relacionados com alterações no seu ambiente

Neste tutorial, irá aprender a resolver problemas relacionados com alterações numa máquina virtual do Azure. Ao ativar o Controlo de alterações, pode controlar as alterações efetuadas ao nível do software, dos ficheiros, dos daemons do Linux, dos Serviços do Windows e das chaves do Registo do Windows nos seus computadores.
A identificação destas alterações de configuração pode ajudar a localizar problemas operacionais em todo o ambiente.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Carregar uma VM para Controlo de alterações e Inventário
> * Pesquisar serviços parados em registos de alterações
> * Configurar o Controlo de alterações
> * Ativar a ligação do Registo de atividades
> * Acionar um evento
> * Ver as alterações

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial, precisa de:

* Uma subscrição do Azure. Se ainda não tiver uma, pode [ativar as vantagens de subscritor do MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) ou [inscrever-se numa conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Um [conta de Automatização](automation-offering-get-started.md) para reter os runbooks de observador e ação e a Tarefa de Observador.
* Uma [máquina virtual](../virtual-machines/windows/quick-create-portal.md) para carregar.

## <a name="log-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão no portal do Azure em http://portal.azure.com.

## <a name="enable-change-tracking-and-inventory"></a>Ativar o Controlo de alterações e Inventário

Primeiro, tem de ativar o Controlo de alterações e Inventário para a VM para este tutorial. Se ativou anteriormente outra solução de automatização para uma VM, este passo não é necessário.

1. No menu da esquerda, selecione **Máquinas virtuais** e selecione uma VM da lista
1. No menu da esquerda, na secção **Operações**, clique em **Inventário**. É aberta a página **Ativar o Controlo de alterações e Inventário**.

É executada uma validação para determinar se a solução Controlo de alterações e Inventário está ativada para esta VM.
A validação inclui a verificação da existência de uma área de trabalho do Log Analytics e da conta de Automatização ligada, e se a solução está na área de trabalho.

A área de trabalho do [Log Analytics](../log-analytics/log-analytics-overview.md?toc=%2fazure%2fautomation%2ftoc.json) serve para recolher dados gerados pelas funcionalidades e serviços, como o Inventário.
A área de trabalho fornece uma localização única para rever e analisar dados de várias origens.

O processo de validação também verifica se a VM está aprovisionada com o Microsoft Monitoring Agent (MMA) e a função de trabalho híbrida.
Este agente serve para comunicar com a VM e obter informações sobre o software instalado.
O processo de validação verifica ainda se a VM está aprovisionada com o Microsoft Monitoring Agent (MMA) e a função de trabalho de runbook híbrida de Automatização.

Se estes pré-requisitos não forem cumpridos, é apresentada uma faixa que lhe dá a opção de ativar a solução.

![Faixa de configuração do carregamento de Controlo de alterações e Inventário](./media/automation-tutorial-troubleshoot-changes/enableinventory.png)

Para ativar a solução, clique na faixa.
Se for detetada a falta de qualquer um dos pré-requisitos que se seguem após a validação, estes serão adicionados automaticamente:

* Área de trabalho do [Log Analytics](../log-analytics/log-analytics-overview.md?toc=%2fazure%2fautomation%2ftoc.json)
* [Automatização](./automation-offering-get-started.md)
* Uma [Função de trabalho de runbook híbrida](./automation-hybrid-runbook-worker.md) está ativada na VM

É aberto o ecrã **Controlo de Alterações e Inventário**. Configure a localização, a área de trabalho do Log Analytics e a conta de Automatização a utilizar e clique em **Ativar**. Se os campos estiverem desativados, significa que outra solução de automatização está ativada para a VM e terá de ser utilizada a mesmo área de trabalho e conta de Automatização.

![Janela para ativar a solução de controlo de alterações](./media/automation-tutorial-troubleshoot-changes/installed-software-enable.png)

A ativação da solução pode demorar até 15 minutos. Durante este período, não deve fechar a janela do browser.
Após a ativação da solução, as informações sobre o software instalado e as alterações efetuadas na VM são transmitidas para o Log Analytics.
Pode demorar entre 30 minutos e 6 horas até que os dados fiquem disponíveis para análise.


## <a name="using-change-tracking-in-log-analytics"></a>Utilizar o Controlo de alterações no Log Analytics

O Controlo de alterações gera dados de registo que são enviados para o Log Analytics. Para pesquisar os registos através da execução de consultas, selecione **Log Analytics** na parte superior da janela **Controlo de alterações**.
Os dados de Controlo de alterações são armazenados abaixo do tipo **ConfigurationChange**. O exemplo de consulta do Log Analytics que se segue devolve todos os Serviços do Windows que foram parados.

```
ConfigurationChange
| where ConfigChangeType == "WindowsServices" and SvcState == "Stopped"
```

Para saber mais sobre a execução e pesquisa de ficheiros de registo no Log Analytics, consulte [Azure Log Analytics](https://docs.loganalytics.io/index).

## <a name="configure-change-tracking"></a>Configurar o Controlo de alterações

O Controlo de alterações permite controlar as alterações de configuração na VM. Os passos que se seguem mostram como configurar o controlo de ficheiros e chaves do Registo.
 
Para escolher os ficheiros e as chaves do Registo que devem ser recolhidos e controlados, selecione **Editar definições** na parte superior da página **Controlo de alterações**.

> [!NOTE]
> O Inventário e o Controlo de alterações utilizam as mesmas definições de coleção, sendo as definições configuradas ao nível da área de trabalho.

Na janela **Configuração da Área de Trabalho**, adicione as chaves do Registo do Windows, os ficheiros do Windows ou os ficheiros do Linux a controlar, conforme descrito nas três secções que se seguem.

### <a name="add-a-windows-registry-key"></a>Adicionar uma chave do Registo do Windows

1. No separador **Registo do Windows**, selecione **Adicionar**.
    É aberta a janela **Adicionar Registo do Windows para Controlo de Alterações**.

   ![Adicionar registo para Controlo de Alterações](./media/automation-vm-change-tracking/change-add-registry.png)

2. Em **Ativado**, selecione **Verdadeiro**.
3. Na caixa **Nome do Item**, introduza um nome amigável.
4. (Opcional) Na caixa **Grupo**, introduza um nome de grupo.
5. Na caixa **Chave de Registo do Windows**, introduza o nome da chave do Registo que quer controlar.
6. Selecione **Guardar**.

### <a name="add-a-windows-file"></a>Adicionar um ficheiro do Windows

1. No separador **Ficheiros do Windows**, selecione **Adicionar**. É aberta a janela **Adicionar Ficheiro do Windows para Controlo de Alterações**.

   ![Adicionar ficheiro do Windows para Controlo de Alterações](./media/automation-vm-change-tracking/change-add-win-file.png)

2. Em **Ativado**, selecione **Verdadeiro**.
3. Na caixa **Nome do Item**, introduza um nome amigável.
4. (Opcional) Na caixa **Grupo**, introduza um nome de grupo.
5. Na caixa **Introduzir Caminho**, introduza o caminho completo e o nome do ficheiro que quer controlar.
6. Selecione **Guardar**.

### <a name="add-a-linux-file"></a>Adicionar um ficheiro do Linux

1. No separador **Ficheiros do Linux**, selecione **Adicionar**. É aberta a janela **Adicionar Ficheiro do Linux para Controlo de Alterações** .

   ![Adicionar ficheiro do Linux para Controlo de Alterações](./media/automation-vm-change-tracking/change-add-linux-file.png)

2. Em **Ativado**, selecione **Verdadeiro**.
3. Na caixa **Nome do Item**, introduza um nome amigável.
4. (Opcional) Na caixa **Grupo**, introduza um nome de grupo.
5. Na caixa **Introduzir Caminho**, introduza o caminho completo e o nome do ficheiro que quer controlar.
6. Na caixa **Tipo de Caminho**, selecione **Ficheiro** ou **Diretório**.
7. Em **Recursão**, para controlar as alterações relacionadas com o caminho especificado e todos os ficheiros e caminhos englobados pelo mesmo, selecione **Ativar**. Para controlar apenas o caminho ou ficheiro selecionado, selecione **Desativar**.
8. Em **Utilizar o Sudo**, para controlar os ficheiros que precisam do comando `sudo` para aceder, selecione **Ativar**. Se tal não for necessário, selecione **Desativar**.
9. Selecione **Guardar**.

## <a name="enable-activity-log-connection"></a>Ativar a ligação do Registo de atividades

Na página **Controlo de alterações** da sua VM, selecione **Gerir Ligação de Registo de Atividades**. Esta tarefa abre a página **Registo de atividades do Azure**. Selecione **Ligar** para ligar o Controlo de alterações ao registo de atividades do Azure para a sua VM.

Com esta definição ativada, navegue para a página **Descrição Geral** da sua VM e selecione **Parar** para parar a VM. Quando lhe for pedido, selecione **Sim** para parar a VM. Quando esta estiver desalocada, selecione **Iniciar** para reiniciar a sua VM.

As operações de paragem e início de uma VM registam um evento no respetivo registo de atividades. Regresse à página **Controlo de alterações**. Selecione o separador **Eventos** na parte inferior da página. Passado algum tempo, os eventos são apresentados no gráfico e na tabela. Tal como no passo anterior, é possível selecionar cada um dos eventos para ver informações detalhadas sobre o evento.

![Ver detalhes da alteração no portal](./media/automation-tutorial-troubleshoot-changes/viewevents.png)

## <a name="view-changes"></a>Ver as alterações

Depois de ativar a solução Controlo de alterações e Inventário, pode ver os resultados na página **Controlo de alterações**.

Na sua VM, selecione **Controlo de alterações** em **OPERAÇÕES**.

![Captura de ecrã que mostra a lista de alterações para a VM](./media/automation-tutorial-troubleshoot-changes/change-tracking-list.png)

O gráfico mostra as alterações ocorridas ao longo do tempo.
Após ter adicionado uma ligação do Registo de Atividades, o gráfico de linhas existente na parte superior apresenta os eventos do Registo de Atividades do Azure.
Cada linha do gráfico de barras representa um tipo diferente de alteração passível de controlo.
Estes tipos incluem daemons do Linux, ficheiros, chaves do Registo do Windows, software e serviços do Windows.
O separador Alterações mostra os detalhes das alterações apresentadas na visualização por ordem descendente de data/hora em que a alteração ocorreu (mais recente primeiro).
No separador **Eventos**, a tabela apresenta os eventos do Registo de Atividades ligados e os respetivos detalhes, com o mais recente listado primeiro.

Como é possível constatar nos resultados, foram efetuadas várias alterações no sistema, incluindo alterações ao nível do software e serviços. Pode utilizar os filtros na parte superior da página para filtrar os resultados consoante o **Tipo de alteração** ou um intervalo de tempo.

Selecione uma alteração do tipo **WindowsServices** para abrir a janela **Detalhes da Alteração**. A janela Detalhes da Alteração mostra detalhes sobre a alteração e os valores antes e após a alteração. Neste caso, o serviço Proteção de Software foi parado.

![Ver detalhes da alteração no portal](./media/automation-tutorial-troubleshoot-changes/change-details.png)

## <a name="next-steps"></a>Próximos Passos

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Carregar uma VM para Controlo de alterações e Inventário
> * Pesquisar serviços parados em registos de alterações
> * Configurar o Controlo de alterações
> * Ativar a ligação do Registo de atividades
> * Acionar um evento
> * Ver as alterações

Avance para a descrição geral da solução Controlo de alterações e Inventário para saber mais acerca dela.

> [!div class="nextstepaction"]
> [Solução Gestão de mudança e Inventário](../log-analytics/log-analytics-change-tracking.md?toc=%2fazure%2fautomation%2ftoc.json)
