---
title: Ver as métricas-chave nos dashboards de gestão de custo do Azure | Microsoft Docs
description: Este artigo descreve como ver as métricas-chave com dashboards na gestão de custo do Azure.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 06/05/2018
ms.topic: conceptual
ms.service: cost-management
manager: dougeby
ms.custom: ''
ms.openlocfilehash: b1dc2e2eca900ca0ae72329c3c373b2d24f1b2e0
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35304100"
---
# <a name="view-key-cost-metrics-with-dashboards"></a>Chave de vista custo métricas com dashboards

Dashboards no Cloudyn fornecem uma vista de alto nível de relatórios. Dashboards permitem-lhe ver as métricas de custo de chave numa única vista. Também fornecem realça tendência de negócio para o ajudar a tomar decisões de negócio importantes.

Dashboards também são utilizados para criar vistas para pessoas com responsabilidades diferentes na sua organização, o que poderá incluir:

- Controlador financeiro
- Proprietário da aplicação ou o projeto
- Engenheiro de DevOps
- Executivos

Dashboards são constituídos por widgets e cada widget é essencialmente uma miniatura do relatório. Clique num widget para abrir o relatório. Quando personalizar relatórios, guardá-las para os meus relatórios e está a ser adicionados ao dashboard.

Versões de dashboard diferem para os utilizadores de gestão (\\<ServerName>\SMS_<SITECODE>\HOTFIX\<KB), Enterprise e Premium Cloudyn. As diferenças são determinadas por níveis de acesso de entidade. Para obter mais informações sobre os níveis de acesso, consulte [níveis de acesso de entidade](tutorial-user-access.md#entity-access-levels).

Depende da disponibilidade do dashboard no tipo de conta do fornecedor de serviço em nuvem que é utilizada quando visualizar os dashboards. O tipo de informações disponíveis e recolhidos pelo Cloudyn afeta relatórios nos dashboards. Por exemplo, se não tiver uma conta AWS não de ver o dashboard do controlador de S3. Da mesma forma, se não ativar o acesso do Azure Resource Manager para Cloudyn, em seguida, que não veja qualquer informações específicas do Azure em widgets do dashboard de otimizador.

Pode utilizar qualquer um dos dashboards premade ou pode criar o seu próprio dashboard com relatórios personalizados. Se não estiver familiarizado com Cloudyn relatórios, consulte [relatórios de gestão de custo de utilização](use-reports.md).

## <a name="create-a-custom-dashboard"></a>Criar um dashboard personalizado

Para começar rapidamente com um dashboard personalizado, pode duplicar um já existente para utilizar as respetivas propriedades. Em seguida, pode modificá-la de acordo com as suas necessidades. No dashboard que pretende copiar, clique em **guardar como**. Apenas pode duplicar dashboards personalizados — não é possível duplicar os dashboards que estão incluídos com Cloudyn.

Para criar um dashboard personalizado:

1. Na home page, clique em **adicionar novo +**. É apresentada a página do Dashboard do meu.  
    ![Dashboard](./media/dashboards/my-dashboard.png)
2. Clique em **adicionar o novo relatório**. É apresentada a caixa de adicionar o relatório.
3. Selecione o relatório que pretende adicionar a miniaplicação do dashboard. O widget é adicionada ao dashboard.
4. Repita os passos anteriores até que o dashboard seja concluído.
5. Para alterar o nome do dashboard, clique no nome do dashboard na home page do Dashboard e escreva o novo nome.

## <a name="modify-a-custom-dashboard"></a>Modificar um dashboard personalizado

Como criar um dashboard personalizado, não é possível modificar os dashboards incluídos com Cloudyn. Para modificar um relatório de dashboard personalizado:

1. No dashboard, localizar o relatório que pretende modificar e clique em **editar**. O relatório é apresentado.
2. Efetue as alterações que pretende para o relatório e clique em **guardar**. O relatório for atualizado e mostra as suas alterações.

## <a name="share-a-custom-dashboard"></a>Partilhar um dashboard personalizado

Pode partilhar um dashboard personalizado com outras pessoas para _pública_ ou _entidade My_. Quando partilha para público, todos os utilizadores podem ver o dashboard. Apenas os utilizadores com acesso à entidade atual podem ver o dashboard ao partilhar a minha entidade. Os passos para partilhar um dashboard personalizado com público e os meus entidade são semelhantes.

Para partilhar um dashboard personalizado para público:

1. No dashboard, clique em **Dashboard definições**. É apresentada a caixa de definições do Dashboard.  
    ![Opções de dashboard](./media/dashboards/dashboard-options.png)
2. Na caixa de definições de Dashboard, clique o símbolo de seta e, em seguida, clique em **pública**. É apresentada a caixa de diálogo de confirmação de Dashboard público.
3. Clique em **Sim**. O dashboard está agora disponível para outros utilizadores.

## <a name="delete-a-custom-dashboard-report"></a>Eliminar um relatório de dashboard personalizado

Pode eliminar um componente de relatórios personalizados a partir do dashboard. Eliminar o relatório a partir do dashboard não eliminou o relatório da lista de relatórios. Em vez disso, ao eliminar o relatório remove-o partir do dashboard apenas.

Para eliminar um componente de dashboard, no componente dashboard, clique em **eliminar**. Ao clicar em **eliminar** imediatamente elimina o componente do dashboard.

## <a name="share-a-dashboard-enterprise"></a>Partilhar um dashboard (Enterprise)

Pode partilhar dashboards personalizados para todos os utilizadores na sua organização ou com os utilizadores da entidade atual. Partilhar um dashboard pode atribuir a outras pessoas uma vista rápida de alto nível do seu KPI. Quando partilha um dashboard, replica automaticamente o dashboard para todos os seus Cloudyn entidades/clientes. Alterações ao dashboard partilhado são automaticamente atualizadas.

Para partilhar um dashboard com todos os utilizadores, incluindo subentidades:

1. Na home page do dashboard, clique em **editar**.
2. Clique em **partilha** e, em seguida, selecione **pública**.
3. É apresentada a caixa de confirmação de Dashboard público Global.
4. Clique em **Sim** para definir o dashboard como dashboard público global.

Para partilhar um dashboard com todos os utilizadores de uma entidade atual:

1. Na home page do Dashboard, clique em **editar**.
2. Clique em **partilha** e, em seguida, selecione **entidade My**.
3. Clique em **Sim** para definir o dashboard como dashboard público.

## <a name="duplicate-a-custom-dashboard"></a>Duplicar um dashboard personalizado

Quando cria um novo dashboard, pode querer utilizar semelhantes propriedades de um dashboard existente. Pode duplicar o dashboard para criar um novo.

Apenas pode duplicar dashboards personalizados. Não é possível duplicar dashboards padrão.

Duplicar dashboard (clone) personalizadas:

1. No Dashboard que pretende duplicado, clique em **guardar como**. É aberto um novo dashboard com o mesmo nome e um número.
2. Mudar o nome do dashboard duplicado e modificá-lo como pretender.

-Ou-

1. Nas definições do Dashboard, clique em **guardar como** na linha do dashboard que pretende duplicado.
2. Abre o dashboard duplicado.
3. Mudar o nome do dashboard e modificá-lo como pretender.

## <a name="set-a-default-dashboard"></a>Definir um dashboard predefinido

Pode definir qualquer dashboard como a predefinição. Defini-la como a predefinição faz com que são apresentados como separador de mais à esquerda na lista de separador dashboard. O dashboard predefinido apresenta quando abrir o portal de Cloudyn.

- Clique no separador de dashboard que pretende definir como predefinição, em seguida, clique em **predefinido** à direita.

-Ou-

1. Clique em **Dashboard definições** para ver a lista de dashboards disponíveis e selecione o dashboard que pretende definir como predefinição.  
    ![Opções de dashboard](./media/dashboards/dashboard-options.png)
2. Clique em **predefinido** na linha do dashboard. É apresentada a caixa de confirmação de Dashboard predefinido.
3. Clique em **Sim**. O dashboard está definido como default.

## <a name="management-dashboard"></a>Dashboard de gestão
A gestão (ou dashboard \\<ServerName>\SMS_<SITECODE>\HOTFIX\<KB para utilizadores \\<ServerName>\SMS_<SITECODE>\HOTFIX\<KB) dashboard inclui destaca os tipos de relatório principal.  
![Dashboard de gestão](./media/dashboards/management-dash.png)

### <a name="cost-entity-summary-enterprise-only"></a>Resumo de custo de entidade (apenas para empresas)
Este widget resume as entidades de custo gerido, incluindo o número de entidades e número de contas.
- Clique em widget para abrir o relatório de detalhes de empresa.

### <a name="cost-over-time"></a>Custo ao longo do tempo
Este widget pode ajudar a detetar as tendências de custo. Destaca os custos para o último dia, com base na tendência dos últimos 30 dias.
- Clique em widget para abrir o relatório de custo real ao longo do tempo para ver e filtrar detalhes adicionais.

### <a name="asset-controller"></a>Controlador ativo
Este widget realça o número de instâncias em execução do dia anterior, acima a tendência de utilização nos últimos 30 dias.
- Clique em widget para abrir o dashboard do controlador ativo.

### <a name="unused-ri-detector"></a>Detector RI não utilizadas
Este widget realça o número de Amazon EC2 reservas não utilizadas.
- Clique em widget para abrir o relatório de atualmente não utilizado reservas onde pode ver o não utilizados reservas pode modificar.

### <a name="cost-by-service"></a>Custo pelo serviço
Este widget realça os custos amortized pelo serviço para os últimos 30 dias. Coloque o cursor sobre o gráfico circular para ver os custos por serviço.
- Clique em widget para abrir o relatório de análise de custos reais.

### <a name="potential-savings"></a>Redução de potenciais
Este widget mostra o tipo de instância de preços recomendações para Amazon EC2 e o RDS do Amazon.
- Clique em abrir o widget o relatório de análise de reduções. Lista os custos por tipos de instância com reduções possíveis.

### <a name="compute-instances---daily-trend"></a>Computação de instâncias - tendência diária
Este widget apresenta as instâncias ativas por tipo, nos últimos 30 dias.
- Clique em widget para abrir o relatório de instâncias ao longo do tempo, onde pode ver uma divisão de todas as instâncias em execução durante os últimos 30 dias.

### <a name="storage-by-department"></a>Armazenamento pelo departamento
Este widget apresenta os serviços de armazenamento utilizados por departamentos. Coloque o cursor sobre o gráfico circular para ver o consumo de armazenamento por departamento.
- Clique em widget para abrir o dashboard do controlador de S3.

## <a name="cost-controller-dashboard"></a>Dashboard de controlador de custos
O dashboard do controlador de custo mostra destaques de alocação de custo predefinidos.  
![Dashboard de controlador de custos](./media/dashboards/cost-controller-dashboard.png)

### <a name="cost-over-time"></a>Custo ao longo do tempo
Este widget ajuda-o a detetar as tendências de custo. Destaca os custos para o último dia, com base na tendência dos últimos 30 dias.
- Clique em widget para abrir o relatório de custo real ao longo do tempo para ver e filtrar detalhes adicionais.

### <a name="monthly-cost-trends"></a>Tendências de custos mensais
Este widget realça gastos amortized prevista e gaste real desde o início do mês.
- Clique em widget para abrir o relatório de custo de projetado de mês atual, que fornece um resumo de custo de data de mês.

Este relatório mostra o custo do início do mês, o custo do mês anterior e o custo do mês atual projetado. O mês atual custo previsto é calculado adicionando o custo mensal atualizado e projecção. A projecção baseia-se no custo monitorizado nos últimos 30 dias.

### <a name="12-month-planner"></a>Planeador de 12 meses
Este widget realça os custos previstos através dos seguintes 12 meses e a poupança de potencial.
- Clique em widget para abrir o relatório de custo projetado anuais.

### <a name="cost-by-service"></a>Custo pelo serviço
Este widget realça os custos amortized pelo serviço para os últimos 30 dias.
- Coloque o cursor sobre o gráfico circular para ver os custos por serviço.
- Clique em widget para abrir o relatório de análise de custos reais.

### <a name="cost-by-account"></a>Custo por conta
Este widget realça os custos amortized pela conta para os últimos 30 dias.
- Coloque o cursor sobre o gráfico circular para ver os custos por conta.
- Clique em widget para abrir o relatório de análise de custos reais.

### <a name="cost-trend-by-day"></a>Tendência do custo por dia
Este widget destaques gastam nos últimos 30 dias.
- Coloque o cursor sobre o gráfico de barras para ver os custos por dia.
- Clique em widget para abrir o relatório de custo real ao longo do tempo.

### <a name="cost-trend-by-month---last-6-months"></a>Tendência do custo por mês - últimos 6 meses

Este widget destaques gastam durante os últimos seis meses.
- Coloque o cursor sobre o gráfico de barras para ver os custos por mês.
- Clique em widget para abrir o relatório de custo real ao longo do tempo.

## <a name="asset-controller-dashboard"></a>Dashboard de controlador ativo

Este dashboard mostra o número de instâncias, discos disponíveis e em utilização, distribuição de tipos de instância e informações de armazenamento a executar.  
![Dashboard de controlador ativo](./media/dashboards/asset-controller-dashboard.png)

### <a name="compute-instances"></a>Instâncias de computação
Este widget mostra o número de instâncias com base na tendência de utilização nos últimos 30 dias em execução.
- Clique em widget para abrir o relatório de instâncias ao longo do tempo.

### <a name="disks"></a>Discos
Este widget realça o número total e o volume de discos, que estão em utilização e disponível.
- Clique em widget para abrir o relatório de discos ativos.

### <a name="instance-type-distribution"></a>Distribuição de tipo de instância
Este widget realça os tipos de instância de um gráfico circular.
- Clique no widget para abrir o relatório de distribuição de instância, que fornece uma repartição das suas instâncias ativas pela agregação selecionada.

### <a name="compute-instances---daily-trend"></a>Computação de instâncias - tendência diária
Este widget realça as instâncias de computação (spot, reservadas e a pedido) por dia nos últimos 30 dias.
- Coloque o cursor sobre o gráfico para ver o número de instâncias de computação por tipo por dia.
- Clique em widget para abrir o relatório de instâncias ao longo do tempo.

### <a name="all-buckets-s3"></a>Todos os registos (S3)
Este widget realça o armazenamento de S3 total e o número de objetos armazenados.
- Clique em widget para abrir o Dashboard de controlador S3. O dashboard ajuda a localizar, analisar e apresentar a utilização atual de armazenamento e as tendências.

### <a name="sql-db-instances-rds"></a>Instâncias de base de dados do SQL Server (RDS)
Este widget realça o número de instâncias de Amazon RDS com base na tendência dos últimos 30 dias em execução.
- Clique em widget para abrir o relatório de RDS instância ao longo do tempo.

## <a name="optimizer-dashboard"></a>O otimizador Dashboard
Este dashboard mostra downsizing recomendações, recursos não utilizados e redução de potenciais.  
![Dashboard de otimizador](./media/dashboards/optimizer-dashboard.png)

### <a name="ri-calculator"></a>Calculadora de RI
Este widget mostra o número de recomendações comprar RI e realça as poupanças anuais potenciais.
- Clique em widget para abrir a Calculadora de instância reservado onde possa determinar quando utilizar a pedido vs reservado planos de preços.

### <a name="sizing"></a>Dimensionamento
Este widget realça o dimensionamento recomendada e a redução de potencial, se implementada.
- Clique em widget para abrir o relatório de recomendações de dimensionamento EC2 eficaz de custo.

### <a name="unused-ri-detector"></a>Detector RI não utilizadas
Este widget realça o número de Amazon EC2 reservas não utilizadas.
- Clique em widget para abrir o relatório de atualmente não utilizado reservas onde pode ver as reservas de não utilizadas pode modificar.

###  <a name="available-disks"></a>Discos disponíveis
Este widget realça o número de discos desanexados na sua implementação.
- Clique em widget para abrir o relatório de discos não ligados.

### <a name="rds-ri-calculator"></a>Calculadora de RI RDS
Este widget realça o número de recomendações de reserva para as instâncias de Amazon RDS e a poupança de potencial.
- Clique em widget para abrir o relatório de RDS RI comprar recomendações onde pode ver as recomendações de Cloudyn para utilizar instâncias reservadas em vez de instâncias a pedido.

### <a name="rds-sizing"></a>Dimensionamento de RDS
Este widget mostra o número de recomendações de dimensionamento e a redução de potenciais.
- Clique em widget para abrir o relatório de RDS dimensionamento recomendações, que mostra detalhadas RDS Amazon recomendações de dimensionamento.

As recomendações de otimização são baseadas nos dados de utilização e desempenho monitorizados no mês passado.

## <a name="s3-tracker-dashboard"></a>Dashboard de controlador de S3
O dashboard de controlador de S3 ajuda a localizar, analisar e apresentar a utilização atual de armazenamento e as tendências.  
![Dashboard de controlador de S3](./media/dashboards/s3-tracker-dashboard.png)

### <a name="all-buckets"></a>Todos os registos
Este widget realça o tamanho total de todos os seus registos, GB e o número total de objetos no seu registos.
- Clique em widget para abrir o relatório de distribuição de S3 tamanho. O relatório ajuda a analisar o tamanho de S3 ao registo, pasta de nível superior, a classe de armazenamento e estado de controlo de versões.

### <a name="bucket-properties"></a>Propriedades de registo
Este widget realça o número total de registos de armazenamento.
- Clique em widget para ver o relatório de propriedades de registo de S3.

### <a name="scan-status"></a>Estado de análise
Este widget realça quando foi feita a última análise de S3 e quando um seguinte irá iniciar.
- Clique em widget para abrir o relatório de estado de análise S3.

### <a name="storage-by-bucket"></a>Armazenamento pelo registo
Este widget realça a percentagem de cada classe de armazenamento de registo está a utilizar.
- Clique em widget para abrir o relatório de distribuição de S3 tamanho. O relatório ajuda a analisar o tamanho de S3 ao registo, pasta de nível superior, a classe de armazenamento e estado de controlo de versões.

### <a name="number-of-objects-by-bucket"></a>Número de objetos pelo registo
Este widget realça o número de objetos por registo em número real e percentagem. Coloque o cursor sobre o registo para ver o total de objetos.
- Clique em widget para abrir o relatório de distribuição de S3 tamanho (análise baseada em).

## <a name="cloud-comparison-dashboard"></a>Dashboard de comparação de nuvem
O dashboard do Cloud comparação ajuda-o a comparar os custos de fornecedores de nuvem diferente com base nos preços, o tipo de CPU e o tamanho da RAM.  
![Dashboard de comparação de nuvem](./media/dashboards/cloud-comparison-dashboard.png)

### <a name="ec2-cost-in-azure-by-instance-type"></a>EC2 Custo no Azure mediante o tipo de instância
Este widget realça os últimos 30 dias de utilização em taxas a pedido. -Comares o custo com a atual EC2 Amazon custo vs a possibilidade de custos no Azure.
- Coloque o cursor sobre barras para comparar os custos por tipo de instância.
- Clique em widget para abrir as transferências sua implementação – relatório de análise de custos.

### <a name="ec2-cost-in-azure"></a>EC2 Custo no Azure
Este widget mostra os custos de Amazon EC2 atuais e compara-os para o Azure. A comparação é baseada nos últimos 30 dias de utilização em taxas a pedido.
- Clique em widget para abrir as transferências sua implementação - relatório de análise de custos.

### <a name="ec2azure-instance-type-mapping"></a>Mapeamento de tipo de instância EC2/Azure
Este widget realça o melhor mapeamento de unidades de computação elástico entre Amazon EC2 e o Azure.
- Clique em widget para abrir o relatório de mapeamento do tipo de instâncias.
