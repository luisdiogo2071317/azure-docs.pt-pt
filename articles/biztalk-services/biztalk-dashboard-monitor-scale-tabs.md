---
title: Dashboard, Monitor, escala, configurar e as ligações híbridas dos serviços BizTalk | Documentos da Microsoft
description: Saiba mais sobre os controles e monitorizar o desempenho de serviços BizTalk
services: biztalk-services
documentationcenter: ''
author: MandiOhlinger
manager: anneta
editor: ''
ms.assetid: 7a1815db-0de2-4274-8be0-198c1b077324
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: 3f4763b5e15d4b9b84e868262a9e8538b8a407a2
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51228832"
---
# <a name="review-the-dashboard-monitor-scale-configure-and-hybrid-connection-tabs"></a>Reveja os separadores do Dashboard, Monitor, Escala, Configurar e Ligação Híbrida

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

Depois de criar o seu BizTalk Service e implementar a sua aplicação, pode alterar algumas das definições do serviço BizTalk e monitorizar o desempenho do aplicativo. 

> [!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)]

Esta ação abre uma nova janela com os seguintes separadores. Este tópico descreve os seguintes separadores.

## <a name="quickstart-quickstartquickstart"></a>(Início rápido![Início Rápido][Quickstart])
Dependendo da edição de serviços do BizTalk, todas as opções apresentadas poderão não estar disponíveis. 

<table border="1">
    <tr>
        <td><strong>Obtenha as ferramentas</strong></td>
        <td>Transferir o SDK de serviços do BizTalk para instalar os modelos de projeto do Visual Studio no seu computador de desenvolvimento no local. Estes modelos criam a <strong>os serviços BizTalk</strong> (bridge) e o <strong>artefatos do BizTalk Service</strong> projetos (transformação) Visual Studio, que são implementados para o seu BizTalk Service.
        <br/><br/>
        <a HREF="https://go.microsoft.com/fwlink/p/?LinkID=302335"> Como posso começar a utilizar o SDK de serviços do BizTalk do Azure </a> e <a HREF="https://go.microsoft.com/fwlink/p/?LinkID=241589">instalar o SDK de serviços BizTalk do Azure</a> lista os passos para começar a utilizar.
        </td>
    </tr>
    <tr>
        <td><strong>Criar contratos de parceiros</strong></td>
        <td>Abre o Portal de serviços do BizTalk do Azure alojada no Azure, onde pode adicionar parceiros e criar X12, AS2 e contratos EDIFACT EDI.
        <br/><br/>
        <a HREF="https://go.microsoft.com/fwlink/p/?LinkID=303653">Configuração de componentes para mensagens de EDI no Portal dos serviços BizTalk</a> lista os passos para começar a utilizar.
        </td>
    </tr>

<tr>
        <td><strong>Saiba mais sobre os serviços BizTalk</strong></td>
        <td>Vá para o <a HREF="https://azure.microsoft.com/documentation/services/biztalk-services/">central de informações</a> para saber mais sobre os serviços BizTalk do Azure.</td>
</tr>
</table>


Na barra de tarefas na parte inferior, pode:

<table border="1">

<tr>
<td><strong>Gerir</strong> a implementação da aplicação</td>
<td>É aberto o portal dos serviços BizTalk do Azure. O Portal de serviços do BizTalk é a entrada para a configuração de EDI, incluindo adicionar parceiros e criar X12, AS2 e contratos EDIFACT.
<br/><br/>
Este é o mesmo que <strong>criar contratos de parceiros</strong> sobre o <strong>início rápido</strong> separador.
<br/><br/>
<a HREF="https://go.microsoft.com/fwlink/p/?LinkID=303653">Configuração de componentes para mensagens de EDI no Portal dos serviços BizTalk</a> fornece mais informações sobre o Portal de serviços do BizTalk.</td>
</tr>

