---
title: Aprovisionar a máquina de Virtual de ciência de dados Windows no Azure | Documentos da Microsoft
description: Configurar e criar uma máquina de Virtual de ciência de dados no Azure para análise e machine learning.
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.assetid: e1467c0f-497b-48f7-96a0-7f806a7bec0b
ms.service: machine-learning
ms.component: data-science-vm
ms.workload: data-services
ms.devlang: na
ms.topic: article
ms.date: 09/10/2017
ms.author: gokuma
ms.openlocfilehash: b749d8a904bc40eba3346cc03d9274236380c80d
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39449758"
---
# <a name="provision-the-windows-data-science-virtual-machine-on-azure"></a>Aprovisionar a máquina de Virtual de ciência de dados Windows no Azure
A máquina de Virtual de ciência de dados do Microsoft é uma imagem de máquina virtual (VM) do Windows Azure previamente instalado e configurado com várias ferramentas populares que são frequentemente utilizadas para análise de dados e machine learning. As ferramentas incluídas são:

* [Azure Machine Learning](../service/index.yml) Bancada de trabalho
* [Microsoft Machine Learning Server](https://docs.microsoft.com/machine-learning-server/index) Developer Edition
* Distribuição do anaconda Python
* Bloco de notas do Jupyter (com o R, Python, PySpark kernels)
* Edição de Comunidade do Visual Studio
* Ambiente de trabalho do Power BI
* SQL Server 2017 Developer Edition
* Instância do Spark de autónomo para o local de desenvolvimento e teste
* [JuliaPro](https://juliacomputing.com/products/juliapro.html)
* Machine learning e ferramentas de análise de dados
  * Profunda estruturas de aprendizagem: um conjunto avançado de fotogramas de IA, incluindo [Microsoft Cognitive Toolkit](https://www.microsoft.com/en-us/cognitive-toolkit/), [TensorFlow](https://www.tensorflow.org/), [Chainer](https://chainer.org/), mxNet, Keras estão incluídas na VM.
  * [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit): um rápido de machine learning sistema dar suporte a técnicas, como online, hash, allreduce, reduções, learning2search, Active Directory e a aprendizagem interativa.
  * [XGBoost](https://xgboost.readthedocs.org/en/latest/): uma ferramenta, fornecendo a implementação de árvore precisos e de elevada.
  * [Rattle](http://rattle.togaware.com/) (o R analítico ferramenta para saber mais facilmente): uma ferramenta que simplifica a introdução à análise de dados e aprendizagem automática no R fácil. Ele inclui a exploração e modelação com a geração automática de código R dados baseados em GUI.
  * [Weka](http://www.cs.waikato.ac.nz/ml/weka/) : uma extração de dados visual e o software em Java de aprendizagem automática.
  * [Apache Drill](https://drill.apache.org/): um motor de consulta de SQL sem esquema para o Hadoop, NoSQL e armazenamento na Cloud.  Oferece suporte a interfaces ODBC e JDBC para permitir a consulta NoSQL e ficheiros a partir de ferramentas de BI padrão, como o Power BI, o Excel, o Tableau.
* Bibliotecas em R e Python para utilizam no Azure Machine Learning e outros serviços do Azure
* Git, incluindo o Git Bash para trabalhar com repositórios de código de origem incluindo o GitHub, Visual Studio Team Services e oferece várias Linux da linha de comandos utilitários populares (incluindo awk, PO, perl, grep, find, wget, curl, etc.) acessível no git bash e o comando linha de comandos. 

Fazer ciência de dados envolve a iteração numa seqüência de tarefas:

1. Encontrar, para carregar e processar previamente os dados
1. Criar e testar modelos
1. Implementação de modelos para consumo em aplicações inteligentes

Os cientistas de dados usar uma variedade de ferramentas para concluir estas tarefas. Pode ser muito demorada para encontrar as versões apropriadas do software e, em seguida, transferir e instalá-los. A máquina de Virtual de ciência de dados do Microsoft pode facilitar este fardo, fornecendo uma imagem de prontos a utilizar que pode ser aprovisionada no Azure com todas as ferramentas de populares vários previamente instalado e configurado. 

A máquina de Virtual de ciência de dados do Microsoft sejam o projeto da análise. Permite-lhe trabalhar em tarefas em várias linguagens, incluindo o R, Python, SQL e c#. O Visual Studio oferece um IDE para desenvolver e testar seu código que é fácil de usar. O SDK do Azure incluídos na VM permite-lhe criar as suas aplicações com vários serviços de plataforma de cloud da Microsoft. 

Existem não existem custos de software para esta imagem VM de ciência de dados. Só paga as tarifas de utilização do Azure que depende o tamanho da máquina virtual, aprovisionar. Obter mais detalhes sobre as tarifas de computação podem ser encontradas na seção de detalhes de preços no [máquina de Virtual de ciência de dados](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.windows-data-science-vm?tab=PlansAndPrice) página. 

## <a name="other-versions-of-the-data-science-virtual-machine"></a>Outras versões da máquina de Virtual de ciência de dados
Uma [Ubuntu](dsvm-ubuntu-intro.md) imagem está disponível, com diversas ferramentas similares e estruturas de aprendizagem profunda adicionais alguns. R [CentOS](linux-dsvm-intro.md) imagem também está disponível. Também oferecemos um [edition do Windows Server 2012](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.standard-data-science-vm) da máquina de virtual de ciência de dados mas algumas ferramentas só estão disponíveis na edição do Windows Server 2016.  Caso contrário, este artigo também se aplica para a edição do Windows Server 2012.

## <a name="prerequisites"></a>Pré-requisitos
Antes de poder criar uma máquina de Virtual de ciência de dados do Microsoft, tem de ter o seguinte:

* **Uma subscrição do Azure**: para obter uma, veja [versão de avaliação gratuita do Azure obter](http://azure.com/free).


## <a name="create-your-microsoft-data-science-virtual-machine"></a>Criar a sua máquina de Virtual de ciência de dados da Microsoft
Para criar uma instância da Máquina Virtual Microsoft dados ciência, siga estes passos:

1. Navegue para a máquina virtual listagem no [portal do Azure](https://portal.azure.com/#create/microsoft-ads.windows-data-science-vmwindows2016).
1. Selecione o **Create** na parte inferior para ser levados num assistente.![ Configurar-data--vm de ciência](./media/provision-vm/configure-data-science-virtual-machine.png)
1. O assistente utilizado para criar a máquina de Virtual de ciência de dados do Microsoft requer **entradas** para cada um da **quatro passos** enumerado à direita da figura. Seguem-se as entradas necessárias para configurar cada um destes passos:
   
   1. **Noções básicas**
      
      1. **Nome**: nome do seu servidor de ciência de dados que está a criar.
      1. **Tipo de disco VM**: escolher entre o SSD ou HDD. Para NC_v1 GPU instância (NVidia Tesla K80 com base), escolha **HDD** como o tipo de disco. 
      1. **Nome de utilizador**: id de início de sessão da conta de administrador.
      1. **Palavra-passe**: palavra-passe da conta de administrador.
      1. **Subscrição**: Se tiver mais de uma subscrição, selecione aquele no qual a máquina está a ser criado e faturadas.
      1. **Grupo de recursos**: pode criar um novo ou utilizar um grupo existente.
      1. **Localização**: selecionar o Datacenter mais adequada. Normalmente, é o Centro de dados que tem a maior parte dos seus dados ou mais se aproxima-se à sua localização física para acesso de rede mais rápido.
   1. **Tamanho**: selecione um dos tipos de servidor que se adequa ao requisito funcional e restrições de custo. Pode obter mais opções de tamanhos de VM ao selecionar "View All".
   1. **Definições**:
      
      1. **Utilizar Managed Disks**: escolha geridos se pretender que o Azure para gerir os discos da VM.  Caso contrário, terá de especificar uma conta de armazenamento nova ou existente. 
      1. **Outros parâmetros**: geralmente, simplesmente usar os valores predefinidos. Se pretender considerar o uso de valores não predefinidos, coloque o cursor sobre a ligação informativa para obter ajuda sobre campos específicos.
    a. **Resumo**: Certifique-se de que todas as informações que introduziu estão corretas e clique em **criar**. **Tenha em atenção**: A VM não tem quaisquer custos adicionais para além de computação para o tamanho de servidor que selecionou no **tamanho** passo. 

> [!NOTE]
> O aprovisionamento deve demorar cerca de 10 a 20 minutos. O estado do provisionamento é apresentado no portal do Azure.
> 
> 

## <a name="how-to-access-the-microsoft-data-science-virtual-machine"></a>Como acessar a máquina de Virtual de ciência de dados do Microsoft
Depois da VM é criada, o ambiente de trabalho remoto, pode nela, usando as credenciais da conta de administrador que configurou no anterior **Noções básicas** secção. 

Assim que a VM é criada e provisionada, está pronto para começar a utilizar as ferramentas de que estão instaladas e configuradas no mesmo. Existem mosaicos do menu Iniciar e ícones para muitas das ferramentas. 


## <a name="tools-installed-on-the-microsoft-data-science-virtual-machine"></a>Ferramentas instaladas numa máquina Virtual Microsoft Data Science



### <a name="microsoft-ml-server-developer-edition"></a>Microsoft ML Server Developer Edition
Se pretender utilizar bibliotecas do Microsoft enterprise para dimensionável R ou Python para a sua análise, a VM tem o Microsoft ML Server Developer edition (anteriormente conhecido como Microsoft R Server) instalado. Microsoft ML Server é uma plataforma de análise de nível empresarial abrangente implantável disponível para R e Python e escalável, comercialmente suportada e seguro. Com uma variedade de estatísticas de macrodados, modelação preditiva e capacidades de machine learning, o servidor de ML suporta uma gama completa de análise – exploração, análise, visualização e modelação. Ao usar e estender o código-fonte aberto R e Python, Microsoft ML Server é totalmente compatível com R / Python scripts, funções e CRAN / pip / aumentar de pacotes de Conda, para analisar dados em enterprise. Também aborda as limitações de memória do R de código aberto ao adicionar o processamento paralelo e em partes de dados. Isto permite-lhe executar análises em dados muito maiores do que o que se encaixa na memória principal.  Visual Studio Community Edition incluído na VM contém as ferramentas R para Visual Studio e o Python tools para extensão do Visual Studio que oferece um IDE completo para trabalhar com R ou Python. Também fornecemos outros IDEs, bem como [RStudio](http://www.rstudio.com) e [edição de Comunidade PyCharm](https://www.jetbrains.com/pycharm/) na VM. 

### <a name="python"></a>Python
Para o desenvolvimento com o Python, distribuição de Anaconda Python 2.7 e 3.6 foi instalada. Essa distribuição contém o Python base juntamente com aproximadamente 300 dos pacotes de análises de dados, engenharia e matemática mais populares. Pode utilizar ferramentas do Python para Visual Studio (das PTVS) que for instalado dentro da edição do Visual Studio 2017 Community ou um dos IDEs agrupados com Anaconda como INATIVA ou Spyder. Pode iniciar um dos seguintes procurando na barra de pesquisa (**ganhar** + **S** chave).

> [!NOTE]
> Para apontar as ferramentas Python para Visual Studio em Anaconda Python 2.7, terá de criar ambientes personalizados para cada versão. Para definir esses caminhos de ambiente em que o Visual Studio 2017 Community Edition, navegue para **ferramentas** -> **ferramentas do Python** -> **ambientes do Python**e, em seguida, clique em **+ personalizado**. 
> 
> 

Anaconda Python 3.6 é instalado em C:\Anaconda e Anaconda Python 2.7 é instalado em c:\Anaconda\envs\python2. Ver [documentação das PTVS](/visualstudio/python/installing-python-interpreters.md) para obter passos detalhados. 

### <a name="jupyter-notebook"></a>Bloco de Notas do Jupyter
Distribuição de anaconda também vem com um bloco de notas do Jupyter, um ambiente para partilhar código e análise. Um servidor de bloco de notas do Jupyter tem sido pré-configuradas com o Python 2.7, Python 3.x, o kernel do PySpark, Julia e R. Há um ícone de área de trabalho com o nome "Bloco de notas Jupyter" para iniciar o servidor do Jupyter e iniciar o browser para aceder ao servidor de bloco de notas. 

Podemos ter compactado vários blocos de notas do exemplo em Python e em R. Os blocos de notas do Jupyter mostram como trabalhar com o Microsoft ML Server, serviços do SQL Server ML (análise na base de dados), Python, Microsoft Cognitive ToolKit, Tensorflow e outras tecnologias do Azure, quando acessar Jupyter. Pode ver a ligação para os exemplos na home page do bloco de notas depois de autenticar para o bloco de notas do Jupyter com a palavra-passe que criou no passo anterior. 

### <a name="visual-studio-2017-community-edition"></a>Visual Studio 2017 Community edition
Visual Studio Community edition instalado na VM. É uma versão gratuita do IDE popular da Microsoft que pode utilizar para fins de avaliação e para equipas de pequenas dimensões. Pode consultar os termos de licenciamento [aqui](https://www.visualstudio.com/support/legal/mt171547).  Abra o Visual Studio, fazendo duplo clique no ícone de área de trabalho ou o **iniciar** menu. Também pode procurar programas com **ganhar** + **S** e inserindo "Visual Studio". Uma vez aqui pode criar projetos em linguagens como c#, Python, R, node. js. Plug-ins são também instalados que seja conveniente para trabalhar com os serviços do Azure como o catálogo de dados do Azure, Azure HDInsight (Hadoop, Spark) e o Azure Data Lake. Agora, existe também um plug-in chamado ```Visual Studio Tools for AI``` forma totalmente integrada que integra-se ao Azure Machine Learning e ajuda-o rapidamente aplicações de ia de compilação. 

> [!NOTE]
> Poderá receber uma mensagem a indicar que o período de avaliação expirou. Introduza as credenciais da conta Microsoft ou criar uma nova conta gratuita para obter acesso ao Visual Studio Community Edition. 
> 
> 

### <a name="sql-server-2017-developer-edition"></a>Edição do SQL Server 2017 Developer
Uma versão de desenvolvedor do SQL Server 2017 com serviços de ML para executar a análise na base de dados é fornecida na VM em R ou Python. Serviços de ML fornecem uma plataforma para desenvolver e implementar aplicações inteligentes. Pode utilizar o rica e poderosa essas linguagens e muitos pacotes da Comunidade para criar modelos e gerar previsões de indisponibilidade para os seus dados do SQL Server. Pode manter analytics próximas dos dados porque os serviços de ML (na base de dados) integra-se o R e Python linguagem dentro do SQL Server. Isso elimina os custos e riscos de segurança associados de movimento de dados.

> [!NOTE]
> A edição de programador do SQL Server só pode ser utilizada para fins de teste e de desenvolvimento. Precisa de uma licença para executá-lo em produção. 
> 
> 

Pode acessar o SQL server, iniciando **SQL Server Management Studio**. O nome da VM é preenchido como o nome do servidor. Utilize a autenticação do Windows quando a sessão iniciada como administrador no Windows. Assim que estiver no SQL Server Management Studio pode criar outros utilizadores, criar bases de dados, importar dados e executar consultas SQL. 

Para ativar a análise de na base de dados com serviços de ML de SQL, execute o seguinte comando como uma ação no SQL Server management studio depois de iniciar sessão como administrador do servidor de tempo. 

        CREATE LOGIN [%COMPUTERNAME%\SQLRUserGroup] FROM WINDOWS 

        (Please replace the %COMPUTERNAME% with your VM name)


### <a name="azure"></a>Azure
Várias ferramentas do Azure são instaladas na VM:

* Existe um atalho de desktop para acessar a documentação do SDK do Azure. 
* **AzCopy**: utilizado para migrar dados de sua conta de armazenamento do Microsoft Azure. Para ver a utilização, escreva **Azcopy** no prompt de comando para ver a utilização. 
* **Explorador de armazenamento do Microsoft Azure**: utilizado para percorrer os objetos que tem armazenados dentro de sua conta de armazenamento do Azure e transfira dados para e do armazenamento do Azure. Pode digitar **Explorador de armazenamento** em pesquisar ou encontrá-lo no menu Iniciar do Windows para aceder a esta ferramenta. 
* **Adlcopy**: utilizado para mover dados para o Azure Data Lake. Para ver a utilização, escreva **adlcopy** numa linha de comandos. 
* **dtui**: utilizado para mover dados para e do Azure Cosmos DB, base de dados NoSQL na cloud. Tipo **dtui** no prompt de comando. 
* **O Azure Data Factory Integration Runtime**: permite que o movimento de dados entre origens de dados no local e na cloud. Ele é usado em ferramentas como o Azure Data Factory. 
* **Microsoft Azure Powershell**: uma ferramenta usada para administrar os recursos do Azure no Powershell linguagem de script também é instalada na sua VM. 

### <a name="power-bi"></a>Power BI
Para ajudar a criar dashboards e visualizações grandes, o **Power BI Desktop** foi instalado. Utilize esta ferramenta para extrair os dados de diferentes origens, para criar os seus dashboards e relatórios e publique-os para a cloud. Para obter informações, consulte a [Power BI](http://powerbi.microsoft.com) site. Pode encontrar o ambiente de trabalho do Power BI no menu Iniciar. 

> [!NOTE]
> Precisa de uma conta do Office 365 para aceder ao Power BI. 
> 
> 

### <a name="azure-machine-learning-workbench"></a>Azure Machine Learning Workbench

O Azure Machine Learning Workbench é um aplicativo de desktop e a interface de linha de comandos. A Bancada de trabalho tem de preparação de dados incorporada aprende os passos de preparação de dados à medida que os executa. Ele também fornece gerenciamento de projetos, histórico de execuções e integração de bloco de notas para reforçar a sua produtividade. Pode tirar partido das melhores arquiteturas de código aberto, incluindo o TensorFlow, Cognitive Toolkit, Spark ML e scikit-Saiba como desenvolver seus modelos. Em DSVM, fornecemos um ícone de área de trabalho para instalar o Azure Machine Learning workbench no diretório do utilizador individuais % LOCALAPPDATA %. Cada utilizador que precisa usar a Bancada de trabalho precisa para fazer uma vez que a ação de clicar duas vezes o ```AzureML Workbench Setup``` ícone de área de trabalho para instalar a sua instância da bancada de trabalho. O Azure Machine Learning também cria e utiliza um ambiente de Python de por utilizador que é extraído do % LOCALAPPDATA%\amlworkbench\python.

## <a name="additional-microsoft-development-tools"></a>Ferramentas de desenvolvimento da Microsoft adicionais
O [ **Microsoft Web Platform Installer** ](https://www.microsoft.com/web/downloads/platform.aspx) pode ser utilizada para detetar e transferir a outras ferramentas de desenvolvimento da Microsoft. Também é um atalho para a ferramenta fornecida no ambiente de trabalho de máquina de Virtual de ciência de dados do Microsoft.  

## <a name="important-directories-on-the-vm"></a>Diretórios importantes na VM
| Item | Diretório |
| --- | --- |
| Configurações de servidor de bloco de notas do Jupyter |C:\ProgramData\jupyter |
| Diretório de raiz de exemplos do bloco de notas do Jupyter |c:\dsvm\notebooks e c:\users\<nome de utilizador > \notebooks|
| Outros exemplos |c:\dsvm\samples |
| Anaconda (predefinição: Python 3.6) |c:\Anaconda |
| Ambiente do anaconda Python 2.7 |c:\Anaconda\envs\python2 |
| Microsoft ML Server autónomo Python  | C:\Program Files\Microsoft\ML Server\PYTHON_SERVER |
| Instância de predefinida R (ML servidor autónomo) |C:\Program Files\Microsoft\ML Server\R_SERVER |
| Diretório de instância na base de dados de serviços de ML de SQL |C:\Program Files\Microsoft SQL Server\MSSQL14. MSSQLSERVER |
| O Azure Machine Learning Workbench (por utilizador) | %localappdata%\amlworkbench | 
| Diversas ferramentas |c:\dsvm\tools |

> [!NOTE]
> Na edição do Windows Server 2012 da edição DSVM e Windows Server 2016 antes de Março de 2018, o ambiente de Anaconda predefinido é o Python 2.7. O ambiente secundário está localizado em c:\Anaconda\envs\py35 do Python 3.5. 
> 
> 

## <a name="next-steps"></a>Próximos Passos
Seguem-se alguns passos seguintes para continuar sua aprendizagem e a exploração. 

* Explore as várias ferramentas de ciência de dados na VM de ciência de dados ao clicar no menu Iniciar e verificar as ferramentas listadas no menu.
* Saiba mais sobre serviços do Azure Machine Learning e Bancada de trabalho, visitando o produto [página de início rápido e tutoriais](../service/index.yml). 
* Navegue para **C:\Program Files\Microsoft\ML Server\R_SERVER\library\RevoScaleR\demoScripts** para amostras que utilizem a biblioteca de pacotes RevoScaleR no R que suporta as análises de dados em escala empresarial.  
* Leia o artigo: [10 coisas que pode fazer na máquina Virtual de ciência de dados](http://aka.ms/dsvmtenthings)
* Aprenda a criar soluções de análise de ponta a ponta sistematicamente com o [Team Data Science Process](../team-data-science-process/index.yml).
* Visite o [Galeria de IA do Azure](http://gallery.cortanaintelligence.com) para machine learning e os dados analytics de exemplo que utilizam o Azure Machine learning e os dados relacionados de serviços no Azure. Também fornecemos um ícone no **iniciar** menu e, na área de trabalho da máquina virtual para essa galeria.

