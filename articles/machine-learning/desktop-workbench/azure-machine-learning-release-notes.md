---
title: Novidades no Azure Machine Learning
description: Este documento fornece detalhes sobre as atualizações para o Azure Machine Learning.
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.service: machine-learning
ms.workload: data-services
ms.topic: reference
ms.date: 03/28/2018
ms.openlocfilehash: e30943426ad68171e1464f828a9c8672b06c975a
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/19/2018
---
# <a name="whats-new-in-azure-machine-learning"></a>Novidades do Azure Machine Learning

Neste artigo, saiba mais sobre os novos lançamentos para [serviços do Azure Machine Learning](../service/overview-what-is-azure-ml.md). 

## <a name="2018-03-sprint-4"></a>2018-03 (sprint 4)
**Número de versão**: 0.1.1801.24353 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;([localizar a versão](../service/known-issues-and-troubleshooting-guide.md#find-the-workbench-build-number))


Bem-vindo à quinta atualização do Workbench do Azure Machine Learning. Muitos das seguintes atualizações que são efetuados como diretos resultados dos seus comentários. . Mantê-las provenha!

**Acentuadas novas funcionalidades e alterações**

- Suporte para executar os scripts em remoto Ubuntu VMs nativamente no seu ambiente, para além de docker remoto com base em execução.
- Nova experiência de ambiente na aplicação do Workbench permite-lhe criar destinos de computação e executar configurações para além da nossa experiência baseada na CLI.
![Separador ambientes](media/azure-machine-learning-release-notes/environment-page.png)
- Relatórios de histórico de execuções personalizável ![imagem executar relatórios de histórico de novo](media/azure-machine-learning-release-notes/new-run-history-reports.png)

**Atualizações de detalhado**

Segue-se uma lista das atualizações de detalhado cada área de componente do Azure Machine Learning neste sprint.

### <a name="workbench-ui"></a>Workbench da IU
- Relatórios de histórico de execuções personalizáveis
  - Configuração de gráficos melhorada para relatórios de histórico de execuções
    - O entrypoints utilizado pode ser alterada
    - Filtros de nível superior podem ser adicionados e modificados ![adicionar filtros](media/azure-machine-learning-release-notes/add-filters.jpg)
    - Gráficos e estatísticas podem ser adicionadas ou modificadas (e arrastar e largar reorganizar).
    ![Criar novo gráficos](media/azure-machine-learning-release-notes/configure-charts.png)

  - CRUD para relatórios de histórico de execuções
  - Mover todos os vista de lista de histórico de execução existente folhas do lado do servidor de relatórios, que funciona como pipelines no executa a partir dos pontos de entrada selecionada.

- Separador ambientes
  - Adicionar novo destino de computação e executar ficheiros de configuração ao seu projeto facilmente ![novo destino de computação](media/azure-machine-learning-release-notes/add-new-environments.png)
  - Gerir e atualizar os ficheiros de configuração utilizando um UX simple, baseada em formulários
  - Botão novo para preparar os seus ambientes para execução

- Melhorias de desempenho para a lista de ficheiros na barra lateral

### <a name="data-preparation"></a>Preparação de dados 
- Workbench de aprendizagem máquina do Azure permite-lhe agora ser capaz de procurar uma coluna com o nome de uma coluna conhecido.


### <a name="experimentation"></a>Experimentação
- Azure Workbench de aprendizagem máquina suporta agora a executar os scripts nativamente no seu próprio ambiente python ou pyspark. Para esta capacidade, o utilizador cria e gere o seu próprio ambiente na VM remota e utiliza o Azure Machine Learning Workbench para executar os scripts em que o destino. Consulte [configurar o serviço do Azure Machine Learning experimentação](experimentation-service-configuration.md) 

### <a name="model-management"></a>Gestão de Modelos
- Suporte para personalizar os contentores implementado: permite personalizar a imagem de contentor, permitindo a instalação das bibliotecas externas utilizando apt get, etc. Já não está limitado a bibliotecas pip instalável. Consulte o [documentação](model-management-custom-container.md) para obter mais informações.
  - Utilize o `--docker-file myDockerStepsFilename` sinalizador e nome de ficheiro com o manifesto, imagem ou os comandos de criação do serviço.
  - Tenha em atenção que a imagem de base Ubuntu e não pode ser modificada.
  - Comando de exemplo: 
  
      ```shell
      $ az ml image create -n myimage -m mymodel.pkl -f score.py --docker-file mydockerstepsfile
      ```



## <a name="2018-01-sprint-3"></a>2018-01 (sprint 3) 
**Número de versão**: 0.1.1712.18263 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;([localizar a versão](../service/known-issues-and-troubleshooting-guide.md#find-the-workbench-build-number))

Seguem-se as atualizações e melhoramentos neste sprint. Muitas destas atualizações que são efetuadas como resultado direto de comentários do utilizador. 


Segue-se uma lista das atualizações de detalhado cada área de componente do Azure Machine Learning neste sprint.

- As atualizações à pilha de autenticação força o início de sessão e a conta de seleção no arranque

### <a name="workbench"></a>Workbench
- Capacidade de instalar/desinstalar a aplicação a partir de adicionar/remover programas
- As atualizações à pilha de autenticação força o início de sessão e a conta de seleção no arranque
- Experiência melhorada de único início de sessão (SSO) no Windows
- Os utilizadores que pertencem a vários inquilinos com credenciais diferentes agora conseguirão iniciar sessão em Workbench

### <a name="ui"></a>IU
- Melhoramentos gerais e correções de erros

### <a name="notebooks"></a>Blocos de notas
- Melhoramentos gerais e correções de erros

### <a name="data-preparation"></a>Preparação de dados 
- Sugestões de automática melhoradas ao efetuar transformações de exemplo por
- Algoritmo melhorado para inspector frequência padrão
- Capacidade de enviar dados de exemplo e comentários ao efetuar transformações de exemplo por ![derivar de imagem de ligação de comentários de envio na transformação de coluna](media/azure-machine-learning-release-notes/SendFeedbackFromDeriveColumn.png)
- Melhoramentos de tempo de execução do Spark
- Scala substituiu Pyspark
- Incapacidade fixa para fechar a dados não é aplicável para o Inspector de séries de tempo 
- Corrigido hora hang de execução de preparação de dados para HDI

### <a name="model-management-cli-updates"></a>Modelo de CLI de gestão de atualizações 
  - Propriedade da subscrição já não é necessária para o aprovisionamento de recursos. Acesso de contribuinte ao grupo de recursos serão suficiente para configurar o ambiente de implementação.
  - Ambiente local ativada gratuitamente configurar subscrições 

## <a name="2017-12-sprint-2-qfe"></a>2017-12 (sprint 2 QFE) 
**Número de versão**: 0.1.1711.15323 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;([localizar a versão](../service/known-issues-and-troubleshooting-guide.md#find-the-workbench-build-number))

Esta é a versão QFE (Quick Fix Engineering), uma versão secundária. -Endereços vários problemas de telemetria e ajuda a equipa de produto para compreender melhor a forma como o produto está a ser utilizado. O conhecimento pode entrar em futuros esforços para melhorar a experiência de produto. 

Além disso, existem duas atualizações importantes:

- Corrigido um erro na preparação de dados, que impediu o inspector de séries de tempo a apresentação em pacotes de preparação de dados.
- Na ferramenta de linha de comandos, já não precisam de ser um proprietário da subscrição do Azure para aprovisionar clusters de ACS de computação do Machine Learning. 

## <a name="2017-12-sprint-2"></a>2017-12 (sprint 2)
**Número de versão**: 0.1.1711.15263 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;([localizar a versão](../service/known-issues-and-troubleshooting-guide.md#find-the-workbench-build-number))

Bem-vindo à actualização terceira do Azure Machine Learning. Esta atualização inclui melhoramentos na aplicação workbench, a Interface de linha de comandos (CLI) e os serviços de back-end. Obrigado muito para enviar os smiles e frowns. Muitos das seguintes atualizações que são efetuados como diretos resultados dos seus comentários. 

**Funcionalidades novas importantes**
- [Suporte para o SQL Server e BD SQL do Azure como uma origem de dados](data-prep-appendix2-supported-data-sources.md#types) 
- [Aprendizagem profunda Spark com suporte para a GPU utilizando MMLSpark](https://github.com/Azure/mmlspark/blob/master/docs/gpu-setup.md)
- [Todos os contentores AML são compatíveis com dispositivos de limite de IoT do Azure quando implementado (não existem passos adicionais necessários)](http://aka.ms/aml-iot-edge-blog)
- Portal do Azure disponível de vistas de lista de modelo registado e os detalhes
- Aceder aos destinos de computação, utilizando a autenticação baseada em chave SSH para além de acesso baseado em nome de utilizador/palavra-passe. 
- Novo Inspector de frequência de padrão nos dados de preparação experiência. 

**As atualizações de detalhado** segue-se uma lista das atualizações de detalhado cada área de componente do Azure Machine Learning neste sprint.

### <a name="installer"></a>Instalador
- Instalador pode atualização automática, de modo a que erros correções e novas funcionalidades podem ser suportadas sem necessidade de reinstalá-lo do utilizador

### <a name="workbench-authentication"></a>Autenticação do Workbench
- Várias correções para o sistema de autenticação. Indique se ainda ocorrerem problemas de início de sessão.
- Alterações de IU que tornam mais fácil localizar as definições do Gestor de Proxy.

### <a name="workbench"></a>Workbench
- Vista de ficheiro só de leitura tem agora em segundo plano azul leve
- Botão Editar movido para a direita para torná-lo mais Detetáveis.
- "dsource", "dprep" e "ipynb" formatos de ficheiro agora podem ser compostos num formato de texto em bruto
- O workbench tem agora uma nova experiência de edição guias de utilizadores para utilizar IDEs externo para editar os scripts e utilize Workbench apenas para editar os tipos de ficheiros que tenham uma experiência avançada de edição (por exemplo, blocos de notas, origens de dados, pacotes de preparação de dados)
- Carregar a lista de áreas de trabalho e projetos de que o utilizador tem acesso ao é significativamente mais rápido agora

### <a name="data-preparation"></a>Preparação de dados 
- Um padrão de frequência Inspector para ver os padrões de cadeia numa coluna. Também pode filtrar os dados utilizando estes padrões. Isto mostra-lhe uma vista semelhante inspector contagens de valor. A diferença é que a frequência de padrão mostra o número de padrões de exclusivos dos dados, em vez de contagens de dados exclusivos. Também pode filtrar a entrada ou saída todas as linhas que se enquadram em determinados padrão.

![Imagem de inspector de frequência de padrão no número de produto](media/azure-machine-learning-release-notes/pattern-inspector-product-number.png)

- Melhorias de desempenho ao Recomendamos casos de contorno para rever na transformação 'deriva, por exemplo, o coluna'

- [Suporte para o SQL Server e BD SQL do Azure como uma origem de dados](data-prep-appendix2-supported-data-sources.md#types) 

![Imagem da criação de uma nova origem de dados do SQL server](media/azure-machine-learning-release-notes/sql-server-data-source.png)

- Ativado "rapidamente" vista de contagens de linha e coluna

![Imagem da contagem de colunas de linha numa tolerância](media/azure-machine-learning-release-notes/row-col-count.png)

- Preparação de dados está ativada em todos os contextos de computação
- Origens de dados que utilizam uma base de dados do SQL Server estão ativadas em todos os contextos de computação
- Dados de preparação grelha colunas podem ser filtradas pelo tipo de dados
- Fixo problema com a conversão de várias colunas à data
- Corrigido o problema que o utilizador selecionar a coluna de resultado como uma origem derivar coluna por exemplo se o utilizador alterar o nome de coluna de saída no modo avançado.

### <a name="job-execution"></a>Execução da tarefa
Agora pode criar e aceder a um destino de computação de tipo remotedocker ou cluster utilizando a autenticação baseada em chave SSH seguindo estes passos:
- Anexar um destino de computação utilizando o seguinte comando na CLI

    ```azure-cli
    $ az ml computetarget attach remotedocker --name "remotevm" --address "remotevm_IP_address" --username "sshuser" --use-azureml-ssh-key
    ```
>[!NOTE]
>Especifica uma opção -k (ou - utilização-azureml--chave ssh) no comando para gerar e utilizar a chave SSH.

- Azure ML Workbench irá gerar uma chave pública e de saída que na sua consola. Inicie sessão no destino computação utilizando o mesmo nome de utilizador e de acréscimo ~/.ssh/authorized_keys ficheiros com esta chave pública.

- Pode preparar este destino de computação e utilizá-lo para execução e do Azure ML Workbench esta chave será utilizada para autenticação.  

Para obter mais informações sobre a criação de destinos de computação, consulte [configurar o serviço do Azure Machine Learning experimentação](experimentation-service-configuration.md)

### <a name="visual-studio-tools-for-ai"></a>Visual Studio Tools para IA
- Foi adicionado suporte para [Visual Studio Tools para AI](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.vstoolsai-vs2017). 

### <a name="command-line-interface-cli"></a>Interface de Linha de Comandos (CLI)
- Adicionar `az ml datasource create` o comando permite criar um ficheiro de origem de dados a partir da linha de comandos

### <a name="model-management-and-operationalization"></a>Gestão de modelo e Operationalization
- [Todos os contentores AML são compatíveis com dispositivos de limite de IoT do Azure quando operationalized (não existem passos adicionais necessários)](http://aka.ms/aml-iot-edge-blog) 
- Melhoramentos de mensagens de erro a CLI o16n
- Correções de erros no portal de gestão de modelo UX  
- Letra consistente tem maiúsculas e minúsculas para atributos de gestão de modelo na página de detalhes
- Em tempo real da classificação de tempo limite de chamadas definido como 60 segundos
- Modelo registado lista e os detalhes vistas disponíveis no portal do Azure

![detalhes de modelo no portal](media/azure-machine-learning-release-notes/model-list.jpg)

![Descrição geral do modelo no portal](media/azure-machine-learning-release-notes/model-overview-portal.jpg)

### <a name="mmlspark"></a>MMLSpark
- Ligação avançada Learning no Spark com [suporte GPU](https://github.com/Azure/mmlspark/blob/master/docs/gpu-setup.md)
- Suporte para modelos do Resource Manager para a implementação de recursos mais fácil
- Suporte para o ecossistema de SparklyR
- [Integração de AZTK](https://github.com/Azure/aztk/wiki/Spark-on-Azure-for-Python-Users#optional-set-up-mmlspark)

### <a name="sample-projects"></a>Projetos de exemplo
- [Iris](https://github.com/Azure/MachineLearningSamples-Iris) e [MMLSpark](https://github.com/Azure/mmlspark) amostras atualizadas com a nova versão do SDK do Azure ML

### <a name="breaking-changes"></a>As alterações de última hora
- Promovido a `--type` no comutador `az ml computetarget attach` para um subcomando. 

    - `az ml computetarget attach --type remotedocker` é agora `az ml computetarget attach remotedocker`
    - `az ml computetarget attach --type cluster` é agora `az ml computetarget attach cluster`

## <a name="2017-11-sprint-1"></a>2017-11 (sprint 1) 
**Número de versão**: 0.1.1710.31013 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;([localizar a versão](../service/known-issues-and-troubleshooting-guide.md#find-the-workbench-build-number))

Nesta versão, fizemos melhoramentos em torno da segurança, estabilidade e maintainability na aplicação workbench, a CLI e a camada de serviços de back-end. Obrigado muito para enviar-nos smiles e frowns. Muitas do abaixo as atualizações que são efetuados como diretos resultados dos seus comentários. Mantê-las provenha!

### <a name="notable-new-features"></a>Funcionalidades novas importantes
- Azure ML está agora disponível em duas regiões do Azure novo: **Europa Ocidental** e **Sudeste asiático**. Que se associam as regiões anteriores do **EUA Leste 2**, **Central EUA oeste**, e **leste da Austrália**, colocar o número total de implementado regiões cinco.
- Iremos ativar a sintaxe de código Python realce na aplicação do Workbench para tornar mais fácil ler e editar o código de origem do Python. 
- Agora pode iniciar a sua IDE favorito diretamente a partir de um ficheiro, em vez do projeto todo.  A abertura de um ficheiro no Workbench e, em seguida, clicando em "Editar" inicia o IDE (atualmente VS Code e PyCharm são suportados) para o ficheiro atual e o projeto.  Também pode clicar na seta junto ao botão Editar para editar o ficheiro no editor de texto do Workbench.  Os ficheiros são só de leitura até que clique em Editar, impedir alterações acidentais.
- A biblioteca de desenho popular `matplotlib` versão 2.1.0 agora vem incluído com a aplicação do Workbench.
- A versão do .NET Core foi atualizado para 2.0 para o motor de preparação de dados. Isto remover o requisito de instalação brew openssl durante a instalação da aplicação no macOS. É também paves a forma de dados mais e extraordinária funcionalidades preparação fique num futuro próximo. 
- Iremos tiver ativado uma home page de aplicação específico da versão, para obter mais relevantes notas de versão e pedidos com base na sua versão atual da aplicação de atualização.
- Se o seu nome de utilizador local tem um espaço no mesmo, a aplicação pode agora ser instalada com êxito. 

### <a name="detailed-updates"></a>Atualizações de detalhado
Segue-se uma lista das atualizações de detalhado cada área de componente do Azure Machine Learning neste sprint.

#### <a name="installer"></a>Instalador
- O instalador de aplicação limpa agora o diretório de instalação criado por uma versão anterior da aplicação.
- Corrigido um erro que reconduz a acumular-a 100% no macOS Sierra elevada do instalador.
- Agora é uma ligação direta para o diretório do installer do utilizador rever os registos do instalador no caso de falha de instalação.
- Instale agora funciona para os utilizadores que tenham espaço no respetivo nome de utilizador.

#### <a name="workbench-authentication"></a>Autenticação do Workbench
- Suporte para a autenticação no Gestor de Proxy.
- O registo é bem sucedida se o utilizador é protegido por uma firewall. 
- Se o utilizador tiver contas de experimentação em várias regiões do Azure e se uma região acontecer fique indisponível, já não bloqueia a aplicação.
- Quando a autenticação não foi concluída e a caixa de diálogo de autenticação está ainda visível, aplicação tenta já não carregar área de trabalho a partir da local cache.

#### <a name="workbench-app"></a>Aplicação do Workbench
- Realce da sintaxe de código de Python está ativada no editor de texto.
- O botão Editar no editor de texto permite-lhe editar o ficheiro tem um IDE (VS Code PyCharm são suportados e) ou no editor de texto incorporado.
- Editor de texto está no modo só de leitura, por predefinição. 
- Guarde as alterações de agora do botão visual estado desativado depois do ficheiro atual é guardado e, por conseguinte, já não está modificado.
- Guarda o Workbench _todos os_ guardadas ficheiros quando iniciar uma execução.
- Workbench memorizou que o último utilizado a área de trabalho no computador local, pelo que é aberta automaticamente.
- Apenas uma única instância do Workbench agora é permitida a execução. Anteriormente podem ser executadas várias instâncias que provocou a problemas quando a funcionar no mesmo projeto.
- Menu ficheiro cujo nome foi alterado "Abrir projeto..." para "Adicionar existente pasta como projeto de..." 
- Mudar de separador está muito mais rápida.
- Ligações de ajuda são adicionadas à caixa de diálogo Configurar IDE.
- O formulário de comentários memorizou agora o endereço de e-mail que introduziu a última vez.
- Área de texto de formulário smiles e frowns agora é maior, pelo que pode enviar-nos comentários mais! 
- O `--owner` mudar texto de ajuda na `az ml workspace create` seja corrigido.
- Adicionámos uma caixa de diálogo "Sobre" para ajudar o utilizador facilmente ver e copiar o número de versão da aplicação.
- Um item de menu "Sugerir uma funcionalidade" é adicionado ao menu de ajuda.
- Nome da conta experimentação agora é visível no título da aplicação de barra, precedente o nome da aplicação "Do Azure Machine Learning Workbench".
- Uma home page específico da versão de aplicação é apresentado baseia-se agora na versão da aplicação detetada.

#### <a name="data-preparation"></a>Preparação de dados 
- Já não podem ser carregadas externo do web site do mapa Inspector para impedir potenciais problemas de segurança.
- Inspetores histograma e contagem de valores tem agora a opção para apresentar o gráfico na escala logarítmica.
- Quando um cálculo está em curso, barra de qualidade de dados mostra agora uma cor diferentes para assinalar o estado "calcular".
- Métricas de coluna agora mostram as estatísticas de colunas de valor categórico.
- O último caráter no nome da origem de dados já não é truncado.
- Pacote de preparação de dados agora permanece aberta quando mudar separadores, resultando ganhos de desempenho percetível.
- Na origem de dados, quando alternar entre a vista de dados e a vista de métrica, a ordem das colunas agora já é alterado.
- Abrir um inválido `.dprep` ou `.dsource` ficheiro já não faz com que Workbench para falhas.
- Pacote de preparação de dados pode agora utiliza o caminho relativo para o resultado no _escrever CSV_ transformação.
- _Manter a coluna_ transformação agora permite ao utilizador adicionar colunas adicionais quando editado.
- _Substitua_ menu agora, na verdade, inicia _substitua o valor_ caixa de diálogo.
- _Substitua o valor_ transformar agora funções conforme esperado em vez de gerar o erro.
- Pacote de preparação de dados utiliza agora o caminho absoluto quando referenciar os ficheiros de dados fora da pasta do projeto, tornando a possível executar o pacote no contexto local com o caminho absoluto para o ficheiro de dados.
- _Ficheiro completo_ como uma estratégia de amostragem é agora suportada ao utilizar o Azure blob como origem de dados.
- Gerar código Python (a partir do pacote de preparação de dados) acarreta agora CR e LF, tornando amigável no Windows.
- _Escolha as métricas_ pendente oculta agora propriedade ao mudar para vista de dados.
- Workbench pode agora processo parquet ficheiros, mesmo quando está a utilizar runtime do Python. Anteriormente Spark só pode ser utilizado quando o processamento de ficheiros de parquet. 
- Filtrar valores numa coluna com _data_ tipo de dados já não provoca o motor de preparação de dados para falhas.
- Vista de métrica agora respeita a atualizações de estratégia de amostragem.
- Remoto amostragem tarefas agora funciona corretamente.

#### <a name="job-execution"></a>Execução da tarefa
- O argumento está agora incluído no registo de histórico de execução.
- Tarefas arrancou CLI agora aparece no painel de tarefas de histórico de execução automaticamente.
- Painel de tarefas mostra agora as tarefas criadas por utilizadores convidados adicionados ao inquilino do Azure AD.
- Cancelar do painel de tarefas e ações de eliminação são mais estáveis.
- Ao clicar no botão de execução, a mensagem de erro é acionada agora se os ficheiros de configuração estão no formato incorreto.
- Aplicação terminação já não interfere com tarefas arrancou na CLI.
- Tarefas arrancou CLI agora continua a spit saída standard out, mesmo após uma hora de execução.
- Melhor mensagens de erro são apresentadas quando ocorre uma falha do pacote de preparação de dados executar no Python/PySpark.
- `az ml experiment clean` Limpa agora imagens de Docker na VM remoto bem.
- `az ml experiment clean` agora funciona corretamente para o destino local no macOS.
- Mensagens de erro quando executa filtragem Docker local ou remoto são limpos cópias de segurança e mais fáceis de ler.
- Melhor mensagem de erro é apresentada quando o nome de nó principal do cluster de HDInsight não está formatado corretamente quando ligado como um destino de execução.
- Melhor mensagem de erro é apresentada quando o segredo não foi encontrado no serviço de credencial. 
- Biblioteca de MMLSpark é atualizada para suportar o Apache Spark 2.2.
- MMLSpark incluem agora o requerente codificação transformação (Mesh codificação) para documentos médicas.
- `matplotlib` versão 2.1.0 está agora incluída fora os caixa com Workbench.

#### <a name="jupyter-notebook"></a>Bloco de Notas do Jupyter
- Pesquisa de nomes do bloco de notas agora funciona corretamente na vista de blocos de notas.
- Agora pode eliminar um bloco de notas na vista de blocos de notas.
- Magic novo `%upload_artifact` é adicionada para carregar ficheiros produzidos no ambiente de execução do bloco de notas para arquivo de dados do histórico de execução.
- Erros de kernel agora estão anexados no estado de tarefa do bloco de notas para depuração mais fácil.
- Servidor do Jupyter agora corretamente encerrado quando o utilizador inicia sessão fora da aplicação.

#### <a name="azure-portal"></a>Portal do Azure
- Conta de experimentação e conta de gestão de modelo agora podem ser criadas em duas regiões do Azure novo: Europa Ocidental e Sudeste asiático.
- Esquema de DevTest da conta de gestão de modelo agora só está disponível quando este é o primeiro a ser criado na subscrição. 
- Ligação de ajuda no portal do Azure é atualizada para apontar para a página de documentação correto.
- Campo de descrição é removido da página de detalhes do Docker imagem, uma vez que não é aplicável.
- Foram adicionados detalhes, incluindo as definições de AppInsights e dimensionamento automático para a página de detalhes do serviço web.
- Página de gestão de modelo compõe agora mesmo cookies de terceiros estão desativados no browser. 

#### <a name="operationalization"></a>Operationalization
- Serviço Web com "pontuação" no nome já não irá falhar.
- Utilizador pode agora criar um ambiente de implementação com apenas o acesso de contribuinte a um grupo de recursos do Azure ou a subscrição. Já não é necessário acesso de proprietário para a subscrição completa.
- Operationalization CLI agora gostar separador-conclusão automática no Linux.
- Serviço de construção de imagem suporta agora criar imagens para serviços/dispositivos do IoT do Azure.

#### <a name="sample-projects"></a>Projetos de exemplo
- [_Classificar Iris_ ](./tutorial-classifying-iris-part-1.md) projeto de exemplo:
    - `iris_pyspark.py` é mudado para `iris_spark.py`.
    - `iris_score.py` é mudado para `score_iris.py`.
    - `iris.dprep` e `iris.dsource` são atualizadas para refletir as atualizações de motor de preparação de dados mais recentes.
    - `iris.ipynb` Bloco de notas é corrigido para funcionar no cluster do HDInsight.
    - Histórico de execução estiver ativado no `iris.ipynb` célula de bloco de notas.
- [_Avançadas preparação de dados utilizando os dados de partilha de bicicleta_ ](./tutorial-bikeshare-dataprep.md) projeto de exemplo passo "A lidar com o erro Value" fixo.
- [_MMLSpark para adultos Census dados_ ](https://github.com/Azure/MachineLearningSamples-mmlspark) projeto de exemplo `docker.runconfig` formato atualizado de JSON para YAML.
- [_Distribuída Hyperparameter otimização_ ](./scenario-distributed-tuning-of-hyperparameters.md) projeto de exemplo`docker.runconfig` formato atualizado de JSON para YAML.
- Novo projeto de exemplo [ _classificação imagem utilizando CNTK_](./scenario-image-classification-using-cntk.md).


## <a name="2017-10-sprint-0"></a>2017 10 (sprint 0) 
**Número de versão**: 0.1.1710.31013 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;([localizar a versão](../service/known-issues-and-troubleshooting-guide.md#find-the-workbench-build-number))

Bem-vindo à primeira atualização do Azure Machine Learning Workbench seguir a nossa pré-visualização pública inicial da Conferência Microsoft Ignite de 2017. As principais atualizações nesta versão estão fiabilidade e estabilização de correções.  Alguns dos problemas críticos que iremos abordados incluem:

### <a name="new-features"></a>Novas funcionalidades
- é agora suportado macOS Sierra elevada

### <a name="bug-fixes"></a>Correções de erros
#### <a name="workbench-experience"></a>Experiência de Workbench
- Arrastar e largar um ficheiro para o Workbench faz com que o Workbench para falhas.
- A janela de terminal no VS Code configurado como um IDE para o Workbench não reconhece _az ml_ comandos.

#### <a name="workbench-authentication"></a>Autenticação do Workbench
Iremos tomado uma série de atualizações para melhorar a vários problemas de início de sessão e autenticação relatados.
- Janela de autenticação mantém popping cópia de segurança, especialmente quando a ligação à Internet não está estável.
- Problemas de Fiabilidade melhorada em torno da expiração do token de autenticação.
- Em alguns casos, a janela de autenticação é apresentado duas vezes.
- Janela principal do Workbench continua a apresentar "autenticar" mensagem quando tiver concluído o processo de autenticação e a caixa de diálogo de pop-up já dispensada.
- Se não houver nenhuma ligação à Internet, a caixa de diálogo de autenticação, aparece com um ecrã branco.

#### <a name="data-preparation"></a>Preparação de dados 
- Quando um valor específico é filtrado, erros e os valores em falta são também filtrados.
- Alterar uma estratégia de amostragem remove subsequentes operações de associação existente.
- Substituir o valor em falta a transformação não tem NaN em consideração.
- A inferência de tipo data emite exceção quando encontrado valor nulo.

#### <a name="job-execution"></a>Execução da tarefa
- Não há nenhuma mensagem de erro encriptado quando não consegue carregar a pasta do projeto porque excedia o limite de tamanho de execução da tarefa.
- Se o script de Python do utilizador altera o diretório de trabalho, os ficheiros escritos para pastas de saídas não são registados. 
- Se for diferente daquela projeto atual pertence à subscrição do Azure Active Directory, a submissão da tarefa resulta um erro 403.
- Quando Docker não estiver presente, nenhuma mensagem de erro encriptado é devolvida se o utilizador tenta utilizar o Docker como um destino de execução.
- ficheiro .runconfig não é guardado automaticamente quando o utilizador clica na _executar_ botão.

#### <a name="jupyter-notebook"></a>Bloco de Notas do Jupyter
- Não é possível iniciar o servidor de bloco de notas se o utilizador utiliza com certos tipos de início de sessão.
- Mensagens de erro de servidor do bloco de notas não superfície nos registos visíveis ao utilizador.

#### <a name="azure-portal"></a>Portal do Azure
- Selecionar o tema escuro do portal do Azure faz com que o painel de gestão de modelo a apresentar como uma caixa negra.

#### <a name="operationalization"></a>Operationalization
- Reutilizar um manifesto para atualizar um serviço web faz com que uma nova imagem de Docker criada com um nome aleatório.
- Não não possível obter os registos de serviço Web do Kubernetes cluster.
- Enganosa mensagem de erro está impresso quando o utilizador tentou criar uma conta de gestão de modelo ou de uma conta de computação de ML e detetar problemas de permissões.

## <a name="next-steps"></a>Passos Seguintes

Leia a descrição geral do [Azure Machine Learning](../service/overview-what-is-azure-ml.md).