<tr>
<td><strong>Informações de ligação</strong> do espaço de nomes do controlo de acesso</td>
<td>Quando seleciona informações de ligação, são apresentados o espaço de nomes de controle de acesso, o emissor predefinido e a chave predefinida. Pode copiar esses valores.
<br/><br/>
Também pode abrir o Portal de controlo de acesso. <a HREF="https://go.microsoft.com/fwlink/p/?LinkID=285670">Criar um espaço de nomes do controlo de acesso</a> fornece mais informações sobre o Portal de controlo de acesso.</td>
</tr>

<tr>
<td><strong>Sincronizar chaves</strong> na conta de armazenamento</td>
<td>Quando cria uma Conta de armazenamento, são criadas automaticamente uma Chave Primária e uma Chave Secundária. Essas chaves de encriptação controlam o acesso à sua conta de armazenamento. O BizTalk Service utiliza automaticamente a chave primária. <strong>Sincronizar chaves</strong> permitir que os utilizadores alternar entre a chave primária e a chave secundária sem interromper o serviço BizTalk.
<br/><br/>
Por exemplo, pretende que o BizTalk Service para utilizar uma nova chave primária para a conta de armazenamento. Para efetuar este procedimento:
<br/><br/>
<ol>
<li>Selecione o seu BizTalk Service e selecione <strong>sincronizar chaves</strong>. Selecione a chave secundária. Ao fazê-lo, o serviço BizTalk é iniciado utilizando a chave secundária.</li>
<li>Selecione a sua conta de armazenamento e voltar a gerar a chave primária. Lembre-se de que o BizTalk Service está a utilizar a chave secundária.</li>
<li>Selecione o seu BizTalk Service e selecione <strong>sincronizar chaves</strong>. Agora, selecione a chave primária. Esta é a nova chave primária é regenerado.</li>
<li>Selecione a sua conta de armazenamento e voltar a gerar a chave secundária.</li>
</ol>
<br/>
Este processo é denominado "efetuar rollover das chaves". O objetivo é permitir que os utilizadores alternar entre a chave primária e a chave secundária sem interromper o serviço BizTalk.</td>
</tr>

<tr>
<td><strong>Eliminar</strong> seu aplicativo</td>
<td>Ao selecionar eliminar, seu BizTalk Service e todos os itens implementados no mesmo são removidos.</td>
</tr>
</table>


## <a name="dashboard"></a>Dashboard
Dependendo da edição de serviços do BizTalk, todas as opções apresentadas poderão não estar disponíveis. 

Ao selecionar o nome do seu BizTalk Service, é apresentado o separador Dashboard. No Dashboard, pode:

##### <a name="usage-overview-shows-the-number-of-used-hybrid-connections"></a>Descrição geral da utilização: Mostra o número de ligações híbridas, utilizado
Também apresenta a utilização de dados em GB. 

##### <a name="metric-graph-shows-a-fixed-list-of-performance-metrics"></a>Gráfico de métrica: Mostra uma lista fixa de métricas de desempenho
Estas métricas fornecem valores em tempo real sobre o estado de funcionamento do serviço BizTalk. Também pode escolher o **relativo** ou **absoluto** valores e o intervalo de tempo **intervalo** das métricas que são apresentadas no gráfico. 

