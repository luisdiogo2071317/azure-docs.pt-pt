---
title: Guia de AI do Azure para soluções de manutenção preditiva | Microsoft Docs
description: Uma descrição completa de ciência de dados que está na base de soluções de manutenção preditiva em vários indústrias verticais.
services: machine-learning
documentationcenter: ''
author: fboylu
manager: jhubbard
editor: cgronlun
ms.assetid: 2e8b66db-91eb-432b-b305-6abccca25620
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/11/2018
ms.author: fboylu
ms.openlocfilehash: 581e9d586f007fe0cd28d60b5558594777656ca8
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/14/2018
---
# <a name="azure-ai-guide-for-predictive-maintenance-solutions"></a>Guia de AI do Azure para soluções de manutenção preditiva

## <a name="summary"></a>Resumo

Manutenção preditiva (**PdM**) é uma aplicação popular de Análise Preditiva, que pode ajudar a empresas em indústrias várias atingir uma utilização elevada do recurso e reduções de custos operacionais. Este guia reúne empresariais e analíticas diretrizes e melhores práticas para desenvolver e implementar soluções de PdM utilizando com êxito o [plataforma Microsoft Azure AI](https://azure.microsoft.com/overview/ai-platform) tecnologia.

Por starters, este Guia apresenta cenários empresariais específicos da indústria e o processo de elegíveis estes cenários para PdM. Os requisitos de dados e técnicas de modelação para criar soluções de PdM também são fornecidas. O conteúdo do guia principal é do processo de ciência de dados - incluindo os passos de preparação de dados, engenharia da funcionalidade, a criação de modelo e operationalization de modelo. Para complementar estes conceitos chaves, este Guia apresenta uma lista de um conjunto de modelos de solução para o ajudar a acelerar PdM desenvolvimento de aplicações. O guia também aponta para recursos de formação útil para o profissional saber mais sobre o AI atrás de ciência de dados. 

### <a name="data-science-guide-overview-and-target-audience"></a>Dados ciência guia descrição geral e o destino público-alvo
O meio primeiro este guia descreve problemas típicos de empresas, as vantagens de implementar PdM para resolver estes problemas e listas de alguns casos de utilização comum. Decisores de negócios (BDMs) serão vantajoso contar com este conteúdo. A segunda metade explica a ciência de dados por trás PdM e fornece uma lista de soluções de PdM criada com os princípios descritos neste guia. Também fornece percursos de aprendizagem e ponteiros para os materiais de formação. Os decisores técnica (TDMs) úteis este conteúdo.

| Começa a utilizar... | Se não estiver... |
|:---------------|:---------------|
| [Cenário de negócio para manutenção preventiva](#Business-case-for-predictive-maintenance) |um negócio decisor (BDM) procura para reduzir o período de inatividade e os custos operacionais e melhorar a utilização do equipamento |
| [Ciência de dados para manutenção preventiva](#Data-Science-for-predictive-maintenance) |um técnico decisor (TDM) avaliar PdM tecnologias para compreender os requisitos de AI de manutenção preditiva e processamento de dados exclusivo |
| [Modelos de solução de manutenção preditiva](#Solution-templates-for-predictive-maintenance)|um arquiteto de software ou que querem rapidamente estar posicionados uma demonstração ou uma prova do conceito de Programador de AI |
| [Recursos de formação para manutenção preventiva](#Training-resources-for-predictive-maintenance) | alguns ou todos os aspetos anteriores e querem saber os conceitos fundamentais atrás técnicas, ferramentas e ciência de dados.

### <a name="prerequisite-knowledge"></a>Dados de conhecimento dos pré-requisitos
O conteúdo BDM não espera que o leitor tem qualquer conhecimento de ciência de dados anterior. Para o conteúdo TDM, é útil conhecimento básico de ciência de dados e estatísticas. Recomenda-se dados de conhecimento dos dados do Azure e AI serviços, Python, R, XML e JSON. São implementadas técnicas de AI nos pacotes de Python e R. Modelos de solução são implementados utilizando os serviços do Azure, SDKs e ferramentas de desenvolvimento.

## <a name="business-case-for-predictive-maintenance"></a>Cenário de negócio para manutenção preventiva

As empresas requerem equipamento crítico para estar em execução em eficiência das horas de ponta e utilização para os respetivos retorno em investimentos de capital. Estes elementos podem ir desde motores das aeronaves, turbines, elevators ou chillers industriais - que custo milhões - a para baixo para aparelhos de uso corrente como fotocopiadoras, café ou a coolers máximo.
- Por predefinição, a maioria das empresas dependem _manutenção corretiva_, onde as partes são substituídas como e quando estes falharem. As medidas corretivas manutenção garante partes são utilizadas totalmente (, por conseguinte, não perder componente vida), mas custos da empresa no período de indisponibilidade, custos e requisitos de manutenção agendada (desativar horas ou localizações inconveniente).
- Na prática de empresas de nível seguinte _manutenção_, onde possam determinar o tempo de vida útil para uma peça e manter ou substituí-lo antes de uma falha. Manutenção evita falhas agendadas e catastrófica. Permanecendo os custos elevados do período de indisponibilidade planeado, under-utilização do componente antes da respetiva duração total de utilização e custos ainda.
- O objetivo de _manutenção preditiva_ é para otimizar o equilíbrio entre as medidas corretivas e preventivas a manutenção, ativando _apenas no tempo_ substituição dos componentes. Esta abordagem só substitui esses componentes quando estão próximo de uma falha. Ao alargar lifespans componente (comparado com a manutenção) e reduzir os custos de custos e de manutenção agendada (através de manutenção corretiva), as empresas podem obter reduções de custos e vantagens competitivos.

## <a name="business-problems-in-pdm"></a>Alguns dos problemas empresariais no PdM
As empresas enfrentam um risco elevado operacional devido a falhas inesperadas e limitada aprofundadas sobre a causa raiz dos problemas nos sistemas complexos. Algumas das perguntas empresarial fundamentais são:

- Deteta anomalias na desempenho do sistema ou do equipamento ou funcionalidade.
- Prever se um recurso pode falhar num futuro próximo.
- Calcule a vida útil restantes de um ativo.
- Identifica as principais causas de falha de um ativo.
- Identifique-se de que as ações de manutenção tem de ser efetuados por quando, sobre um recurso.

As instruções de objetivo típico de PdM são:

- Reduza o risco operacional de equipamento crítico de missão.
- Aumente a taxa de rentabilidade no ativos ao prever falhas antes de ocorrem.
- Controlar os custos de manutenção, Ativando as operações de manutenção de just-in-time.
- Reduzir attrition de cliente, melhorar a imagem de marca e vendas perdidas.
- Custos de inventário inferiores, reduzindo os níveis de inventário ao prever o momento de reordenação.
- Detete padrões ligadas a vários problemas de manutenção.
- Forneça KPIs (indicadores chave de desempenho), tais como pontuações de estado de funcionamento para condições de recurso.
- Calcule restantes tempo de vida dos ativos.
- Recomendamos as atividades de manutenção atempadamente.
- Ative apenas no inventário de tempo ao fazer uma estimativa de datas de ordem de substituição de partes.

Estas instruções de objetivo são os pontos de partida para:

- _cientistas de dados_ para analisar e resolver problemas específicos de preditivos.
- _arquitetos e programadores de nuvem_ para juntar-se de uma solução ponto a ponto.

## <a name="qualifying-problems-for-predictive-maintenance"></a>Elegíveis problemas de manutenção preditiva
É importante realçar que nem todos os casos de utilização ou problemas empresariais podem ser efetivamente resolvidos por PdM. Existem três critérios de nenhuns importantes que têm de ser considerada durante a seleção do problema:

- O problema tem de ser preditiva natureza; ou seja, deverá haver um destino ou um resultado para prever. O problema, também deverá ter um caminho de limpar de ação para evitar falhas de quando estes forem detetados.
- O problema deve ter um registo de histórico operacional de equipamento que contém _resultados boas e incorretos_. O conjunto de ações que tomou para mitigar resultados incorretos também deve estar disponível como parte destes registos. Relatórios de erros, os registos de manutenção de degradação do desempenho, reparar e registos de substituir também são importantes. Além disso, também são úteis reparações undertaken para melhorá-los e registos de substituição.
- O histórico de registado deve ser refletido no _relevantes_ dados que são de _suficientes_ suficiente qualidade para suportar o caso de utilização. Para obter mais informações sobre a importância de dados e sufficiency, consulte [requisitos de dados para manutenção preventiva](#Data-requirements-for-predictive-maintenance).
- Por fim, as empresas devem ter especialistas de domínio que tenham uma compreensão clara do problema. Estes devem ter conhecimento dos processos internos e práticas ajudar o analista compreender e interpretar os dados serem. Também deve ser capazes de efetuar as alterações necessárias para os processos de negócio existentes para ajudar a recolher os dados adequados para os problemas, se necessário.

## <a name="sample-pdm-use-cases"></a>PdM casos de utilização de exemplo
Esta secção centra-se numa coleção de PdM casos de utilização de várias indústrias como aeroespacial, utilitários e transportes. Cada secção começa com um problema empresarial e descreve as vantagens de PdM, os dados relevantes envolvente o problema empresarial e, finalmente, as vantagens de uma solução de PdM.

| Problema empresarial | Beneficia de PdM |
|:-----------------|-------------------|
|**Aviation**      |                   |
|_Trânsito atraso e cancelamentos_ devido a problemas de mechanical. Falhas que não podem ser reparadas no tempo podem causar flights terá de ser cancelada e perturbar o agendamento e de operações. |Soluções de PdM podem prever a probabilidade de uma aeronave que está a ser atrasado ou cancelada devido a falhas mechanical.|
|_Falha de partes do motor de aeronave_: substituições de parte do motor de aeronave são entre as tarefas de manutenção mais comuns na indústria companhia aérea. Soluções de manutenção exigem uma gestão de disponibilidade as cotações de componentes, a entrega e o planeamento|A capacidade de recolher intelligence na fiabilidade de componente leva a redução substancial nos custos de investimento.|
|**Departamento financeiro** |                         |
|_Falha de ATM_ é um problema comum na indústria banca. O problema aqui é a probabilidade de que uma transação de withdrawal monetários ATM obtém interrompida devido a uma falha papel encravado ou parte no fornecedor de monetários de relatório. Com base na predições de falhas de transação, ATMs podem ser servidos proativamente para impedir a ocorrer falhas.| Em vez de permitir que a máquina para falhar a meio através de uma transação, é a alternativa desejável programa a máquina para negar o serviço com base na predição.|
|**Energia** |                          |
|_Vento falhas turbine_: turbines vento são a origem de energia principal environmentally responsável países e envolvem os custos de capital elevados. Um componente chave no turbines vento é o motor de gerador de dimensões. uma vez compõe o turbine ineficaz. Também é muito dispendiosa corrigir.|Prever KPIs, tais como MTTF (hora a falha) pode ajudar as empresas de energia evitar falhas de turbine e certifique-se de período de indisponibilidade mínimo. Probabilidades de falha informar técnicos para monitorizar turbines que são provável que falhem em breve e agendar regimes baseados no tempo de manutenção. Modelos preditivos fornecem informações sobre a diferentes fatores que contribuem para a falha, o que ajuda a técnicos melhor compreende que as principais causas dos problemas.|
|_Falhas de disjuntor_: distribuição de eletricidade homes e as empresas requer energia as linhas a estar operacional permanente para garantir a entrega de energia. Circuito breakers ajudar a limitar ou evitar danos energia linhas durante a sobrecarga ou adverso Meteorologia condições. O problema empresarial aqui é prever disjuntor falhas.| Soluções de PdM ajudam a reduzir os custos de reparação e aumentar o tempo de vida do equipamento como breakers circuito. Ajudam a melhorar a qualidade da rede energia ao reduzir falhas inesperadas e interrupções de serviço.|
|**Transportes e logística** |    |
|_Falhas de porta de abordagem_: empresas de grande abordagem fornecem um serviço de pilha completa para milhões de elevators funcionais em todo o mundo. Segurança de abordagem, fiabilidade e disponibilidade são as preocupações principais para os seus clientes. Estas empresas controlam estas e vários outros atributos através de sensores, para ajudá-los com as medidas corretivas e preventivo a manutenção. Numa abordagem funcionar está incorretamente ocorrer o problema de cliente mais prominent sistematicamente portas abordagem. Neste caso é o problema empresarial fornecer uma aplicação preditiva de base de dados de conhecimento que prevê que o potencial causa de falhas de porta.| Elevators são os investimentos em capital para potencialmente um tempo de vida do ano de 20-30. Para que cada venda potencial pode ser altamente competitiva; Por conseguinte expetativas para o suporte e atendimento são elevadas. Manutenção preditiva pode fornecer a estas empresas com vantagens sobre os seus concorrentes no respetivo produto e ofertas de serviço.|
|_Roda falhas_: roda conta falhas para metade de todos os preparar derailments e o custo billions para o setor do lado global. Falhas de rodas também fazer com que rails para deteriorate, por vezes, fazendo com que o lado interromper prematuramente. Quebras de lado levar a eventos catastrófica, tais como derailments. Para evitar essas instâncias, railways monitorizar o desempenho de rodas e substituí-los de forma preventivo. O problema empresarial aqui é a predição de falhas de rodas.| Manutenção preditiva de rodas irão ajudá-lo com a substituição de just-in-time de rodas |
|_Falhas de porta de formação subway_: um motivo principais atrasos nas operações de subway é falhas de porta de formação automóveis. O problema empresarial aqui é prever falhas de porta de formação.|Deteção inicial de uma falha de porta ou o número de dias até uma falha de porta, irá ajudar a otimizar o negócio preparar porta agendas de manutenção.|

Obtém a secção seguinte para os detalhes sobre como as vantagens de PdM analisados acima.

## <a name="data-science-for-predictive-maintenance"></a>Ciência de dados para manutenção preventiva

Esta secção fornece orientações gerais de princípios de ciência de dados e recomendado para PdM. Destina-se para o ajudar a TDM, arquiteto de solução ou um programador compreender os pré-requisitos e o processo para criar aplicações de AI ponto-a-ponto para PdM. Pode ler nesta secção, juntamente com uma revisão das demonstrações e modelos de prova do conceito listados em [modelos de solução de manutenção preditiva](#Solution-templates-for-predictive-maintenance). Em seguida, pode utilizar estes princípios e as melhores práticas para implementar a sua solução PdM no Azure.

> [!NOTE]
> Este guia destina-se não ensinar o leitor de ciência de dados. São fornecidas várias origens útil para ler mais na secção de [recursos de formação para manutenção preventiva](#Training-resources-for-predictive-maintenance). O [modelos de solução](#Solution-templates-for-predictive-maintenance) indicados no manual de demonstrar estes AI as técnicas de para problemas de PdM específicos.

## <a name="data-requirements-for-predictive-maintenance"></a>Requisitos de dados para manutenção preventiva

A taxa de êxito de qualquer learning depende (a) a qualidade dos que está a ser taught e (b) a capacidade do learner. Modelos preditivos saiba padrões de dados históricos e prever futuros resultados com determinadas probabilidade com base nestes padrões observados. Depende da precisão preditiva um modelo no relevancy, sufficiency e qualidade dos dados de formação e teste. Os novos dados é 'classificados' utilizando este modelo devem ter as mesmas funcionalidades e o esquema, como os dados de formação/teste. As características de funcionalidade (tipo, densidade, distribuição e assim sucessivamente) de dados nova devem corresponder ao que os conjuntos de dados de formação e teste. O foco desta secção é esses requisitos de dados.

### <a name="relevant-data"></a>Dados relevantes

Em primeiro lugar, os dados tem de ser _relevantes para o problema_. Considere o _roda falha_ utilizar caso mencionado acima - os dados de formação devem conter as funcionalidades relacionadas com as operações de rodas. Se o problema foi para prever a falha do _sistema traction_, tem dos dados de formação para abranger todos os componentes diferentes para o sistema traction. O primeiro caso destina-se um componente específico, enquanto o segundo caso destina-se a falha de um subsistema de maior. A recomendação geral é para estruturar a sistemas de predição sobre componentes específicos em vez de subsistemas de maior, uma vez que a última opção irá ter mais dispersos dados. A especialista de domínio (consulte [elegíveis problemas de manutenção preditiva](#Qualifying-problems-for-predictive-maintenance)) deve ajudar a selecionar os mais relevantes subconjuntos de dados para a análise. As origens de dados relevantes são abordadas em maior detalhe no [dados durante a preparação da manutenção preditiva](#Data-preparation-for-predictive-maintenance).

### <a name="sufficient-data"></a>Dados suficientes
Duas questões são mais comuns em relação a dados do histórico de falha: (1) "quantos eventos de falha são necessários para preparar um modelo?" (2) "registos quantos são considerados como estando"suficiente"?" Existem não existem respostas definitiva, mas apenas regras geral. Para (1), mais o número de eventos de falha, uma melhor o modelo. Para (2), e o número exato de eventos de falha depende dos dados e o contexto do problema que está a ser resolvido. Mas no lado flip, se uma máquina falhar demasiadas vezes, em seguida, a empresa irá substituir, que irá reduzir as instâncias de falha. Aqui, a documentação de orientação do domínio especialista é importante. No entanto, existem métodos para lidar com o problema de _eventos raros_. Estes são abordados na secção [processar dados imbalanced](#Handling-imbalanced-data).

### <a name="quality-data"></a>Dados de qualidade
A qualidade dos dados é crítica - a cada valor de atributo de previsão da receita tem de ser _exata_ em conjunto com o valor da variável de destino. Qualidade de dados é uma área bem studied na gestão de estatísticas e dados e, por conseguinte, fora do âmbito deste guia.

> [!NOTE]
> Existem vários recursos e produtos enterprise para fornecer dados de qualidade. Um exemplo de referências é fornecido abaixo:
> - Dasu, T, Johnson, extração de dados t. exploratórias e dados de limpeza, Wiley, 2003.
> - [Análise de dados exploratórias, Wikipedia](https://en.wikipedia.org/wiki/Exploratory_data_analysis)
> - [Dados quantitativos Hellerstein, J, limpeza para bases de dados grandes](http://db.cs.berkeley.edu/jmh/papers/cleaning-unece.pdf)
> - [loo no Jonge Alemanha, E, van der, M, introdução aos dados de limpeza com R](https://cran.r-project.org/doc/contrib/de_Jonge+van_der_Loo-Introduction_to_data_cleaning_with_R.pdf)

## <a name="data-preparation-for-predictive-maintenance"></a>Preparação de dados para manutenção preventiva

### <a name="data-sources"></a>Origens de dados

As origens de dados relevantes para manutenção preventiva incluem, mas não estão limitadas a:
- Histórico de falha
- Histórico de reparação/manutenção
- Condições de operação de máquina
- Metadados de equipamento

#### <a name="failure-history"></a>Histórico de falha
Eventos de falha são raros PdM aplicações. No entanto, quando a criação de modelos de previsão, o algoritmo necessita saber mais sobre padrão operacionais normais de um componente, bem como os padrões de falha. Por isso, os dados de formação devem conter um número suficiente de exemplos de ambas as categorias. Histórico de substituição de registos e em partes de manutenção são boas origens para encontrar eventos de falha. Com a ajuda de alguns dados de conhecimento do domínio, anomalias nos dados de formação também podem ser definidas como falhas.

#### <a name="maintenancerepair-history"></a>Histórico de reparação/manutenção
Histórico de manutenção de um ativo contém detalhes sobre os componentes substituídos, atividades de reparação executadas etc. Estes eventos padrões de degradação de registo. Ausência destas informações fundamental dos dados de formação pode originar resultados de modelo enganosa. Também é possível encontrar o histórico de falha no histórico de manutenção como códigos de erro especiais ou datas de ordem de partes. Origens de dados adicionais, que influencia os padrões de falha devem ser investigadas e fornecidas por especialistas de domínio.

#### <a name="machine-operating-conditions"></a>Condições de operação de máquina
Dados de transmissão em fluxo de sensor com base (ou outros) do equipamento na operação são uma origem de dados importantes. Um chave pressuposto no PdM é que o estado de funcionamento de uma máquina degrada ao longo do tempo durante a operação de rotina. Os dados deverá conter vários de tempo de funcionalidades que capturar este padrão de envelhecimento e qualquer anomalias que leva a degradação do. O aspeto temporal dos dados é necessário para o algoritmo saber mais falha e padrões de não-falhas, ao longo do tempo. O algoritmo com base nestes pontos de dados, aprende a prever quantos mais unidades de tempo que uma máquina pode continuar a funcionar antes de falhar.

#### <a name="static-feature-data"></a>Dados da funcionalidade estático
Funcionalidades estáticas são metadados sobre o equipamento. Os exemplos são o equipamento disponibilizar, modelo, data fabricada, inicie a data do serviço, a localização do sistema e outras especificações técnicas.

Exemplos de dados relevantes para o [casos de utilização de exemplo PdM](#Sample-PdM-use-cases) são apresentadas abaixo:

| Caso de utilização | Exemplos de dados relevantes |
|:---------|---------------------------|
|_Atraso de trânsito e cancelamentos_ | Informações de rota de voo sob a forma de registos de página e legs voo. Dados de fase de voo incluem detalhes encaminhamento como data distanciamento/chegada, hora, airport, layovers etc. Registo de página inclui uma série de códigos de erro e manutenção registados pelo técnico de manutenção de zero.|
|_Falha de partes do motor de aeronave_ | Recolher os dados de sensores da aeronave que fornece informações sobre a condição de várias partes. Registos de manutenção ajudam a identificar quando ocorreram de falhas do componente e foram substituídas.|
|_Falha de ATM_ | As leituras dos sensores para cada transação (depositar monetários/verificação) e dispensing de monetários. As informações na medição de intervalo entre notas, tenha em atenção a espessura das, tenha em atenção a distância de chegada, verifique os atributos etc. Registos de manutenção que fornecem códigos de erro, informações de reparação, pela última vez o fornecedor de monetários foi refilled.|
|_Falha do vento turbine_ | Sensores monitorizar condições turbine como a temperatura vento direção, energia gerados, velocidade de gerador etc. Recolha de dados de vários vento turbines de farms vento localizados em várias regiões. Normalmente, cada turbine tem vários as leituras dos sensores reencaminhar medidas num intervalo de tempo fixo.|
|_Falhas de disjuntor automático_ | Registos de manutenção que incluem ações corretivas, preventivo e systematic. Dados operacionais, que incluem automáticas e manuais comandos enviados para o circuito breakers, tal como para ações de abrir e fechar. Metadados do dispositivo como data de fabrico, localização, modelo, etc. Especificações de disjuntor como níveis de tensão de geolocalização, condições de ambiente.|
|_Falhas de porta de abordagem_| Abordagem metadados, tais como o tipo de abordagem, fabricada data, frequência de manutenção, tipo de criação e assim sucessivamente. Informações operacionais, tais como o número de ciclos de porta, tempo médio de porta de fechar. Histórico de falha com causas.|
|_Falhas de rodas_ | Dados de sensores que roda de medidas de aceleração, braking instâncias, distância despertar, velocidade, etc. As informações de estático no rodas, como fabricante, fabricado data. Dados de falha inferidos a partir de base de dados de ordem de parte que controlam as datas de ordem e quantidades.|
|_Falhas de porta de formação subway_ | Abrir a porta e o fecho, outros dados operacionais, como condição atual de portas de formação. Dados estáticos inclui identificador de recurso, a hora e colunas de valor de condição.|

### <a name="data-types"></a>Tipos de dados
Tendo em conta as origens de dados acima, os dois tipos de dados principal observados em PdM domínio são:

- _Dados temporais_: telemetria operacional, as condições de máquina, tipos de ordem de trabalho, os códigos de prioridade que terão carimbos no momento da gravação. Falha, histórico de utilização e manutenção/reparar, terá também carimbos associados a cada evento.
- _Dados estáticos_: funcionalidades de máquina e funcionalidades de operador em geral são estáticas, uma vez que descrevem as especificações técnicas de máquinas ou atributos de operador. Se estas funcionalidades, podem alterar ao longo do tempo, deve também têm os carimbos associados aos mesmos.

As variáveis de previsão da receita e de destino devem ser de pré-processados/transformado [tipos de dados numéricos, categórico e outros](https://www.statsdirect.com/help/basics/measurement_scales.htm) consoante o algoritmo que está a ser utilizado.

### <a name="data-preprocessing"></a>Dados pré-processamentos
Como um pré-requisito para _engenharia da funcionalidade_, preparar os dados a partir de vários fluxos para compor um esquema a partir da qual seja fácil compilar funcionalidades. Visualize os dados primeiro como uma tabela de registos. Cada linha na tabela representa uma instância de formação e as colunas que representam _previsão da receita_ funcionalidades (também denominadas atributos independentes ou variáveis). Organizar os dados de forma a que é a último colunas a _destino_ (variável dependentes). Para cada instância de formação, atribuir um _etiqueta_ como o valor desta coluna.

Para dados temporais, divida a duração de dados de sensores em unidades de tempo. Cada registo deve pertencer a uma unidade de tempo para um recurso, _e deve oferecer informações distintas_. Unidades de tempo são definidas com base nas necessidades de negócio em múltiplos de segundos, minutos, horas, dias, meses, e assim sucessivamente. A unidade de tempo _tem de ser o mesmo que a frequência de recolha de dados_. Se a frequência é elevada, os dados não podem mostrar qualquer diferença significativa de uma unidade para outro. Por exemplo, suponha que o ambiente temperatura foi recolhida cada 10 segundos. Só é utilizar esse mesmo intervalo para os dados de formação enche o número de exemplos sem fornecer informações adicionais. Neste caso, uma melhor estratégia seria utilizar média os dados mais de 10 minutos, ou uma hora com base na justificação de negócio.

Para dados estáticos,
- _Registos de manutenção_: dados não processados de manutenção tem um identificador de recurso e timestamp com informações sobre as atividades de manutenção que ter sido executadas num determinado ponto no tempo. Transformar as atividades de manutenção para _categórico_ colunas, onde cada descritor de categoria exclusivamente mapeia para uma ação de manutenção específico. O esquema para os registos de manutenção inclui identificador de recurso, a hora e a ação de manutenção.

- _Registos de falhas_: falhas ou os motivos da falha podem ser registados como códigos de erro específico ou eventos de falha definidos por condições empresariais específicos. Nos casos em que o equipamento tem vários códigos de erro, o domínio especialista deve identificar aqueles que são relevantes para a variável de destino. Utilizar o restante códigos de erro ou condições para construir _previsão da receita_ funcionalidades que se correlacionam com estas falhas. O esquema para os registos de falhas inclui o identificador de recurso, hora, falha ou motivo da falha - se disponível.

- _Metadados de máquina e operador_: intercale os dados de máquina e o operador de esquemas para associar um recurso com o operador, juntamente com os respetivos atributos. Inclui o identificador de recurso, recursos, identificador de operador, operador e das funcionalidades e o esquema para condições de máquina.

Outros dados pré-processamentos passos incluem _lidar com os valores em falta_ e _normalização_ de valores de atributo. Ver um debate detalhado está fora do âmbito deste guia - consulte a secção seguinte para algumas referências útil.

Com o procedimento acima pré-processados origens de dados no local, a transformação final antes de engenharia da funcionalidade associar as tabelas acima, com base no identificador de recurso e timestamp. A tabela resultante iria ter valores nulos para a coluna de falha quando a máquina está em funcionamento normal. Estes valores nulos podem imputed por um indicador para um funcionamento normal. Utilize esta coluna de falha para criar _etiquetas para o modelo preditivo_. Para obter mais informações, consulte a secção sobre [modelação técnicas para manutenção preventiva](#Modeling-techniques-for-predictive-maintenance).

## <a name="feature-engineering"></a>Com engenharia
Engenharia da funcionalidade é o primeiro passo antes dos dados de modelação. A função no processo de ciência de dados [é descrito aqui](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/create-features). A _funcionalidade_ é um atributo preditivo para o modelo - como temperatura, pressão, vibration e assim sucessivamente. Para PdM, engenharia da funcionalidade envolve abstrair o estado de funcionamento de uma máquina através de dados históricos recolhidos ao longo de uma duração de dimensão considerável. Nesse sentido, é diferente do respetivos elementos de rede, como monitorização remota, deteção de anomalias e a deteção de falha. 

### <a name="time-windows"></a>Intervalos de tempo
Remoto monitorização envolve reporting os eventos que ocorrem no _pontos no tempo_. Modelos de deteção de anomalias avaliar fluxos de entrada (pontuação) de dados para anomalias de sinalizador a partir de pontos no tempo. A deteção de falha classifica falhas para ser de tipos específicos à medida que ocorrem pontos no tempo. Em contrapartida, PdM envolve prever falhas através de um _período de tempo futuros_, com base nas funcionalidades que representam o comportamento da máquina através de _período de tempo históricos_. Para PdM, dados de funcionalidade pontos individuais de tempo de são demasiado inúteis ser preditiva. Para que os dados para cada funcionalidade tem de ser _smoothened_ pela agregação de pontos de dados através de intervalos de tempo.

### <a name="lag-features"></a>Funcionalidades de desfasamento
Os requisitos comerciais definem até que ponto o modelo tem de prever no futuro. Por sua vez, esta duração ajuda a definir 'até que ponto back o modelo tem de ter um aspeto' para tornar estas predições. Este 'procura volta' período denomina-se a _desfasamento_, e funcionalidades foi desenvolvidas durante este período de atraso são denominadas _lag funcionalidades_. Esta secção descreve as funcionalidades de desfasamento que podem ser construídas a partir de origens de dados com carimbos e a criação de funcionalidade das origens de dados estáticos. Normalmente, as funcionalidades de desfasamento estão _numérica_ natureza.

> [!IMPORTANT]
> O tamanho da janela é determinado através de experimentação e deve ser finalizado com a ajuda de uma especialista de domínio. A mesma advertência se para a seleção e a definição do desfasamento funcionalidades, as agregações e o tipo do windows.

#### <a name="rolling-aggregates"></a>A anular agregados
Para cada registo de um ativo, uma janela do tamanho do "M" graduais é escolhida como o número de unidades de tempo para as agregações de computação. Funcionalidades de desfasamento, em seguida, são calculadas utilizando os períodos de W _antes da data_ desse registo. Na figura 1, as linhas azuis mostram valores de sensor registados para um recurso para cada unidade de tempo. Estes denotam uma média móvel dos últimos valores de funcionalidade numa janela de tamanho W = 3. A média graduais é calculada ao longo de todos os registos com carimbos no intervalo t<sub>1</sub> (cor de laranja) para t<sub>2</sub> (em verde). O valor para W é, geralmente, nos minutos ou horas, dependendo da natureza dos dados. Mas alguns problemas, escolha uma grande W (diga de 12 meses) podem fornecer o histórico completo de um recurso de até à vez do registo.

![Figura 1. A anular funcionalidades agregadas](./media/cortana-analytics-playbook-predictive-maintenance/rolling-aggregate-features.png) figura 1. A anular funcionalidades agregadas

Exemplos de sucessiva agregados através de uma janela de tempo são contagem, média, medidas CUMESUM (sum cumulativa), valores mínimo/máximo. Além disso, a variância, desvio-padrão e contagem de valores atípicos para além de desvios padrão de N são frequentemente utilizados. Exemplos de agregados que podem ser aplicados para a [casos de utilização](#Sample-PdM-use-cases) neste guia são listados abaixo. 
- _Trânsito atraso_: contagem de códigos de erro ao longo do último dia/semana.
- _Falha de parte do motor de aeronave_: sucessiva significa, o desvio-padrão e soma ao longo do último dia, semana, etc. Esta métrica deve ser determinada, juntamente com o domínio de negócio especialista.
- _Falhas de ATM_: sucessiva significa mediana, intervalo, desvios padrão, a contagem de valores atípicos para além de desvios padrão três, CUMESUM superior e inferior.
- _Falhas de porta de formação subway_: contagem de eventos ao longo do último dia, semana, duas semanas etc.
- _Falhas de disjuntor_: falha de conta ao longo da última semana, ano, três anos etc.

Outra técnica útil na PdM é para capturar as alterações de tendência, picos e alterações ao nível utilizando algoritmos de detetam anomalias nos dados.

#### <a name="tumbling-aggregates"></a>Agrega em cascata
Para cada etiqueta de um recurso, de uma janela do tamanho do registo _W -<sub>k</sub>_  estiver definida, onde _k_ é o número de janelas de tamanho _W_. Os agregados, em seguida, são criados ao longo do _k_ _em cascata windows_ _W-k, W -<sub>(k-1)</sub>,..., W -<sub>2</sub>, W -<sub>1</sub>_  para os períodos de antes de carimbo de um registo. _k_ pode ser um número reduzido para capturar os efeitos de curta duração ou um grande número de captura de padrões de degradação de longo prazo. (consulte a figura 2).

![Figura 2. Funcionalidades de agregação em cascata](./media/cortana-analytics-playbook-predictive-maintenance/tumbling-aggregate-features.png) figura 2. Em cascata funcionalidades agregadas

Por exemplo, lag funcionalidades para o caso de utilização do vento turbines pode ser criado com W = 1 e k = 3. Estes implica o desfasamento para cada um dos últimos três meses utilizando valores atípicos superior e inferior.

### <a name="static-features"></a>Funcionalidades estáticas

As especificações técnicas de equipamento como data de fabrico, número de modelo, localização, se alguns exemplos das funcionalidades estáticos. São tratadas como _categórico_ variáveis para a modelação. Alguns exemplos para o caso de utilização disjuntor são tensão, atual, a capacidade de energia, do tipo de transformador e origem de energia. Para falhas de rodas, o tipo de rodas tire (alloy vs steel) é um exemplo.

Os esforços de preparação de dados até ao momento abordados devem fazer com que os dados que está a ser organizados conforme mostrado abaixo. Dados de formação, teste e validação devem ter este esquema lógico (Este exemplo mostra o tempo em unidades de dias).

| ID do Recurso | Hora | <Feature Columns> | Etiqueta |
| ---- | ---- | --- | --- |
| A123 |1 dia | . . . | . |
| A123 |Dia 2 | . . . | . |
| ...  |...   | . . . | . |
| B234 |1 dia | . . . | . |
| B234 |Dia 2 | . . . | . |
| ...  |...   | . . . | . |

O último passo engenharia da funcionalidade é a **etiquetas** da variável de destino. Este processo é dependente a técnica de modelação. Por sua vez, a técnica de modelação depende o problema empresarial e natureza dos dados disponíveis. Etiquetagem mencionado na secção seguinte.

> [!IMPORTANT]
> Preparação de dados e de engenharia da funcionalidade são tão importantes como modelação técnicas para chegam soluções de PdM com êxito. A especialista de domínio e o profissional devem investir muito tempo em que chegam à funcionalidades à direita e os dados para o modelo. Um exemplo pequeno books muitos no engenharia da funcionalidade são listados abaixo:
> - Pyle, d. dados de preparação de dados (Morgan Kaufmann série em sistemas de gestão de dados), de extração 1999
> - Zheng, recomeço, Casari, engenharia da funcionalidade de recomeço do Machine Learning: princípios e técnicas de cientistas de dados, O'Reilly, 2018.
> - Dong G. Liu, H. (Editores), da funcionalidade de engenharia de Machine Learning e análise de dados (Chapman & Hall/CRC extração de dados e série de deteção de dados de conhecimento), prima CRC, 2018.

## <a name="modeling-techniques-for-predictive-maintenance"></a>Técnicas de modelação de manutenção preditiva

Esta secção descreve as técnicas de modelação principal para problemas de PdM, juntamente com os respetivos métodos de construção de etiqueta específica. Tenha em atenção que uma técnica de modelação única pode ser utilizada em diferentes indústrias. A técnica de modelação se encontra emparelhada o problema de ciência de dados, em vez do contexto dos dados em execução.

> [!IMPORTANT]
> A escolha de etiquetas para cenários de falha e a estratégia de etiquetas  
> deve ser determinado em consultation com o domínio especialista.

### <a name="binary-classification"></a>classificação binária
Classificação binária é utilizada para _prever a probabilidade de um fragmento do equipamento falha num período de tempo futuros_ - chamado o _período horizon futuras X_. X é determinado pelo problema empresarial e em execução, os dados no consultation com o domínio especialista. Os exemplos são:
- _tempo de antecedência mínimo_ necessário substituir os componentes, implementar recursos de manutenção, efetuar a manutenção para evitar um problema que é provável que ocorrem durante esse período.
- _contagem mínima de eventos_ que podem ocorrer antes de ocorrer um problema.

Nesta técnica, os dois tipos de exemplos de formação são identificados. Um exemplo positivo, _que indica uma falha_, com etiqueta = 1. Um exemplo negativo, o que indica as operações normais, com etiqueta = 0. A variável de destino e, por conseguinte, os valores de etiqueta, são _categórico_. O modelo deve identificar cada exemplo novo como provável que falhem ou a funcionar normalmente durante a próxima X unidades de tempo.

#### <a name="label-construction-for-binary-classification"></a>Construção da etiqueta para classificação binária
A pergunta aqui é: "o que é a probabilidade do elemento falhará as próximas X unidades de tempo?" Para responder a esta pergunta, os registos de etiqueta X antes da falha de um ativo como "prestes a falha" (etiqueta = 1) e Etiquetar todos os outros registos como estando "normal" (etiqueta = 0). (consulte a figura 3).

![Figura 3. Etiquetas para classificação binária](./media/cortana-analytics-playbook-predictive-maintenance/labelling-for-binary-classification.png) figura 3. Etiquetas para classificação binária

Exemplos de etiquetagem estratégia para algumas das casos de utilização são listados abaixo.
- _Trânsito atrasos_: X poderá ser selecionado como 1 dia, para prever atrasos nas próximas 24 horas. Em seguida, todos os flights que estão dentro de 24 horas antes de falhas estão identificadas como 1.
- _Monetários ATM dispense falhas_: pode ser um objetivo de determinar a probabilidade de falha de uma transação na próxima hora de um. Nesse caso, todas as transações que tenham acontecido dentro da hora anterior da falha são identificadas como 1. Para prever a probabilidade de falhas através de moeda de N seguinte notas dispensed, todas as notas dispensed dentro as notas de N últimos de uma falha são identificadas como 1.
- _Falhas de disjuntor_: pode ser o objetivo de prever a falha do comando disjuntor seguinte. Nesse caso, os X é escolhido para ser um comando futuro.
- _Dar formação sobre falhas de porta_: X poderá ser selecionado como dois dias.
- _Vento falhas turbine_: X poderá ser selecionado como dois meses.

### <a name="regression-for-predictive-maintenance"></a>Regressão para manutenção preventiva
Modelos de regressão são utilizados para _a vida útil restantes (RUL) de um recurso de computação_. RUL está definida como a quantidade de tempo que um recurso está operacional antes de ocorrer a seguinte falha. Cada exemplo de formação é um registo a que pertence a uma unidade de tempo _nY_ para um recurso, onde _n_ é o múltiplo. O modelo deve calcular a RUL de cada novo exemplo como um _número contínuo_. Este número indica o período de tempo restante antes da falha.

#### <a name="label-construction-for-regression"></a>Etiqueta de construção de regressão
A pergunta aqui é: "O que é a vida útil restantes (RUL) do equipamento?" Para cada registo antes da falha, calcule a etiqueta para ser o número de unidades de tempo restante antes da falha seguinte. Neste método, as etiquetas são variáveis contínuas. (Consulte a figura 4)

![Figura 4. Etiquetagem para regressão](./media/cortana-analytics-playbook-predictive-maintenance/labelling-for-regression.png) figura 4. Etiquetagem para regressão

Para regressão, a etiquetagem é feito com referência ao ponto de falha. Não é possível o cálculo sem saberem quanto o elemento tem survived antes de uma falha. Por isso, por outro lado a classificação do binária, ativos sem falhas nos dados não podem ser utilizados para a modelação. Este problema é abordado melhor outra técnica análises chamada [sobrevivência Analysis](https://en.wikipedia.org/wiki/Survival_analysis). Mas possíveis complicações podem surgir quando aplicar esta técnica para casos de utilização de PdM que envolvem dados vários de tempo com intervalos frequentes. Para obter mais informações sobre a análise de sobrevivência, consulte [numa-pager](https://www.cscu.cornell.edu/news/statnews/stnews78.pdf).

### <a name="multi-class-classification-for-predictive-maintenance"></a>Classificação de classe multi para manutenção preventiva
Técnicas de classificação de classe multi podem ser utilizadas em soluções de PdM para dois cenários:
- Prever _dois futuros resultados_: é o resultado primeiro _um intervalo de tempo para falha_ para um recurso. O elemento é atribuído a um dos vários possíveis períodos de tempo. O resultado segundo é a probabilidade de falhas num período futuro devido a _um dos vários raiz faz com que_. Este predição permite que a equipa de engagement manutenção a observar sintomas e agendas de manutenção de plano.
- Prever _causa mais provável_ de uma falha indicada. Este resultado recomenda o conjunto correto de ações de manutenção para corrigir uma falha. Uma lista de causas raiz e reparações recomendadas classificados pode ajudar a atribuir prioridades respetivas ações de reparação após uma falha de técnicos.

#### <a name="label-construction-for-multi-class-classification"></a>Construção da etiqueta para a classe multi classificação
A pergunta aqui é: "o que é a probabilidade de um recurso falhará as próximas _nZ_ unidades de tempo em que _n_ é o número de períodos?" Para responder a esta questão, etiqueta nZ registos antes da falha de um ativo utilizando registos de tempo (3Z, 2Z, Z). Etiqueta de todos os outros regista como "normal" (etiqueta = 0). Neste método, a variável de destino contém _categórico_ valores. (Consulte a figura 5).

![Figura 5. Etiquetas para classificação de várias classes para predição de falha de tempo](./media/cortana-analytics-playbook-predictive-maintenance/labelling-for-multiclass-classification-for-failure-time-prediction.png) figura 5. Etiquetas para classificação de classe multi para predição de falha de tempo

A pergunta aqui é: "o que é a probabilidade do elemento falhará as próximas X unidades de tempo devido a causa de raiz/problema _P<sub>posso</sub>_?" onde _posso_ é o número de causas raiz possíveis. Para responder a esta pergunta, os registos de etiqueta X antes da falha de um ativo como "prestes a falhar devido a causa raiz _P<sub>posso</sub>_" (etiqueta = _P<sub>posso</sub>_). Etiqueta de todos os outros registos como estando "normal" (etiqueta = 0). Neste método além disso, as etiquetas são categórico (consulte a figura 6).

![Figura 6. Etiquetas para classificação de várias classes para a predição de causa raiz](./media/cortana-analytics-playbook-predictive-maintenance/labelling-for-multiclass-classification-for-root-cause-prediction.png) figura 6. Etiquetas para classificação de classe multi para predição causa de raiz

O modelo atribui uma probabilidade de falhas devido a cada _P<sub>posso</sub>_  , bem como a probabilidade de não falha. Estes probabilidades podem ser ordenadas por magnitude para permitir a previsão dos problemas que são mais provável ocorrer no futuro.

A pergunta aqui é: "que ações de manutenção é recomendada após uma falha?" Para responder a esta questão, etiquetagem _não necessita de um horizon futura para ser selecionadas_, porque o modelo não é a previsão falha com no futuro. Esta é apenas a previsão mais provavelmente causa raiz _depois da falha já tiver ocorrido_.

## <a name="training-validation-and-testing-methods-for-predictive-maintenance"></a>Métodos de teste para manutenção preditiva, formação e validação
O [o processo de ciência de dados de equipa](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/overview) fornece uma cobertura total do ciclo de formação teste validar modelo. Esta secção aborda aspetos exclusivos para PdM.

### <a name="cross-validation"></a>Validação de se estender
O objetivo de [cruzada validação](https://en.wikipedia.org/wiki/Cross-validation_(statistics)) consiste em definir um conjunto de dados para "teste" do modelo na fase de preparação. Este conjunto de dados é denominado o _validação conjunto_. Problemas de limite de ajuda a esta técnica como _overfitting_ e dá um conhecimentos aprofundados sobre como o modelo irá generalize a um conjunto de dados independente. Ou seja, um desconhecido conjunto de dados, que pode ser um problema real. A rotina de formação e teste para PdM tem de ter em consideração os aspetos variando tempo para melhor generalizar unseen dados futuras.

Muitos algoritmos de machine learning dependem de um número de sintonização que pode ser alteradas significativamente o desempenho do modelo. Os valores ideal de sintonização estas não são calculados automaticamente quando se prepara o modelo. Estes devem ser especificados pelo scientist a dados. Existem várias formas de localizar boas valores de sintonização.

As mais comuns um é _k subconjuntos de validação validação cruzada_ que divide os exemplos aleatoriamente para _k_ subconjuntos de validação. Para cada conjunto de valores de sintonização, execute o algoritmo do learning _k_ vezes. Em cada iteração, utilize os exemplos de subconjuntos de validação atual como um conjunto de validação e o resto dos exemplos como um conjunto de preparação. Preparar o algoritmo através de exemplos de formação e as métricas de desempenho de computação através de exemplos de validação. No final deste ciclo, calcular a média de _k_ métricas de desempenho. Para cada conjunto de valores de hyperparameter, escolha aqueles que tenham o melhor desempenho médio. A tarefa da escolha de sintonização é frequentemente experimental natureza.

No PdM problemas, dados são registados como uma série de tempo de eventos ter várias origens de dados. Estes registos podem ser ordenados, de acordo com a hora de etiquetagem. Por conseguinte, se o conjunto de dados é dividido _aleatoriamente_ num conjunto de preparação e de validação, _alguns exemplos de formação podem ser posterior no tempo à alguns exemplos de validação_. Desempenho futuro de valores de hyperparameter irá ser estimado com base em alguns dados que chegaram _antes_ modelo foi preparado. Estas estimativas poderão excessivamente otimista, especialmente se a série de tempo não estiver estacionário e medida que evolui ao longo do tempo. Como resultado, os valores de hyperparameter escolhido poderão ser inferior ao ideal.

A forma recomendada é dividir os exemplos em formação e validação definido _dependentes da hora_ , de forma onde todos os exemplos de validação são posteriores no tempo que todos os exemplos de formação. Para cada conjunto de valores de hyperparameter, dar formação sobre o algoritmo ao longo do conjunto de dados de preparação. Medir o desempenho do modelo através do mesmo conjunto de validação. Escolha os valores de hyperparameter que mostram o melhor desempenho. Valores de Hyperparameter escolhidos por formação/validação resultado da divisão do desempenho de melhor futuras modelo que com os valores escolhidos aleatoriamente por validação cruzada.

O modelo final pode ser gerado por um algoritmo de aprendizagem sobre dados de formação completa utilizando os valores de hyperparameter melhor de preparação.

### <a name="testing-for-model-performance"></a>Testar desempenho de modelo
Uma vez que é criado um modelo, não é necessária uma estimativa do respetivo desempenho futura em novos dados. É uma boa estimar a métrica de desempenho de valores de hyperparameter processada através do conjunto de validação, ou uma métrica de desempenho médio calculada a partir da validação cruzada. Estas estimativas são frequentemente excessivamente otimista. As empresas e, muitas vezes, podem ter algumas diretrizes adicionais em deseja testar o modelo.

A forma recomendada de PdM é para dividir os exemplos em formação, validação, e nos conjuntos de dados de teste _dependentes da hora_ forma. Todos os exemplos de teste devem ser posteriores no tempo que todos os exemplos de formação e a validação. Após a divisão gerar o modelo e medir o desempenho dele, conforme descrito anteriormente.

Quando a série de tempo é estacionário e fácil prever, abordagens aleatórias e dependentes da hora geram semelhantes estimativas de desempenho futura. Mas, quando séries de tempo não estacionário e/ou disco rígido prever, a abordagem de dependentes da hora irá gerar mais realistas estimativas de desempenho futura.

### <a name="time-dependent-split"></a>Divisão de dependentes da hora
Esta secção descreve as melhores práticas para implementar a divisão de dependentes da hora. Uma divisão bidirecional dependentes da hora entre formação e teste conjuntos é descrita abaixo.

Um fluxo de eventos de timestamped como medidas de sensores vários partem do princípio. Defina as funcionalidades e as etiquetas de formação e exemplos de teste ao longo do tempo de frames que contêm vários eventos. Por exemplo, para classificação binária, criar funcionalidades com base em eventos anteriores e criar etiquetas com base em eventos futuros no "X" unidades de tempo no futuro (consulte as secções em [engenharia da funcionalidade](#Feature-engineering) e [modelação técnicas](#Modeling-techniques-applied-to-PdM-use-cases)). Assim, o intervalo de tempo de etiquetas de um exemplo é fornecido mais tarde do que o intervalo de tempo das respetivas funcionalidades.

Para dividir dependentes da hora, escolha um _formação tempo serem T<sub>c</sub>_  no qual pretende dar formação sobre um modelo com otimizados utilizando os dados históricos até T de sintonização<sub>c</sub>. Para evitar a fuga de etiquetas futuras que ultrapassam T<sub>c</sub> para os dados de formação, escolha a hora mais recente para exemplos de formação de etiqueta ser X unidades antes de T<sub>c</sub>. No exemplo mostrado na figura 7, cada quadrada representa um registo no conjunto de dados onde as funcionalidades e as etiquetas são calculadas conforme descrito acima. A ilustração mostra os registos que devem entrar em formação e testar conjuntos para X = 2 e W = 3:

![Figura 7. Dependentes da hora de divisão de classificação binária](./media/cortana-analytics-playbook-predictive-maintenance/time-dependent-split-for-binary-classification.png) figura 7. Dividir dependentes da hora de classificação binária

Os verdes quadrados representam registos que pertencem às unidades de tempo que podem ser utilizadas para formação. Cada exemplo de formação é gerado pelo considerar passado três períodos de geração de funcionalidade e dois períodos futuros de etiquetagem antes de T<sub>c</sub>. Quando qualquer parte dos dois períodos de futuros ultrapassa o T<sub>c</sub>, excluir esse exemplo do conjunto de dados de formação, porque não existem visibilidade pressupõe-se para além de T<sub>c</sub>.

Os quadrados preto representam os registos de final identificado conjunto de dados que não deve ser utilizada no conjunto de dados de formação, dado a restrição acima. Estes registos também não serão utilizados no teste de dados, uma vez que são antes de T<sub>c</sub>. Além disso, as respetivas etiquetas frames Jumbo tempo parcialmente dependem do intervalo de tempo de preparação, que não é ideal. Dados de formação e teste devem ter frames Jumbo tempo etiquetas separadas para evitar a fuga de informações da etiqueta.

Permite a sobreposição entre formação e testar exemplos que tenham carimbos quase T técnica abordada até ao momento<sub>c</sub>. Uma solução para alcançarem maior separação é para excluir os exemplos que se encontrem dentro de unidades de tempo de W de T<sub>c</sub> do teste definido. Mas esse uma divisão agressiva depende da disponibilidade de dados ample.

Modelos de regressão utilizados para prever RUL são mais gravemente afetados pelo problema a fuga. Utilizando o método de divisão aleatório leva a Alpine ajuste superior. Para problemas de regressão, a divisão deve ser, de modo que os registos que pertencem a recursos com falhas antes de T<sub>c</sub> vá para o conjunto de preparação. Registos de recursos com falhas depois de serem enviadas para o conjunto de teste.

Outro procedimento recomendado para dividir os dados para formação e testar consiste em utilizar uma divisão por ID de recurso. A divisão deve ser de forma a que nenhum dos recursos utilizados no conjunto de preparação são utilizadas no testar o desempenho do modelo. Através desta abordagem, um modelo tem melhor oportunidade de fornecer realistas mais resultados com recursos novos.

### <a name="handling-imbalanced-data"></a>Processar dados imbalanced
Em problemas de classificação, se existirem mais exemplos de uma classe que dos outros, o conjunto de dados possui correspondências é denominado _imbalanced_. Idealmente, suficiente representantes de cada classe nos dados de formação são preferenciais para ativar a diferenciação entre diferentes classes. Se uma classe for inferior a 10% dos dados, os dados considera-se que seja imbalanced. A classe underrepresented é chamada um _classe minority_.

Muitos problemas de PdM enfrentam essas imbalanced conjuntos de dados, onde uma classe é gravemente underrepresented em comparação comparados a outra classe, ou classes. Em algumas situações, a classe de minority poderá constituem apenas 0,001% dos total de pontos de dados. Não é exclusivo para PdM desequilíbrio de classe. Outros domínios onde falhas e anomalias são ocorrências raras enfrentam um problema semelhante, para obter exemplos, deteção de fraudes e intrusão de rede. Estas falhas Certifique-se exemplos de classe minority.

Com desequilíbrio de classe nos dados, desempenho de algoritmos de aprendizagem mais padrão for comprometido, uma vez que estes têm como objetivo para minimizar a taxa de erros geral. Para um conjunto de dados com 99% negativo e exemplos de positivo de 1%, um modelo pode ser apresentado com precisão de 99% por etiquetagem todas as instâncias como negativo. Mas o modelo incorrectamente vai classificar todos os exemplos positivos; por isso, mesmo que esteja a precisão elevada, o algoritmo não é útil. Por conseguinte, as métricas de avaliação convencionais, tais como _precisão geral na taxa de erros_ são insuficientes para imbalanced learning. Se deparam com imbalanced conjuntos de dados, outras métricas são utilizadas para avaliação do modelo:
- precisão
- Recuperar
- F1 pontuações
- Custo ajustado ROC (características de sistema operativo de recetor)

Para mais informações sobre estas métricas, consulte [modelo avaliação](#Model-evaluation).

No entanto, existem alguns métodos de que o ajudam a classe de resolver o problema de desequilíbrio. Dois dos principais são _amostragem técnicas_ e _custo learning confidencial_.

#### <a name="sampling-methods"></a>Métodos de amostragem
Imbalanced learning envolve a utilização de métodos para modificar o conjunto de dados de formação para um conjunto de dados com balanceamento de amostragem. Métodos de amostragem não são aplicadas ao conjunto de teste. Apesar de existirem várias técnicas de amostragem, a maioria das campanhas diretamente reencaminhar são _oversampling aleatório_ e _em amostragem_.

_Oversampling aleatório_ envolve a seleção de uma amostra aleatória de classe minority, replicar estes exemplos e adicioná-los ao conjunto de dados de preparação. Por conseguinte, o número de exemplos na classe minority é aumentado e, eventualmente, equilibrar o número de exemplos de diferentes classes. Uma desvantagem dos oversampling é que várias instâncias de determinados exemplos podem fazer com que o classificador de tornar-se demasiado específico, originando ajuste superior. O modelo pode mostrar precisão formação elevada, mas o respetivo desempenho nos dados de teste unseen pode ser inferior ao ideal.

Por outro lado, _aleatórias em amostragem_ é a seleção de uma amostra aleatória de uma classe de maioria e remover esses exemplos do conjunto de dados de preparação. No entanto, a remoção de exemplos da classe de maioria pode fazer com que o classificador de perder relativas à classe de maioria de conceitos importantes. _A amostragem híbrida_ onde classe minority é excessiva amostras e classe de maioria é em-amostragem ao mesmo tempo é outra abordagem viável.

Existem várias técnicas de amostragem sofisticadas. Técnica escolhida depende as propriedades de dados e resultados de experimentações iterativos por scientist a dados.

#### <a name="cost-sensitive-learning"></a>Custo learning confidencial
No PdM, falhas que constituem a classe de minority são mais importante que exemplos normais. Por isso, o foco é principalmente desempenho o algoritmo falhas. Incorretamente prever uma classe positiva como classe negativa pode custo mais do que o se vice-versa. Esta situação é normalmente denominada como unequal perda ou assimétrico custo de elementos incorrectamente classificar diferentes classes. O classificador ideal deve fornecer a exatidão da previsão elevada através de classe minority, sem comprometer a precisão para a classe de maioria.

Existem várias formas para alcançar esta balancear. Para mitigar o problema de perda de unequal, atribuir um custo de elevada a classificação incorrecta da classe minority e tentar minimizar o custo global. Como algoritmos _SVMs (suporte Vetor máquinas)_ adotar este método inerentemente, ao permitir que o custo de exemplos positivos e negativos especificado durante a preparação. Da mesma forma, os aumentos métodos tais como _elevada árvores de decisões_ normalmente mostrar um bom desempenho com dados imbalanced.

## <a name="model-evaluation"></a>Modelo de avaliação
A classificação incorrecta é um problema significativo para cenários de PdM onde o custo de alarmes false no negócio é elevado. Por exemplo, a decisão de manter uma aeronave em com base numa predição incorreta de falha do motor pode interromper as agendas e viajam planos. Colocar offline uma máquina de uma linha de assemblagem pode levar a perda de receitas. Por isso, avaliação de modelo com as métricas de desempenho direita contra dados novos de teste é fundamental.

Métricas de desempenho comuns utilizadas para avaliar PdM modelos são abordadas abaixo:

- [Precisão](https://en.wikipedia.org/wiki/Accuracy_and_precision) é a métrica mais popular utilizada para descrever o desempenho de um classificador. Mas precisão é confidencial para as distribuições de dados e é uma medida ineficaz para cenários com imbalanced conjuntos de dados. Outras métricas são utilizadas em vez disso. Ferramentas como [matriz confusão](https://en.wikipedia.org/wiki/Confusion_matrix) são utilizados para computação e pelo motivo sobre a precisão do modelo.
- [Precisão](https://en.wikipedia.org/wiki/Precision_and_recall) de PdM modelos relacionados com a taxa de alarmes false. Menor precisão do modelo, geralmente, corresponde a uma taxa superior do alarmes false.
- [Recuperar](https://en.wikipedia.org/wiki/Precision_and_recall) taxa indica o número de falhas no conjunto de teste corretamente foram identificado pelo modelo. Taxas de devolução de chamada superiores significam que o modelo for bem sucedido identificar as falhas de verdadeiras.
- [F1 pontuação](https://en.wikipedia.org/wiki/F1_score) é a média harmonic de precisão e devolução de chamada, com o respetivo valor vão entre 0 (Pior) para 1 (melhor).

Para classificação binária,
- [Recetor operativo curvas (ROC)](https://en.wikipedia.org/wiki/Receiver_operating_characteristic) também é uma métrica popular. Em curvas ROC, o desempenho do modelo é interpretado com base num fixo ponto de sistema operativo no ROC.
- Mas para problemas de PdM, _tabelas decile_ e _gráficos de comparação de precisão_ são mais informativo. Concentre-se apenas na classe positivo (falhas) e fornecer uma visão mais complexa do desempenho de algoritmo de curvas ROC.
  - _Tabelas de decile_ são criadas utilizando os exemplos de teste de uma ordem descendente de probabilidades de falha. Os exemplos ordenados, em seguida, são agrupados em deciles (10% de amostras com maior probabilidade, em seguida, 20%, 30% e assim sucessivamente). O /(random baseline) rácio (verdadeiro taxa positivo) para cada decile ajuda a calcular o desempenho de algoritmo em cada decile. A linha de base aleatória demora nos valores 0.1, 0,2 e assim sucessivamente.
  - [Comparação de precisão de gráficos](http://www2.cs.uregina.ca/~dbd/cs831/notes/lift_chart/lift_chart.html) desenhar decile Verdadeiro positivo taxa versus aleatória taxa positiva verdadeira para todos os deciles. Os primeiro deciles normalmente são o foco de resultados, uma vez que mostram os ganhos de maiores. Primeiro deciles também podem ser vistos como representativo para "em risco," quando utilizado para PdM.

## <a name="model-operationalization-for-predictive-maintenance"></a>Modelo operationalization para manutenção preventiva

O benefício do exercício de ciência de dados é realizados apenas quando o modelo treinado é efetuado operacional. Ou seja, o modelo tem de ser implementado para os sistemas de negócio para tornar as predições com base nos dados de novo, anteriormente unseen,  Os novos dados devem ter exatamente a _assinatura modelo_ do modelo treinado de duas formas:
- todas as funcionalidades tem de estar presentes em cada instância de lógica (diga uma linha numa tabela) dos dados de novo.
- os novos dados têm de ser previamente processados e cada uma das funcionalidades foi desenvolvido, exatamente da mesma forma como os dados de preparação.

O processo de acima é indicado na literature académicas e da indústria de várias formas. Mas as seguintes declarações significam a mesma coisa:
- _Pontuar novos dados_ utilizando o modelo
- _Aplicar o modelo_ para novos dados
- _Operacionalize_ o modelo
- _Implementar_ o modelo
- _Executar o modelo_ contra dados novos

Conforme indicado anteriormente, operationalization de modelo para PdM é diferente do respetivos elementos. Cenários que envolvam a deteção de anomalias e a deteção de falha, normalmente, implementam _online classificação_ (também denominado _em tempo real de classificação_). Aqui, o modelo _pontuações_ cada registo de entrada e devolve uma predição. Para a deteção de anomalias, a predição é uma indicação de que ocorreu uma anomalias (exemplo: SVM de uma classe). Para a deteção de falha, seria o tipo ou a classe de falha.

Em contrapartida, PdM envolve _a classificação de lote_. Para conformidade com a assinatura de modelo, as funcionalidades em novos dados tem de ser projetadas da mesma forma como os dados de preparação. Para grandes conjuntos de dados que é típicos para novos dados, as funcionalidades são agregadas ao longo de intervalos de tempo e pontuadas no batch. A classificação de lote é geralmente feita em sistemas distribuídos como [Spark](http://spark.apache.org/) ou [do Azure Batch](https://docs.microsoft.com/azure/batch/batch-api-basics). Existem alguns alternativas - inferior ao ideal – ambas:
- Transmissão em fluxo motores de dados suportam agregação ao longo do windows na memória. Por isso, foi argued que suportam a classificação online. Mas estes sistemas são adequados para dados no windows estreito da hora ou elementos dispersos vasta windows. Estes podem não dimensione bem para os dados em por vasta intervalos de tempo, conforme visto no PdM cenários.
- Se a classificação de lote não estiver disponível, a solução é para se adaptar a classificação online para processar novos dados em lotes pequenos cada vez.

## <a name="solution-templates-for-predictive-maintenance"></a>Modelos de solução de manutenção preditiva

A secção final deste guia fornece uma lista de modelos de solução PdM, tutoriais e experimentações implementadas no Azure. Estas aplicações PdM podem ser implementadas para uma subscrição do Azure dentro de minutos em alguns casos. Podem ser utilizadas como prova de conceito demonstrações, sandboxes para experimentar alternativas ou Aceleradores para implementações de produção real. Estes modelos estão localizados no [galeria do Azure AI](http://gallery.azure.ai) ou [Azure GitHub](https://github.com/Azure). Estes exemplos diferentes serão revertidos para este modelo de solução ao longo do tempo.

| # | Cargo | Descrição |
|--:|:------|-------------|
| 1 | [Manutenção preditiva do Azure Machine Learning exemplo](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance) |Exemplo de PdM para prever falha através de unidades de tempo de N seguintes. Este exemplo foi escrito como um projeto do Azure ML Workbench e é ideal para principiantes para PdM. [Documentação adicional](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/scenario-predictive-maintenance) relacionados com este exemplo.|
| 2 | [Modelo de solução da manutenção preditiva do Azure](https://github.com/Azure/AI-PredictiveMaintenance) | Uma arquitetura de ponto a ponto para demonstrar a vários cenários de PdM. Este modelo apresenta dois cenários: o primeiro é um nova caso de utilização de classificação de condição de falha em tempo real. O segundo cenário é simplesmente uma integração da solução [1] para este modelo de solução. -Demonstra como reutilizar a mesma infraestrutura implementada para adicionar outros cenários de novos ou existentes.|
| 3 | [Ligação avançada de aprendizagem para manutenção preventiva](https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance) | Bloco de notas do Azure com uma solução de demonstração de utilizar redes LSTM (memória de curto prazo longo) (uma classe de redes Neurais periódica) para manutenção preditiva, com um [blogue esta amostra](https://azure.microsoft.com/blog/deep-learning-for-predictive-maintenance).|
| 4 | [Guia de modelação preditiva manutenção R](https://gallery.azure.ai/Notebook/Predictive-Maintenance-Modelling-Guide-R-Notebook-1) | Guia de modelação PdM com scripts em R.|
| 5 | [Manutenção preditiva do Azure para aeroespacial](https://gallery.azure.ai/Solution/Predictive-Maintenance-for-Aerospace-1) | Um dos modelos de solução PdM primeiro com base no v 1.0 do Azure ML para manutenção de aeronave. Este guia teve este projeto. |
| 6 | [Toolkit de AI do Azure para o limite de IoT](https://github.com/Azure/ai-toolkit-iot-edge) | AI no limite IoT utilizando TensorFlow; Toolkit de pacotes profundas learning modelos nos contentores do Docker compatível com o Azure IoT Edge e expor esses modelos como REST APIs.
| 7 | [Manutenção preditiva do IoT do Azure](https://github.com/Azure/azure-iot-predictive-maintenance) | Azure IoT Suite PCS - solução pré-configurada. Modelo de PdM de manutenção das aeronaves com o IoT Suite. [Outro documento](https://docs.microsoft.com/azure/iot-suite/iot-suite-predictive-overview) e [instruções](https://docs.microsoft.com/azure/iot-suite/iot-suite-predictive-walkthrough) relacionados com o mesmo projeto. |
| 8 | [Modelo de manutenção preditiva, utilizando os serviços de R do SQL Server](https://gallery.azure.ai/Tutorial/Predictive-Maintenance-Template-with-SQL-Server-R-Services-1) | Demonstração de cenário de vida útil restantes com base nos serviços de R. |
| 9 | [Guia de modelação de manutenção preditiva](https://gallery.azure.ai/Collection/Predictive-Maintenance-Modelling-Guide-1) | Funcionalidade de conjunto de dados de manutenção das aeronaves foi desenvolvida utilizando R com [experimentações](https://gallery.azure.ai/Experiment/Predictive-Maintenance-Modelling-Guide-Experiment-1) e [conjuntos de dados](https://gallery.azure.ai/Experiment/Predictive-Maintenance-Modelling-Guide-Data-Sets-1) e [bloco de notas do Azure](https://gallery.azure.ai/Notebook/Predictive-Maintenance-Modelling-Guide-R-Notebook-1) e [experimentações](https://gallery.azure.ai/Experiment/Predictive-Maintenance-Step-1-of-3-data-preparation-and-feature-engineering-2)no v 1.0 do AzureML|

## <a name="training-resources-for-predictive-maintenance"></a>Recursos de formação para manutenção preventiva

O [percurso de aprendizagem do Azure AI para manutenção preventiva](https://github.com/Azure/AI-PredictiveMaintenance/blob/master/docs/azure-ai-learning-path-for-predictive-maintenance.md) fornece o material de formação para uma compreensão mais aprofundada sobre os conceitos e bibliotecas atrás os algoritmos e técnicas utilizadas no PdM problemas. 

O Microsoft Azure oferece conteúdo livre e formação sobre conceitos gerais de AI e prática.

| Recursos de formação  | Disponibilidade |
|:-------------------|--------------|
| [Programador de AI no Azure](http://azure.microsoft.com/training/learning-paths/azure-ai-developer) | Público |
| [Microsoft AI escola](http://aischool.microsoft.com/learning-paths) | Público |
| [Aprendizagem de AI do Azure a partir do GitHub](http://azure.github.io/learnanalytics/public) | Público |
| [LinkedIn Learning](http://www.linkedin.com/learning) | Público |
| [Microsoft AI Youtube Webinars](https://www.youtube.com/watch?v=NvrH7_KKzoM&t=4s) | Público |
| [Microsoft AI Mostrar](http://channel9.msdn.com/Shows/AI-Show) | Público |
| [LearnAI@MS](http://learnanalytics.microsoft.com) | Para parceiros Microsoft |
| [Rede de parceiro da Microsoft](http://learningportal.microsoft.com) | Para parceiros Microsoft |

Além disso, MOOCS livres (courses online para abrir em grande escala) no AI são oferecidas online por instituições académicas como Stanford e MIT e outras empresas educational.