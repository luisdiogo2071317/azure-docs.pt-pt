---
title: Descrição geral do Azure Time Series Insights | Documentos da Microsoft
description: Descrição geral do Azure Time Series Insights
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: anshan
ms.workload: big-data
ms.topic: overview
ms.date: 12/05/2018
ms.openlocfilehash: 9843a01ed3c96b362e17718e9035c378da6c3cf2
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2018
ms.locfileid: "53083398"
---
# <a name="azure-time-series-insights-overview"></a>Descrição geral do Azure Time Series Insights

O Azure Time Series Insights (TSI) é um ponto-a-ponto plataforma-como-serviço para ingerir, processar, armazenar e consultar dados de dimensionamento IoT contextualizados altamente otimizado de série de tempo, que é ideais para exploração de dados ad-hoc, bem como a análise operacional. O Azure TSI é uma oferta que satisfazem amplo precisa de implementações de IoT industriais de serviço exclusivamente extensível e personalizado.

## <a name="video"></a>Vídeo

Neste vídeo, fornecemos uma visão geral do Azure Time Series Insights (pré-visualização), uma plataforma de análise de IoT com base na cloud.

> [!VIDEO https://channel9.msdn.com/Shows/Internet-of-Things-Show/Azure-Time-Series-Insights-e2e-solution-for-industrial-IoT-analytics/player]

## <a name="defining-iot-data"></a>Definir os dados de IoT

Dados de IoT são todos os dados "industriais" disponíveis em organizações com utilização intensiva de recurso. Dados de IoT, muitas vezes, são altamente não estruturados, pois elas enviadas por um diversos recursos que registam medidas bastante desnecessárias, como temperatura, movimento, humidade. Além disso, estes fluxos de dados com freqüência são caracterizados por intervalos significativos, mensagens danificadas e leituras FALSO. Dados a partir desses fluxos tem de ser limpos antes de ocorre qualquer análise. Dados de IoT, muitas vezes, são significativos apenas no contexto de entradas de dados adicionais de originais, como CRM ou ERP) ou origens de dados de terceiros (por exemplo, informações sobre o clima ou localização).

Como resultado, apenas uma fração negligenciável desses dados, é utilizada para fins operacionais e empresariais. Esses dados fornecem informações consistentes, abrangentes, atuais e corretas para análise e relatórios de negócios. Ativar recolhidos IoT dados em informações acionáveis, portanto, requerem algumas capacidades principais:

* Processamento de dados para limpar, filtrar, fazer a interpolação, transformar e preparar dados para análise
* Estrutura para navegar e compreender os dados (para normalizar e contextualizar os dados)
* Armazenamento rentável para um período de retenção infinito / longo de décadas, vale a pena de dados brutos, bem como (derivadas) processados

Fluxo de dados de IoT típico pode ser descrito da seguinte forma:

  ![Fluxo de dados de IoT][1]

## <a name="azure-time-series-insights-for-industrial-iot"></a>O Azure Time Series Insights para o industrial IoT

O atual cenário de IoT é diversificado. Ele inclui os clientes que abrange o fabrico, automóvel, energia, utilitários, edifícios inteligentes e setores de consultoria. Os cenários incluem a exploração de dados ad-hoc em que a forma dos dados é desconhecido, bem como operacional análise ao longo de dados (explicitamente modelados) esquematizados a eficiência operacional da unidade. Esses cenários normalmente existem lado a lado e oferecer suporte a diferentes casos de utilização. Capacidades da plataforma, como em várias camadas de armazenamento (semiativos e amovíveis), a capacidade de armazenar o valor de décadas de dados de séries de tempo e a capacidade de explicitamente modelar e otimizar as consultas para asset com base em informações operacionais estão se tornando da chave para o sucesso de as empresas de IoT industriais e seus revolução digital.

Azure TSI é uma oferta de plataforma-como-um-serviço de ponto a ponto abrangente para de exploração de dados de IoT, bem como das informações operacionais. O TSI oferece um serviço cloud totalmente gerido para análise de dados de séries de tempo de escala de IoT.

Os clientes podem armazenar dados não processados num arquivo de esquema, dentro da memória. Os clientes, em seguida, podem executar consultas ad-hoc interativa através de um mecanismo de consulta distribuída e tirar partido do nosso usuário avançada de API experiência para visualizar milhares de milhões de eventos em segundos. Saiba mais sobre nossos [capacidades de exploração de dados](./time-series-insights-overview.md).

O TSI também oferece recursos de informações operacionais atualmente em pré-visualização. Em conjunto com a exploração interativa de dados e informações operacionais, o TSI permite aos clientes obter mais valor dos dados recolhidos a partir de recursos de IoT. Especificamente, a oferta de pré-visualização suporta as principais capacidades seguintes:

