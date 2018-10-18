---
title: Monitorizar o estado de funcionamento da máquina virtual com o Azure Monitor para VMs | Documentos da Microsoft
description: Este artigo descreve como compreender o estado de funcionamento da máquina virtual e do sistema operacional subjacente com o Azure Monitor para as VMs.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/15/2018
ms.author: magoedte
ms.openlocfilehash: 84314f64d8a96e65f63cb5c6051f7f5e902cd682
ms.sourcegitcommit: f20e43e436bfeafd333da75754cd32d405903b07
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/17/2018
ms.locfileid: "49387826"
---
# <a name="understand-the-health-of-your-azure-virtual-machines-with-azure-monitor-for-vms"></a>Compreender o estado de funcionamento das suas máquinas virtuais do Azure com o Azure Monitor para VMs
Azure inclui vários serviços que executar individualmente uma tarefa ou função específica no espaço de monitorização, mas o fornecimento de uma perspectiva detalhada do Estado de funcionamento do sistema operativo alojada em máquinas virtuais do Azure não estava disponível.  Enquanto poderia monitorar para condições diferentes usando o Log Analytics ou no Azure Monitor, que não foram projetados para modelar e representam o estado de funcionamento dos componentes principais ou de estado de funcionamento geral da máquina virtual.  Com o Azure Monitor para a funcionalidade de estado de funcionamento de VMs, proativamente monitoriza a disponibilidade e desempenho do Windows ou Linux SO convidado com um modelo que representam os principais componentes e suas relações, os critérios que especifica como medir o estado de funcionamento desses componentes, e o alerte quando é detetada uma condição de mau estado de funcionamento.  

Visualizar o estado de funcionamento geral da VM do Azure e subjacentes o sistema operativo podem ser observados de duas perspetivas com o Azure Monitor de estado de funcionamento de VMs, diretamente a partir da máquina virtual ou em todas as VMs num grupo de recursos do Azure Monitor.

Este artigo ajuda-o a compreender como avaliar rapidamente, investigar e resolver problemas de estado de funcionamento detetados.

Para obter informações sobre como configurar o Azure Monitor para VMs, veja [ativar o Azure Monitor para VMs](monitoring-vminsights-onboard.md).

## <a name="monitoring-configuration-details"></a>Detalhes de configuração de monitorização
Esta seção descreve os critérios de estado de funcionamento do padrão definidos para monitorizar o Windows Azure e máquinas virtuais do Linux. Todos os critérios de estado de funcionamento são previamente configurados para o alerta quando for cumprida a condição de mau estado de funcionamento. 

### <a name="windows-vms"></a>VMs do Windows

- Disponível em Megabytes de memória 
- Segundos de média de disco por gravação (disco lógico)
- Segundos de média de disco por gravação (disco)
- Segundos de disco lógico médio por leitura
- Segundos de disco lógico médio por transferência
- Segundos de média de disco por leitura
- Segundos de disco médio por transferência
- Comprimento de fila de disco atual (disco lógico)
- Comprimento de fila de disco atual (disco)
- Tempo de inatividade de percentagem de disco
- Erro de sistema de ficheiros ou corrupção
- Espaço livre de disco lógico (%) Baixa
- Disco lógico (MB) de espaço livre reduzido
- Tempo de inatividade de percentagem de disco lógico
- Páginas de memória por segundo
- Largura de banda de percentagem utilizada de leitura
- Largura de banda percentagem utilizada Total
- Largura de banda de percentagem utilizada de escrita
- Percentagem de memória consolidada em utilização
- Tempo de inatividade de percentagem de disco físico
- Estado de funcionamento de serviço de cliente DHCP
- Estado de funcionamento de serviço de cliente DNS
- Estado de funcionamento de serviço RPC
- Estado de funcionamento de serviço de servidor
- Percentagem de utilização da total CPU
- Estado de funcionamento de serviço de registo de eventos de Windows
- Estado de funcionamento do serviço do Windows Firewall
- Estado de funcionamento do serviço do Windows gestão remota

