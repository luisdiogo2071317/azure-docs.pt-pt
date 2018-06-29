---
title: 'O Azure Data Factory: Perguntas mais frequentes | Microsoft Docs'
description: Obtenha respostas às perguntas mais frequentes sobre o Azure Data Factory.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.assetid: 532dec5a-7261-4770-8f54-bfe527918058
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: shlo
ms.openlocfilehash: ebe8745db06113d0508d86554bf031a4235c8e44
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37045954"
---
# <a name="azure-data-factory-faq"></a>FAQ do Azure Data Factory
Este artigo fornece respostas às perguntas mais frequentes sobre o Azure Data Factory.  

## <a name="what-is-azure-data-factory"></a>O que é o Azure Data Factory? 
Fábrica de dados é um serviço de integração de dados completamente gerido, baseado na nuvem, que automatiza o movimento e a transformação de dados. Como uma fábrica que opera equipamento para transformar raw materiais em bens acabados, do Azure Data Factory orquestra os serviços existentes que recolhem dados não processados e transformação-los em informações prontas a utilizar. 

Ao utilizar o Azure Data Factory, pode criar fluxos de trabalho condicionada por dados para mover dados entre no local e na nuvem arquivos de dados. Pode processar e serviços, tais como o tempo de execução de integração do Azure HDInsight, o Azure Data Lake Analytics e o SQL Server Integration Services (SSIS) de computação de transformação de dados através da utilização. 

Com o Data Factory, pode executar o processamento de dados num serviço em nuvem baseado no Azure ou no seu próprio ambiente de computação personalizada alojada, tais como SSIS, SQL Server ou Oracle. Depois de criar um pipeline que executa a ação que é necessário, pode agendá-la para executar periodicamente (por exemplo, horário, diário ou semanal), agendamento de janela de tempo ou o pipeline de Acionador de uma ocorrência de evento. Para obter mais informações, veja [Introdução ao Azure Data Factory](introduction.md).

### <a name="control-flows-and-scale"></a>Fluxos de controlo e escala 
Para suportar a integração de diversos fluxos e padrões no armazém de dados modernas, fábrica de dados permite modelação de pipeline de dados flexíveis que inclui o fluxo de controlo total paradigmas incluindo condicional execução, de programação de ramificação nos pipelines de dados, e transmita explicitamente parâmetros dentro e entre estes fluxos. Fluxo de controlo também abrange transformar dados através da emissão de atividade de motores de execução externo e capacidades de fluxo de dados, incluindo o movimento de dados à escala, através da atividade de cópia.

Data Factory fornece liberdade para modelar qualquer estilo de fluxo que tem necessárias para a integração de dados e que podem ser distribuídos a pedido ou repetidamente numa agenda. Alguns fluxos comuns que este modelo permite são:   

- Fluxos de controlo:
    - Atividades de cadeia numa sequência dentro de um pipeline.
    - Atividades de sucursal dentro de um pipeline.
    - Parâmetros
        - Definir os parâmetros ao nível do pipeline e passar os argumentos ao invocar o pipeline a pedido ou a partir de um acionador.
        - As atividades podem consumir os argumentos transmitidos para o pipeline
    - Transmissão de estado personalizado
        - Os resultados das atividades, incluindo o estado, podem ser consumidos por uma atividade subsequente no pipeline.
    - Contentores de ciclo
        - For-each 
- Fluxos com base no acionador:
    - Podem ser acionadas pipelines a pedido ou tempo de relógio lateral.
- Fluxos de diferenças:
    - Utilizar parâmetros e definir o nível máximo para a cópia de diferenças ao mover as tabelas de dimensões ou de referência de um arquivo relacional, no local ou na nuvem, para carregar os dados para o lake. 

Para obter mais informações, consulte [Tutorial: controlar fluxos](tutorial-control-flow.md).

### <a name="transform-your-data-at-scale-with-code-free-pipelines"></a>Transformar os seus dados à escala com pipelines livre de código
A nova experiência de ferramentas baseadas no browser fornece pipeline sem código de criação e implementação com uma experiência de baseada na web moderna, interativa.

