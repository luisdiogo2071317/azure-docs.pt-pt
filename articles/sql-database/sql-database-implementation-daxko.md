---
title: Estudo de caso do Azure - Daxko/CSI de base de dados SQL do Azure | Documentos da Microsoft
description: Saiba mais sobre como Daxko/CSI utiliza a base de dados SQL para acelerar o seu ciclo de desenvolvimento e para melhorar o serviço ao cliente e o desempenho
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: reference
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: carlrab
ms.openlocfilehash: ee06039242afe5b5e92cf2fecc4c3fa9bff0b78b
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/23/2018
ms.locfileid: "39214374"
---
# <a name="daxkocsi-used-azure-to-accelerate-its-development-cycle-and-to-enhance-its-customer-services-and-performance"></a>Daxko/CSI utilizado o Azure para acelerar o seu ciclo de desenvolvimento e para melhorar o serviço ao cliente e o desempenho
![Logótipo de Daxko/CSI](./media/sql-database-implementation-daxko/csidaxkologo25.png)

Software Daxko/CSI enfrentam um desafio: a base de clientes dos centros de adequação e recriação de foi a crescer rapidamente, graças ao sucesso de sua solução de software de empresarial abrangente, mas a acompanhar as necessidades de infra-estrutura de TI para que o cliente cada vez maior base testada da empresa, a equipe de TI. A empresa foi cada vez mais restrita por crescente sobrecarga de operações, particularmente para o gerenciamento de seus bancos de dados de cada vez mais. Pior ainda, essa sobrecarga de operações foi cortando nos recursos de desenvolvimento de novas iniciativas, como novos recursos de mobilidade para software da empresa.

De acordo com David Molina, diretor de desenvolvimento do produto em Daxko/CSI, fornecido pelo Azure CSI Software com o modelo plataforma-como-serviço (PaaS) que necessitava de para simplificar a gestão de base de dados, a aumentar a escalabilidade e a libertar recursos se concentrar em software em vez de ops. "Base de dados SQL do azure foi uma excelente opção para nós. Não ter de se preocupar sobre a manutenção de um SQL Server, um cluster de ativação pós-falha e tudo o que precisa de outra infra-estrutura foi ideal para nós."

Desde a migrar para o Azure, o Software de CSI precisa uma equipe de operações de apenas dois para gerir mais de 600 bases de dados do cliente. A empresa utiliza conjuntos elásticos da base de dados do Azure SQL para mover bases de dados do cliente com base no tamanho e precisa.

Molina continua, "os nossos clientes sentiram a alteração imediatamente. Antes de conjuntos elásticos, ocasionalmente tinham tempos limite e outros problemas durante períodos de rajada. Com os conjuntos elásticos do Azure, podem expandir conforme necessário e utilizar o software sem problemas."

Além de melhorar o desempenho para os clientes, os conjuntos elásticos Azure liberados recursos de Software de CSI para se focar no desenvolvimento de novos serviços e funcionalidades, em vez de lidar com operações e gerenciamento. Esses recursos de TI ajudaram a Software de CSI melhorar a sua oferta, SpectrumNG, para ajudar a interagir com os membros de gym, melhorar a eficiência da Equipe, de software de empresa e dê a equipe e membros acesso móvel para tarefas interativas e notificações em tempo real.

Azure ajudou também CSI Software acelerar e melhorar o desenvolvimento e o ciclo de controle de qualidade (QA), permitindo que opções de automatização. Com a implementação do Azure da empresa, os gerentes de compilação podem empacotar componentes com o clique de um botão. Tal como descrito Molina, "como parte do ciclo de lançamento, controle de qualidade é agora capaz de implantar num ambiente de teste no Azure, que imita estreitamente nossa pilha de produção. Podemos implementar compilações imediatamente ao nosso ambiente de desenvolvimento para verificar as alterações. Isso é uma grande vantagem para nós, porque não temos a paridade para teste antes disso."

## <a name="offloading-to-the-cloud"></a>Descarga para a cloud
Antes de passar para a cloud, o Software de CSI tinha criado com êxito a sua própria infraestrutura multi-inquilino num datacenter local em Houston. Como a empresa expandido, ele enfrentam cada vez maiores problemas crescentes de compra, aprovisionamento e manutenção de todo o hardware e software necessários para suportar a seus clientes. IT pessoal para processar operações tornou-se outro afunilamento que levou a uma lentidão no aprovisionamento de novos recursos e a implementar novos serviços aos clientes.