* A dimensionável, desempenho e armazenamento de dados de série de tempo com otimização de custos que permite uma solução de IoT com base na cloud para o valor dos anos de tendência de dados de séries de tempo em segundos.
* Suporte de modelo semântico para descrever o domínio e os metadados associados aos sinais derivados e não derivados de ativos e dispositivos.
* Experiência do usuário que combina informações de dados com base no ativo com análises de dados do ad-hoc poderosas, para impulsionar inteligência comercial e operacional
* Integração com avançados de aprendizagem automática e ferramentas de análise, como o Azure Databricks, Apache Spark, Azure Machine Learning, blocos de notas do Jupyter, Power BI, etc., para ajudar os clientes a resolver desafios de dados de séries de tempo e impulsionar a eficiência operacional.

Em conjunto exploração de dados e informações operacional são oferecidas com uma simple pay as you go modelo para processamento de dados, armazenamento e a consulta de preços, deste modo, proporciona aos clientes um modelo muito mais escalonável para se adequar às suas necessidades empresariais em evolução.

Segue-se um diagrama de fluxo de dados de alto nível que representa os recursos atualizados:

  ![Principais capacidades][2]

Com a introdução dessas capacidades essenciais de IoT industriais, o Azure TSI fornece as seguintes grandes vantagens:

| | |
| ---| ---|
| **Em várias camadas de armazenamento para dados de séries de tempo de escala de IoT** | Com um pipeline de processamento de dados comuns para a ingestão de dados, os clientes têm os dados de capacidade do arquivo no armazenamento de acesso pouco frequente para consultas interativas e/ou armazenamento de esporádico para armazenar grandes volumes de dados. Os clientes podem tirar partido de alto desempenho, com base em asset [consultas](./time-series-insights-update-tsq.md). |
| **Modelo de série de tempo para o contexto de telemetria não processados e derivar informações baseadas no elemento** | Os clientes podem a contextualizar os dados de telemetria não processados com o descritivo [modelo de série de tempo](./time-series-insights-update-tsm.md) e derivar as informações operacionais avançada com otimização de desempenho e custo altamente consultas com base no dispositivo. |
| **Integração total com outras soluções de dados** | Uma vez que são de dados no Azure TSI [armazenados](./time-series-insights-update-storage-ingress.md) nos ficheiros de Apache Parquet aberto, os clientes podem integrar facilmente com outras soluções de dados (primeira ou de terceiros) para cenários de ponto-a-ponto, incluindo a inteligência comercial, advanced máquina Learning, Análise Preditiva, etc. |
| **Perto de exploração de dados em tempo real** | [O Explorador do Azure TSI](./time-series-insights-update-explorer.md) experiência de utilizador fornece a visualização para todos os dados de transmissão em fluxo através do pipeline de ingestão. Pouco tempo depois de ligar uma origem de evento, os clientes podem ver, explorar e consultar dados de eventos para validar se um dispositivo é emitir dados conforme esperado e monitorização um recurso de IoT para o estado de funcionamento, produtividade e eficácia global. |
| **Causa análise e deteção de anomalias** | [O Explorador do Azure TSI](./time-series-insights-update-explorer.md) suporta vistas padrão e o ponto de vista para realizar e guardar várias etapas, a causa raiz, a análise. Em combinação com o Azure Stream Analytics, os clientes podem utilizar o Azure TSI para detetar alertas e anomalias em tempo quase real. |
| **Criar aplicativos personalizados na plataforma do TSI** | Azure TSI suporta o [JavaScript SDK](./tutorial-explore-js-client-lib.md). O SDK fornece controles avançados e acesso simplificado às consultas. O SDK permite que os clientes podem criar aplicações IoT personalizadas com base no Azure TSI para se adequar às suas necessidades empresariais específicas. Os clientes também podem utilizar o Azure TSI [APIs de consulta](./time-series-insights-update-tsq.md) diretamente aos dados da unidade em aplicações de IoT personalizadas. |

## <a name="next-steps"></a>Passos Seguintes

Está pronto para começar a utilizar com o Azure TSI (pré-visualização):

> [!div class="nextstepaction"]
> [Leia o guia de início rápido](./time-series-insights-update-quickstart.md)

Saiba mais sobre casos de utilização:

> [!div class="nextstepaction"]
> [Casos de utilização do Azure TSI](./time-series-insights-update-use-cases.md)

<!-- Images -->
[1]: media/v2-update-overview/overview_one.png
[2]: media/v2-update-overview/overview_two.png