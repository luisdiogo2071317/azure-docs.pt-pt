---
title: O Machine Learning Studio perguntas mais frequentes (FAQ)
titleSuffix: Azure Machine Learning Studio
description: 'Azure Machine Learning Studio: Perguntas frequentes sobre faturação, capacidades e limitações de um serviço cloud de modelação preditiva simplificada.'
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: ericlicoding
ms.author: amlstudiodocs
ms.custom: seodec18
ms.date: 06/02/2017
ms.openlocfilehash: 8c381ca02dcfb194b2807735415ef894d72f74fa
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/31/2019
ms.locfileid: "55462284"
---
# <a name="azure-machine-learning-studio-faq-capabilities-and-limitations"></a>Do Azure Machine Learning Studio FAQ: capacidades e limitações
Seguem-se algumas perguntas mais frequentes (FAQ) e as respostas correspondentes sobre o Azure Machine Learning, um serviço cloud para desenvolver modelos preditivos e operacionalizar soluções através de serviços Web. 

## <a name="general-questions"></a>Perguntas gerais
**O que é o Machine Learning Studio?**

Machine Learning Studio é um ambiente de tela de arrastar e soltar que acessar usando um navegador da web. O Machine Learning Studio aloja uma palete de módulos numa interface de composição visual que o ajuda criar um fluxo de trabalho de ciência de dados completo sob a forma de uma experimentação.

Para obter mais informações sobre o Machine Learning Studio, consulte [O que é o Machine Learning Studio?](what-is-ml-studio.md)

**O que é o serviço de API do Machine Learning?**

O serviço de API do Machine Learning permite-lhe implementar modelos preditivos, tal como os incorporados no Machine Learning Studio, como serviços Web dimensionáveis e tolerantes a falhas. Os serviços Web criados pelo serviço de API do Machine Learning são APIs REST que disponibilizam uma interface para comunicação entre aplicações externos e os seus modelos analíticos preditivos.

Para obter mais informações, veja [How to consume an Azure Machine Learning Web service](consume-web-services.md) (Como consumir um serviço Web do Azure Machine Learning).

**Onde estão apresentados os meus serviços Web clássicos? Onde estão apresentados os meus novos serviços Web (baseados no Azure Resource Manager)?**