Software de CSI de analisar as opções de nuvem para eliminar essa sobrecarga, para que ele podia se concentrar no seu código, em vez de suas operações. A empresa detetados muitos dos fornecedores de cloud superior apenas oferecem soluções do infraestrutura-como-serviço (IaaS) que ainda necessitem de uma grande equipe de TI para gerir a pilha de IaaS. No final, o Software de CSI determinado que a solução de PaaS do Azure era a melhor opção para suas necessidades. Molina explica, "O Azure obtém o hardware e software de sistema fora do caminho, para que possa dedicar a nossas ofertas de software, reduzindo o nosso custos gerais de TI."

## <a name="making-the-transition-to-azure"></a>Fazendo a transição para o Azure
Depois de selecionar o Azure para a sua solução de PaaS, o Software de CSI começou a migrar a sua infraestrutura de back-end e as bases de dados para a cloud. Antes do Azure, os clientes de SpectrumNG necessários para instalar uma aplicação de cliente que se comunique com um serviço do Windows Communication Foundation (WCF) no back-end. De acordo com Molina, "Embora alguns clientes alojados tudo em seus próprios datacenters, criámos o produto seja multi-inquilino. Hospedávamos tudo num Data Center em Houston, usando o SQL Server como o arquivo de dados.

"O nosso produto, oferta também incluído um membro com acesso à web portal (escrito no ASP.net), que foi criado para ser de etiqueta white de acordo com presença na web do cliente e uma API de SOAP para suportar as páginas online e qualquer integração de terceiros."

A migração para a cloud não demorou muito para a arquitetura. De acordo com Molina, "A maior parte do esforço de lidar com a modificação da forma que Lemos informações do ficheiro de configuração, uma modificação de cadeia de ligação centralizado e automatizar o empacotamento, carregar e implementação dos nossos lançamentos."

Para desenvolver a automação de compilação, os engenheiros de Software de CSI utilizadas do Azure PowerShell e REST APIs para criar pacotes e carregá-los para um ambiente de teste para a versão todas as noites.
A transição geral para uma implementação com base na cloud do Azure passou rapidamente e sem problemas, a equipe de TI de Software de CSI. Molina explica, "em todos, tínhamos um ambiente de beta na cloud em três a quatro semanas a partir de colocar no projeto. Essa foi uma vitória surpreendente para nós."

Depois de configurar e o ambiente de teste, o Software de CSI começou a migrar os clientes. Para os clientes já a utilizar o Software de CSI de alojamento, a transição foi quase totalmente integrada. Para os clientes a migrar de uma implementação no local, a migração para a cloud levou algum tempo adicional, mas foi ainda principalmente complicação para os clientes e o Software de CSI.

Para os da novos clientes, Software de CSI equipe de TI utiliza o seguinte processo para acumulá-los para o Azure:

1. Scripts do PowerShell do Azure são utilizados para criar uma nova base de dados do cliente; todos os clientes começam numa camada de premium para garantir débito suficiente inicial para a transição.
2. Sempre que possível, o Software de CSI utiliza o Assistente de migração do SQL do Azure para mover os dados existentes para uma instância de base de dados do Azure SQL.
3. Por fim, Microsoft SQL Server Integration Services (SSIS) são utilizadas para reconciliar qualquer discrepância nos dados de ou para realizar qualquer limpeza de dados conforme necessário.

Hoje em dia, cerca de 99 por cento dos clientes do Software de CSI estão alojados no Azure, em quatro datacenters regionais (Norte, E.u. a centro Sul, leste e oeste). Ao ter centros de dados na região geográfica de cada cliente, a latência é mantida num mínimo.

## <a name="azure-elastic-pools-free-up-it-resources"></a>Conjuntos elásticos do Azure, liberar recursos de TI
Vários recursos do Azure ajudaram a Software de CSI shift a sua infraestrutura e operações concentra-se para a funcionalidade e o desenvolvimento com foco. Talvez a maior vantagem tem sido a partir de conjuntos elásticos.

Atualmente, o Software de CSI fornece cerca 550 bases de dados para os clientes. Antes de conjuntos elásticos, era difícil de gerenciar que muitos bancos de dados dentro de uma estrutura de camadas. Os gerentes de operações precisavam atribuir os escalões de desempenho com base nas necessidades de rajada de clientes, as quais necessário significativo IT recursos sobrecarga. Com os conjuntos elásticos, gestores podem atribuir os inquilinos um premium ou um conjunto padrão, conforme adequado e, em seguida, mover os clientes com base no tamanho e precisa. Os clientes sentiram os efeitos dos conjuntos elásticos quase imediatamente; antes de conjuntos elásticos, os clientes tinham de tempos limite e outros problemas durante períodos de rajada-utilização, mas com os conjuntos elásticos, os clientes poderão experienciar rajadas de atividade, conforme necessário e podem continuar a utilizar SpectrumNG sem problemas.