Para programadores de dados do elemento visual e engenheiros de dados, a IU da Web de ADF é o ambiente de estrutura sem código que irá utilizar para criar pipelines. Este está totalmente integrado com o Visual Studio Git de Online e fornece integração para CI/CD e desenvolvimento iterativo com opções de depuração.

### <a name="rich-cross-platform-sdks-for-advanced-users"></a>Avançada cruzada SDKs de plataforma para utilizadores avançados
Se for um utilizador avançado e à procura de uma interface programática, ADF V2 fornece um conjunto avançado de SDKs que pode ser utilizado para criar, gerir, monitorizar pipelines com a sua IDE favorito
1.  SDK Python
2.  CLI do PowerShell
3.  SDK do c# os utilizadores também podem tirar partido as APIs REST documentado a interface com ADF V2

### <a name="iterative-development-and-debugging-using-visual-tools"></a>Desenvolvimento iterativo e depuração utilizando ferramentas do visual
Ferramentas visual de fábrica de dados (ADF) do Azure permitem-lhe fazer iterativo desenvolvimento e depuração. Pode criar os seus pipelines e teste for executado utilizando a capacidade de depuração na tela pipeline sem ter de escrever uma única linha de código. Pode ver os resultados da sua execuções do teste na janela de saída da sua tela do pipeline. Depois do execução do teste for bem sucedida, pode adicionar mais atividades para o pipeline e continuar a depuração de forma iterativa. Também pode cancelar a execução do teste, assim que estiverem em curso. Não são necessárias para publicar as suas alterações para o serviço fábrica de dados antes de clicar em depuração. Esta ação é útil em cenários em que pretenda Certifique-se de que a nova adições ou alterações conforme esperado antes de atualizar os fluxos de trabalho de fábrica de dados no desenvolvimento, teste, ou prod ambientes. 

### <a name="deploy-ssis-packages-to-azure"></a>Implementar pacotes do SSIS no Azure 
Se pretender mover as cargas de trabalho SSIS, pode criar uma fábrica de dados e aprovisionar um tempo de execução de integração de SSIS do Azure. O tempo de execução de integração de SSIS do Azure é um cluster completamente gerido de VMs do Azure (nós) que estão dedicados para executar os pacotes SSIS na nuvem. Para obter instruções passo a passo, consulte o [pacotes SSIS implementar no Azure](tutorial-create-azure-ssis-runtime-portal.md) tutorial. 
 
### <a name="sdks"></a>SDKs
Se for um utilizador avançado e à procura de uma interface programática, ADF fornece um conjunto avançado de SDKs que pode utilizar para criar, gerir ou monitorizar pipelines utilizando o IDE favorito. Suporte de idiomas inclui .NET, o PowerShell, o Python e REST.

### <a name="monitoring"></a>Monitorização
Pode monitorizar as fábricas de dados através do PowerShell, SDK ou as ferramentas de monitorização Visual na interface de utilizador do browser. Pode monitorizar e gerir a pedido, acionamento com base e relógio orientadas por fluxos personalizados de forma eficaz e eficiente. Cancelar existentes tarefas, consulte falhas num instante, desagregar para obter mensagens de erro detalhadas e depurar os problemas tudo a partir de um painel único de transparência sem mudar ou navegar novamente e estabelecido entre ecrãs de contexto. 

### <a name="new-features-for-ssis-in-adf"></a>Novas funcionalidades para SSIS no ADF
Desde a versão pré-visualização pública inicial de 2017, o Data Factory adicionou as seguintes funcionalidades de SSIS:

-   Suporte para três configurações mais/variantes de BD SQL do Azure da base de dados () ao catálogo SSIS do anfitrião dos projetos/pacotes (SSISDB):
-   BD SQL do Azure com pontos finais do serviço de VNet
-   Instância gerida (MI)
-   Conjunto Elástico
-   Suporte para o Azure Resource Manager rede Virtual (VNet) em cima de uma VNet clássica que vão ser preteridas no futuro – Isto permite-lhe inserir/associação o tempo de execução de integração de SSIS do Azure (IR) para uma VNet que está configurada para a BD SQL do Azure com os pontos finais de serviço de VNet/MI / no local acesso a dados, consulte: https://docs.microsoft.com/en-us/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network 
-   Suporte para a autenticação do Azure Active Directory (AAD) em cima de autenticação do SQL Server para ligar ao seu SSISDB - Isto permite-lhe utilizar autenticação do AAD com seu ADF geridos serviço de identidade (MSI)
-   Suporte para colocar a sua própria licença do SQL Server no local para mais significativos beneficiar de reduções de custos significativas de opção do benefício de híbrida do Azure (AHB)
-   Suporte para o Enterprise Edition do Azure SSIS IR que permite-lhe utilizar funcionalidades avançadas/premium, a configuração personalizada para instalar componentes/extensões adicionais e 3rd ecossistema de terceiros, consulte: https://blogs.msdn.microsoft.com/ssis/2018/04/27/enterprise-edition-custom-setup-and-3rd-party-extensibility-for-ssis-in-adf/ 
-   Uma integração mais profunda do SSIS no ADF que lhe permite invocar/acionador primeira classe executar o pacote de SSIS atividades no ADF pipelines e agendá-los através do SSMS, consulte: https://blogs.msdn.microsoft.com/ssis/2018/05/23/modernize-and-extend-your-etlelt-workflows-with-ssis-activities-in-adf-pipelines/ 


## <a name="what-is-integration-runtime"></a>O que é o tempo de execução de integração?
Tempo de execução de integração é a infraestrutura de computação que é utilizada pelo Azure Data Factory para fornecer as seguintes capacidades de integração de dados em vários ambientes de rede:

- **Movimento de dados**: para o movimento de dados, o tempo de execução de integração move os dados entre os arquivos de dados de origem e destino, ao fornecer suporte para conectores incorporados, conversão do formato, o mapeamento de colunas e performant e transferência de dados dimensionável.
- **Emitir atividades**: de transformação, tempo de execução de integração fornecem a capacidade para executar nativamente pacotes SSIS.
- **Executar pacotes SSIS**: nativamente executa pacotes SSIS num ambiente gerido de computação do Azure. O IR também suporta a distribuição e monitorização das atividades de monitorização em execução em vários serviços de computação, com o Azure HDInsight, Azure Machine Learning, Base de Dados SQL do Azure, SQL Server, e mais.

Pode implementar uma ou várias instâncias do tempo de execução de integração conforme necessário para mover e transforme dados. Tempo de execução de integração pode executar numa rede pública do Azure ou numa rede privada (no local, rede Virtual do Azure ou nuvem privada virtual Amazon Web Services [VPC]). 

Para obter mais informações, veja [Integration runtime no Azure Data Factory](concepts-integration-runtime.md).

