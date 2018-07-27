---
title: Vários mestres à escala global com o Azure Cosmos DB | Documentos da Microsoft
description: ''
services: cosmos-db
author: rimman
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: rimman
ms.openlocfilehash: 4911a302bf9055948827a72f2e631663b8be741e
ms.sourcegitcommit: a5eb246d79a462519775a9705ebf562f0444e4ec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/26/2018
ms.locfileid: "39265795"
---
# <a name="multi-master-at-global-scale-with-azure-cosmos-db"></a>Com vários mestres à escala global com o Azure Cosmos DB 
 
Desenvolver globalmente distribuído aplicativos que respondem com latências locais enquanto mantém os modos de exibição consistentes dos dados em todo o mundo é um problema desafiador. Os clientes utilizam bases de dados globalmente distribuídos, uma vez que eles precisam para melhorar a latência de acesso de dados, obter dados de elevada disponibilidade, certifique-se a recuperação após desastre garantido e para atender às suas necessidades de negócio. Múltiplos principais no Azure Cosmos DB fornece altos níveis de disponibilidade (99,999%), a latência de milissegundo de dígito para escrever dados e a escalabilidade com suporte de resolução de conflito incorporado, abrangente e flexível. Esses recursos simplificam significativamente o desenvolvimento de aplicações distribuídas globalmente. Para aplicações distribuídas globalmente, o suporte de vários mestre é crucial. 

![Arquitetura de vários mestre](./media/multi-region-writers/multi-master-architecture.png)

Com o suporte de vários mestre do Azure Cosmos DB, pode efetuar escritas em contentores de dados (por exemplo, coleções, gráficos, tabelas) distribuídos em qualquer lugar do mundo. Pode atualizar os dados em qualquer região que está associado a sua conta de base de dados. Estas atualizações de dados podem propagar de forma assíncrona. Além de fornecer um acesso rápido e latência de escrita aos seus dados, com vários mestres também fornece uma solução prática para ativação pós-falha e balanceamento de carga problemas. Em resumo, com o Azure Cosmos DB tem latência de escrita de < 10 ms no percentil de 99 em qualquer parte do mundo, a escrita de 99,999% e a disponibilidade de leitura em qualquer parte do mundo e a capacidade de dimensionar os dois escrever e ler o débito em qualquer lugar em todo o mundo.   

