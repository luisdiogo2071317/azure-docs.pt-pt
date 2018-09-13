---
title: Estudo de caso do Azure - a Snelstart de base de dados SQL do Azure | Documentos da Microsoft
description: Saiba mais sobre como a SnelStart utiliza a base de dados SQL para expandiu rapidamente os seus serviços empresariais a uma taxa de 1000 novas SQL do Azure bases de dados por mês
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: reference
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: carlrab
ms.openlocfilehash: c919a3cb47017d2f65b141e358ab318f4b764627
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/12/2018
ms.locfileid: "44713719"
---
# <a name="with-azure-snelstart-has-rapidly-expanded-its-business-services-at-a-rate-of-1000-new-azure-sql-databases-per-month"></a>Com o Azure, a SnelStart expandiu rapidamente seus serviços empresariais a uma taxa de 1000 novas SQL do Azure bases de dados por mês
![SnelStartLogo](./media/sql-database-implementation-snelstart/snelstartlogo.png)

A SnelStart faz software popular financeiros e empresas-gerenciamento para empresas de pequeno e médio porte (SMBs) nos países baixos. Seus 55.000 clientes são servidos por uma equipe de 110 funcionários, incluindo uma equipe de TI de 35. Ao mudar de ambiente de trabalho software para uma oferta de software-como-serviço (SaaS) criada no Azure, a SnelStart feitas o máximo de serviços internos, automatizando o gerenciamento usando o ambiente familiar em c# e otimização de desempenho e escalabilidade ao nenhuma over - ou aprovisionamento em que as empresas utilizam conjuntos elásticos. Utilizar o Azure fornece a SnelStart fluidez de clientes móveis entre no local e na cloud.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-Case-Study-SnelStart/player]
> 
> 

## <a name="why-snelstart-extended-services-from-the-desktop-to-the-cloud"></a>Por que motivo a SnelStart estendido serviços da área de trabalho para a cloud
> "Trabalhar com o Azure significa que podemos oferecer software mais rapidamente, rapidamente reagir às necessidades dos clientes e dimensionar soluções quando a procura aumenta."
> 
> — Henry Been, Arquiteto de Software
> 
> 

A SnelStart foi executada uma empresa de software com êxito durante anos, com um modelo de desenvolvimento tradicionais: de código, empacotar, são enviados e repita. Ao longo do tempo, o ritmo das mudanças cresceu mais rapidamente e com mais rapidez. Regulamentações alterados frequentemente e os clientes tinham de formas mais fácil de processar registos financeiros e colaborar com os seus contadores e governamentais para se manter atualizado com essas alterações.

"Envio de software aos clientes era dispendiosa e restritiva,", de acordo com Henry Been, arquiteto de software. "Produção, empacotamento e custos de remessa limitado a frequência com que foi lançarmos software. Tivemos que as atualizações de pacotes para remessas periódicas, mas isso ficou difícil em função das necessidades de nossos clientes. Podemos pode nunca ter a garantia de que os nossos clientes atualizados para a versão mais recente do produto. Por conseguinte, tivemos que suporte a várias versões, fazendo com que a tarefa de suporte muito difícil bem."

Foi adiciona, "nós Queríamos uma forma para programar e lançar atualizações num acelerado ritmo, para que pudéssemos inove mais depressa e criar novos serviços para os nossos clientes. Também gostaríamos de uma forma de automatizar mais processos para simplificar as necessidades de administração de empresas de nossos clientes."

Para a SnelStart, a solução era expandir os seus serviços ao tornar-se de um fornecedor de SaaS baseado na nuvem. A plataforma de base de dados do Azure SQL ajudou a SnelStart consegui-lo sem incorrer a sobrecarga IT principais que exigiria uma solução de infraestrutura-como-serviço (IaaS).

O modelo de nuvem também permite que a SnelStart corrigir bugs e fornecer novas funcionalidades rapidamente, sem que os clientes de terem de atualização de software. Em conformidade com foi, "usando os serviços cloud do Azure permite-nos atuar rapidamente com a alternância de requisitos de terceiros. Em vez de enviar uma nova versão para milhares de clientes, estamos pode adaptar informações enviadas pelo nosso aplicativo de desktop para novos formatos necessários por terceiros."