### <a name="linux-vms"></a>VMs do Linux
- Média de disco Disco seg/transferência 
- Média de disco Disco seg/leitura 
- Média de disco Disco seg/escritas 
- Estado de funcionamento do disco
- Espaço livre em disco lógico
- Disco lógico % de espaço livre
- Disco lógico % de Inodes livres
- Estado de funcionamento de placa de rede
- Tempo de DPC percentagem do processador
- Processador de percentagem de tempo do processador
- Tempo de processador de percentagem total
- Tempo de DPC de percentagem total
- Sistema operativo de Megabytes de memória disponíveis

## <a name="sign-in-to-the-azure-portal"></a>Iniciar sessão no portal do Azure
Inicie sessão no [portal do Azure](https://portal.azure.com). 

## <a name="introduction-to-health-experience"></a>Introdução à experiência de estado de funcionamento
Antes de explorar a utilizar a funcionalidade de estado de funcionamento de uma única máquina virtual ou o grupo de VMs, é importante, fornecemos uma breve introdução para compreender como as informações são apresentadas e o que representam as visualizações.  

## <a name="view-health-directly-from-a-virtual-machine"></a>Ver estado de funcionamento diretamente a partir de uma máquina virtual 
Para ver o estado de funcionamento de uma VM do Azure, selecione **Insights (pré-visualização)** no painel esquerdo da máquina virtual. Na página de informações VM, **estado de funcionamento** está aberta por predefinição e mostra a vista de estado de funcionamento da VM.  

![O Azure Monitor para descrição geral do Estado de funcionamento de VMs de uma máquina virtual do Azure selecionada](./media/monitoring-vminsights-health/vminsights-directvm-health.png)

Sobre o **estado de funcionamento** separador, na secção **estado de funcionamento da VM do convidado**, a tabela mostra o estado de funcionamento atual da sua máquina virtual e o número total de alertas de estado de funcionamento da VM gerados por um componente em mau estado de funcionamento. Consulte a [alertas e uma gestão de alertas](#alerting-and-alert-management) para obter mais detalhes.  

Os Estados de funcionamento definidos para uma VM são: 

* **Bom estado de funcionamento** – nenhum problema detetado para a VM e está a funcionar conforme necessário.  
* **Crítico** – são detetados um ou mais problemas críticos, que precisam ser abordadas para restaurar a funcionalidade normal, conforme o esperado. 
* **Aviso** -forem detetados problemas de um ou mais, que precisam ser abordadas ou a condição de estado de funcionamento pode tornar-se crítico.  
* **Desconhecido** – se o serviço não conseguiu estabelecer uma ligação com a VM, o estado muda para um Estado desconhecido.  

Selecionando **ver o diagnóstico de estado de funcionamento** abre uma página que mostra todos os componentes da VM, critérios de estado de funcionamento associado, alterações de estado e outros problemas importantes encontrados por monitorizar componentes relacionados com a VM. Para obter mais informações, consulte [diagnóstico de estado de funcionamento](#health-diagnostics). 

Sob o **estado de funcionamento do componente** secção, a tabela mostra um Estado de rollup de estado de funcionamento das categorias principais do desempenho monitorizados pelos critérios de estado de funcionamento para essas áreas, especificamente **CPU**,  **Memória**, **disco**, e **rede**.  Selecionar qualquer um dos componentes é aberta uma página com todos os critérios de estado de funcionamento individuais monitorização aspectos desse componente e o estado do respetivo estado de funcionamento de cada.  

Ao aceder ao estado de funcionamento a partir de uma VM do Azure com o sistema operativo do Windows, o estado de funcionamento dos serviços de Windows core de 5 principais são apresentados na secção **Core dos serviços de estado de funcionamento**.  Selecionar qualquer um dos serviços é aberta uma página listando os critérios de estado de funcionamento monitorização esse componente e o estado de integridade.  Ao clicar no nome dos critérios de estado de funcionamento irá abrir o painel de propriedades e a partir daqui pode rever os detalhes de configuração, incluindo se os critérios de estado de funcionamento tem um alerta correspondente do Azure Monitor definido. Para obter mais informações, consulte [diagnóstico de estado de funcionamento e trabalhar com os critérios de estado de funcionamento](#health-diagnostics).  

## <a name="aggregate-virtual-machine-perspective"></a>Ponto de vista de máquina de virtual agregado
Para ver a recolha de estado de funcionamento para todas as suas máquinas virtuais num grupo de recursos, na lista de navegação no portal, selecione **do Azure Monitor** e, em seguida, selecione **máquinas virtuais (pré-visualização)**.  

![Vista do Azure Monitor de monitorização de informações de VM](./media/monitoring-vminsights-health/vminsights-aggregate-health.png)

Partir do **subscrição** e **grupo de recursos** listas pendentes, selecione o grupo de recursos adequados que inclui as VMs relacionados ao grupo, para ver o respetivo estado de funcionamento comunicadas.  A seleção de apenas aplica-se para a funcionalidade de estado de funcionamento e não vão ser transferidos para o desempenho ou de mapa.

Sobre o **estado de funcionamento** separador, é possível saber o seguinte:

* O número de VMs está num estado crítico ou mau estado de funcionamento, versus quantas estão em bom estado ou não a submeter dados (referidos como um Estado desconhecido)?
* Qual as VMs por sistema operativo (SO) estão a enviar relatórios mau estado de funcionamento e quantos?
* O número de VMs é mau estado de funcionamento devido a um problema detetado com um processador, disco, memória ou um adaptador de rede, categorizados por Estado de funcionamento?  
* O número de VMs é mau estado de funcionamento devido a um problema detetado com um serviço de sistema operacional principal, categorizado por Estado de funcionamento?

Aqui pode rapidamente identificar os principais problemas críticos detetados pelos critérios de estado de funcionamento de monitoramento proativo da VM e rever os detalhes de alerta de estado de funcionamento da VM e o artigo de conhecimento associado a intenção de auxiliar no diagnóstico e correção do problema.  Selecione qualquer uma das gravidades para abrir o [todos os alertas](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md#all-alerts-page) página filtrada por esse gravidade.

O **distribuição VM pelo sistema operativo** lista mostra as VMs listadas por edição do Windows ou a distribuição de Linux, juntamente com suas versões. Em cada categoria de sistema operativo, as VMs são divididas de forma ainda mais com base no estado de funcionamento da VM. 

![Ponto de vista de distribuição de máquina virtual de VM Insights](./media/monitoring-vminsights-health/vminsights-vmdistribution-by-os.png)

Os Estados de funcionamento definidos para uma VM são: 

* **Bom estado de funcionamento** – nenhum problema detetado para a VM e está a funcionar conforme necessário.  
* **Crítico** – são detetados um ou mais problemas críticos, que precisam ser abordadas para restaurar a funcionalidade normal, conforme o esperado. 
* **Aviso** -forem detetados problemas de um ou mais, que precisam ser abordadas ou a condição de estado de funcionamento pode tornar-se crítico.  
* **Desconhecido** – se o serviço não conseguiu estabelecer uma ligação com a VM, o estado muda para um Estado desconhecido.  

Pode clicar em qualquer item de coluna - **contagem VM**, **crítico**, **aviso**, **bom estado de funcionamento** ou **desconhecido** para desagregar os **máquinas virtuais** página Ver uma lista de resultados filtrados que correspondem a coluna selecionada. Por exemplo, se quisermos rever todas as VMs em execução **Red Hat Enterprise Linux versão 7.5**, clique nas **contagem VM** valor para o sistema operacional e abrirá a página seguinte, as máquinas virtuais que correspondam a listagem esse filtro e o respetivo estado de funcionamento atualmente conhecidos.  

![Exemplo de rollup de VMs de Linux do Red Hat](./media/monitoring-vminsights-health/vminsights-rollup-vm-rehl-01.png)
 
Sobre o **máquinas virtuais** página, se selecionar o nome de uma VM sob a coluna **nome da VM**, será direcionado para a página de instância VM com mais detalhes sobre os alertas e problemas de critérios de estado de funcionamento identificado que são que afetam a VM selecionada.  A partir daqui, pode filtrar os detalhes de estado de funcionamento ao clicar em **estado de funcionamento** ícone no canto superior esquerdo da página para ver quais componentes estão em mau estado de funcionamento ou pode ver os alertas de estado de funcionamento da VM gerados por um componente em mau estado de funcionamento categorizadas por gravidade do alerta.    

Na vista de lista VM, clicar no nome de uma VM abre o **estado de funcionamento** página, para que selecionou VM, da mesma forma como se selecionou **Insights (pré-visualização)** da VM diretamente.

![Informações VM de uma máquina virtual do Azure selecionada](./media/monitoring-vminsights-health/vminsights-directvm-health.png)

Aqui mostra um rollup **estado de funcionamento** para a máquina virtual e **alertas**, categorizados por gravidade, que representam os alertas de estado de funcionamento da VM gerados quando o estado de funcionamento muda de estado de funcionamento para mau estado de funcionamento para um critérios de estado de funcionamento.  Selecionando **VMs em condição crítica** abrirá uma página com uma lista de uma ou mais VMs que estão num Estado de funcionamento crítico.  Clicar no estado de funcionamento para uma das VMs na lista mostrará os **diagnóstico de estado de funcionamento** vista da VM.  Aqui pode descobrir quais critérios de estado de funcionamento está refletindo um problema de estado de funcionamento. Quando o **diagnóstico de estado de funcionamento** é aberta a página, mostra todos os componentes da VM e os respetivos critérios de estado de funcionamento associado com o estado de funcionamento atual.  Consulte a [diagnóstico de estado de funcionamento](#health-diagnostics) secção para obter mais detalhes.  

Selecionando **ver todos os critérios de estado de funcionamento** abre uma página que mostra uma lista de todos os critérios de estado de funcionamento disponíveis com esta funcionalidade.  As informações podem ser mais filtradas com base nas seguintes opções:

* **Tipo de** – existem três tipos de estado de funcionamento tipos de critérios para avaliar condições e rollup de estado de funcionamento geral da VM monitorizada.  
    a. **Unidade** – medem algum aspeto da máquina virtual. Este tipo de critérios de estado de funcionamento pode ser verificar um contador de desempenho para determinar o desempenho do componente, executar um script para efetuar uma transação sintética ou monitorizar um evento que indica um erro.  Por predefinição, o filtro está definido para a unidade.  
    b. **Dependência** -fornece o rollup de estado de funcionamento entre diferentes entidades. Estes critérios de estado de funcionamento permite que o estado de funcionamento de uma entidade a depender do Estado de funcionamento de outro tipo de entidade que ele depende para operação concluída com êxito.  
    c. **Agregação** -fornece um Estado de funcionamento combinado de critérios de estado de funcionamento semelhante. Critérios de estado de funcionamento de unidades e de dependência serão normalmente configurados sob um critério de agregação do Estado de funcionamento. Além de fornecer uma melhor organização geral dos muitos critérios de estado de funcionamento de diferentes destinados a uma entidade, o critério de agregação do Estado de funcionamento fornece um Estado de funcionamento exclusivo para as categorias distintas das entidades.

* **Categoria** -tipo de critérios de estado de funcionamento utilizado para os critérios de grupo do tipo semelhante para fins de relatórios.  Eles são **disponibilidade** ou **desempenho**.

Pode explorar ainda mais baixo para ver quais as instâncias estão em mau estado de funcionamento ao clicar num valor baixo a **mau estado de funcionamento do componente** coluna.  Na página, uma tabela lista os componentes, que estão num Estado de funcionamento crítico.    

## <a name="health-diagnostics"></a>Diagnóstico de estado de funcionamento
**Diagnóstico de estado de funcionamento** página permite-lhe ver todos os componentes da VM, critérios de estado de funcionamento associado, alterações de estado, e outros problemas importantes encontrados por objetos de monitorização relacionados à VM. 

![Exemplo de página de diagnóstico de estado de funcionamento para uma VM](./media/monitoring-vminsights-health/health-diagnostics-page-01.png)

Pode iniciar o diagnóstico de estado de funcionamento das seguintes formas.

* Por Estado de funcionamento de rollup para todas as VMs a partir da perspetiva VM de agregação no Azure Monitor.  Sobre o **estado de funcionamento** página, clique no ícone para **crítico**, **aviso**, **bom estado de funcionamento**, ou **desconhecido** Estado de funcionamento na seção **estado de funcionamento da VM do convidado** e Desagregue até a página que apresenta uma lista de todas as VMs correspondentes dessa categoria filtrada.  Ao clicar nos valores a **estado de funcionamento** coluna abrirá o diagnóstico de estado de funcionamento de âmbito para essa VM específica.      

* Pelo sistema de operativo a partir da perspetiva VM de agregação no Azure Monitor. Sob **distribuição de VM**, selecionar qualquer um dos valores da coluna irá abrir o **máquinas virtuais** página e retornar uma lista na tabela que correspondem a categoria filtrada.  Clique no valor sob **estado de funcionamento** coluna abre o diagnóstico de estado de funcionamento para a VM selecionada.    
 
* Da VM no Monitor do Azure para VMs do convidado **estado de funcionamento** separador, selecionando **ver diagnóstico de estado de funcionamento** 

Diagnóstico de estado de funcionamento organiza as informações de estado de funcionamento nas seguintes categorias: 

* Disponibilidade
* Desempenho
 
Todos os critérios de estado de funcionamento definidos para um destino selecionado na categoria apropriada. 

Estado de funcionamento para os critérios de estado de funcionamento é definido por um de três Estados – *crítico*, *aviso*, e *bom estado de funcionamento*. Há outro estado *desconhecido*, que não está associado ao estado de funcionamento, mas representa o estado de monitorização conhecido pela funcionalidade.  

A tabela seguinte fornece detalhes sobre os Estados de funcionamento representados no diagnóstico de estado de funcionamento.

|Ícone |Estado de funcionamento |Significado |
|-----|-------------|------------|
| |Bom estado de funcionamento |O estado de funcionamento é bom estado de funcionamento se ele estiver dentro as condições de estado de funcionamento definidos. No caso de um monitor de rollup principal, rolls-up do Estado de funcionamento e reflete o estado mais favorável ou pior do filho.|
| |Crítica |O estado de funcionamento é crítico, se não se encontra dentro da condição de estado de funcionamento definidos. No caso de um monitor de rollup principal, rolls-up do Estado de funcionamento e reflete o estado mais favorável ou pior do filho.|
| |Aviso |O estado de funcionamento indica um aviso se estiver entre dois limiares para a condição de estado de funcionamento definidos, onde um indica uma *aviso* estado e a outra indica um *crítico* estado. Em caso de um rollup principal monitor, se um ou mais dos filhos estão num Estado de aviso, em seguida, irá refletir o pai *aviso* estado. Se houver um filho que está numa *crítico* e outro filho num *aviso* Estado, o rollup principal mostrará um Estado de funcionamento *crítico*.|
| |Desconhecidos |O estado de funcionamento está numa *desconhecido* estado quando o estado de funcionamento não é possível calcular por várias razões, tais como não é possível recolher dados, serviço etc não inicializado.| 

Página de diagnóstico de estado de funcionamento tem três seções principais:

* Modelo do Componente 
* Critérios de Estado de Funcionamento
* Alterações de Estado 

![Secções da página de diagnóstico de estado de funcionamento](./media/monitoring-vminsights-health/health-diagnostics-page-02.png)

### <a name="component-model"></a>Modelo de componente
A coluna mais à esquerda numa página de diagnóstico de estado de funcionamento é o modelo de componente. Todos os componentes e suas instâncias detetadas, que estão associadas a VM, são apresentadas nesta coluna. 

No exemplo a seguir, os componentes detetados são disco, disco lógico, processador, memória e sistema operativo. Várias instâncias desses componentes são detetadas e apresentadas nesta coluna, com duas instâncias do disco lógico **/**, **/arranque**, e   **/mnt/recurso**, uma instância do adaptador de rede **eth0**, duas instâncias do disco **sda** e **sdb**, duas instâncias de processador **0 e 1**e uma **versão de servidor do Red Hat Enterprise Linux 7.4 (Maipo) (sistema operativo)**. 

![Modelo de componente de exemplo apresentado no diagnóstico de estado de funcionamento](./media/monitoring-vminsights-health/health-diagnostics-page-component.png)

### <a name="health-criteria"></a>Critérios de estado de funcionamento
A coluna centro na página de diagnóstico de estado de funcionamento é o **critérios de estado de funcionamento** coluna. O modelo de estado de funcionamento definido para a VM é apresentado numa árvore hierárquica. O modelo de estado de funcionamento para uma VM é composta por unidade de dependência e os critérios de estado de funcionamento agregado.  

![Critérios de estado de funcionamento de exemplo apresentados no diagnóstico de estado de funcionamento](./media/monitoring-vminsights-health/health-diagnostics-page-healthcriteria.png)

Um critério de estado de funcionamento mede o estado de funcionamento da instância monitorizado com alguns critérios, o que poderia ser um valor de limiar ou um Estado de uma entidade, etc. Um critério de estado de funcionamento tem duas ou três Estados de funcionamento, tal como descrito na secção acima. Em qualquer determinado momento, o critério de estado de funcionamento pode estar em apenas um dos seus Estados potenciais. 

O estado de funcionamento geral de um destino é determinado a partir do Estado de funcionamento de cada um dos respetivos critérios de estado de funcionamento definidos no modelo de estado de funcionamento. Esta será uma combinação de critérios de estado de funcionamento direcionados diretamente para o destino, os critérios de estado de funcionamento direcionados para os componentes agregados ao destino por meio de um critério de estado de funcionamento de dependência. Esta hierarquia é ilustrada na **critérios de estado de funcionamento** secção da página Diagnóstico de estado de funcionamento. A política de rollup do Estado de funcionamento é parte da configuração dos critérios de estado de funcionamento de agregação e dependência. Pode encontrar uma lista de conjunto predefinido de critérios de estado de funcionamento em execução como parte desta funcionalidade na seção [detalhes de configuração da monitorização](#monitoring-configuration-details).  

No exemplo a seguir, o critério de estado de funcionamento agregado **Rollup de serviços de Windows Core** para uma VM baseada no Windows, avalia o estado de funcionamento dos serviços do Windows mais críticos, com base nos critérios de estado de funcionamento do serviço individual. O estado de cada serviço, como DNS, etc. DHCP, são avaliadas e o estado de funcionamento é agregado ao critério de estado de funcionamento de rollup correspondente (conforme mostrado abaixo).  

![Exemplo de rollup do Estado de funcionamento](./media/monitoring-vminsights-health/health-diagnostics-windows-svc-rollup.png)

O estado de funcionamento a **Rollup de serviços de Windows Core** para o estado de funcionamento é agregado **disponibilidade do sistema operativo**, que, eventualmente, faz o rollup para o **disponibilidade** da VM. 

Critérios de estado de funcionamento **unidade** tipo pode ter a respetiva configuração modificada clicando no link a elipse até o momento certo e selecionando **Mostrar detalhes** para abrir o painel de configuração. 

![Configurar um exemplo de critérios de estado de funcionamento](./media/monitoring-vminsights-health/health-diagnostics-linuxvm-example-03.png)

No painel de configuração para os critérios de estado de funcionamento selecionado, neste exemplo o **disco lógico % espaço livre** pode ser configurado com um valor numérico diferente para o respetivo limiar, pois é um monitor de dois Estados, que significa que ele só é alterado de bom estado de funcionamento para crítico.  Outro critério de estado de funcionamento pode ser três Estados, onde pode configurar um valor para o limiar de estado de funcionamento de aviso e crítico.  

>[!NOTE]
>Aplicar a alteração de configuração de critérios de estado de funcionamento para uma instância é aplicada a todas as instâncias monitorizadas.  Por exemplo, se selecionar **/mnt/recurso** e modificar a **disco lógico % espaço livre** limiar não se aplica a apenas essa instância, mas todas as outras instâncias do disco lógico detetado e monitorizado no VM.
>

![Configurar um critério de estado de funcionamento de um exemplo de monitor de unidade](./media/monitoring-vminsights-health/health-diagnostics-linuxvm-example-04.png)

Se quiser saber mais sobre o indicador de estado de funcionamento, artigos de conhecimento são incluídos que ajuda a identificar problemas, causas e resoluções.  Clique nas **ver informações** link na página e este será aberto um novo separador no browser que mostra o artigo de conhecimento específico.  Em qualquer altura, pode rever todos os artigos de conhecimento de critério de estado de funcionamento incluídos com o Azure Monitor para a funcionalidade de estado de funcionamento de VMs [aqui](https://docs.microsoft.com/azure/monitoring/infrastructure-health/).
  
### <a name="state-changes"></a>Alterações de estado
A coluna mais à direita na página de diagnóstico de estado de funcionamento é **as alterações de estado**. Ele lista todas as alterações de estado associadas com os critérios de estado de funcionamento que está selecionado no **critérios de estado de funcionamento** seção ou a alteração do Estado da VM se uma VM tiver sido selecionada a partir do **modelo de componente** ou **Critérios de estado de funcionamento** coluna da tabela. 

![Alterações de estado de exemplo apresentadas no diagnóstico de estado de funcionamento](./media/monitoring-vminsights-health/health-diagnostics-page-statechanges.png)

Esta secção compreende o estado de critérios de estado de funcionamento e o tempo associado ordenados pelo Estado mais recente na parte superior.   

### <a name="association-of-component-model-health-criteria-and-state-change-columns"></a>Associação de modelo de componente, critérios de estado de funcionamento e estado alterar colunas 
As três colunas são interligadas entre si. Quando um usuário seleciona uma instância detetada no modelo de componente, o **critérios de estado de funcionamento** secção é filtrada para essa exibição de componente e proporcionalmente a **alteração de estado** é atualizado com base selecionada critérios de estado de funcionamento. 

![Exemplo de seleção de instância monitorizada e os resultados](./media/monitoring-vminsights-health/health-diagnostics-linuxvm-example-02.png)

No exemplo acima, quando um seleciona **(disco lógico) de /mnt**, a árvore de critérios de estado de funcionamento é filtrada para **/mnt (disco lógico)**. O **disponibilidade** e **desempenho** separadores são filtrados em conformidade demasiado. O **de alteração de estado** coluna mostra a alteração de estado com base na disponibilidade dos **/mnt (disco lógico)**. 

Para ver o estado de funcionamento atualizado, pode atualizar a página de diagnóstico de estado de funcionamento ao clicar o **atualizar** ligação.  Se existir uma atualização do Estado de funcionamento do critério de estado de funcionamento com base no intervalo de consulta predefinido, essa tarefa permite-lhe evitar espera e reflete o estado de funcionamento mais recente.  O **estado de funcionamento de critérios** é um filtro que lhe permite definir o âmbito os resultados com base no estado do Estado de funcionamento selecionado - bom estado de funcionamento, aviso, crítico, desconhecido e tudo.  O **última atualização** tempo no canto superior direito representa a última vez em quando a página de diagnóstico de estado de funcionamento foi atualizada.  

## <a name="alerting-and-alert-management"></a>Gestão de alerta e alerta 
Monitor do Azure para a funcionalidade de estado de funcionamento de VMs integra [alertas do Azure](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md) e emite um alerta quando os critérios de estado de funcionamento predefinido alterar de bom estado de funcionamento para um mau estado de funcionamento, quando a condição for detectada. Alertas são categorizados por gravidade - gravidade 0 a 4, com 0 representando o nível de gravidade mais elevado de gravidade.  

Número total de alertas de estado de funcionamento da VM categorizados por gravidade está disponível na **estado de funcionamento** dashboard na secção **alertas**. Ao selecionar o número total de alertas ou o número correspondente a um nível de gravidade, o **alertas** página abre e apresenta uma lista de todos os alertas que correspondam a sua seleção.  Por exemplo, se tiver selecionado a linha correspondente **nível de gravidade 1**, em seguida, verá a vista seguinte:

![Exemplo de todos os alertas de nível de gravidade 1](./media/monitoring-vminsights-health/vminsights-sev1-alerts-01.png)

Sobre o **alertas** página, ele apenas não tem como escopo para mostrar alertas correspondentes a sua seleção, mas também são filtradas pela **tipo de recurso** para mostrar apenas os alertas de estado de funcionamento gerados pelo recurso de máquina virtual.  Isto é refletido na lista de alertas, abaixo da coluna **recurso de destino**, onde ele mostra que a VM do Azure, o alerta foi gerado para quando a condição de mau estado de funcionamento dos critérios de estado de funcionamento específico foi cumprida.  

Alertas a partir de outros tipos de recursos ou serviços não se destinam a serem incluídos nesta vista, como alertas de registo com base no Log Analytics consultas ou alerta de métrica que normalmente seria ver do padrão do Azure Monitor [todos os alertas](../monitoring-and-diagnostics/monitoring-overview-alerts.md#all-alerts-page) página. 

Pode filtrar esta vista ao selecionar os valores nos menus de lista pendente na parte superior da página.

|Coluna |Descrição | 
|-------|------------| 
|Subscrição |Selecione uma subscrição do Azure. Apenas os alertas na subscrição selecionada são incluídos na vista. | 
|Grupo de Recursos |Selecione um grupo de recursos. Apenas os alertas com destinos no grupo de recursos selecionado estão incluídos na vista. | 
|Tipo de recurso |Selecione um ou mais tipos de recursos. Por predefinição, apenas os alertas de destino **máquinas virtuais** é selecionado e incluído nesta vista. Esta coluna só está disponível depois de um grupo de recursos foi especificado. | 
|Recurso |Selecione um recurso. Apenas alertas com esse recurso como um destino estão incluídas na vista. Esta coluna apenas está disponível após foi especificado um tipo de recurso. | 
|Gravidade |Opte por uma gravidade de alerta ou selecione *todos os* para incluir alertas de todas as gravidades. | 
|Condição do Monitor |Selecione uma condição do monitor para filtrar alertas se eles tiverem sido *Fired* pelo sistema ou *resolvido* pelo sistema se a condição já não estiver ativa. Ou selecione *todos os* para incluir alertas de todas as condições. | 
|Estado de alerta |Selecione um Estado de alerta *New*, *confirmação*, *fechado*, ou selecione *todos os* para incluir alertas de todos os Estados. | 
|Monitorizar serviço |Selecione um serviço ou selecione *todos os* para incluir todos os serviços. Apenas alertas a partir da *VM Insights* são suportados para esta funcionalidade.| 
|Intervalo de tempo| Apenas os alertas acionados dentro da janela de tempo selecionado estão incluídos na vista. Os valores suportados são a última hora, últimas 24 horas, últimos 7 dias e os últimos 30 dias. | 

O **detalhes do alerta** página é apresentada quando seleciona um alerta, fornecendo detalhes do alerta e permitindo que altere seu estado. Para saber mais sobre como gerir alertas, veja [criar, ver e gerir alertas ao utilizar o Azure Monitor](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md).  

>[!NOTE]
>Neste momento, não é suportada para criar novos alertas com base nos critérios de estado de funcionamento ou modificar o estado de funcionamento regras de alerta existentes no Azure Monitor no portal.  
>

![Painel de detalhes do alerta para um alerta selecionado](./media/monitoring-vminsights-health/alert-details-pane-01.png)

Alerta de estado também pode ser alterado para um ou vários alertas, selecionando-os e, em seguida, selecionando **alterar o estado** partir a **todos os alertas** página, no canto superior esquerdo. Sobre o **alterar o estado de alerta** painel selecionar um dos Estados, adicione uma descrição da alteração no **comentário** campo e, em seguida, clique em **Ok** para consolidar as alterações. Enquanto as informações são verificadas e as alterações são aplicadas, pode acompanhar o progresso em **notificações** no menu.  

## <a name="next-steps"></a>Passos Seguintes
Para identificar afunilamentos e a utilização geral com o desempenho de VMs, veja [vista de desempenho da VM do Azure](monitoring-vminsights-performance.md), ou para ver dependências de aplicações detetadas, consulte [vista de Azure Monitor para o mapa de VMs](monitoring-vminsights-maps.md). 