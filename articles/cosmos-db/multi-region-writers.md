---
title: Multi-mestre à escala global com o Azure Cosmos DB | Microsoft Docs
description: ''
services: cosmos-db
author: rimman
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: rimman
ms.openlocfilehash: cc66b2f506d81a7ba10b26c3b24287472e890682
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34724912"
---
# <a name="multi-master-at-global-scale-with-azure-cosmos-db"></a>Mestre multi à escala global com o Azure Cosmos DB 
 
Desenvolver globalmente distribuídas aplicações respondem com latências locais enquanto mantém consistentes vistas dos dados em todo o mundo é um problema um desafio. Os clientes utilizar bases de dados globalmente distribuídas, porque necessitam para melhorar a latência de acesso de dados, alcançar dados elevada disponibilidade, certifique-se a recuperação após desastre garantido e para satisfazer os seus requisitos empresariais. Mestre multi do BD Azure Cosmos fornece elevados níveis de disponibilidade (99.999%), latência de dígito milissegundo para escrever dados e a escalabilidade com suporte de resolução de conflito incorporado flexível e abrangente. Estas funcionalidades significativamente simplificam o desenvolvimento de aplicações distribuídas global. Para aplicações distribuídas global, é fundamental multi-mestre suporte. 

![Arquitetura multi-mestre](./media/multi-region-writers/multi-master-architecture.png)

Com suporte multi-mestre de BD do Cosmos do Azure, pode efetuar operações de escrita em contentores de dados (por exemplo, coleções, gráficos, tabelas) distribuídos em qualquer lugar do mundo. Pode atualizar os dados em qualquer região que está associado a sua conta de base de dados. Estas atualizações de dados podem propagar-se no modo assíncrono. Além de fornecer acesso rápido e latência escrita aos seus dados, o mestre multi também fornece uma solução de prática para ativação pós-falha e balanceamento de carga de problemas. Em resumo, com base de dados do Azure Cosmos depara-se a latência de escrita de < 10 ms, o percentil 99th em qualquer local no mundo, 99.999% escrita e leitura disponibilidade em qualquer parte do mundo e a capacidade de dimensionar ambos escrevem e leem débito em qualquer lugar em todo o mundo.   