## <a name="azure-active-geo-replication-accelerates-reporting"></a>Georreplicação ativa do Azure acelera a geração de relatórios
Vários clientes do Software de CSI também estão a tirar partido da georreplicação ativa do Azure. Com replicação geográfica activa, até quatro legíveis secundárias bases de dados pode ser configurado nas regiões de datacenter idêntica ou diferente. Software de CSI faz uso de georreplicação ativa de duas formas: primeiro, as bases de dados secundárias estão disponíveis no caso de uma falha do datacenter ou a incapacidade de ligar à base de dados primária; e, em segundo lugar, as bases de dados secundárias são legíveis e podem ser utilizados para a descarga de cargas de trabalho de só de leitura como tarefas de relatório. Alguns clientes do Software de CSI utilizarem este benefício para acelerar os fluxos de trabalho de geração de relatórios.

## <a name="csi-software-application-logic-and-architecture"></a>Arquitetura e lógica do aplicativo de Software de CSI
SpectrumNG utiliza funções da web. Uma vez que o aplicativo é multi-inquilino, um serviço WCF é usado para processar o pedido de ligação inicial de clientes. Como Molina afirma, "o pedido identifica cada cliente, que, em seguida, permite-na criar uma cadeia de ligação horizontalmente para seus bancos de dados para fazer qualquer coisa que precisamos fazer."

Para a camada de web do seu serviço, o Software de CSI tira partido do dimensionamento automático do Azure, com base no dia e hora. Recursos disponíveis são automaticamente aumentou para acomodar a maior utilização durante o horário comercial, de acordo com o fuso horário de cada datacenter regional. Recursos também são definidos como reduzir verticalmente a fim de semana, quando as necessidades dos clientes são menores.

![Arquitetura de Daxko/CSI](./media/sql-database-implementation-daxko/figure1.png)

Figura 1. Uma função de trabalho de serviços cloud desenha dados estruturados da base de dados do Azure SQL e dados semi-estruturados do armazenamento de tabelas. Os utilizadores de SpectrumNG interagirem com que dados através de uma nuvem dos serviços de função da web.

## <a name="using-web-apps-and-a-web-plan-tier-for-mobile-apps"></a>Utilizar aplicações web e uma camada de plano web para as aplicações móveis
Utilizar a base de dados do SQL Azure liberados recursos para o Software de CSI para ativar novas iniciativas, incluindo uma plataforma completa de móvel com base numa API personalizada alojada nas aplicações web do Azure. A plataforma permite que os membros de gym e funcionários utilizem os dispositivos móveis para verificar as agendas, classes de livros e receber mensagens.

A plataforma utiliza arquitetura orientada a serviços (SOA) para criar um único componente — como um sistema de ponto de venda (POS) ou um sistema de vendas, mova-o rapidamente para outro plano de web e, em seguida, prepare um serviço para dar suporte a esse componente, mantendo todo o resto no plano web original. Esta capacidade proporciona uma flexibilidade enorme CSI Software e ajuda a manter os custos baixos.

## <a name="azure-lets-csi-software-developers-focus-on-apps-and-services"></a>O Azure permite-Software de CSI desenvolvedores se concentram em aplicações e serviços
Base de dados SQL do Azure não é apenas uma vantagem para os clientes de SpectrumNG, que gostam do serviço rápido e confiável, também é uma grande vitória para Software de CSI equipe de TI e desenvolvedores. Ao descarregar o ops para o Azure na cloud, o Software de CSI reduzida a sobrecarga para recursos e a infraestrutura, acelerou muito respetivo ciclos de desenvolvimento e já não precisa micromanage bases de dados para otimizar o desempenho dos seus inquilinos.

## <a name="more-information"></a>Mais informações
* Para saber mais sobre conjuntos elásticos do Azure, veja [conjuntos elásticos](sql-database-elastic-pool.md).
* Para saber mais sobre as ferramentas de base de dados e dimensionamento elástico, veja [ferramentas de bases de dados elásticas e o dimensionamento elástico](sql-database-elastic-scale-get-started.md).
* Para saber mais sobre como migrar uma base de dados do SQL Server, consulte [migrar uma base de dados do SQL Server para o Azure](sql-database-cloud-migrate.md).
* Para saber mais sobre a georreplicação ativa, veja [georreplicação ativa](sql-database-geo-replication-overview.md).
* Para saber mais sobre o Azure Service Bus, veja [do Azure Service Bus](https://azure.microsoft.com/services/service-bus/).
* Para saber mais sobre o dimensionamento automático, veja [dimensionamento de serviços cloud](../cloud-services/cloud-services-how-to-scale-portal.md).