## <a name="what-is-the-limit-on-the-number-of-integration-runtimes"></a>O que é o limite no número de tempos de execução de integração?
Não há nenhum disco rígido limite no número de instâncias de tempo de execução de integração que pode ter uma fábrica de dados. No entanto, há um limite no número de núcleos VM que o tempo de execução de integração, pode utilizar por subscrição para a execução de pacotes SSIS. Para obter mais informações, consulte [limita a fábrica de dados](../azure-subscription-service-limits.md#data-factory-limits).

## <a name="what-are-the-top-level-concepts-of-azure-data-factory"></a>Quais são os conceitos de nível superior do Azure Data Factory?
Uma subscrição do Azure pode ter uma ou mais instâncias do Azure Data Factory (ou fábricas de dados). O Azure Data Factory contém quatro componentes chave que funcionam em conjunto como uma plataforma em que pode compor condicionada por dados fluxos de trabalho com os passos de mover e transforme dados.

### <a name="pipelines"></a>Pipelines
Uma fábrica de dados pode ter um ou mais pipelines. Um pipeline é um agrupamento lógico de atividades para efetuar uma unidade de trabalho. Em conjunto, as atividades num pipeline executam tarefas. Por exemplo, um pipeline pode conter um grupo de atividades que ingestão de dados de um blob do Azure e, em seguida, executar uma consulta do Hive num cluster do HDInsight para criar partições de dados. A vantagem é que pode utilizar um pipeline para gerir as atividades como um conjunto em vez de ter que gerir individualmente cada atividade. Em conjunto pode encadeiam as atividades num pipeline operá-los sequencialmente, ou pode operá-los de forma independente, em paralelo.

### <a name="activity"></a>Atividade
As atividades representam uma fase de processamento num pipeline. Por exemplo, pode utilizar um *cópia* atividade para copiar dados de um arquivo de dados para outro arquivo de dados. Da mesma forma, pode utilizar uma atividade do ramo de registo, que executa uma consulta do Hive num cluster do Azure HDInsight para transformar ou analisar os dados. O Data Factory suporta três tipos de atividades: atividades de movimento de dados, atividades de transformação de dados e atividades de controlo.

### <a name="datasets"></a>Conjuntos de dados
Os conjuntos de dados representam estruturas de dados nos arquivos de dados, que simplesmente apontam ou referenciam os dados que pretende utilizar nas suas atividades como entrada ou saída. 

### <a name="linked-services"></a>Serviços ligados
Os serviços ligados são muito semelhantes às cadeias de ligação, que definem as informações de ligação necessárias para que o Data Factory se possa ligar a recursos externos. Considerá-lo desta forma: um serviço ligado define a ligação à origem de dados e um conjunto de dados representa a estrutura dos dados. Por exemplo, um serviço ligado do Storage do Azure Especifica a cadeia de ligação para ligar à conta do Storage do Azure. E um conjunto de dados de Blobs do Azure Especifica a pasta que contém os dados e o contentor de blob.

Serviços ligados tem duas finalidades no Data Factory:

- Para representar um *arquivo de dados* que inclui, mas não está limitado a uma instância do SQL Server no local, uma instância de base de dados Oracle, uma partilha de ficheiros ou uma conta de armazenamento de Blobs do Azure. Para obter uma lista dos arquivos de dados suportadas, consulte [atividade de cópia no Azure Data Factory](copy-activity-overview.md).
- Para representar um *recurso de computação* que pode alojar a execução de uma atividade. Por exemplo, a atividade do ramo de registo do HDInsight é executada num cluster do HDInsight Hadoop. Para obter uma lista de atividades de transformação e ambientes de computação suportados, consulte [transformar dados no Azure Data Factory](transform-data.md).

### <a name="triggers"></a>Acionadores
Acionadores representam unidades de processamento que determinam quando é arrancou uma execução de pipeline. Existem diferentes tipos de acionadores para diferentes tipos de eventos. 

### <a name="pipeline-runs"></a>Execuções de pipeline
Um pipeline executar é uma instância de uma execução de pipeline. Normalmente, instanciar um pipeline executar transferindo argumentos para os parâmetros que são definidos no pipeline. Pode passar os argumentos manualmente ou na definição do acionador.

### <a name="parameters"></a>Parâmetros
Os parâmetros são pares chave-valor numa configuração só de leitura. Definir os parâmetros de um pipeline e passar os argumentos para os parâmetros definidos durante a execução de um contexto de execução. O contexto de execução é criado por um acionador ou a partir de um pipeline que executar manualmente. As atividades dentro do pipeline consomem os valores dos parâmetros.

Um conjunto de dados é um parâmetro com tipo seguro e uma entidade que pode reutilizar ou de referência. Uma atividade pode referenciar conjuntos de dados e pode consumir as propriedades que são definidas na definição do conjunto de dados.

Um serviço ligado também é um parâmetro com tipo seguro que contém informações de ligação para um arquivo de dados ou num ambiente de computação. Também é uma entidade que pode reutilizar ou de referência.

### <a name="control-flows"></a>Fluxos de controlo
Atividades de pipeline, que incluem o encadeamento atividades numa sequência, ramificação, parâmetros definidos ao nível do pipeline, orquestram a fluxos de controlo e argumentos que passar como invocam o pipeline a pedido ou a partir de um acionador. Controlo fluxos também incluem Estado personalizadas a transmitir e ciclo contentores (ou seja, para cada iterators).


Para obter mais informações sobre os conceitos do Data Factory, veja os artigos seguintes:

- [Conjunto de dados e serviços ligados](concepts-datasets-linked-services.md)
- [Pipelines e atividades](concepts-pipelines-activities.md)
- [Integration runtime](concepts-integration-runtime.md) (Runtime de integração)

## <a name="what-is-the-pricing-model-for-data-factory"></a>O que é o modelo de preços do Data Factory?
Para o Azure Data Factory detalhes de preços, consulte [detalhes de preços do Data Factory](https://azure.microsoft.com/pricing/details/data-factory/).

## <a name="how-can-i-stay-up-to-date-with-information-about-data-factory"></a>Como posso permaneça atualizado com informações sobre o Data Factory?
Para obter as informações mais atualizadas sobre o Azure Data Factory, vá para os seguintes sites:

- [Blogue](https://azure.microsoft.com/blog/tag/azure-data-factory/)
- [Home page da documentação](/azure/data-factory)
- [Home page do produto](https://azure.microsoft.com/services/data-factory/)

## <a name="technical-deep-dive"></a>Descrição detalhada da técnica 

### <a name="how-can-i-schedule-a-pipeline"></a>Como agendar a um pipeline? 
Pode utilizar o acionador de programador ou acionador de janela de tempo para agendar um pipeline. O acionador utiliza uma agenda de calendário do relógio de firewall e pode utilizá-la para agendar pipelines periodicamente ou através da utilização baseada em Calendário periódica padrões (por exemplo, semanalmente nos segundas às 18: 00 e às quintas-em 9 PM). Para mais informações, consulte [Execuções de pipelines e acionadores](concepts-pipeline-execution-triggers.md).

### <a name="can-i-pass-parameters-to-a-pipeline-run"></a>Pode passar parâmetros para uma execução de pipeline?
Sim, os parâmetros são um conceito de primeira classe, nível superior na ADF. Pode definir os parâmetros ao nível do pipeline e passar os argumentos como executar o pipeline de executar a pedido ou utilizando um acionador.  

### <a name="can-i-define-default-values-for-the-pipeline-parameters"></a>Pode definir os valores predefinidos para os parâmetros de pipeline? 
Sim. Pode definir os valores predefinidos para os parâmetros nos pipelines. 

### <a name="can-an-activity-in-a-pipeline-consume-arguments-that-are-passed-to-a-pipeline-run"></a>Uma atividade num pipeline pode consumir a argumentos que são transmitidos para um pipeline executar? 
Sim. Cada atividade no pipeline de pode consumir o valor do parâmetro que foi transmitido para o pipeline e execute com a `@parameter` construir. 

### <a name="can-an-activity-output-property-be-consumed-in-another-activity"></a>Uma propriedade de saída da atividade pode ser utilizada noutra atividade? 
Sim. Uma saída da atividade pode ser utilizada numa atividade subsequente com a `@activity` construir.
 
### <a name="how-do-i-gracefully-handle-null-values-in-an-activity-output"></a>Como posso lidar graciosamente com valores nulos existentes numa saída da atividade? 
Pode utilizar o `@coalesce` construir em expressões de lidar graciosamente com valores nulos. 

## <a name="next-steps"></a>Passos Seguintes
Para obter instruções passo a passo Criar uma fábrica de dados, consulte os seguintes tutoriais:

- [Início rápido: Criar uma fábrica de dados](quickstart-create-data-factory-dot-net.md)
- [Tutorial: Copiar dados na nuvem](tutorial-copy-data-dot-net.md)
