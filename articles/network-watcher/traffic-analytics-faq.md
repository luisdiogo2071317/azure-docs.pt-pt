---
title: Análise de tráfego do Azure perguntas mais frequentes | Microsoft Docs
description: Obtenha respostas a algumas perguntas mais frequentes sobre a análise de tráfego.
services: network-watcher
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/08/2018
ms.author: jdial
ms.openlocfilehash: 78449a527f5ee1410530ded18a11cb8c6a5dded5
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="traffic-analytics-frequently-asked-questions"></a>Análise de tráfego perguntas mais frequentes

1.  Quais são os pré-requisitos para utilizar a análise de tráfego?

    Análise de tráfego requer os seguintes pré-requisitos:

    - Uma subscrição do observador de rede ativada
    - Registos de fluxo NSG ativados para os NSGs que pretende monitorizar
    - Uma conta de armazenamento do Azure, para armazenar em bruto flog registos
    - Uma área de trabalho de análise de registos (OMS), com acesso de escrita e leitura
    - Deve ser atribuída à conta as seguintes ações no fornecedor Network:

        - Microsoft.Network/applicationGateways/read
        - Microsoft.Network/connections/read
        - Microsoft.Network/loadBalancers/read 
        - Microsoft.Network/localNetworkGateways/read 
        - Microsoft.Network/networkInterfaces/read 
        - Microsoft.Network/networkSecurityGroups/read 
        - Microsoft.Network/publicIPAddresses/read
        - Microsoft.Network/routeTables/read
        - Microsoft.Network/virtualNetworkGateways/read 
        - Microsoft.Network/virtualNetworks/read

2.  Em que regiões do Azure estão disponíveis na análise de tráfego?

    Enquanto está na versão de pré-visualização, pode utilizar a análise de tráfego para NSGs em qualquer um dos seguintes **regiões suportadas**: Central EUA oeste, EUA leste, EUA Leste 2, Norte Central nos, Sul Central nos, EUA Central, EUA oeste, EUA oeste-2, Europa Ocidental, Europa do Norte , RU oeste, RU-Sul, leste da Austrália e Sudeste da Austrália. A área de trabalho de análise de registos tem de existir o Central EUA oeste, EUA leste, Europa Ocidental, Sudeste da Austrália ou a região-sul do RU.

3.  Pode NSGs ativar o fluxo de registos para estar em regiões diferentes a minha área de trabalho do OMS?

    Sim

4.  Podem ser configurados vários NSGs dentro de uma única área de trabalho?

    Sim

5.  Pode utilizar uma área de trabalho do OMS existente?

    Sim, se selecionou uma área de trabalho existente, certifique-se de que foi migrada para o novo idioma de consulta. Se não pretender atualizar a área de trabalho; terá de criar um novo. Para obter mais informações sobre o novo idioma de consulta, consulte [Log Analytics do Azure atualizar para a nova pesquisa de registo](../log-analytics/log-analytics-log-search-upgrade.md).

6.  Pode ser minha conta do Storage do Azure numa subscrição e a minha área de trabalho do OMS ser numa subscrição diferente?

    Sim

7.  Pode armazenar os registos não processados na outra conta de armazenamento na subscrição diferente?

    Não. Pode armazenar os registos não processados em qualquer conta de armazenamento onde um NSG é ativado para os registos de fluxo, no entanto, a conta de armazenamento e os registos em bruto tem de ser na mesma subscrição e região.

8.  Se receber um erro "Não encontrada" ao configurar um NSG para análise de tráfego, como pode posso resolvê-lo?

    Selecione uma região suportada listados 2 em questão. Se selecionar uma região não suportado, receberá um erro "Não encontrada".

9.  Em registos de fluxo NSG, posso estou ao obter o NSG estado como "Falha ao carregar", o que fazer em seguida?

    O fornecedor de insights tem de estar registado para o fluxo de registo para funcionar corretamente. Se não tem a certeza se o fornecedor de insights está registado ou não para a sua subscrição, substitua *xxxxx-xxxxx-xxxxxx-xxxx* no comando seguinte e, em seguida, execute os seguintes comandos a partir do PowerShell:

    ```powershell-interactive
    **Select-AzureRmSubscription** -SubscriptionId xxxxx-xxxxx-xxxxxx-xxxx
    **Register-AzureRmResourceProvider** -ProviderNamespace Microsoft.Insights
    ```

10. Posso ter configurado a solução. Por que razão estou não ver nada no dashboard?

    O dashboard pode demorar até 30 minutos a aparecer na primeira vez. A solução deve primeiro agregar dados suficientes para o mesmo derivar insights significativos, antes de quaisquer relatórios são gerados. 