Para obter uma descrição destas métricas de desempenho, aceda a [métricas disponíveis](#Metrics) neste tópico.

##### <a name="quick-glance-lists-your-biztalk-service-properties"></a>Leitura rápida: Listas as propriedades de seu BizTalk Service
<table border="1">

<tr>
<td><strong>Atualizar as credenciais da base de dados de controlo</strong></td>
<td>Muda o nome de utilizador e palavra-passe utilizada para iniciar sessão na base de dados de controlo.</td>
</tr>
<tr>
<td><strong>Atualizar o certificado SSL</strong></td>
<td>Pode atualizar o BizTalk Service para utilizar um certificado SSL diferente. Um certificado SSL autoassinado é criado automaticamente quando <a HREF="https://go.microsoft.com/fwlink/p/?LinkID=302280">criar o serviço BizTalk</a>.</td>
</tr>
<tr>
<td><strong>Transferir certificado</strong></td>
<td>Pode transferir o certificado SSL utilizado pelo BizTalk Service para um computador local.</td>
</tr>
<tr>
<td><strong>Estado</strong></td>
<td>Apresenta o estado atual do seu BizTalk Service. Ver <a HREF="https://go.microsoft.com/fwlink/p/?LinkID=329870">os serviços BizTalk: gráfico de estado do serviço</a>. </td>
</tr>
<tr>
<td><strong>URL do serviço</strong></td>
<td>O URL para o seu serviço BizTalk. Este é o mesmo que o <strong>URL de domínio</strong> introduzido quando o serviço BizTalk é criado.</td>
</tr>
<tr>
<td><strong>Endereço IP Virtual público (VIP)</strong></td>
<td>O endereço IP atribuído ao seu serviço BizTalk. Ele é utilizado para todos os pontos finais de entrada e é o endereço de origem para o tráfego de saída. Este endereço IP pertence ao seu serviço BizTalk, desde que ele é criado. Se eliminar o serviço BizTalk, é atribuído o endereço IP para outro serviço BizTalk.</td>
</tr>
<tr>
<td><strong>Espaço de nomes do ACS</strong></td>
<td>Efetua a autenticação com o BizTalk Service.</td>
</tr>
<tr>
<td><strong>Edição</strong></td>
<td>Listas a edição introduzido quando o serviço BizTalk é criado.</td>
</tr>
<tr>
<td><strong>Localização</strong></td>
<td>Apresenta a região geográfica que aloja o seu serviço BizTalk.</td>
</tr>
<tr>
<td><strong>Criado</strong></td>
<td>Apresenta a data e hora que foi criado o BizTalk Service.</td>
</tr>
<tr>
<td><strong>Base de dados de controlo</strong></td>
<td>O nome de base de dados do Azure SQL que armazena as tabelas de controlo utilizadas pelo BizTalk Service. 
<br/><br/>
<a HREF="https://go.microsoft.com/fwlink/p/?LinkID=302280">Requisitos Explained</a> fornece detalhes sobre a base de dados de controlo.</td>
</tr>
<tr>
<td><strong>Armazenamento de monitorização/arquivo</strong></td>
<td>O nome da conta de armazenamento do Azure que armazena o resultado da monitorização do seu BizTalk Service.
<br/><br/>
<a HREF="https://go.microsoft.com/fwlink/p/?LinkID=302280">Requisitos Explained</a> fornece detalhes sobre a conta de armazenamento.</td>
</tr>
<tr>
<td><strong>Nome da subscrição</strong></td>
<td>Lista a subscrição que aloja o seu serviço BizTalk. A subscrição controla o acesso.</td>
</tr>
<tr>
<td><strong>ID de subscrição</strong></td>
<td>Quando é criada uma subscrição, um ID de subscrição é gerado automaticamente. Ao utilizar REST APIs, poderá ter de introduzir o ID de subscrição.</td>
</tr>
</table>

[BizTalk Services: Aprovisionamento](https://go.microsoft.com/fwlink/p/?LinkID=302280) mostra os passos para criar um BizTalk Service.

##### <a name="manage-connection-information-sync-keys-and-delete-in-the-task-bar"></a>Gerir, informações de ligação, sincronizar chaves e Delete na barra de tarefas:
<table border="1">

<tr>
<td><strong>Gerir</strong> a implementação da aplicação</td>
<td>É aberto o Portal de serviços BizTalk do Azure. O Portal de serviços do BizTalk é a entrada para a configuração de EDI, incluindo adicionar parceiros e criar X12, AS2 e contratos EDIFACT.
<br/><br/>
Este é o mesmo que <strong>criar contratos de parceiros</strong> sobre o <strong>início rápido</strong> separador.
<br/><br/>
<a HREF="https://go.microsoft.com/fwlink/p/?LinkID=303653">Configuração de componentes para mensagens de EDI no Portal dos serviços BizTalk</a> fornece mais informações sobre o Portal de serviços do BizTalk.</td>
</tr>
<tr>
<td><strong>Informações de ligação</strong> do espaço de nomes do controlo de acesso</td>
<td>Exibe o espaço de nomes de controle de acesso, o emissor predefinido e valores de chave predefinida; que podem ser copiado.
<br/><br/>
Também pode abrir o Portal de controlo de acesso. Este Portal de controlo de acesso é igual a utilizar a opção de Active Directory no painel de navegação esquerdo.
<br/><br/>
<a HREF="https://go.microsoft.com/fwlink/p/?LinkID=285670">Gerir o seu espaço de nomes do ACS</a> fornece mais informações sobre o Portal de controlo de acesso.</td>
</tr>
<tr>
<td><strong>Sincronizar chaves</strong> na conta de armazenamento</td>
<td>Quando cria uma Conta de armazenamento, são criadas automaticamente uma Chave Primária e uma Chave Secundária. Essas chaves de encriptação controlam o acesso à sua conta de armazenamento. O BizTalk Service utiliza automaticamente a chave primária. <strong>Sincronizar chaves</strong> permitir que os utilizadores alternar entre a chave primária e a chave secundária sem interromper o serviço BizTalk.
<br/><br/>
Por exemplo, pretende que o BizTalk Service para utilizar uma nova chave primária para a conta de armazenamento. Para efetuar este procedimento:
<br/><br/>
<ol>
<li>Selecione o seu BizTalk Service e selecione <strong>sincronizar chaves</strong>. Selecione a chave secundária. Ao fazê-lo, o serviço BizTalk é iniciado utilizando a chave secundária.</li>
<li>Selecione a sua conta de armazenamento e voltar a gerar a chave primária. Lembre-se de que o BizTalk Service está a utilizar a chave secundária.</li>
<li>Selecione o seu BizTalk Service e selecione <strong>sincronizar chaves</strong>. Agora, selecione a chave primária. Esta é a nova chave primária é regenerado.</li>
<li>Selecione a sua conta de armazenamento e voltar a gerar a chave secundária.</li>
</ol>
<br/>
Este processo é denominado "efetuar rollover das chaves". O objetivo é permitir que os utilizadores alternar entre a chave primária e a chave secundária sem interromper o serviço BizTalk.</td>
</tr>

<tr>
<td><strong>Eliminar</strong> seu aplicativo</td>
<td>O serviço BizTalk e todos os itens implementados no mesmo são removidos.</td>
</tr>
</table>


## <a name="monitor"></a>Monitorizar
Não é aplicável para a edição gratuita.

Ao selecionar o nome do seu BizTalk Service, no separador Monitor está disponível e apresenta o seguinte:

##### <a name="metric-graph-displays-the-selected-performance-metrics"></a>Gráfico de métrica: Apresenta as métricas de desempenho selecionados
Estas métricas fornecem valores em tempo real sobre o estado de funcionamento do serviço BizTalk. Escolha as métricas de desempenho são apresentadas. Um máximo de seis métricas de desempenho pode ser exibido em simultâneo. 

Também pode escolher o **relativo** ou **absoluto** valores e o intervalo de tempo **intervalo** das métricas que são apresentadas. 

##### <a name="to-remove-or-display-metrics-in-the-graph"></a>Para remover ou ver métricas no gráfico:
1. Selecione o **Monitor** separador.
2. Selecione **adicionar métricas** na barra de tarefas:  
   ![Selecione adicionar métricas][AddMetrics]
3. Verificar as métricas de desempenho que pretende apresentar.
4. Selecione a marca de verificação a devolver para o **Monitor** separador.
5. Selecione o círculo junto a métrica para apresentar o valor dessa métrica no gráfico.  
   
    Por exemplo, o **utilização da CPU** métrica está a cinzento; o resultado não é apresentado no gráfico:  
   ![Métrica de utilização da CPU está a cinzento][GrayedMetric]  
   
    Selecione o desativada, o círculo para ativar a **utilização da CPU** métrica para apresentar o resultado no gráfico:  
   ![Métrica de utilização da CPU está ativada][EnabledMetric]
6. Para remover uma métrica do gráfico de apresentação e a lista, selecione **eliminar métrica** na barra de tarefas. Para adicionar a métrica back para a lista, selecione **adicionar métricas** na barra de tarefas, verifique a métrica e selecione a marca de verificação a devolver para o **Monitor** separador. Selecione o desativada o círculo para ativar a métrica.

## <a name="Metrics"></a>Métricas disponíveis
Os seguintes contadores de desempenho/métricas estão disponíveis:

<table border="1">

<tr>
<td><strong>Latência de RountdTrip</strong></td>
<td>Apresenta o tempo médio decorrido em milissegundos (ms) para processar uma mensagem desde o momento em que a mensagem é recebida até que a mensagem é totalmente processada pelo serviço BizTalk em todas as pontes. São contabilizadas apenas as mensagens processadas com êxito.<br/><br/>
Quando ocorrem os seguintes eventos, é criado um timestamp:
<ul>
<li>Mensagem entra o gateway</li>
<li>Mensagem é encaminhada para o destino</li>
<li>Resposta de destino é recebida</li>
<li>Resposta de confirmação de destino enviada para o gateway</li>
</ul>
<br/>
Esta métrica mostra o resultado do cálculo seguinte:
<br/><br/>
[Destino confirmação a resposta é enviada para o gateway] - [mensagem entra o gateway]</td>
</tr>
<tr>
<td><strong>Falhas na origem</strong></td>
<td>Apresenta o número total de mensagens que falhou pelo serviço BizTalk quando recebendo mensagens dos pontos de extremidade de origem.</td>
</tr>
<tr>
<td><strong>Utilização da CPU</strong></td>
<td>Apresenta a % de média de tempo do processador de todas as instâncias de função.</td>
</tr>
<tr>
<td><strong>Latência de processamento</strong></td>
<td>Apresenta o tempo médio que em milissegundos (ms) para processar uma mensagem o serviço BizTalk em todas as pontes, excluindo o tempo gasto em destinos. São contabilizadas apenas as mensagens processadas com êxito.<br/><br/>
Quando cada um dos seguintes eventos ocorrem, é criado um timestamp:

<ul>
<li>Mensagem entra o gateway</li>
<li>Mensagem é encaminhada para o destino</li>
<li>Resposta de destino é recebida</li>
<li>Resposta de confirmação de destino enviada para o gateway</li>
</ul>
<br/>Esta métrica mostra o resultado do cálculo seguinte:<br/><br/>
[Destino confirmação a resposta é enviada para o gateway] - [entra o gateway de mensagem] - [é recebida a resposta do destino] + [mensagem é encaminhada para o destino]</td>
</tr>
<tr>
<td><strong>Falhas no processo</strong></td>
<td>Apresenta o número total de mensagens que falharam durante o processamento pelo serviço BizTalk em todas as pontes dentro de um intervalo de tempo.</td>
</tr>
<tr>
<td><strong>Mensagens enviadas</strong></td>
<td>Apresenta o número total de mensagens enviadas pelo serviço BizTalk em todas as pontes dentro de um intervalo de tempo. Esta métrica é incrementada quando uma mensagem enviada a partir de um pipeline atinge o destino da rota. Esta métrica não indica que uma mensagem é processada com êxito.<br/><br/>
Num cenário de solicitação-resposta, a métrica é incrementada quando o destino da rota envia uma confirmação de receção para o pipeline.</td>
</tr>
<tr>
<td><strong>Mensagens recebidas</strong></td>
<td>Apresenta o número total de mensagens recebidas pelo serviço BizTalk em todas as pontes dentro de um intervalo de tempo. Esta métrica é incrementada quando uma nova mensagem é recebida pelo pipeline.</td>
</tr>
<tr>
<td><strong>Mensagens no processo</strong></td>
<td>Apresenta o número total de mensagens atualmente a ser processado pelo serviço BizTalk dentro de um intervalo de tempo.</td>
</tr>
<tr>
<td><strong>Mensagens processadas</strong></td>
<td>Apresenta o número total de mensagens processada com êxito pelo serviço BizTalk em todas as pontes dentro de um intervalo de tempo. Esta métrica é incrementada quando uma mensagem é recebida com êxito pelo pipeline e encaminhada com êxito para o destino.</td>
</tr>
</table>


## <a name="scale"></a>Escala
No separador de dimensionamento, pode adicionar ou subtrair o número de unidades utilizadas pelo BizTalk Service. Por predefinição, existe uma unidade configurada. Unidades adicionais podem ser adicionadas para dimensionar o serviço BizTalk. Quando aumenta a escala, estará aumentando o débito. A quantidade de recursos também aumenta, incluindo pontes implementados, contratos, ligações de LOB e poder de processamento. Por exemplo, aumenta a escala de 1 unidade a 2 unidades. Nesta situação, pode implementar duplicar o número de pontes, faça duplo os contratos, duas vezes as ligações de LOB e duas vezes o poder de processamento.

Algumas edições do BizTalk oferece uma opção de dimensionamento. Nesta situação, é permitida uma unidade. Para determinar quantas unidades de sua edição pode ser dimensionada, consulte [os serviços BizTalk: gráfico de edições](biztalk-editions-feature-chart.md).

Aumentar o número de unidades pode afetar a preços. Se aumentar as unidades, selecionando **guardar** exibe uma mensagem a indicar se a faturação é afetada. Em seguida, escolha de continuar. Quando aumenta o número de unidades, as alterações de estado do BizTalk Service do Active Directory para atualização. No estado de atualização, o seu BizTalk Service continua a ser executado.

[Serviços BizTalk: Gráfico de edições](biztalk-editions-feature-chart.md) define uma "unidade".

## <a name="configure"></a>Configurar
Não é aplicável às ligações híbridas.

Define o estado de cópia de segurança como None ou automática. Quando definido como None, não existem cópias de segurança são criadas automaticamente. Quando definido como automático, configure a localização de cópia de segurança, a frequência da cópia de segurança e durante quanto tempo para manter os ficheiros de cópia de segurança. 

[Serviços BizTalk: Cópia de segurança e restaurar](biztalk-backup-restore.md) fornece os detalhes. 

## <a name="HybridConnections"></a>Ligações híbridas
Ligações híbridas ligar uma aplicação do Azure, como aplicações Web ou de aplicações móveis no serviço de aplicações do Azure, a um recurso no local que utiliza uma porta TCP estática, como o SQL Server, MySQL, APIs da Web HTTP e a maioria dos serviços Web personalizados. Ligações híbridas são geridas dos serviços BizTalk.

Para criar ou gerir ligações híbridas dos serviços BizTalk do Azure, veja [ligações híbridas](integration-hybrid-connection-overview.md).

## <a name="next"></a>Seguinte
Agora que está familiarizado com os diferentes separadores, pode saber mais sobre as funcionalidades de serviços BizTalk do Azure:

* [Serviços BizTalk: limitação](biztalk-throttling-thresholds.md)  
* [Serviços BizTalk: Nome e Chave do Emissor](biztalk-issuer-name-issuer-key.md)  
* [Serviços BizTalk: Cópia de segurança e Restauro](biztalk-backup-restore.md)

## <a name="see-also"></a>Consultar Também
* [Ligações Híbridas](integration-hybrid-connection-overview.md)  
* [Serviços BizTalk: Programador, básico, Standard e Premium gráfico de edições](biztalk-editions-feature-chart.md)  
* [Os serviços BizTalk: aprovisionamento](biztalk-provision-services.md)  
* [Serviços BizTalk: Gráfico de estado do serviço BizTalk](biztalk-service-state-chart.md)  
* [Como posso começar a utilizar o SDK dos Serviços BizTalk do Azure](https://go.microsoft.com/fwlink/p/?LinkID=302335)

[Quickstart]: ./media/biztalk-dashboard-monitor-scale-tabs/QuickStartIcon.png
[AddMetrics]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_AddMetrics.png
[GrayedMetric]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_GrayedMetric.png
[EnabledMetric]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_EnabledMetric.png

