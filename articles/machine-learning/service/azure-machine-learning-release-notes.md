---
title: O que há de novo no Azure Machine Learning
description: Este documento fornece detalhes sobre as atualizações para o Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: reference
author: hning86
ms.author: haining
ms.reviewer: j-martens
ms.date: 10/24/2018
ms.openlocfilehash: 0927ae14d232ec2ce3a46bb66ed535deabbe0138
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/26/2018
ms.locfileid: "50158691"
---
# <a name="azure-machine-learning-service-release-notes"></a>Notas de versão de serviço do Azure Machine Learning

Neste artigo, saiba mais sobre as versões de serviço do Azure Machine Learning. 

## <a name="2018-10-12"></a>2018-10 a 12

### <a name="azure-machine-learning-sdk-for-python-v0168"></a>Azure Machine Learning SDK for Python v0.1.68

+ **Novos recursos**
  * Suporte para inquilino vários ao criar nova área de trabalho.

+ **Alterações recentes**
  * **Na próxima versão futura** *Workspace.compute_targets, arquivos de dados, experiências, imagens, modelos* e *webservices* irá tornar-se propriedades em vez de métodos. Por exemplo, substitua *Workspace.compute_targets()* com *Workspace.compute_targets*.

+ **Erros corrigidos**
  * A versão da biblioteca de pynacl já não tem de ser afixado ao implementar o serviço web.

### <a name="azure-machine-learning-data-prep-sdk-v030"></a>SDK v0.3.0 de preparação de dados do Azure Machine Learning

+ **Novos recursos**
  * Adicionado o método transform_partition_with_file(script_path), que permite que os utilizadores passem o caminho de um ficheiro de Python para executar

## <a name="2018-10-01"></a>2018-10-01