Os serviços Web criados com o modelo de implementação Clássica e os serviços Web criados com o novo modelo de implementação Azure Resource Manager são apresentados no portal dos [Serviços Web do Microsoft Azure Machine Learning](https://services.azureml.net/).

Os serviços Web clássicos também são apresentados em [Machine Learning Studio](http://studio.azureml.net), no separador **Serviços Web**.

## <a name="azure-machine-learning-questions"></a>Perguntas sobre o Azure Machine Learning
**O que são os Serviços Web do Azure Machine Learning?**

Os serviços Web Machine Learning proporcionam uma interface entre uma aplicação e um modelo de classificação de fluxo de trabalho do Machine Learning. As aplicações externas podem utilizar o Azure Machine Learning para comunicar com um modelo de classificação de fluxo de trabalho do Machine Learning. Uma chamada para o serviço Web do Machine Learning devolve resultados de predição para uma aplicação externa. Para fazer uma chamada para um serviço Web, tem de transmitir uma chave de API que foi criada quando implementou esse serviço. Os serviços Web Machine Learning baseiam-se em REST, uma opção de arquitetura popular para projetos de programação Web.

O Azure Machine Learning tem dois tipos de serviços Web:

* Serviço de solicitação-resposta (RRS): Uma baixa latência, o serviço altamente dimensionável, que fornece uma interface para os modelos sem monitorização de estado criados e implementados através da utilização de Machine Learning Studio.
* Serviço de execução de lote (BES): Um serviço assíncrono que pontua um lote de registos de dados.

Existem várias formas de utilizar a API REST e aceder ao serviço Web. Por exemplo, pode escrever uma aplicação em C#, R ou Python com o código de exemplo que foi gerado para si quando implementou o serviço Web.

O código de exemplo está disponível:
- Na página Consumo do serviço Web no portal dos Serviços Web do Azure Machine Learning
- Na página Ajuda da API no dashboard do serviço Web no Machine Learning Studio

Também pode utilizar o livro de exemplo do Microsoft Excel que é criado para si e que está disponível no dashboard do serviço Web no Machine Learning Studio.

**Quais são as principais atualizações ao Azure Machine Learning?**

Para saber quais são as atualizações mais recentes, veja [What's new in Azure Machine Learning (Novidades no Azure Machine Learning)](../../active-directory/fundamentals/whats-new.md).

## <a name="import-and-export-data-for-machine-learning"></a>Importar e exportar dados para o Machine Learning
**Que origens de dados são suportadas pelo Machine Learning?**

Pode transferir dados para uma experimentação do Machine Learning Studio de três formas:

- Carregar um ficheiro local como um conjunto de dados
- Utilizar um módulo para importar dados a partir de serviços de dados na cloud
- Importar um conjunto de dados guardado a partir do outra experimentação

Para saber mais sobre os formatos de ficheiro suportados, veja [Import training data into Machine Learning Studio (Importar dados de formação para o Machine Learning Studio)](import-data.md).

### <a id="ModuleLimit"></a>Qual o tamanho máximo do conjunto de dados para os meus módulos?
Os módulos no Machine Learning Studio suportam conjuntos de dados até 10 GB de dados numéricos em casos de utilização comum. Se um módulo precisar de mais do que uma entrada, 10 GB é o valor do tamanho total de todas as entradas de dados. Também pode utilizar consultas do Hive ou da Base de Dados SQL do Azure para extrair exemplos de conjuntos de dados maiores ou utilizar o pré-processamento de Aprendizagem por Contagens antes da ingestão.  

Os seguintes tipos de dados podem ser expandidos para conjuntos de dados de maiores dimensões durante a normalização da funcionalidade, estando limitados a menos de 10 GB:

* Dispersos
* Categórico
* Cadeias
* Dados binários

Os seguintes módulos estão limitados a conjuntos de dados inferiores a 10 GB:

* Módulos de recomendador
* Módulo Synthetic Minority Oversampling Technique (SMOTE)
* Módulos de script: R, Python, SQL
* Módulos onde o tamanho dos dados de saída pode ser superior ao tamanho dos dados de entrada, tais como Associação ou Hashing de Funcionalidade
* Validação Cruzada, Hiperparâmetros do Modelo de Otimização, Regressão Ordinal e Multicasse “One-vs-All”, quando o número de iterações é muito grande

### <a id="UploadLimit"></a>Quais são os limites para o carregamento de dados?
Para conjuntos de dados superiores a alguns GB, carregue os dados para o Armazenamento do Azure ou para a Base de Dados SQL do Azure ou utilize o Azure HDInsight em vez de os carregar diretamente a partir de um ficheiro local.

**Posso ler dados do Amazon S3?**

Se tiver uma pequena quantidade de dados e quiser expô-los através de um URL HTTP, pode utilizar o módulo [Importar Dados][import-data]. Para quantidades de dados maiores, transfira primeiro para o Armazenamento do Azure e, depois, utilize o módulo [Importar dados][import-data] para incluí-los na sua experimentação.
<!--

<SEE CLOUD DS PROCESS>
-->

**Existe uma capacidade de entrada de imagem incorporada?**

Pode saber mais sobre a capacidade de entrada de imagem na referência [Importar imagens][image-reader].

## <a name="modules"></a>Módulos
**O algoritmo, a origem de dados, o formato de dados ou a operação de transformação de dados que procuro não está no Azure Machine Learning Studio. Quais são as minhas opções?**

Pode visitar o [fórum de comentários dos utilizadores](https://go.microsoft.com/fwlink/?LinkId=404231) para ver os pedidos de funcionalidades que estamos a acompanhar. Adicione o seu voto a um pedido se já tiver sido pedida uma funcionalidade que procura. Se a capacidade que está à procura não existir, crie um pedido novo. Também pode ver o estado do seu pedido neste fórum. Estamos a controlar com mais atenção esta lista e a atualizar frequentemente o estado de disponibilidade das funcionalidades. Além disso, pode utilizar o suporte incorporado para R e Python para criar transformações personalizadas conforme necessário.

**Posso usar o meu código existente no Machine Learning Studio?**

Sim, pode usar o seu código existente para o R ou Python no Machine Learning Studio, executá-lo na mesma experiência com os formandos do Azure Machine Learning Studio e implementar a solução como um serviço Web através do Azure Machine Learning. Para obter mais informações, consulte [Expandir a sua experiência com R](extend-your-experiment-with-r.md) e [Executar os scripts do Machine Learning Python no Azure Machine Learning Studio](execute-python-scripts.md).

**É possível utilizar algo semelhante a [PMML](http://en.wikipedia.org/wiki/Predictive_Model_Markup_Language) para definir um modelo?**

Não, a Predictive Model Markup Language (PMML) não é suportada. Pode utilizar código R e Python personalizado para definir um módulo.

**Quantos módulos posso executar em paralelo na minha experiência?**  

Pode executar até quatro módulos em paralelo numa experimentação.

## <a name="data-processing"></a>Processamento de dados
**Existe uma capacidade para visualizar dados (para além das visualizações de R) interativamente na experiência?**

Clique na saída de um módulo para visualizar os dados e obter estatísticas.

**Quando pré-visualizo resultados ou dados num browser, o número de linhas e colunas é limitado. Porquê?**

Uma vez que é possível que sejam enviadas grandes quantidades de dados para o browser, o tamanho dos dados é limitado, para evitar que o Machine Learning fique mais lento. Para visualizar todos os dados/resultados, é melhor transferir os dados e utilizar o Excel ou outra ferramenta.

## <a name="algorithms"></a>Algoritmos
**Que algoritmos existentes são suportados no Machine Learning Studio?**

O Machine Learning Studio dispõe de algoritmos avançados, como Árvores de Decisão Escaláveis, Sistemas de Recomendação Bayesianos, Redes Neurais Profundas e Selvas de Decisão desenvolvidas junto da Microsoft Research. Estão também incluídos pacotes de código aberto e dimensionáveis de machine learning, como o Vowpal Wabbit. O Machine Learning Studio suporta algoritmos de machine learning para classificação de várias classes e binária, regressão e clustering. Consulte a lista completa de [Módulos do Machine Learning][machine-learning-modules].

**Sugere automaticamente o algoritmo correto do Machine Learning a utilizar com os meus dados?**

Não, mas o Machine Learning Studio proporciona várias formas de comparar os resultados de cada algoritmo para determinar o algoritmo certo para o seu problema.

**Segue alguma diretriz para escolher um algoritmo em vez de outro relativamente aos algoritmos fornecidos?**

Veja [How to choose an algorithm (Como escolher um algoritmo)](algorithm-choice.md).

**Os algoritmos fornecidos são escritos em R ou Python?**

Não, estes algoritmos são principalmente escritos em linguagens compiladas, para permitir um desempenho melhor.

**São fornecidos alguns detalhes dos algoritmos?**

A documentação disponibiliza algumas informações acerca dos algoritmos e são descritos os parâmetros de otimização para melhorar o algoritmo.  

**A formação online é suportada?**

Não, atualmente só é suportada a reparametrização programática.

**Posso visualizar as camadas de um Modelo de Rede Neural com o módulo incorporado?**

Não.

**Posso criar os meus próprios módulos em C# ou noutra linguagem?**

Atualmente, só pode utilizar R para criar novos módulos personalizados.

## <a name="r-module"></a>Módulo de R
**Os pacotes R estão disponíveis no Machine Learning Studio?**

O Machine Learning Studio suporta mais de 400 pacotes CRAN R hoje e esta é a [lista atual](http://az754797.vo.msecnd.net/docs/RPackages.xlsx) de todos os pacotes incluídos. Além disso, veja [Extend your experiment with R (Expandir a sua experiência com R) ](extend-your-experiment-with-r.md) para saber como obter esta lista manualmente. Se o pacote que pretende não estiver nesta lista, indique-o no [fórum de comentários dos utilizadores](https://go.microsoft.com/fwlink/?LinkId=404231).

**É possível criar um módulo R personalizado?**

Sim, consulte [Autor dos módulos R personalizados no Azure Machine Learning](custom-r-modules.md) para obter mais informações.

**Existe um ambiente REPL para R?**

Não, não existe nenhum ambiente Read-Eval-Print-Loop (REPL) para R no Studio.

## <a name="python-module"></a>Módulo de Python
**É possível criar um módulo Python personalizado?**

Atualmente não, mas pode utilizar um ou mais módulos [Executar script do Python][python] para obter o mesmo resultado.

**Existe um ambiente REPL para Python?**

Pode utilizar os Jupyter Notebooks no Machine Learning Studio. Para obter mais informações, consulte [Introducing Jupyter Notebooks in Azure Machine Learning Studio (Introdução do Bloco de Notas do Jupyter no Azure Machine Learning Studio)](http://blogs.technet.com/b/machinelearning/archive/2015/07/24/introducing-jupyter-notebooks-in-azure-ml-studio.aspx).

## <a name="web-service"></a>Serviço Web

**Como posso reparametrizar os modelos do Azure Machine Learning de forma programática?**

Utilize as APIs de reparametrização. Para obter mais informações, veja [Retrain Machine Learning models programmatically (Reparametrizar modelos do Machine Learning programaticamente)](retrain-models-programmatically.md). O código de exemplo também está disponível em [Microsoft Azure Machine Learning Retraining Demo (Demonstração de Reparametrização do Microsoft Azure Machine)](https://azuremlretrain.codeplex.com/).

**Posso implementar o modelo localmente ou numa aplicação que não tenha ligação à Internet?**

Não.

**Existe uma latência de linha de base que seja esperada para todos os serviços Web?**

Veja os [Azure subscription limits (limites das subscrições do Azure)](../../azure-subscription-service-limits.md)

**Quando deveria querer executar o meu modelo preditivo como um serviço de Execução de Lotes em comparação com um serviço de Resposta a um Pedido?**

O serviço de resposta a um pedido (RRS) é um serviço Web de latência baixa e alta escala que é utilizado para fornecer uma interface para modelos sem monitorização de estado que são criados e implementados a partir do ambiente de experiência. O Serviço de Execução de Lotes (BES) é um serviço que classifica, de forma assíncrona, um lote de registos de dados. A entrada para BES é como a entrada de dados que o RRS utiliza. A principal diferença é que BES lê um bloco de registos a partir de várias origens, como o Armazenamento de Blobs do Azure, o Armazenamento de Tabelas do Azure, a Base de Dados SQL do Azure, o HDInsight (consulta do Hive) e origens HTTP. Para obter mais informações, veja [How to consume an Azure Machine Learning Web service](consume-web-services.md) (Como consumir um serviço Web do Azure Machine Learning).

**Como posso atualizar o modelo para o serviço Web implementado?**

Para atualizar um modelo preditivo para um serviço já implementado, modifique e volte a executar a experimentação que utilizou para criar e guardar o modelo preparado. Assim que tiver uma nova versão do modelo preparado disponível, o Machine Learning Studio pergunta-lhe se quer atualizar o seu serviço Web. Para obter detalhes sobre como atualizar um serviço Web implementado, veja [Deploy a Machine Learning web service (Implementar um serviço Web Machine Learning)](publish-a-machine-learning-web-service.md).

Também pode utilizar as APIs para a reparametrização.
Para obter mais informações, veja [Retrain Machine Learning models programmatically (Reparametrizar modelos do Machine Learning programaticamente)](retrain-models-programmatically.md). O código de exemplo também está disponível em [Microsoft Azure Machine Learning Retraining Demo (Demonstração de Reparametrização do Microsoft Azure Machine)](https://azuremlretrain.codeplex.com/).

**Como posso monitorizar o meu serviço Web implementado na produção?**

Depois de implementar um modelo preditivo, pode monitorizá-lo a partir do portal dos Serviços Web do Azure Machine Learning. Cada serviço implementado tem o seu próprio dashboard onde pode ver informações de monitorização relativamente a esse serviço. Para obter mais informações sobre como gerir os seus serviços Web implementados, veja [Manage a Web service using the Azure Machine Learning Web Services portal (Gerir um serviço Web com o portal dos Serviços Web do Azure Machine Learning)](manage-new-webservice.md) e [Manage an Azure Machine Learning workspace (Gerir uma área de trabalho do Azure Machine Learning)](manage-workspace.md).

**Existe um local onde posso ver a saída da minha RRS/BES?**

Para RRS, vê normalmente o resultado na resposta do serviço Web. Também pode escrevê-lo no Armazenamento de Blobs do Azure. Para BES, o resultado é escrito num blob por predefinição. Também pode utilizar o módulo [Exportar Dados][export-data] para escrever a saída numa base de dados ou numa tabela.

**Só posso criar serviços Web a partir de modelos criados no Machine Learning Studio?**

Não, também pode utilizar o Jupyter Notebooks e o RStudio para criar serviços Web diretamente.

**Onde posso encontrar informações sobre códigos de erro?**

Consulte [Códigos de erro do módulo Machine Learning](https://msdn.microsoft.com/library/azure/dn905910.aspx) para obter uma lista de códigos de erro e descrições.

**O que é a escalabilidade do serviço Web?**

Atualmente, o ponto final predefinido está aprovisionado com 20 pedidos de RRS em simultâneo por ponto final. Pode dimensioná-lo para 200 pedidos em simultâneo por ponto final e dimensionar cada serviço Web para 10 000 pontos finais por serviço Web, conforme descrito em [Scaling a Web Service (Dimensionar um Serviço Web)](scaling-webservice.md). Para BES, cada ponto final pode processar 40 pedidos de uma só vez e os pedidos adicionais que ultrapassem os 40 são colocados em fila de espera. Estes pedidos em fila de espera são executados automaticamente à medida que a fila avança.

**As tarefas de R são distribuídas pelos nós?**

Não.  

**Que quantidade de dados posso utilizar para formação?**

Os módulos no Machine Learning Studio suportam conjuntos de dados até 10 GB de dados numéricos em casos de utilização comum. Se um módulo precisar de mais do que uma entrada, o tamanho total de todas as entradas de dados são 10 GB. Também pode extrair exemplos de conjuntos de dados maiores através de consultas de Hive e da Base de Dados SQL do Azure ou do pré-processamento com os módulos [Aprendizagem por Contagens][counts] antes da ingestão.  

Os seguintes tipos de dados podem ser expandidos para conjuntos de dados de maiores dimensões durante a normalização da funcionalidade, estando limitados a menos de 10 GB:

* Dispersos
* Categórico
* Cadeias
* Dados binários

Os seguintes módulos estão limitados a conjuntos de dados inferiores a 10 GB:

* Módulos de recomendador
* Módulo Synthetic Minority Oversampling Technique (SMOTE)
* Módulos de script: R, Python, SQL
* Módulos onde o tamanho dos dados de saída pode ser superior ao tamanho dos dados de entrada, tais como Associação ou Hashing de Funcionalidade
* Validação Cruzada, Hiperparâmetros do Modelo de Otimização, Regressão Ordinal e Multicasse “One-vs-All”, quando o número de iterações é muito grande

Para conjuntos de dados superiores a alguns GB, carregue os dados para o Armazenamento do Azure ou para a Base de Dados SQL do Azure ou utilize o HDInsight em vez de os carregar diretamente a partir de um ficheiro local.

**Existem limitações ao tamanho do vetor?**

Linhas e colunas são limitadas para a limitação de .NET do máx. Int cada: 2,147,483,647.

**Posso ajustar o tamanho da máquina virtual que executa o serviço Web?**

Não.  

## <a name="security-and-availability"></a>Segurança e a disponibilidade
**Quem tem acesso ao ponto final de http do serviço Web por predefinição? Como posso restringir o acesso ao ponto final?**

Depois de implementar um serviço Web, é criado um ponto final predefinido para esse serviço. O ponto final predefinido pode ser chamado através da respetiva chave de API. Pode adicionar mais pontos finais com as respetivas chaves a partir do portal clássico dos Serviços Web ou programaticamente, mediante a utilização das APIs de Gestão do Serviço Web. As chaves de acesso são necessárias para solicitar o serviço Web. Para obter mais informações, veja [How to consume an Azure Machine Learning Web service](consume-web-services.md) (Como consumir um serviço Web do Azure Machine Learning).

**O que acontece se não for possível encontrar a minha conta de armazenamento do Azure?**

O Machine Learning Studio baseia-se numa conta de armazenamento do Azure fornecida pelo utilizador para guardar dados intermédios quando executa o fluxo de trabalho. Esta conta de armazenamento é fornecida ao Machine Learning Studio no momento em que é criada uma área de trabalho. Depois de a área de trabalho ter sido criada, se a conta de armazenamento tiver sido eliminada e já não puder ser encontrada, a área de trabalho deixa de funcionar e todas as experimentações da mesma falham.

Caso tenha eliminado acidentalmente a conta de armazenamento, recrie-a com o mesmo nome na mesma região que a conta de armazenamento eliminada. Depois disso, volte a sincronizar a chave de acesso.

**O que acontece se a chave de acesso da minha conta de armazenamento estiver dessincronizada?**

O Machine Learning Studio baseia-se numa conta de armazenamento do Azure fornecida pelo utilizador para armazenar dados intermédios quando executa o fluxo de trabalho. Esta conta de armazenamento é fornecida ao Machine Learning Studio no momento em que é criada uma área de trabalho e as chaves de acesso são associadas a essa área de trabalho. Se as chaves de acesso forem alteradas, depois de criada a área de trabalho, esta deixa de poder aceder à conta de armazenamento. Deixa de funcionar e todas as experimentações na mesma falham.

Se tiver alterado as chaves de acesso da conta de armazenamento, utilize o portal do Azure e volte a sincronizá-las na área de trabalho.  


## <a name="billing-questions"></a>Perguntas sobre faturação

Para obter informações sobre faturação e preços, consulte [Preços do Machine Learning](https://azure.microsoft.com/pricing/details/machine-learning/).


<!-- Module References -->
[image-reader]: https://msdn.microsoft.com/library/azure/893f8c57-1d36-456d-a47b-d29ae67f5d84/
[join]: https://msdn.microsoft.com/library/azure/124865f7-e901-4656-adac-f4cb08248099/
[machine-learning-modules]: https://msdn.microsoft.com/library/azure/6d9e2516-1343-4859-a3dc-9673ccec9edc/
[partition-and-sample]: https://msdn.microsoft.com/library/azure/a8726e34-1b3e-4515-b59a-3e4a475654b8/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[export-data]: https://msdn.microsoft.com/library/azure/7A391181-B6A7-4AD4-B82D-E419C0D6522C
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[python]: https://msdn.microsoft.com/library/azure/CDB56F95-7F4C-404D-BDE7-5BB972E6F232
[counts]: https://msdn.microsoft.com/library/azure/dn913056.aspx