## <a name="a-modern-company-with-traditional-roots"></a>Uma empresa moderna com raízes tradicionais
A SnelStart é uma empresa moderna, ágil, de alta tecnologia com raízes humilde que existem desde a 1964, quando os fundadores iniciado a empresa como um fabricante de partes de instrumento musical. O coração da empresa de software a SnelStart realmente começou a bater na década de 1980, durante a proliferação de no computador pessoal. A empresa precisava de uma melhor alternativa para o software de gestão de contas disponível no momento, portanto, levou é importante em suas próprias mãos. 1982, a empresa criou a base do que, eventualmente, se tornaria a SnelStart software de contabilidade. Desde o início, o software foi admiramos para sua simplicidade e velocidade — tanto que a empresa, eventualmente, alterar o foco e reinventou numa empresa de software.

Em 1995, a SnelStart lançou sua primeira aplicação de contábeis para Windows. A aplicação, desenvolvida em bases de dados do Microsoft Visual Basic 1.0 e o Microsoft Access, ajudou a aumentar com que o cliente base de mais de 5.000 usuários.

Hoje em dia, a SnelStart é um fornecedor de principais de uma linha de negócio (LOB) e a aplicação de administração de empresas direcionada para SMBs holandês e empresários Self-empregados. Como Carlo Kuip, arquiteto de TI, diz, "o nosso objetivo é fornecer automatização de 100 por cento dos serviços de administração de negócios para os nossos clientes."

## <a name="optimizing-performance-and-cost-with-elastic-pools"></a>Otimizando o desempenho e custo com os conjuntos elásticos
A SnelStart foi uma grande escala das primeiras a adotar conjuntos elásticos. Conjuntos elásticos ajudam a empresa a limitar os custos e gerenciar os requisitos de desempenho com mais eficiência. Em conformidade com foi, "ao utilizar os conjuntos elásticos, que podemos otimizar desempenho com base nas necessidades dos nossos clientes, sem sobreaprovisionar. Se tivéssemos que aprovisionar com base na carga de pico, seria muito dispendioso. Em vez disso, a opção para partilhar recursos entre várias bases de dados de baixa utilização nos permite criar uma solução que tiver um bom desempenho e é rentável. "

## <a name="azure-sql-databases-help-containerize-data-for-isolation-and-security"></a>Bases de dados SQL do Azure ajudam a colocar num contentor de dados de segurança e isolamento
Base de dados SQL do Azure permite a SnelStart facilidade e transparência mover dos clientes no local os dados de administração de empresas para o Azure. A base de dados do SQL do Azure é um contentor conveniente que fornece o isolamento, um limite de autenticação, autorização e capacidades de cópia de segurança e restauro fácil. Bases de dados fornecem um modelo conceitual bem adequado para a administração de negócios. De acordo com Carlo Kuip, arquiteto de TI, "itens dentro do limite este contentor contêm dados confidenciais que são fundamentais para uma empresa, e armazenar esses itens numa base de dados isolada mantém-los bem protegidos. Podemos gerir a autorização ao nível da base de dados e até automatizar a gestão e o Escalamento horizontal destas bases de dados sem a necessidade de administradores de banco de dados (DBAs) da equipe de."

O Azure SQL Data Warehouse também desempenha um papel na história a SnelStart de segurança e gerenciamento, ajudando a empresa recolha dados de telemetria, como deteção de intrusões, o registo de atividade do utilizador e conectividade.

## <a name="azure-removes-overhead-so-that-developers-can-spend-more-time-delivering-value"></a>Azure remove sobrecarga para que os desenvolvedores podem passar mais tempo fornecendo valor
O modelo de plataforma do Azure removido a sobrecarga de infraestrutura e ativada a SnelStart automatizar Implantações usando bibliotecas de gestão do c#. Como Kuip afirma, "conseguimos expandir nossas operações atuais com uma equipe muito pequena aumentando simultaneamente as opções de recuperação após desastre, a velocidade e escalabilidade para nossos clientes. A mudança para o desenvolvimento de serviços liberado recursos se concentrar em novos serviços e funcionalidades, em vez de apenas atualizar código existente para se manter atualizado com novas regulamentações ou códigos de imposto." Ele adiciona, "por automatizando o gerenciamento e com o SaaS oferece, somos capazes de fornecer mais valor para nossos clientes, sem ter de fazer grandes investimentos em equipe operacional." Por exemplo, usando conjuntos elásticos e do Azure, a SnelStart conseguiu adicionar uma variedade de novas funcionalidades, incluindo a integração de dados do cliente mais robusta com bancos, faturação novos serviços, verificações de plano de fundo de pequenas empresas e serviços de e-mail.

> "No apenas os primeiros meses de 2016, iremos expandida nosso implementações de base de dados do Azure SQL a partir de aproximadamente 5,500 para mais de 12 000, e estamos atualmente a adicionar aproximadamente 1.000 bancos de dados por mês."
> 
> — Henry Been, Arquiteto de Software
> 
> 