### <a name="azure-machine-learning-sdk-for-python-v0165"></a>Azure Machine Learning SDK for Python v0.1.65
[Versão 0.1.65](https://pypi.org/project/azureml-sdk/0.1.65) inclui novos recursos, mais documentação, correções de erros e muito mais [blocos de notas de exemplo](https://aka.ms/aml-notebooks).

Ver [a lista de problemas conhecidos](resource-known-issues.md) para saber mais sobre erros conhecidos e soluções alternativas.

+ **Alterações recentes**
  * Workspace.experiments, Workspace.models, Workspace.compute_targets, Workspace.images, dicionário de retorno Workspace.web_services, anteriormente devolvido lista. Ver [azureml.core.Workspace](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py) documentação da API.

  * Aprendizagem automática removido normalizado média quadrática erro as métricas principais.

+ **HyperDrive**
  * Várias correções de erros de HyperDrive para Bayesianos, melhorias de desempenho para receber chamadas de métricas. 
  * Atualização do Tensorflow 1.10 de 1,9 
  * Otimização de imagem de docker para o arranque a frio. 
  * Tarefa 's agora comunicar o estado correto, mesmo que eles sair com o erro de código que 0. 
  * Validação do atributo de RunConfig no SDK. 
  * Objeto de HyperDrive executar suporta Cancelar semelhante a uma execução regular: há necessidade de passar todos os parâmetros. 
  * Melhorias de widget para manter o estado de valores de lista pendente para execuções distribuídas e HyperDrive execuções. 
  * TensorBoard e outro ficheiros de suporte de registo fixo para o servidor de parâmetros. 
  * Suporte a MPI Intel (r) no lado do serviço. 
  * Bugfix para otimização de parâmetro para distribuído correção execução durante a validação no BatchAI. 
  * Gestor de contexto agora identifica a instância primária. 

+ **Experiência do portal do Azure**
  * log_table() e log_row() são suportadas nos detalhes de execução. 
  * Crie automaticamente os gráficos para tabelas e linhas com 1,2 ou 3 colunas numéricas e uma coluna de categórica opcional.

+ **Aprendizagem automática**
  * Documentação e melhorado processamento de erros 
  * Corrigido problemas de desempenho de obtenção de propriedade de execução. 
  * Foi corrigido problema de execução de continuar. 
  * Foram corrigidos ensembling erros de iteração.
  * Treinamento fixo pendurado bug no MAC OS.
  * Downsampling macro média PR/curva cor MULTICLASSE no cenário de validação personalizada.
  * Remover a lógica de índice extra.
  * Remover o filtro de get_output API.

+ **Pipelines**
  * Adicionar um método Pipeline.publish() para publicar um pipeline diretamente, sem a necessidade de uma execução de executar primeira.   
  * Adicionar um método PipelineRun.get_pipeline_runs() para buscar o pipeline é executado que foram gerado a partir de um pipeline publicado.

+ **Project Brainwave**
  * Suporte atualizado para novos modelos de IA, disponíveis no FPGAs.

### <a name="azure-machine-learning-data-prep-sdk-v020"></a>SDK v0.2.0 de preparação de dados do Azure Machine Learning
[Versão 0.2.0](https://pypi.org/project/azureml-dataprep/0.2.0/) inclui seguintes funcionalidades e bugfixes:

+ **Novos recursos**
  * Suporte para acesso frequente uma codificação
  * Suporte para a transformação de quantile
   
+ **Erro corrigido:**
  * Funciona com qualquer versão de Tornado, sem a necessidade de mudar a sua versão de Tornado
  * Contagens de valores para todos os valores, não apenas o três de principais

## <a name="2018-09-public-preview-refresh"></a>2018-09 (atualização da pré-visualização pública)

A nova e totalmente atualizada a versão do Azure Machine Learning: Saiba mais sobre esta versão: https://azure.microsoft.com/blog/what-s-new-in-azure-machine-learning-service/

## <a name="older-notes-sept-2017---jun-2018"></a>Notas de mais antigas: Setembro de 2017 - Junho de 2018
### <a name="2018-05-sprint-5"></a>05 de 2018 (sprint 5)

Com esta versão do Azure Machine Learning, pode:
+ Caracterização de imagens com uma versão quantificada de utilizar o ResNet 50, treinar um classificador com base nesses recursos, e [implementar esse modelo para um FPGA no Azure](../service/how-to-deploy-fpga-web-service.md) para inferência de latência Ultra baixas.

+ Criar e implementar elevada precisão de aprendizagem automática e modelos de aprendizagem profunda com rapidamente [personalizado pacotes do Azure Machine Learning](../desktop-workbench/reference-python-package-overview.md) para os seguintes domínios:
  + [Visão do computador](../desktop-workbench/how-to-build-deploy-image-classification-models.md)
  + [Análise de texto](../desktop-workbench/how-to-build-deploy-text-classification-models.md)
  + [Previsão](../desktop-workbench/how-to-build-deploy-forecast-models.md)

### <a name="2018-03-sprint-4"></a>2018-03 (sprint 4)
**Número de versão**: 0.1.1801.24353 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;([encontrar a versão](../desktop-workbench/known-issues-and-troubleshooting-guide.md#find-the-workbench-build-number))


Muitos das seguintes atualizações são feitos direta resultados dos seus comentários. . Continuar a trazer!

**Importantes novos recursos e alterações**

- Suporte para executar seus scripts nas VMs do Ubuntu remoto nativamente no seu próprio ambiente, além do docker remoto com base em execução.
- Nova experiência de ambiente na bancada de trabalho de aplicação permite-lhe criar destinos de computação e executar configurações, além de nossa experiência com o CLI.
![Separador ambientes](media/azure-machine-learning-release-notes/environment-page.png)
- Relatórios de histórico de execuções personalizável ![imagem de executar relatórios de histórico de novo](media/azure-machine-learning-release-notes/new-run-history-reports.png)

**Atualizações detalhadas**

Segue-se uma lista de atualizações detalhadas em cada área de componente do Azure Machine Learning nesse sprint.

#### <a name="workbench-ui"></a>A Bancada de trabalho da interface do Usuário
- Relatórios de histórico de execuções personalizáveis
  - Configuração de gráficos melhorado para relatórios de histórico de execuções
    - O entrypoints utilizado pode ser alterado
    - Filtros de nível superior podem ser adicionados e modificados ![adicionar filtros](media/azure-machine-learning-release-notes/add-filters.jpg)
    - Estatísticas e gráficos, podem ser adicionadas ou modificadas (e arrastar-e-soltar reorganizadas).
    ![Criar novos gráficos](media/azure-machine-learning-release-notes/configure-charts.png)

  - CRUD para relatórios de histórico de execuções
  - Mover todos os vista de lista do histórico de execuções existente configurações para o lado do servidor de relatórios, que funciona como pipelines de é executado a partir de pontos de entrada selecionada.

- Separador ambientes
  - Adicionar novo destino de computação e executar arquivos de configuração ao seu projeto facilmente ![novo destino de computação](media/azure-machine-learning-release-notes/add-new-environments.png)
  - Gerir e atualizar os ficheiros de configuração com uma experiência de Usuário simples e baseada em formulários
  - Botão novo para a preparação de seus ambientes de execução

- Melhorias de desempenho para a lista de ficheiros na barra lateral

#### <a name="data-preparation"></a>Preparação de dados 
- Agora o Azure Machine Learning Workbench permite-lhe ser capaz de procurar por uma coluna com o nome da coluna conhecidos.


#### <a name="experimentation"></a>Experimentação
- O Azure Machine Learning Workbench agora suporta a execução dos scripts de forma nativa no seu próprio ambiente de python ou pyspark. Para esta capacidade, o utilizador cria e gere o seu próprio ambiente na VM remota e utilize o Azure Machine Learning Workbench para executar seus scripts em que visam. Consulte [configurar o serviço do Azure Machine Learning experimentação](../desktop-workbench/experimentation-service-configuration.md) 

#### <a name="model-management"></a>Gestão de Modelos
- Suporte para personalizar os contentores implementados: permite personalizar a imagem de contentor, permitindo que a instalação de bibliotecas externas com apt-get, etc. Já não está limitado às bibliotecas instaláveis no pip. Consulte a [documentação](../desktop-workbench/model-management-custom-container.md) para obter mais informações.
  - Utilize o `--docker-file myDockerStepsFilename` sinalizador e nome de ficheiro com o manifesto, imagem ou comandos de criação do serviço.
  - Tenha em atenção que a imagem base é o Ubuntu e não pode ser modificada.
  - Comando de exemplo: 
  
      ```shell
      $ az ml image create -n myimage -m mymodel.pkl -f score.py --docker-file mydockerstepsfile
      ```



### <a name="2018-01-sprint-3"></a>2018-01 (sprint 3) 
**Número de versão**: 0.1.1712.18263 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;([encontrar a versão](../desktop-workbench/known-issues-and-troubleshooting-guide.md#find-the-workbench-build-number))

Seguem-se as atualizações e melhoramentos nesse sprint. Muitas destas atualizações são efetuadas como resultado direto de comentários dos utilizadores. 


Segue-se uma lista de atualizações detalhadas em cada área de componente do Azure Machine Learning nesse sprint.

- Seleção de início de sessão e a conta na inicialização de força de atualizações para a pilha de autenticação

#### <a name="workbench"></a>Workbench
- Capacidade de instalar/desinstalar a aplicação a partir de adicionar/remover programas
- Seleção de início de sessão e a conta na inicialização de força de atualizações para a pilha de autenticação
- Experiência melhorada de único início de sessão (SSO) no Windows
- Os utilizadores que pertencem a múltiplos inquilinos com credenciais diferentes agora poderão iniciar sessão no Workbench

#### <a name="ui"></a>INTERFACE DO USUÁRIO
- Melhoramentos gerais e correções de erros

#### <a name="notebooks"></a>Blocos de notas
- Melhoramentos gerais e correções de erros

#### <a name="data-preparation"></a>Preparação de dados 
- Sugestões de automática melhoradas ao efetuar transformações por exemplo
- Algoritmo melhorado para o inspetor de frequência de padrão
- Capacidade de enviar dados de exemplo e comentários ao efetuar transformações por exemplo ![derivar de imagem da ligação de comentários de envio na transformação de coluna](media/azure-machine-learning-release-notes/SendFeedbackFromDeriveColumn.png)
- Aprimoramentos de tempo de execução do Spark
- Scala substituiu Pyspark
- Foi corrigida incapacidade para fechar a dados não é aplicável para o Inspetor de série de tempo 
- Foi corrigido o tempo de travamento para a execução de preparação de dados para HDI

#### <a name="model-management-cli-updates"></a>Modelos de CLI de gestão de atualizações 
  - Propriedade da subscrição já não é necessária para o aprovisionamento de recursos. Acesso de Contribuidor ao grupo de recursos será suficiente para configurar o ambiente de implantação.
  - Configurar o ambiente local habilitados gratuitamente subscrições 

### <a name="2017-12-sprint-2-qfe"></a>12-2017 (sprint 2 QFE) 
**Número de versão**: 0.1.1711.15323 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;([encontrar a versão](../desktop-workbench/known-issues-and-troubleshooting-guide.md#find-the-workbench-build-number))

Esta é a versão QFE (Quick Fix Engineering), uma versão secundária. Ele aborda vários problemas de telemetria e ajuda a equipe de produto para compreender melhor como o produto está a ser utilizado. O conhecimento pode entrar em futuros esforços para melhorar a experiência de produto. 

Além disso, existem duas atualizações importantes:

- Foi corrigido um erro na preparação de dados que impediu o inspetor de série de tempo de ser apresentado nos pacotes de preparação de dados.
- Na ferramenta de linha de comando, já não precisa de ser um proprietário de subscrição do Azure para aprovisionar clusters de ACS de computação do Machine Learning. 

### <a name="2017-12-sprint-2"></a>12 de 2017 (sprint 2)
**Número de versão**: 0.1.1711.15263 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;([encontrar a versão](../desktop-workbench/known-issues-and-troubleshooting-guide.md#find-the-workbench-build-number))

Bem-vindo à terceira atualização do Azure Machine Learning. Esta atualização inclui aprimoramentos na aplicação workbench, a Interface de linha de comandos (CLI) e os serviços de back-end. Muito obrigado para enviar os smiles e frowns. Muitos das seguintes atualizações são feitos direta resultados dos seus comentários. 

**Funcionalidades novas importantes**
- [Suporte para SQL Server e o Azure SQL DB como uma origem de dados](../desktop-workbench/data-prep-appendix2-supported-data-sources.md#types) 
- [Aprendizagem profunda no Spark com o suporte GPU usando o MMLSpark](https://github.com/Azure/mmlspark/blob/master/docs/gpu-setup.md)
- [Todos os contentores AML são compatíveis com dispositivos Azure IoT Edge, quando implementado (não existem passos Extras necessários)](http://aka.ms/aml-iot-edge-blog)
- Portal do Azure disponível de vistas de lista de modelo registado e os detalhes
- Aceder aos destinos de computação com a autenticação baseada em chave SSH, além de acesso baseado em nome de utilizador/palavra-passe. 
- Inspetor de frequência de padrão de novo nos dados de preparação de experiência. 

**Detalhadas atualizações** segue-se uma lista de atualizações detalhadas em cada área de componente do Azure Machine Learning nesse sprint.

#### <a name="installer"></a>Instalador
- Instalador pode atualização automática, de modo a que correções de bugs e novos recursos podem ser suportados sem necessidade de reinstalá-lo de utilizador

#### <a name="workbench-authentication"></a>Autenticação da bancada de trabalho
- Várias correções para o sistema de autenticação. Indique se ainda ocorrerem problemas de início de sessão.
- Alterações de interface do Usuário que seja mais fácil encontrar as definições do Gestor de Proxy.

#### <a name="workbench"></a>Workbench
- Vista de ficheiros só de leitura tem agora o plano de fundo azul claro
- Botão Editar movido para a direita para torná-lo mais detectáveis.
- "dsource", "dprep" e formatos de arquivo de "ipynb" agora podem ser compostos num formato de texto não processado
- A Bancada de trabalho tem agora uma nova experiência de edição que orienta os utilizadores na direção do uso de IDEs externo para editar scripts e usar a Bancada de trabalho apenas para editar os tipos de ficheiros que tenham uma experiência de edição sofisticada (como blocos de notas, origens de dados, pacotes de preparação de dados)
- A carregar a lista de áreas de trabalho e os projetos que o utilizador tem acesso a é significativamente mais rápida agora

#### <a name="data-preparation"></a>Preparação de dados 
- Inspetor de frequência um padrão para ver os padrões de cadeia de caracteres numa coluna. Também pode filtrar os dados com estes padrões. Isto mostra-lhe uma vista semelhante para o inspetor de contagens de valores. A diferença é que o padrão de frequência mostra as contagens dos padrões exclusivos dos dados, em vez das contagens de dados exclusivos. Também pode filtrar e reduzir todas as linhas que se enquadram num determinado padrão.

![Imagem de inspetor de frequência padrão no número do produto](media/azure-machine-learning-release-notes/pattern-inspector-product-number.png)

- Melhorias de desempenho ao recomendando casos extremos para rever na transformação "derivar coluna por exemplo"

- [Suporte para SQL Server e o Azure SQL DB como uma origem de dados](../desktop-workbench/data-prep-appendix2-supported-data-sources.md#types) 

![Imagem da criação de uma nova origem de dados do SQL server](media/azure-machine-learning-release-notes/sql-server-data-source.png)

- Ativada "visão geral" vista de contagens de linhas e colunas

![Imagem da linha de contagem de colunas numa tolerância](media/azure-machine-learning-release-notes/row-col-count.png)

- Preparação de dados está ativada em todos os contextos de computação
- Origens de dados que utilizam uma base de dados do SQL Server estão ativadas em todos os contextos de computação
- Grelha colunas podem ser filtradas pelo tipo de dados de preparação de dados
- Foi corrigido o problema com a conversão de várias colunas até à data
- Foi corrigido o problema que o utilizador pode selecionar a coluna de saída como uma origem de derivar coluna por exemplo se o utilizador alterou o nome de coluna de saída no modo avançado.

#### <a name="job-execution"></a>Execução da tarefa
Agora pode criar e acessar um destino de computação de tipo remotedocker ou cluster utilizando a autenticação baseada em chave SSH seguindo estes passos:
- Anexar um destino de computação com o seguinte comando na CLI

    ```azure-cli
    $ az ml computetarget attach remotedocker --name "remotevm" --address "remotevm_IP_address" --username "sshuser" --use-azureml-ssh-key
    ```
>[!NOTE]
>Especifica uma opção -k (ou --use-azureml-ssh-key) no comando para gerar e utilizar a chave SSH.

- O Azure ML Workbench irá gerar uma chave pública e de saída que na sua consola. Inicie sessão para a computação de destino usando o mesmo nome de utilizador e acrescentar ficheiro ~/.ssh/authorized_keys com esta chave pública.

- Pode preparar o destino de computação e utilizá-lo para execução e o Azure ML Workbench esta chave será utilizada para autenticação.  

Para obter mais informações sobre a criação de destinos de computação, consulte [configurar o serviço do Azure Machine Learning experimentação](../desktop-workbench/experimentation-service-configuration.md)

#### <a name="visual-studio-tools-for-ai"></a>Visual Studio Tools para IA
- Foi adicionado suporte para [Visual Studio Tools para IA](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.vstoolsai-vs2017). 

#### <a name="command-line-interface-cli"></a>Interface de Linha de Comandos (CLI)
- Adicionado `az ml datasource create` permite de comando para criar um arquivo de origem de dados a partir da linha de comandos

#### <a name="model-management-and-operationalization"></a>Gestão de modelos e a Operacionalização
- [Todos os contentores AML são compatíveis com dispositivos Azure IoT Edge quando operacionalizados (não existem passos Extras necessários)](http://aka.ms/aml-iot-edge-blog) 
- Melhorias de mensagens de erro na o16n CLI
- Correções de erros no portal de gestão de modelo UX  
- Letra consistente letras maiúsculas e minúsculas para atributos de gestão de modelo na página de detalhes
- Em tempo real de tempo limite de chamadas de classificação definida como 60 segundos
- Modelo registado lista e detalhes modos de exibição disponíveis no portal do Azure

![Detalhes do modelo no portal](media/azure-machine-learning-release-notes/model-list.jpg)

![visão geral do modelo no portal](media/azure-machine-learning-release-notes/model-overview-portal.jpg)

#### <a name="mmlspark"></a>MMLSpark
- Aprendizagem aprofundada em Spark com [suporte GPU](https://github.com/Azure/mmlspark/blob/master/docs/gpu-setup.md)
- Suporte para modelos do Resource Manager para implementação de recursos mais fácil
- Suporte para o ecossistema de SparklyR
- [Integração de AZTK](https://github.com/Azure/aztk/wiki/Spark-on-Azure-for-Python-Users#optional-set-up-mmlspark)

#### <a name="sample-projects"></a>Projetos de exemplo
- [Iris](https://github.com/Azure/MachineLearningSamples-Iris) e [MMLSpark](https://github.com/Azure/mmlspark) exemplos atualizados com a nova versão do SDK do Azure ML

#### <a name="breaking-changes"></a>Alterações interruptivas
- Promovido a `--type` comutador no `az ml computetarget attach` para um subcomando. 

    - `az ml computetarget attach --type remotedocker` está agora `az ml computetarget attach remotedocker`
    - `az ml computetarget attach --type cluster` está agora `az ml computetarget attach cluster`

### <a name="2017-11-sprint-1"></a>11 de 2017 (sprint 1) 
**Número de versão**: 0.1.1710.31013 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;([encontrar a versão](../desktop-workbench/known-issues-and-troubleshooting-guide.md#find-the-workbench-build-number))

Nesta versão, fizemos melhorias em torno de segurança, estabilidade e facilidade de manutenção na aplicação da bancada de trabalho, a CLI e a camada de serviços de back-end. Agradecemos muito por enviar-nos smiles e frowns. Muitas do abaixo as atualizações são efetuadas como diretos resultados dos seus comentários. Continuar a trazer!

#### <a name="notable-new-features"></a>Funcionalidades novas importantes
- O Azure ML está agora disponível em duas novas regiões do Azure: **Europa Ocidental** e **Sudeste asiático**. Que se associam as regiões anteriores do **E.U.A. Leste 2**, **e.u.a. Centro-Oeste**, e **leste da Austrália**, totalizando o número de implementado regiões a cinco.
- Ativámos a sintaxe de código de Python realce na aplicação da bancada de trabalho para que seja mais fácil de ler e editar o código-fonte Python. 
- Agora, pode iniciar o seu IDE favorito diretamente a partir de um ficheiro, em vez de todo o projeto.  Abrir um ficheiro na bancada de trabalho e, em seguida, clicar em "Editar" inicia seu IDE (atualmente o VS Code e PyCharm são suportadas) para o ficheiro atual e o projeto.  Também pode clicar na seta junto ao botão Editar para editar o ficheiro no editor de texto do Workbench.  Ficheiros são só de leitura até que clique em Editar, impedir alterações acidentais.
- A popular biblioteca de desenho `matplotlib` versão 2.1.0 agora é fornecido com a aplicação Workbench.
- Que atualizamos a versão do .NET Core 2.0 para o mecanismo de preparação de dados. Isso removido o requisito de instalação brew openssl durante a instalação da aplicação em macOS. Ele também abre o caminho para os dados mais interessantes recursos de preparação por vir no futuro próximo. 
- Ativámos uma página inicial da aplicação específico da versão, para que obtenha mais relevantes notas de versão e atualizar pedidos com base na sua versão atual da aplicação.
- Se o seu nome de utilizador local tem um espaço no mesmo, a aplicação pode agora ser instalada com êxito. 

#### <a name="detailed-updates"></a>Atualizações detalhadas
Segue-se uma lista de atualizações detalhadas em cada área de componente do Azure Machine Learning nesse sprint.

##### <a name="installer"></a>Instalador
- Instalador de aplicação limpa agora o diretório de instalação criado com uma versão mais antiga da aplicação.
- Foi corrigido um erro que leva a acumular-se a 100% no macOS High Sierra do instalador.
- Agora é uma ligação direta para o diretório de instalador para o utilizador rever os registos do instalador em caso de falha de instalação.
- Instale agora funciona para os utilizadores que tenham espaço no respetivo nome de utilizador.

##### <a name="workbench-authentication"></a>Autenticação da bancada de trabalho
- Suporte para a autenticação no Gestor de Proxy.
- Iniciar sessão agora é bem-sucedida se o utilizador é protegido por uma firewall. 
- Se o usuário tiver contas de experimentação em várias regiões do Azure, e se uma região ficar indisponível, a aplicação já não são paradas.
- Quando a autenticação não está concluída e a caixa de diálogo de autenticação é ainda estiver visível, o aplicativo já não tenta carregar a área de trabalho do local cache.

##### <a name="workbench-app"></a>Aplicação Workbench
- Realce de sintaxe de código do Python está ativada no editor de texto.
- O botão de edição no editor de texto permite-lhe editar o arquivo num IDE (VS Code e PyCharm são suportadas) ou no editor de texto incorporado.
- Editor de texto está no modo só de leitura, por predefinição. 
- Guarde as alterações de agora de estado visual de botão desativado depois do ficheiro atual está guardado e, por conseguinte, já não modificado.
- Guarda a Bancada de trabalho _todos os_ não guardadas ficheiros quando iniciar uma execução.
- Se lembra a Bancada de trabalho, que a última utilizada a área de trabalho na máquina local, para que ele abre-se automaticamente.
- Agora, apenas uma única instância da bancada de trabalho é permitida a execução. Anteriormente podem ser iniciadas várias instâncias que causava problemas quando funciona no mesmo projeto.
- Menu de ficheiro nome mudado "Open Project..." para "Adicionar existente pasta como Project..." 
- Mudar de separador agora é muito mais rápido.
- Ligações de ajuda são adicionadas à caixa de diálogo de configuração IDE.
- O formulário de comentários lembra, agora, o endereço de e-mail que introduziu a última vez.
- Área de texto de formulário smiles e frowns agora é o maior, para que possa enviar-nos comentários mais! 
- O `--owner` mudar texto de ajuda em `az ml workspace create` seja corrigido.
- Adicionámos uma caixa de diálogo "Sobre" para o ajudar a utilizador facilmente ver e copiar o número da versão da aplicação.
- Um item de menu "Sugerir uma funcionalidade" é adicionado ao menu de ajuda.
- Nome da conta de experimentação agora está visível na aplicação na barra de título, anterior o nome da aplicação "Azure Machine Learning Workbench".
- Uma home page de aplicação específico da versão é apresentada agora com base na versão da aplicação detetada.

##### <a name="data-preparation"></a>Preparação de dados 
- Site da web externo já não pode ser carregado de Inspetor de mapas para evitar possíveis problemas de segurança.
- Inspetores de histograma e contagem de valores tem agora a opção para mostrar o gráfico na escala logarítmica.
- Quando um cálculo está em curso, a barra de qualidade de dados mostra agora uma cor diferente para sinalizar o estado "calcular".
- Métricas de coluna agora mostram as estatísticas de colunas de valores categóricos.
- O último caráter no nome da origem de dados já não é truncado.
- Pacote de preparação de dados agora permanece aberto ao alternar guias, resultando ganhos de desempenho considerável.
- Origem de dados, ao alternar entre a exibição de dados e a vista de métrica, a ordem das colunas agora já muda.
- Abrir um inválido `.dprep` ou `.dsource` ficheiro já não Bancada de trabalho a falha.
- Pacote de preparação de dados pode agora utiliza o caminho relativo de saída em _escrever para CSV_ transformar.
- _Manter coluna_ transformação agora permite que o utilizador adicionar mais colunas quando editado.
- _Substituir isso_ menu agora realmente inicia _substituir valor_ caixa de diálogo.
- _Substitua o valor_ transformar agora funções conforme o esperado em vez de gerar o erro.
- Pacote de preparação de dados utiliza agora o caminho absoluto ao referenciar arquivos de dados fora da pasta de projeto, tornando possível executar o pacote no contexto local com o caminho absoluto para o ficheiro de dados.
- _Ficheiro completo_ como uma estratégia de amostragem é agora suportada ao utilizar o Azure blob como origem de dados.
- Gerar código Python (a partir de pacote de preparação de dados) agora carrega CR e LF, tornando amigável no Windows.
- _Escolher métricas_ suspensa oculta agora propriedade ao mudar para a vista de dados.
- Agora os ficheiros parquet de processo, mesmo quando está a utilizar o runtime do Python, pode Bancada de trabalho. Anteriormente Spark só pode ser usado durante o processamento de ficheiros parquet. 
- Filtrar valores numa coluna com _data_ tipo de dados já não faz com que o mecanismo de preparação de dados, a falha.
- Vista de métrica agora respeita as atualizações de estratégia de amostragem.
- Remoto amostragem tarefas agora funciona corretamente.

##### <a name="job-execution"></a>Execução da tarefa
- Argumento agora está incluído no registo de histórico de execuções.
- Tarefas iniciadas CLI agora aparece no painel de trabalho do histórico de execução automaticamente.
- Painel de trabalho mostra agora as tarefas criadas por utilizadores convidados adicionados ao inquilino do Azure AD.
- Cancelar do painel de trabalho e ações de eliminação são mais estáveis.
- Ao clicar no botão de execução, a mensagem de erro é acionada agora se os ficheiros de configuração estão no formato incorreto.
- Terminação de aplicação já não interfere com tarefas iniciadas na CLI.
- Tarefas iniciadas CLI agora continua a extraí standard-out, mesmo após uma hora de execução.
- Mensagens de erro melhor são apresentadas quando ocorre uma falha do pacote de preparação de dados executado em Python/PySpark.
- `az ml experiment clean` agora limpa imagens do Docker na VM remota também.
- `az ml experiment clean` agora funciona corretamente para o destino local em macOS.
- Mensagens de erro quando executa o direcionamento Docker local ou remoto são limpos de segurança e mais fácil de ler.
- É apresentada a mensagem de erro melhor quando o nome de nó principal do cluster de HDInsight não está formatado corretamente quando anexado como um destino de execução.
- É apresentada a mensagem de erro melhor quando o segredo não foi encontrado no serviço de credenciais. 
- Biblioteca de MMLSpark é atualizada para suportar o Apache Spark 2.2.
- MMLSpark agora incluem o assunto codificação transformação (codificação de malha) para documentos médicos.
- `matplotlib` a versão 2.1.0 é agora fornecido fora-prontos com a Bancada de trabalho.

##### <a name="jupyter-notebook"></a>Bloco de Notas do Jupyter
- Pesquisa de nome do bloco de notas agora funciona corretamente na vista de blocos de notas.
- Agora pode eliminar um bloco de notas na vista de blocos de notas.
- Nova mágica `%upload_artifact` é adicionado para carregar ficheiros produzidos no ambiente de execução do bloco de notas no arquivo de dados do histórico de execuções.
- Erros de kernel são agora apresentados no estado da tarefa de bloco de notas para depuração mais fácil.
- Servidor de Jupyter agora corretamente encerrado quando o utilizador inicia sessão fora da aplicação.

##### <a name="azure-portal"></a>Portal do Azure
- Conta de experimentação e a conta de gestão de modelos agora podem ser criados em duas novas regiões do Azure: Europa Ocidental e Sudeste asiático.
- Esquema de Dev/Test de conta de gestão de modelo agora está apenas disponível quando ele é o primeiro a ser criada na subscrição. 
- Ligação de ajuda no portal do Azure é atualizada para apontar para a página de documentação correta.
- Campo de descrição é removido da página de detalhes de imagem do Docker, uma vez que não é aplicável.
- Detalhes incluindo as definições de AppInsights e dimensionamento automático são adicionados para a página de detalhes do serviço web.
- Página de gestão de modelo renderiza agora mesmo que os cookies de terceiros estão desativadas no browser. 

##### <a name="operationalization"></a>Operacionalização
- Serviço Web com "score" em seu nome já não falha.
- Utilizador pode agora criar um ambiente de implantação com apenas o acesso de contribuinte a um grupo de recursos do Azure ou a subscrição. Já não é necessário acesso de proprietário para a subscrição completa.
- Operacionalização CLI agora gosta de preenchimento automático separador no Linux.
- Serviço de construção de imagem agora dá suporte a imagens de construção para serviços/dispositivos de IoT do Azure.

##### <a name="sample-projects"></a>Projetos de exemplo
- [_Classificar íris_ ](../desktop-workbench/tutorial-classifying-iris-part-1.md) projeto de exemplo:
    - `iris_pyspark.py` foi mudado para `iris_spark.py`.
    - `iris_score.py` foi mudado para `score_iris.py`.
    - `iris.dprep` e `iris.dsource` foram atualizadas para refletir as atualizações do mecanismo de preparação de dados mais recentes.
    - `iris.ipynb` Bloco de notas é acrescentado a trabalhar num cluster do HDInsight.
    - Histórico de execuções é ativado no `iris.ipynb` célula de bloco de notas.
- [_Advanced preparação de dados com dados de partilha de bicicletas_ ](../desktop-workbench/tutorial-bikeshare-dataprep.md) projeto de exemplo "Lidar com valor de erro" passo fixado.
- [_MMLSpark nos dados de censo para adultos_ ](https://github.com/Azure/MachineLearningSamples-mmlspark) projeto de exemplo `docker.runconfig` formato atualizado de JSON para YAML.
- [_Otimização de hiper-parâmetros distribuída_ ](../desktop-workbench/scenario-distributed-tuning-of-hyperparameters.md) projeto de exemplo`docker.runconfig` formato atualizado de JSON para YAML.
- Novo projeto de exemplo [ _classificação de imagens com o CNTK_](../desktop-workbench/scenario-image-classification-using-cntk.md).


### <a name="2017-10-sprint-0"></a>10 de 2017 (sprint 0) 
**Número de versão**: 0.1.1710.31013 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;([encontrar a versão](../desktop-workbench/known-issues-and-troubleshooting-guide.md#find-the-workbench-build-number))

Bem-vindo à primeira atualização do Azure Machine Learning Workbench, seguindo a nossa pré-visualização pública inicial na Conferência Microsoft Ignite 2017. As principais atualizações nesta versão estão a confiabilidade e a estabilização corrige.  Alguns dos problemas críticos que resolvidos incluem:

#### <a name="new-features"></a>Novos recursos
- o macOS High Sierra é agora suportado

#### <a name="bug-fixes"></a>Correções de erros
##### <a name="workbench-experience"></a>Experiência da bancada de trabalho
- Arrastar e soltar um arquivo na bancada de trabalho faz com que a Bancada de trabalho de falhas.
- A janela de terminal no VS Code, configurado como um IDE para a Bancada de trabalho não reconhece _az ml_ comandos.

##### <a name="workbench-authentication"></a>Autenticação da bancada de trabalho
Fizemos um número de atualizações para melhorar a vários problemas de início de sessão e autenticação relatados.
- Janela de autenticação mantém pop-up, especialmente quando a ligação à Internet não esteja estável.
- Problemas de Fiabilidade melhorada em torno de expiração do token de autenticação.
- Em alguns casos, a autenticação é apresentada a janela duas vezes.
- Janela principal da bancada de trabalho continua a apresentar "autenticar" mensagem quando terminar o processo de autenticação e a caixa de diálogo pop-up já dispensada.
- Se não houver sem ligação à Internet, a caixa de diálogo de autenticação é aberto com uma tela em branco.

##### <a name="data-preparation"></a>Preparação de dados 
- Quando um valor específico é filtrado, erros e valores em falta são também filtrados.
- Alterar uma estratégia de amostragem remove operações subsequentes de junção existentes.
- Substituir o valor em falta transformação tem NaN em consideração.
- Inferência de tipo de data gera a exceção quando encontrado valor nulo.

##### <a name="job-execution"></a>Execução da tarefa
- Não existe nenhuma mensagem de erro claras quando a execução da tarefa não consegue carregar pasta do projeto porque excedeu o limite de tamanho.
- Se o script de Python do utilizador alterar o diretório de trabalho, os ficheiros escritos para pastas de saídas não são contabilizados. 
- Se a subscrição do Azure Active Directory é diferente daquela que pertence o projeto atual, a submissão da tarefa resulta um erro 403.
- Quando o Docker não estiver presente, nenhuma mensagem de erro claras é devolvida se o utilizador tenta utilizar o Docker como um destino de execução.
- ficheiro .runconfig não é guardado automaticamente quando o usuário clica em _executar_ botão.

##### <a name="jupyter-notebook"></a>Bloco de Notas do Jupyter
- Não é possível iniciar o servidor se o utilizador utiliza com certos tipos de início de sessão.
- Mensagens de erro de servidor do bloco de notas não surgir nos registos visíveis ao utilizador.

##### <a name="azure-portal"></a>Portal do Azure
- Selecionar o tema no escuro do portal do Azure faz com que o painel de gestão de modelos para serem apresentadas como uma caixa preta.

##### <a name="operationalization"></a>Operacionalização
- Reutilizar um manifesto para atualizar um serviço web faz com que uma nova imagem de Docker criada com um nome aleatório.
- Não não possível obter os registos do serviço Web do cluster de Kubernetes.
- Mensagem de erro equivocadas é impresso quando o utilizador tentou criar uma conta de gestão de modelos ou uma conta de computação do ML e detetar problemas de permissões.

## <a name="next-steps"></a>Passos Seguintes

Leia a descrição geral do [Azure Machine Learning](../service/overview-what-is-azure-ml.md).