11.  Se receber a seguinte mensagem: "não foi possível localizar quaisquer dados nesta área de trabalho para o intervalo de tempo selecionado. Experimente alterar o intervalo de tempo ou selecione uma área de trabalho diferentes", como posso resolvê-lo?

        Experimente as seguintes opções:
        - Experimente alterar o intervalo de tempo na barra superior.
        - Selecione uma área de trabalho de análise do registo diferente na barra superior
        - Tente aceder à análise do tráfego após 30 minutos, se tiver sido recentemente ativado
    
        Se os problemas persistirem, criar problemas no [fórum de voz do utilizador](https://feedback.azure.com/forums/217313-networking?category_id=195844).

12.  Se receber a seguinte mensagem: "1) a analisar o NSG fluir registos pela primeira vez. Este processo pode demorar 20-30 minutos a concluir. Verifique novamente daqui a algum tempo. 2) se o passo anterior não funciona e a sua área de trabalho está sob o SKU livre, em seguida, verifique a utilização da área de trabalho aqui para validar através de quota, ou consulte a perguntas mais frequentes para obter mais informações", como posso resolvê-lo?

        Poderá receber o erro pelos seguintes motivos:
        - Análise de tráfego pode ter sido ativada recentemente e poderá não ainda tem agregados dados suficientes para o mesmo derivar insights significativos.
        - A área de trabalho do OMS está sob o SKU livre e infringido, os limites de quota. Neste caso, terá de utilizar uma área de trabalho um SKU com maior capacidade.
    
        Se os problemas persistirem, criar problemas no [fórum de voz do utilizador](https://feedback.azure.com/forums/217313-networking?category_id=195844).
    
13.  Se receber a seguinte mensagem: "temos e não existem informações de fluxos de dados de recursos (topologia). Entretanto, clique aqui para ver os dados de recursos e consulte Perguntas mais frequentes para obter mais informações", como resolvê-lo?

        Está a ver as informações de recursos no dashboard; No entanto, não existem estatísticas relacionadas com o fluxo estão presentes. Dados não podem existir devido a não existem fluxos de comunicação entre os recursos. Aguarde a 60 minutos e Reverificar estado. Se tiver a certeza de que os fluxos de comunicação entre recursos existem e criar problemas no [fórum de voz do utilizador](https://feedback.azure.com/forums/217313-networking?category_id=195844).

14.  Como a análise de tráfego tem um preço?

        Análise de tráfego é limitado otimização registos reduzidos e armazenar os registos avançados na área de trabalho de análise de registos. Enquanto na pré-visualização, análise de tráfego não é faturada otimização registos reduzidos, no entanto, retenção de dados numa área de trabalho está sujeito a faturação às taxas de publicada. Esta resposta será atualizada depois de preços para análise de tráfego estão disponível.

15.  Como navegar através de teclado na vista de mapa Georreplicação?

        A página de mapa georreplicação contém duas secções principais:
    
        - **Faixa**: A faixa colocada na parte superior do mapa Georreplicação fornece a capacidade para selecionar os filtros de distribuição de tráfego através de botões como a análise de ativado/tráfego de análise de implementação/Active/Inactive/tráfego de Deployment/No não ativado/tráfego do tráfego malicioso de países/Benign/maliciosos/permitidos e informações de legenda. Na seleção de botões definidos, o respetivo filtro é aplicado no mapa, como se um utilizador seleciona o botão Filtro "Ativo" na faixa, em seguida, o mapa realça dos centros de dados "Ativos" na sua implementação.
        - **Mapa**: colocada abaixo a faixa de secção do mapa mostra a distribuição de tráfego entre os datacenters do Azure e países.
    
        **Navegação do teclado na faixa**
    
        - Por predefinição, a seleção na página de mapa de georreplicação para a faixa é o filtro "Azure DCs" botão.
        - Para navegar para outra botão de filtros, pode utilizar o `Tab` ou `Right arrow` chave para mover seguinte. Para navegar para trás, utilize `Shift+Tab` ou `Left arrow` chave. Precedência de direção de navegação de avanço é esquerda para a direita, seguido de parte superior para parte inferior.
        - Prima a `Enter` ou `Down` tecla de seta para aplicar o filtro selecionado. Um ou vários nós na secção mapa com base na seleção de filtro e a implementação, são realçados.
            - Alternar entre **faixa** e **mapa**, prima `Ctrl+F6`.
        
        **Navegação do teclado no mapa**
    
        - Depois de ter selecionado qualquer filtro na faixa e premido `Ctrl+F6`, foco é movido para um de nós realçados (**datacenter do Azure** ou **país/região**) na vista de mapa.
        - Para navegar para outros nós realçados no mapa pode utilizar o `Tab` ou `Right arrow` chaves para reencaminhar movimento, e `Shift+Tab` ou `Left arrow` chave para movimento com versões anteriores.
        - Para selecionar qualquer nó realçado no mapa, utilize o `Enter` ou `Down arrow` chave.
        - Seleção de quaisquer de nós, de foco move para o **informações ferramenta caixa** para o nó. Por predefinição, foco são transmitidos para o botão fechado no **informações ferramenta caixa**. Para obter mais navegar no interior **caixa** ver, utilize `Right` e `Left arrow` chaves mover reencaminhar e trás, respetivamente. Premir `Enter` tem o mesmo efeito que selecionar o botão focado no **informações ferramenta caixa**.
        - Premir `Tab` enquanto o foco incide no **informações ferramenta caixa**, move o foco para os pontos finais no continente mesmo que o nó selecionado. Pode utilizar o `Right` e `Left arrow` chaves de navegar por estes pontos finais.
        - Para navegar para outro cluster de pontos finais/continentes de fluxo, utilize `Tab` para movimento direta e `Shift+Tab` para movimento com versões anteriores.
        - Depois do foco incide na `Continent clusters`, utilize o `Enter` ou `Down` teclas de seta para destacar os pontos finais dentro do cluster continente antes. Para navegar através de pontos finais e o botão de fechar a caixa de informações do cluster continente antes, utilize o `Right` ou `Left arrow` chave para movimento direta e com versões anteriores, respetivamente. Em qualquer ponto final, pode utilizar `Shift+L` para mudar para a linha de ligação a partir do nó selecionado para o ponto final. Premir `Shift+L` navega-a novamente para o ponto final selecionado.
        
        Em qualquer fase:
    
        - `ESC` Fecha a seleção expandida.
        - O `UP Arrow` chave efetua a mesma ação como `ESC`. O `Down arrow` chave efetua a mesma ação como `Enter`.
        - Utilize `Shift+Plus` para aplicar zoom no, e `Shift+Minus` para reduzir.