Gestão de base de dados é ainda mais automatizada ao utilizar a funcionalidade de tarefas elásticas. Como Kuip afirma, "altamente Agradecemos a deteção automática de bases de dados numa instância do SQL DB [server]." Isso permite que a SnelStart executar operações de gestão nos dinamicamente cada vez maior base de clientes sem sobrecarga adicional.

A SnelStart também está a desenvolver uma API que age como um mediador entre os dados dos clientes e os aplicativos criados por parceiros de software de terceiros. Como Kuip Estados, "Esta API irá ativar a outros fornecedores adicionar funcionalidades à nossa software, como a eliminação de entrada de dados para notas fiscais e outros documentos." Os clientes já não terá de digitar manualmente as notas fiscais ao seu software de gestão de contas de pequenas empresas; o software a SnelStart trocarão diretamente as informações necessárias. Isso permite que os clientes associar as suas tarefas de administração de empresas com o ecossistema de informações que está por vir de transformação digital no setor.  

## <a name="how-azure-services-enable-saas-for-snelstart"></a>Como SaaS de ativar a serviços do Azure para a SnelStart
Ao utilizar o Azure, a SnelStart pode atender a seus clientes e os respetivos contadores de forma totalmente integrada na cloud. Por exemplo, os clientes e accountants podem partilhar informações ao aceder diretamente a API de cliente da SnelStart, alojada no Azure. Kuip afirma, "estes serviços reutilizáveis estão disponíveis para as nossas aplicações web do lado do cliente, e eles fornecem a infraestrutura e as funções para permitir o acesso à administração de negócios para clientes e para software de terceiros utilizados pelos clientes comuns. Os exemplos incluem fornecer funcionalidades de configuração do produto, gerenciamento de regras de firewall e gerenciamento de processos de longa execução, como cópias de segurança."

> Nosso objetivo é fornecer a automatização de 100 por cento dos serviços de administração de negócios para os nossos clientes." 
> 
> — Carlo Kuip, Arquiteto de TI
> 
> 

Além disso, a SnelStart serviços da web permitem que os clientes e accountants facilmente aceder aos dados nos conjuntos elásticos da base de dados do Azure SQL. Esse modelo de SaaS, juntamente com a elasticidade da base de dados e o Azure Resource Manager, fornece a SnelStart com recursos de escalabilidade, que complementam as todas as implementações do Azure. A implementação é totalmente automatizada usando bibliotecas de gestão do c#.

![A SnelStart arquitetura](./media/sql-database-implementation-snelstart/figure1.png)

Figura 1. A partir de Junho de 2016, a SnelStart tem mais do que 11,000 bases de dados e mais de 50 conjuntos elásticos

## <a name="simplicity-from-the-cloud"></a>Simplicidade da cloud
Desde que migrou para uma solução com base na cloud do Azure, a SnelStart foi capaz de suportar o crescimento rápido de cliente, oferecendo recursos inovadores e serviços. Em conformidade com foi, "com o Azure, podemos disponibilizar atualizações quase contínua para os nossos clientes, sem a expandir a nossa equipa de operações. E podemos obter todas as outras excelentes funcionalidades do Azure — como a recuperação após desastre e de escalabilidade — agrupado em. "

> "Quando foi realmente sobre em Redmond... Recebi uma chamada de um desenvolvedor de volta na Holanda, realizar-me sobre um problema específico. Foi possível obter o Microsoft para oferecer uma alteração no respetivo ambiente de produção no prazo de 48 horas para resolver o problema."
> 
> — Henry Been, Arquiteto de Software
> 
> 

A SnelStart appreciates também a parceria forte já desenvolveram com a equipa do Microsoft Azure SQL DB. Como Kuip Estados, "temos discussões sobre funcionalidades e como utilizar a tecnologia, que é algo bem-vindos em ambos os lados."
O objetivo de imediato para a SnelStart é continuam a aumentar seus clientes satisfeitos base. Como foi afirma, "Sem as limitações técnicas e recursos que tínhamos como um ISV, não tem qualquer limite até que ponto podemos pode crescer."

## <a name="more-information"></a>Mais informações
* Para saber mais sobre conjuntos elásticos do Azure, veja [conjuntos elásticos](sql-database-elastic-pool.md).
* Para saber mais sobre o Azure SQL Data Warehouse, veja [SQL Data Warehouse](https://azure.microsoft.com/documentation/services/sql-data-warehouse/)
* Para saber mais sobre a SnelStart, veja [a SnelStart](http://www.snelstart.nl).