> [!IMPORTANT]
> O suporte de vários mestre está em pré-visualização privada, para utilizar a versão de pré-visualização [Inscreva-se](#sign-up-for-multi-master-support) agora.

## <a name="sign-up-for-multi-master-support"></a>Inscrever-se no suporte de vários mestre

Se já tiver uma subscrição do Azure, pode inscrever-se para participar no programa de pré-visualização de vários mestres no portal do Azure. Se está a começar a utilizar o Azure, inscreva-se numa [avaliação gratuita](https://azure.microsoft.com/free), em que pode obter acesso gratuito ao Azure Cosmos DB durante 12 meses. Conclua os seguintes passos para pedir acesso para o programa de pré-visualização de vários mestres.

1. No [portal do Azure](https://portal.azure.com), clique em **Criar um recurso** > **Bases de dados** > **Azure Cosmos DB**.  

2. Na página da nova conta, dê um nome para a sua conta do Azure Cosmos DB, escolha a API, subscrição, grupo de recursos e localização.  

3. Em seguida selecione **Inscreva-se na pré-visualização hoje mesmo** sob o campo de pré-visualização principal múltipla.  

   ![Inscreva-se para a pré-visualização de vários mestre](./media/multi-region-writers/sign-up-for-multi-master-preview.png)

4. Na **Inscreva-se na pré-visualização hoje mesmo** painel, clique em **OK**. Depois de submeter o pedido, o estado muda para **com aprovação pendente** no painel de criação da conta.  

Depois de submeter o pedido, receberá uma notificação por e-mail que o seu pedido foi aprovado. Devido ao elevado volume de pedidos, deverá receber a notificação dentro de uma semana. Não tem de criar um pedido de suporte para concluir o pedido. Os pedidos serão analisados pela ordem em que foram recebidos.

## <a name="a-simple-multi-master-example--content-publishing"></a>Um exemplo de vários mestre simples – conteúdo de publicação  

Vamos examinar um cenário do mundo real que descreve como utilizar o suporte de vários mestre com o Azure Cosmos DB. Considere uma plataforma de publicação de conteúdo criada no Azure Cosmos DB. Seguem-se alguns requisitos que esta plataforma tem de cumprir para obter uma experiência de utilizador fantástica para os editores e consumidores. 

* Autores e assinantes são espalhar por todos o mundo.  

* Os autores tem de publicar artigos (escrita) para a respetiva região (mais próxima dos) local.  

* Os autores de tem os leitores/subscritores dos respetivos artigos que são distribuídos em todo o mundo.  

* Os assinantes devem receber uma notificação quando novas publicações são publicadas.  

* Os assinantes tem de ser capazes de ler os artigos da respetiva região local. Também deve ser capazes de adicionar revisões os artigos seguintes.  

* Qualquer pessoa, incluindo o autor dos artigos deve ser capaz de ver todas as revisões anexados a artigos de uma região local.  

Partindo do princípio de milhões de consumidores e publicadores com milhares de milhões de artigos, em breve temos que confrontar problemas de dimensionamento, juntamente com assegurando uma localidade de acesso que são. Um caso de utilização é um candidato perfeito para o Azure Cosmos DB com vários mestres. 

## <a name="benefits-of-having-multi-master-support"></a>Benefícios de ter suporte de vários mestre 

Suporte de vários mestre é essencial para aplicações distribuídas globalmente. Com vários mestres é composto [várias regiões mestres](distribute-data-globally.md) que igualmente participar num escrita em qualquer lugar modelo (padrão de ativo-ativo) e é utilizado para garantir que dados estão disponíveis em qualquer altura em que precisa. Atualizações feitas a uma região individual assincronamente sejam propagadas para todas as outras regiões (que por sua vez, são regiões mestres em seus próprios). Regiões do Azure do Cosmos DB operacional regiões como principais numa configuração de vários mestre automaticamente funcionam para convergir os dados de todas as réplicas e certifique-se [global consistência e integridade dos dados](consistency-levels.md). A imagem seguinte mostra a replicação de leitura/escrita para um único controlador e com vários mestres.

![Single-mestre e de vários mestres](./media/multi-region-writers/single-vs-multi-master.png)

Implementação de múltiplos principais por conta própria adiciona fardo em que os desenvolvedores. Os clientes em grande escala que tentam implementar múltiplos principais por conta própria podem passar a centenas de horas configurando e testando uma configuração de vários mestre em todo o mundo, e muitos têm um conjunto dedicado de engenheiros cuja única função é monitorar e manter a múltiplos principais replicação. Criar e gerir a configuração com múltiplos principais por conta própria demora algum tempo, recursos para fora do próprio aplicativo a inovar e resulta em custos muito superiores. O Azure Cosmos DB fornece suporte de vários mestre "out-of-the-box" e remove essa sobrecarga dos desenvolvedores.  

Em resumo, com vários mestres fornece as seguintes vantagens:

* **Melhor recuperação após desastre, escrever a disponibilidade e ativação pós-falha**-múltiplos principais podem ser utilizados para preservar a elevada disponibilidade de uma base de dados de missão crítica até um limite superior. Por exemplo, uma base de dados de vários mestre pode replicar dados de uma região para uma região de ativação pós-falha quando a região primária fica indisponível devido a uma falha ou um desastre regional. Uma região de ativação pós-falha irá servir como uma região principal totalmente funcional para suportar a aplicação. Com vários mestres fornece mais proteção de "persistência" com respeito a desastres naturais, falhas de energia ou sabotage, porque restantes regiões podem utilizar geograficamente diferentes vários modelos de estrutura mestres com uma disponibilidade de escrita garantida > 99,999%. 

* **Melhorada a latência de escrita para os utilizadores finais** – quanto mais próximo seus dados (que estão a funcionar) são para os usuários finais, melhor será a experiência. Por exemplo, se tiver utilizadores na Europa, mas a base de dados está nos EUA ou da Austrália, a latência adicionada é de aproximadamente 140 ms e 300 ms para as respetivas regiões. Atrasos são inaceitáveis para começar para muitos jogos populares, requisitos de banca ou aplicativos interativos (web ou móveis). Latência desempenha uma grande parte na percepção do cliente de uma experiência de alta qualidade e foi provou para afetar o comportamento dos utilizadores até certo ponto notável. Como melhora a tecnologia e, especialmente com o advento do AR, VR e MR, exigindo experiências envolventes e naturais ainda mais, agora, eles precisam produzir os sistemas de software com requisitos de latência rigorosas. Portanto, ter disponíveis localmente aplicações e dados (conteúdo para as aplicações) é mais importante. Com múltiplos principais no Azure Cosmos DB, o desempenho é tão rápido leituras e escritas de regular local e aprimorados globalmente por distribuição geográfica.  

* **Melhorias na escalabilidade de escrita e débito de escrita** - os com vários mestres dará um débito mais elevado e maior utilização oferecendo vários modelos de consistência com a correção garante e cópias de segurança de SLAs. 

  ![Dimensionar o débito de escrita com múltiplos principais](./media/multi-region-writers/scale-write-throughput.png)

* **Melhor suporte para ambientes desconectados (por exemplo, dispositivos de ponta)** -multimestre permite aos utilizadores para replicar todas ou um subconjunto de dados de um dispositivo do edge para uma região mais próxima num ambiente desligado. Este cenário é típico de sistemas de automatização, onde o laptop de um indivíduo (um dispositivo desconectado) armazena um subconjunto de dados relacionados com os vendedores individuais de força de vendas. Mestres regiões na cloud que estão localizados em qualquer lugar no mundo podem funcionar como o destino da cópia dos dispositivos de ponta remotos.  

* **O balanceamento de carga** -com múltiplos principais, a carga entre o aplicativo pode ser reequilibrada ao mover os utilizadores/cargas de trabalho de uma região muito carregada para regiões em que a carga é distribuída uniformemente. Capacidade pode ser facilmente estendida adicionando uma nova região e, em seguida, mudar algumas escritas para a nova região de escrita. 

* **Melhor utilização de capacidade aprovisionada** – com múltiplos principais, para cargas de trabalho de escrita intensiva e mistas, podem saturar a capacidade aprovisionada em várias regiões....  Em alguns casos, que pode redistribuir leituras e gravações mais igualmente, portanto, ele requer menos débito para ser aprovisionado e leva a mais economia de custos para os clientes.  

* **Arquiteturas de aplicações mais simples e mais resiliente** -aplicativos mover para a configuração de vários mestre obterem a garantia de resiliência de dados.  Com o Azure Cosmos DB ocultar toda aquela complexidade, pode simplificar consideravelmente a arquitetura e design do aplicativo. 

* **Sem riscos e teste de ativação pós-falha** -teste de ativação pós-falha não terão qualquer degradação no débito de escrita. Com vários mestres, todas as outras regiões são full-principais, para que a ativação pós-falha não terá muito impacto sobre o débito de escrita.  

* **Reduzir o Custo Total de Ownership(TCO) e DevOps** -, distribuição global, de escalabilidade e desempenho a cumprir os objetivos de tempo de recuperação, muitas vezes, são caros devido a suplementos caros ou manter uma infraestrutura de cópia de segurança que está Inativos até que desastre Strikes. Com o Azure Cosmos DB com vários mestres suportado por SLAs líderes da indústria, os desenvolvedores já não necessitam de criação e manutenção de lógica"cola de back-end" propriamente ditas e obtém um paz de espírito executar as cargas de trabalho de missão crítica. 

## <a name="use-cases-where-multi-master-support-is-needed"></a>Onde é necessário o suporte de vários mestre de casos de utilização

Existem vários casos de utilização de múltiplos principais no Azure Cosmos DB: 

* **IoT** -Azure Cosmos DB com vários mestres permite a implementação distribuída simplificada do processamento de dados de IoT. Implementações de distribuída geograficamente edge que utilizem livres de conflitos de CRDT replicados dados de tipos, muitas vezes, precisam para controlar os dados de séries de tempo a partir de várias localizações. Cada dispositivo pode ser que se encontrem para uma das regiões mais próximos e um dispositivo pode ser transportado (por exemplo, um carro) e dinamicamente pode ser movidas de forma a escrever para outra região.  

* **E-Commerce** -garantir o excelente experiência de usuário em cenários de comércio eletrónico tem de alta disponibilidade e resiliência para cenários de falha. No caso de falha de uma região, sessões de utilizador, Active Directory, carrinhos de compras gostaria de listas tem de forma totalmente integrada captado por outra região sem perda de estado. Até lá, as atualizações feitas pelo usuário devem ser tratadas adequadamente (exemplo, adiciona e remove do carrinho de compras tem de transferir sobre). Com vários mestres, Azure Cosmos DB pode lidar com tais cenários, com uma transição tranqüila entre regiões ativas, mantendo uma exibição consistente do ponto de vista do usuário. 

* **Deteção de fraudes/anomalias** -, muitas vezes, os aplicativos que monitorizam a atividade do utilizador ou a atividade de conta são distribuídos globalmente e deve controlar vários eventos em simultâneo. Ao criar e manter as pontuações para um utilizador, ações de regiões geográficas diferentes em simultâneo tem de atualizar as pontuações para manter o inline de métricas de risco. O Azure Cosmos DB pode garantir que os desenvolvedores não é preciso manipular os cenários de conflito ao nível da aplicação. 

* **Colaboração** - para aplicações com base no popularidade dos artigos como bens em liquidação ou suporte de dados para serem consumidos etc. Controlo popularidade em regiões geográficas pode são complicada, especialmente quando royalty preciso tomar decisões de publicidade de tempo real ou paga devem se transformar em. Classificação, classificação e relatórios de entre várias regiões em todo o mundo, em tempo real com o Azure Cosmos DB permitem aos programadores disponibilizarem recursos com pouco esforço e sem comprometer a latências. 

* **Medição** - contagem e regular a utilização (tais como chamadas de API, utilizado de minutos de transações/segundo,) pode ser implementado globalmente com facilidade usando o Azure Cosmos DB com vários mestres. Resolução de conflito incorporado assegura a ambos os precisão das contagens e regulamento em tempo real. 

* **Personalização** – se quiser manter distribuídos geograficamente contadores que acionam ações como fidelidade aponta prémios ou implementação de sessão de utilizador personalizadas visualiza, a elevada disponibilidade e simplificado distribuída geograficamente Contagem fornecida pelo Azure Cosmos DB, permite que aplicativos entrega de elevado desempenho, com simplicidade. 

## <a name="conflict-resolution-with-multi-master"></a>Resolução de conflitos com múltiplos principais 

Com múltiplos principais, o desafio é muitas vezes que as réplicas de dois (ou mais), do mesmo registo podem ser atualizadas simultaneamente por escritores diferentes em dois ou mais regiões diferentes. Escritas simultâneas podem levar a duas versões diferentes do mesmo registo e sem resolução de conflito incorporado e o aplicativo em si tem de efetuar a resolução de conflito para resolver essa inconsistência.  

**Exemplo** -vamos supor que está a utilizar do Azure Cosmos DB como armazenamento de persistência para um aplicativo de carrinho de compras e esta aplicação é implementado em duas regiões: E.U.A. leste e E.U.A. oeste.  Se aproximadamente ao mesmo tempo, um utilizador em são Francisco adiciona um item ao seu carrinho de compras (por exemplo, um livro) durante um processo de gestão de inventário nos EUA Leste invalida um item de carrinho compras diferentes (por exemplo, um novo telefone) para esse mesmo usuário em resposta a um s notificação de upplier que tinha foi adiada da data de lançamento. Na hora T1, os registos de carrinho de compras em duas regiões são diferentes. A base de dados irá utilizar a replicação e o mecanismo de resolução de conflito para resolver essa inconsistência e, eventualmente, uma das duas versões do carrinho de compras será selecionada. Usando a heurística de resolução de conflito aplicada com mais frequência por vários mestres bases de dados (por exemplo, última escrita wins), é impossível que o utilizador ou aplicação para prever a qual versão será selecionada. Em ambos os casos, os dados são perdidos ou comportamentos inesperados podem ocorrer. Se a versão de região Leste é selecionada, é perdida, em seguida, a seleção do usuário de um novo item de compra (ou seja, um livro) e se a região Oeste for selecionada, em seguida, o item escolhido anteriormente (ou seja, phone) ainda está em ao carrinho. De qualquer forma, as informações são perdidas. Por fim, o processo de inspecionar a compras carrinho entre horas T1 e T2 vai ver também o comportamento não determinística. Por exemplo, um processo em segundo plano que seleciona o armazém de preenchimento e atualiza o carrinho de custos de envio produziria os resultados que entram em conflito com o conteúdo eventual de ao carrinho. Se o processo está em execução na região oeste e 1 alternativo torna-se realidade, ele seria o envio de custos de computação dois itens, apesar do carrinho em breve poderá ter apenas um item, o livro. 

O Azure Cosmos DB implementa a lógica para lidar com gravações em conflito dentro do motor de base de dados em si. Azure Cosmos DB oferece **suporte de resolução de conflito abrangente e flexível** oferecendo o conflito de vários modelos de resolução, incluindo automático (CRDT conflito livre replicados tipos de dados), última escrever Wins (LWW) e Custom ( Procedimento armazenado) para a resolução automática de conflitos. Os modelos de resolução de conflitos para providenciar garantias de correção e a consistência e remover a carga de que os desenvolvedores precisa se preocupar sobre a consistência, disponibilidade, desempenho, latência de replicação e complexas combinações de eventos em ativações pós-falha de georreplicação e conflitos de escrita em várias regiões.  

  ![Resolução de conflitos de MULT-mestre](./media/multi-region-writers/multi-master-conflict-resolution-blade.png)

Terá de 3 tipos de modelos de resolução de conflito oferecidos pelo Azure Cosmos DB. A semântica dos modelos de resolução de conflito é as seguintes: 

**Automática** -esta é a política de resolução de conflito de predefinição. Selecionar esta política faz com que o Azure Cosmos DB resolver automaticamente as atualizações em conflito no lado do servidor e fornecer fortes-eventual-garante a consistência. Internamente, o Azure Cosmos DB implementa resolução automática de conflitos por tirar partido conflito-gratuito-replicados-tipos de dados (CRDTs) dentro do motor de base de dados.  

**Última-escrita-Wins (LWW)** - escolher esta política permite-lhe resolver conflitos com base no sistema definido sincronizados a propriedade timestamp ou uma propriedade personalizada definida sobre a versão em conflito dos registos. A resolução de conflito ocorre no lado do servidor e a versão com o carimbo de hora mais recente é selecionada como vencedor.  

**Custom** -pode registrar uma lógica de resolução de conflito de aplicação definida registrando-se um procedimento armazenado. O procedimento armazenado será invocado após a deteção de conflitos de atualização dos auspícios de uma transação de base de dados, no lado do servidor. Se selecionar a opção, mas conseguir registar um procedimento armazenado (ou se o procedimento armazenado lança uma exceção em tempo de execução), pode aceder a todas as versões em conflito através do Feed de conflitos e resolvê-los individualmente.  

## <a name="next-steps"></a>Passos Seguintes  

Neste artigo com aprendizagem como utilizar e distribuída globalmente com vários mestres com o Azure Cosmos DB. Em seguida, veja os seguintes recursos: 

* [Saiba mais sobre como o Azure Cosmos DB suporta a distribuição global](distribute-data-globally.md)  

* [Saiba mais sobre as ativações pós-falha automática no Azure Cosmos DB](regional-failover.md)  

* [Saiba mais sobre a consistência global com o Azure Cosmos DB](consistency-levels.md)  

* Desenvolver com várias regiões com o Azure Cosmos DB - [API do SQL](tutorial-global-distribution-sql-api.md), [API do MongoDB](tutorial-global-distribution-mongodb.md), ou [API de tabela](tutorial-global-distribution-table.md)  