> [!IMPORTANT]
> Suporte multi-mestre está em pré-visualização privada, para utilizar a versão de pré-visualização, [inscrever](#sign-up-for-multi-master-support) agora.

## <a name="sign-up-for-multi-master-support"></a>Inscrever-se para obter suporte multi-mestre

Se já tiver uma subscrição do Azure, pode inscrever-se para aderir ao programa de pré-visualização multi-mestre no portal do Azure. Se estiver familiarizado com o Azure, inscreva-se um [avaliação gratuita](https://azure.microsoft.com/free) onde pode obter de acesso gratuito a base de dados do Azure Cosmos de 12 meses. Conclua os seguintes passos para pedir acesso para o programa de pré-visualização multi-mestre.

1. No [portal do Azure](https://portal.azure.com), clique em **crie um recurso** > **bases de dados** > **Azure Cosmos DB**.  

2. Na página de nova conta, forneça um nome para a sua conta de base de dados do Azure Cosmos, escolha a API, subscrição, grupo de recursos e localização.  

3. Em seguida selecione **inscrever-se na pré-visualização hoje mesmo** sob o campo de várias Mater pré-visualização.  

   ![Inscreva-se multi-mestre pré-visualização](./media/multi-region-writers/sign-up-for-multi-master-preview.png)

4. No **inscrever-se na pré-visualização hoje mesmo** painel, clique em **OK**. Depois de submeter o pedido, o estado é alterado para **com aprovação pendente** no painel de criação de conta.  

Depois de submeter o pedido, receberá uma notificação por e-mail que o seu pedido tiver sido aprovado. Devido ao elevado volume de pedidos, deverá receber a notificação dentro de uma semana. Não é necessário criar um pedido de suporte para concluir o pedido. Pedidos serão analisados pela ordem em que foram recebidos.

## <a name="a-simple-multi-master-example--content-publishing"></a>Publicação de um exemplo simples multi-mestre – conteúdo  

Vamos ver um cenário do mundo real que descreve como utilizar o suporte multi-mestre com base de dados do Azure Cosmos. Considere uma plataforma de publicação conteúda incorporada numa base de dados do Azure Cosmos. Seguem-se alguns requisitos que esta plataforma tem de cumprir para uma experiência de utilizador excelente para consumidores e Publicadores. 

* Autores e os subscritores são distribuídos todas as através do mundo.  

* Os autores tem de publicar artigos (escrita) para os respetivos região local (mais próximo).  

* Os autores tem leitores/subscritores dos respetivos artigos são distribuídos em todo o mundo.  

* Os subscritores devem receber uma notificação quando novos artigos são publicados.  

* Subscritores tem de conseguir ler artigos da sua região local. Também deve ser capazes de adicionar revisões para estes artigos.  

* Qualquer pessoa, incluindo o autor dos artigos deve ser capaz de ver todas as revisões exposta artigos a partir de uma região local.  

Partindo do princípio de milhões de consumidores e publicadores com billions dos artigos, logo que temos de confront os problemas de dimensionamento, juntamente com guaranteeing localidade de acesso. Um caso de utilização é uma candidata perfeita para mestre multi da BD do Cosmos do Azure. 

## <a name="benefits-of-having-multi-master-support"></a>Vantagens de ter suporte multi-mestre 

Suporte multi-mestre é essencial para aplicações distribuídas global. Mestre multi é composto [várias regiões mestres](distribute-data-globally.md) que igualmente participar num escrita em qualquer lugar modelo (ativo-ativo padrão) e é utilizado para garantir que dados estão disponíveis em qualquer altura em que precisa. As atualizações efetuadas a uma região individuais no modo assíncrono são propagadas para todas as regiões (o que por sua vez, são regiões principais no seu próprio). Regiões do Azure da BD do Cosmos automaticamente a funcionar como mestre regiões numa configuração multi-mestre funcionam para convergir os dados de todas as réplicas e certifique-se [global consistência e integridade dos dados](consistency-levels.md). A imagem seguinte mostra a replicação de leitura/escrita para um único principal e o mestre de mult.

![Mestre de único e multi-mestre](./media/multi-region-writers/single-vs-multi-master.png)

Implementar mestre multi no seu próprio adiciona fardo programadores. Clientes em grande escala que tentam implementar mestre multi por si só podem passar centenas de horas que configurar e testar uma configuração multi-mestre em todo o mundo, e muitas têm um conjunto dedicado de engenheiros cujo trabalho único está a monitorizar e manter o mestre de multi replicação. Criar e gerir a configuração multi-mestre no seu próprio leva tempo, recursos na direção oposta ao innovating na aplicação em si e resulta em custos muito superiores. BD do Cosmos do Azure fornece suporte multi-mestre "out-of-a-box" e remove esta sobrecarga de programadores.  

Em resumo, mestre multi fornece as seguintes vantagens:

* **Melhor de recuperação após desastre, escrever disponibilidade e a ativação pós-falha**-master multi pode ser utilizado para preservar a elevada disponibilidade de uma base de dados fundamentais para um limite superior. Por exemplo, uma base de dados multi-mestre pode replicar dados de uma região para uma região de ativação pós-falha quando a região primária fica indisponível devido a uma falha ou um desastre regional. Uma região de ativação pós-falha irá servir como uma região principal totalmente funcional para suportar a aplicação. Mestre multi fornece maior "survivability" proteção relativamente a perante desastres naturais, falhas de energia, ou sabotagem instalações ou ambos porque restantes regiões podem estar num geograficamente diferentes vários modelos de estrutura mestres com uma disponibilidade garantida escrita > 99.999%. 

* **Melhorado latência de escrita para os utilizadores finais** - quanto mais próximo os dados (que estão a funcionar) são para os utilizadores finais, melhores serão a experiência. Por exemplo, se tiver utilizadores na Europa, mas a base de dados está nos E.U.A. ou da Austrália, a latência adicionada é aproximadamente 140 ms e 300 ms para as respetivas regiões. Atrasos são inaceitável para começar de muitos jogos populares, requisitos de banca ou aplicações interativas (web ou móveis). Latência desempenha uma grande parte na perceção do cliente de uma experiência de alta qualidade e foi foi tentativa a afetar o comportamento dos utilizadores algumas extensão percetível. Como melhora a tecnologia e especialmente com a introdução de AR, VR e MR, exigir experiências immersive e lifelike ainda mais, os programadores agora precisam produzir sistemas de software com requisitos de latência rigorosos. Por conseguinte, as aplicações disponíveis localmente e os dados (conteúdo para as aplicações) é ter mais importante. Com multi mestra na base de dados do Azure Cosmos, o desempenho é como rápidos regular local lê e escreve e avançado globalmente por distribuição de georreplicação.  

* **Escalabilidade de escrita e débito de escrita melhorados** - mestre multi proporcionará a maior débito e maior utilização oferecendo vários modelos de consistência com correcção garante e cópias de segurança de SLA. 

  ![Dimensionamento de escrita de débito com multi mestre](./media/multi-region-writers/scale-write-throughput.png)

* **Melhor suporte para ambientes desligados (por exemplo, dispositivos de limite)** -master multi permite aos utilizadores para replicar todos os ou um subconjunto dos dados de um dispositivo de limite para uma região mais próxima num ambiente desligado. Este cenário é típico de vendas forçar sistemas de automatização, onde o portátil de um indivíduo (um dispositivo desligado) armazena um subconjunto de dados relacionados com o salesperson individuais. Regiões principais na nuvem que estão localizadas em qualquer local no mundo podem funcionar como o destino da cópia dos dispositivos de limite remoto.  

* **Balanceamento de carga** -com multi mestre, a carga entre a aplicação pode ser reequilibrada movendo os utilizadores/cargas de trabalho a partir de uma região descontos elevados carregada para regiões onde carga é distribuída uniformemente pelos. Escreva capacidade pode ser facilmente expandida adicionando uma região novo e, em seguida, mudar algumas operações de escrita para a região de novo. 

* **Melhor utilização de capacidade aprovisionada** – com multi mestre, para cargas de trabalho pesado de escrita e mistas, pode sob saturar a capacidade aprovisionada em várias regiões...  Em alguns casos, que pode redistribuir leituras e escritas mais igualmente, pelo que necessita de menos débito a ser aprovisionado e leva a reduções de custos mais clientes.  

* **Arquiteturas de aplicações mais simples e mais resiliente** -aplicações mover a configuração multi-mestre obterem garantidas resiliência de dados.  Com a BD do Cosmos Azure ocultar todos os a complexidade, pode simplificar substancialmente a arquitetura e estrutura de aplicação. 

* **Liberte risco de ativação pós-falha de teste** -testar a ativação pós-falha não terá qualquer degradação de débito de escrita no. Com a multi mestre, todas as outras regiões são completo-modelos de estrutura mestres, para que ativação pós-falha não terá muito impacto no débito de escrita.  

* **Reduzir o Custo Total de Ownership(TCO) e DevOps** -escalabilidade, desempenho, distribuição global, a cumprir os objetivos de tempo de recuperação, muitas vezes, são dispendiosos devido a suplementos dispendiosas ou manter uma infraestrutura de cópia de segurança que está Inativos até após desastre Strikes. Com base de dados do Azure Cosmos mestre multi cópia de segurança de SLAs líder da indústria, os programadores já não necessitam de criação e manutenção "back-end" glue "lógica da" próprios e obter uma tranquilidade executar as respetivas cargas de trabalho fundamentais. 

## <a name="use-cases-where-multi-master-support-is-needed"></a>Onde é necessário suporte multi-mestre de casos de utilização

Existem vários casos de utilização para mestre multi do BD Azure Cosmos: 

* **IoT** -master multi da BD do Cosmos Azure permite a implementação distribuída simplificada de processamento de dados do IoT. Implementações de limite geo-distribuição que utilizem CRDT conflito livre replicados dados tipos muitas vezes necessitam para controlar os dados de séries de tempo de várias localizações. Cada dispositivo pode ser homed para uma das regiões mais próximos, e um dispositivo pode viajam (por exemplo, um carro) e pode ser dinamicamente rehomed escrever noutra região.  

* **Comércio** -Assuring experiência de utilizador excelente em cenários de ecommerce tem disponibilidade elevada e resiliência para cenários de falha. No caso de falha de uma região, as sessões de utilizador, compras carts, Active Directory pretendem listas tem de ser totalmente integrada captado por outra região sem perda de estado. Em provisório, as atualizações efetuadas pelo utilizador tem de ser processadas adequadamente (exemplo, adiciona e remove da carrinho de compras tem de transferir ao longo do). Com várias mestre, base de dados do Azure Cosmos pode processar tais cenários corretamente, com uma transição tranquila entre regiões ativas, mantendo em simultâneo uma vista consistente de ponto de vista do utilizador. 

* **Deteção de fraudes/anomalias** -muitas vezes, as aplicações que monitorizam a atividade do utilizador ou a atividade de conta são distribuídas global e tem manter controlar dos vários eventos em simultâneo. Ao criar e manter as pontuações das classificações para um utilizador, as ações de diferentes regiões geográficas tem de atualizar simultaneamente pontuações para manter o inline de métricas de risco. BD do Azure do Cosmos pode garantir que os programadores não tem de processar cenários de conflito ao nível da aplicação. 

* **Colaboração** – para aplicações que classificação com base no popularidade dos artigos, tais como bens venda ou suporte de dados a ser consumidos etc. Controlo popularidade em regiões geográficas pode obter complicada, especialmente quando royalties têm de ser decisões de publicidade de tempo real ou paga a ser efetuada. Classificação, ordenação e relatórios em várias regiões em todo o mundo, em tempo real com base de dados do Azure Cosmos permitem aos programadores fornecer funcionalidades com pouca esforço e sem comprometer as latências. 

* **Medição** - contando e regular a utilização (tais como chamadas de API, transações por segundo, minutos utilizado) podem ser implementadas globalmente com simplicidade utilizando o mestre de base de dados do Azure Cosmos multi. Resolução de conflito incorporado garante ambas a precisão das contagens e regulamento em tempo real. 

* **Personalização** - se estiver a manutenção dos contadores distribuídos geograficamente que acionam ações como loyalty pontos awards ou implementar a sessão de utilizador personalizada vistas, a elevada disponibilidade e simplificada geo-distribuição Contagem fornecida pelo Azure Cosmos DB, permite que aplicações entregar elevado desempenho com simplicidade. 

## <a name="conflict-resolution-with-multi-master"></a>Resolução de conflitos com multi mestre 

Com várias mestre, o desafio é frequentemente que dois (ou mais) réplicas o registo do mesmo podem ser atualizadas em simultâneo por escritores diferentes em dois ou mais regiões diferentes. Escritas em simultâneo poderão provocar duas versões diferentes do mesmo registo e sem resolução de conflito incorporado e a própria aplicação tem de efetuar a resolução de conflitos para resolver este inconsistência.  

**Exemplo** -vamos partem do princípio de que está a utilizar base de dados do Azure Cosmos como o arquivo de persistência para uma aplicação de carrinho de compras e esta aplicação é implementado em duas regiões: EUA leste e EUA oeste.  Se, aproximadamente, ao mesmo tempo, um utilizador em são Francisco adiciona um item o carrinho de compras (por exemplo, um livro) durante um processo de gestão de inventário nos E.U.A. Leste invalida um item de carrinho compras diferente (por exemplo, um novo telefone) para esse mesmo utilizador em resposta a um s notificação de upplier que tinha foi atrasada à data de lançamento. Na hora T1, os registos de carrinho de compras em duas regiões são diferentes. A base de dados utilizará a replicação e o mecanismo de resolução de conflitos para resolver este inconsistência e, eventualmente, irá ser selecionada uma das duas versões do carrinho de compras. Utilizar a heurística de resolução de conflito aplicadas com mais frequência por multi-mestre bases de dados (por exemplo, última escrita wins), é impossível para o utilizador ou aplicação para prever que versão será selecionada. Em ambos os casos, os dados são perdidos ou pode ocorrer um comportamento inesperado. Se a versão de região Leste for selecionada, em seguida, a seleção do utilizador de um novo item de compra (ou seja, um livro) perde-se e se a região Oeste for selecionada, em seguida, o item anteriormente escolhido (ou seja, o telefone) ainda está em ao carrinho. Qualquer forma, as informações são perdidas. Por fim, qualquer outro processo inspecionar de compras carrinho entre as horas T1 e T2 vai Consulte também o comportamento não determinística. Por exemplo, um processo em segundo plano que seleciona o armazém de cumprimento e atualiza o carrinho os custos de envio produziria resultados que estão em conflito com o conteúdo eventual ao carrinho. Se o processo está em execução na região oeste e 1 alternativo fica realidade, seria computação os custos de envio para dois itens,, apesar do carrinho em breve poderá ter apenas um item, o livro. 

BD do Azure do Cosmos implementa lógica para processar escritas em conflito dentro do motor de base de dados própria. BD do Azure do Cosmos oferece **suporte de resolução de conflito abrangente e flexível** , oferecendo conflito vários modelos de resolução, incluindo automático (livre de conflito de CRDT replicado tipos de dados), última escrita Wins (LWW) e (personalizada Procedimento armazenado) para a resolução de conflito automática. Os modelos de resolução de conflito fornecem garantias de que está correto e de consistência e remova a carga de que os programadores têm de refletir sobre a consistência, disponibilidade, desempenho, latência de replicação e complexas combinações de eventos em ativações pós-falha de georreplicação e conflitos de escrita por várias regiões.  

  ![Resolução de conflitos de MULT-master](./media/multi-region-writers/multi-master-conflict-resolution-blade.png)

Terá de 3 tipos de modelos de resolução de conflito oferecidos pelo Azure Cosmos DB. A semântica de modelos de resolução de conflito é as seguintes: 

**Automática** -esta é a política de resolução de conflito de predefinição. Selecionar esta política faz com que a base de dados do Azure Cosmos resolver automaticamente as atualizações em conflito no lado do servidor e forneça-eventual-consistência forte garante. Internamente, a base de dados do Azure Cosmos implementa resolução de conflitos automática pelo aproveitamento conflito-Free-replicados-tipos de dados (CRDTs) dentro do motor de base de dados.  

**Último-escrita-Wins (LWW)** - escolher esta política permite-lhe resolver conflitos com base no sistema definido sincronizado a propriedade timestamp ou uma propriedade personalizada definida na versão dos registos em conflito. A resolução de conflito ocorre no lado do servidor e a versão com o timestamp mais recente estiver selecionada como o winner.  

**Personalizada** -pode registar uma lógica de resolução de conflito de uma aplicação definida ao registar um procedimento armazenado. O procedimento armazenado irá obter invocado mediante a deteção de conflitos de atualização em auspices de uma transação de base de dados, no lado do servidor. Se selecionar a opção, mas não conseguir registar um procedimento armazenado (ou se o procedimento armazenado emite uma exceção durante a execução), pode aceder a todas as versões em conflito através do Feed de conflitos e resolvê-los individualmente.  

## <a name="next-steps"></a>Passos Seguintes  

Neste artigo learnt como utilizar mestre multi globalmente distribuída com base de dados do Azure Cosmos. Em seguida, observe os seguintes recursos: 

* [Saiba mais sobre como a base de dados do Azure Cosmos suporta distribuição global](distribute-data-globally.md)  

* [Saiba mais sobre as ativações pós-falha automáticas do BD Azure Cosmos](regional-failover.md)  

* [Saiba mais sobre o global consistência com a base de dados do Azure Cosmos](consistency-levels.md)  

* Desenvolver com várias regiões utilizando a BD do Cosmos Azure - [API do SQL Server](tutorial-global-distribution-sql-api.md), [MongoDB API](tutorial-global-distribution-mongodb.md), ou [API de tabela](tutorial-global-distribution-table.md)  
