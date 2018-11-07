---
title: Classificação de imagens aéreas | Documentos da Microsoft
description: Fornece instruções para o cenário do mundo real sobre a classificação de imagens aéreas
author: mawah
ms.author: mawah
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.topic: article
ms.service: machine-learning
ms.component: core
services: machine-learning
ms.workload: data-services
ms.date: 12/13/2017
ROBOTS: NOINDEX
ms.openlocfilehash: e71de2d543c8f29b7195e74d574855bcbe739162
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51260555"
---
# <a name="aerial-image-classification"></a>Classificação de imagens aéreas

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 



Este exemplo demonstra como utilizar o Azure Machine Learning Workbench para coordenar o treinamento distribuído e de operacionalização de modelos de classificação de imagens. Podemos usar duas abordagens para treinamento: (i) refinar uma rede neural profunda com um [do Azure Batch AI](https://docs.microsoft.com/azure/batch-ai/) cluster GPU e (ii) utilizando o [Microsoft Machine Learning para Apache Spark (MMLSpark)](https://github.com/Azure/mmlspark) pacote caracterização de imagens através de modelos de CNTK pré-preparadas com e para preparar classificadores usando os recursos derivados. Podemos, em seguida, aplicar os modelos treinados de forma paralela para conjuntos de imagem grande na cloud com uma [do Azure HDInsight Spark](https://azure.microsoft.com/services/hdinsight/apache-spark/) cluster, permitindo-nos aumentar a velocidade de treinamento e operacionalização ao adicionar ou remover nós de trabalho.

Este exemplo destaca as duas abordagens para transferir learning, que tira partido pré-preparadas com modelos para evitar os custos de redes neurais profundas de treinamento do zero. Reparametrização uma rede neural profunda normalmente requer a computação do GPU, mas pode levar a maior precisão, quando o conjunto de treinamento é suficientemente grande. Treinar um classificador simple nas imagens de caracterizadas não necessita de computação do GPU, é inerentemente rápida e dimensionável de forma arbitrária e se encaixa menos parâmetros. Este método, por conseguinte, é uma excelente opção quando alguns exemplos de treinamento estão disponíveis, como é muitas vezes o caso para casos de utilização personalizado. 

O cluster do Spark utilizado neste exemplo tem 40 nós de trabalho e os custos ~$40/hr operar; os recursos de cluster de IA do Batch incluem oito GPUs e custam ~$10/hr para operar. Concluir estas instruções demora cerca de três horas. Quando tiver terminado, siga as instruções de limpeza para remover os recursos que criou e deixar de incorrer em custos.

## <a name="link-to-the-gallery-github-repository"></a>Ligação para o repositório do GitHub de galeria

O repositório do GitHub público para este cenário do mundo real contém todos os materiais, incluindo exemplos de código, necessários para este exemplo:

[https://github.com/Azure/MachineLearningSamples-AerialImageClassification](https://github.com/Azure/MachineLearningSamples-AerialImageClassification)

## <a name="use-case-description"></a>Descrição de caso de utilização

Neste cenário, para preparar modelos de machine learning para classificar o tipo de ' s land mostrado nas imagens aéreas de gráficos de medidor 224 224 medidor x. Modelos de classificação de utilização de ' s Land podem ser utilizados para controlar urbanization, deforestation, perda de wetlands e outras principais tendências ambientais com periodicamente coletadas imagens aéreas. Podemos ter preparado os conjuntos de formação e a validação de imagem com base nas imagens dos EUA Programa de imagens de agricultura nacionais e de ' s land utilizam etiquetas publicadas pelo Governo Base de dados do National terra capa. Imagens de exemplo em cada classe de utilização de ' s land são mostradas aqui:

![Regiões de exemplo para cada ' s land utilizam etiqueta](media/scenario-aerial-image-classification/example-labels.PNG)

Depois de treinamento e a validar o modelo de classificador, podemos irá aplicá-la para imagens aéreas expansão Middlesex County, MA – Nova Inglaterra acordo de investigação da página inicial da Microsoft e Centro de desenvolvimento (SUPLENTES) – para demonstrar como esses modelos podem ser utilizados para estudar a tendências urbanas na desenvolvimento.

Para produzir um classificador de imagem a utilizar a aprendizagem de transferência, cientistas de dados, muitas vezes, vários modelos com uma variedade de métodos e selecionar o máximo de elevado desempenho modelo. O Azure Machine Learning Workbench pode ajudar a cientistas de coordenam o treinamento em ambientes de computação diferentes, controlam e comparam o desempenho de vários modelos de dados e aplicar um modelo escolhido para grandes conjuntos de dados na cloud.

## <a name="scenario-structure"></a>Estrutura do cenário

Neste exemplo, os dados de imagem e pré-preparadas com modelos são hospedados numa conta de armazenamento do Azure. Um cluster do Azure HDInsight Spark lê esses arquivos e constrói um modelo de classificação de imagem usando o MMLSpark. O modelo preparado e respetivas previsões, em seguida, são escritos para a conta de armazenamento, onde podem ser analisados e visualizados por um bloco de notas do Jupyter em execução localmente. O Azure Machine Learning Workbench coordena a execução remota de scripts no cluster do Spark. Ele também controla as métricas de precisão de vários modelos de formação através de métodos diferentes, permitindo que o usuário selecionar o máximo de elevado desempenho modelo.

![Esquema para o cenário de mundo real de classificação de imagens aéreas](media/scenario-aerial-image-classification/scenario-schematic.PNG)

Estas instruções passo a passo começam ao servir de orientação a criação e a preparação de uma conta de armazenamento do Azure e o cluster do Spark, incluindo a instalação de transferência e de dependência de dados. Em seguida, descrevem como iniciar tarefas de formação e comparar o desempenho dos modelos de resultantes. Por fim, eles ilustram como aplicar um modelo escolhido para um conjunto de imagem grande no cluster do Spark e analisar os resultados da predição localmente.


## <a name="set-up-the-execution-environment"></a>Configurar o ambiente de execução

As instruções a seguir descreve o processo de configuração de ambiente de execução para este exemplo.

### <a name="prerequisites"></a>Pré-requisitos
- Uma [conta do Azure](https://azure.microsoft.com/free/) (avaliações gratuitas estão disponíveis)
    - Irá criar um cluster do Spark do HDInsight com 40 nós de trabalho (168 núcleos no total). Certifique-se de que a conta tem núcleos suficientes disponíveis ao rever "utilização + quotas" separador para a sua subscrição no portal do Azure.
       - Se tiver menos núcleos disponíveis, pode modificar o modelo de cluster do HDInsight para reduzir o número de workers aprovisionado. Instruções para isto são apresentados na secção "Criar o cluster do Spark do HDInsight".
    - Este exemplo cria um cluster do Batch AI Training com dois NC6 (1 GPU, 6 de vCPU) VMs. Certifique-se de que a conta tem núcleos suficientes disponíveis na região E.U.A. leste, revendo "utilização + quotas" separador para a sua subscrição no portal do Azure.
- [Azure Machine Learning Workbench](../service/overview-what-is-azure-ml.md)
    - Siga os [instalar e criar o guia de introdução](../desktop-workbench/quickstart-installation.md) para instalar o Azure Machine Learning Workbench e criar a experimentação e contas de gestão de modelo.
- [IA do batch](https://github.com/Azure/BatchAI) Python SDK e CLI do Azure
    - Conclua as seguintes secções do [Leiame de receitas de IA do Batch](https://github.com/Azure/BatchAI/tree/master/recipes):
        - "Pré-requisitos"
        - "Criar e colocar a sua aplicação do Azure Active Directory (AAD)"
        - "Registar fornecedores de recursos de BatchAI" (em "Recipes execução com a CLI do Azure")
        - "Instalar o cliente de gestão do Azure Batch AI"
        - "Instalar o SDK de Python do Azure"
    - Registo o ID de cliente, segredo e ID de inquilino da aplicação do Azure Active Directory que é direcionado para o criar. Irá utilizar essas credenciais mais tarde neste tutorial.
    - No momento da elaboração deste artigo, o Azure Machine Learning Workbench e o Azure Batch AI utilizam bifurcações separadas da CLI do Azure. Para maior clareza, nos Referimos versões a Bancada de trabalho da CLI como "uma CLI iniciada a partir do Azure Machine Learning Workbench" e a versão geral (que inclui o Batch AI) como "CLI do Azure."
- [AzCopy](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy), um gratuita utilitário para coordenar a transferência de ficheiros entre contas de armazenamento do Azure
    - Certifique-se de que a pasta que contém o executável de AzCopy está na variável de ambiente PATH do sistema. (Estão disponíveis instruções sobre como modificar variáveis de ambiente [aqui](https://support.microsoft.com/help/310519/how-to-manage-environment-variables-in-windows-xp).)
- Um cliente SSH; Recomendamos [PuTTY](http://www.putty.org/).

Este exemplo foi testado num PC com Windows 10; deve ser capaz de executá-lo a partir de qualquer computador Windows, incluindo máquinas de virtuais de ciência de dados do Azure. A CLI do Azure foi instalada a partir de um MSI de acordo com a [estas instruções](https://github.com/Azure/azure-sdk-for-python/wiki/Contributing-to-the-tests#getting-azure-credentials). Pequenas modificações podem ser necessárias (por exemplo, as alterações para filepaths) ao executar este exemplo em macOS.

### <a name="set-up-azure-resources"></a>Configurar recursos do Azure

Este exemplo requer um cluster do Spark do HDInsight e uma conta de armazenamento do Azure a hospedagem de arquivos relevantes. Siga estas instruções para criar esses recursos num novo grupo de recursos do Azure:

#### <a name="create-a-new-workbench-project"></a>Criar um novo projeto do Workbench

Crie um novo projeto com este exemplo como um modelo:
1.  Abrir o Azure Machine Learning Workbench
2.  Sobre o **projetos** página, clique no **+** iniciar sessão e selecionar **novo projeto**
3.  Na **criar novo projeto** painel, preencha as informações para o novo projeto
4.  Na **modelos de projeto de pesquisa** caixa de pesquisa, escreva "Classificação de imagens aéreas" e selecione o modelo
5.  Clique em **Criar**.
 
#### <a name="create-the-resource-group"></a>Criar o grupo de recursos

1. Depois de carregar o seu projeto no Azure Machine Learning Workbench, abra uma Interface de linha de comandos (CLI), ao clicar em ficheiro -> Abrir linha de comandos.
    Utilize esta versão da CLI para a maioria do tutorial. (Sempre que for indicado, é-lhe perguntado para abrir outra versão da CLI para preparar os recursos do Batch AI.)

1. A partir da interface de linha de comandos, inicie sessão sua conta do Azure executando o seguinte comando:

    ```
    az login
    ```

    É-lhe perguntado para visitar uma URL e escreva um código temporário fornecido; o Web site pede as suas credenciais de conta do Azure.
    
1. Quando o início de sessão estiver concluído, voltar para a CLI e execute o seguinte comando para determinar quais as subscrições do Azure estão disponíveis para a sua conta do Azure:

    ```
    az account list
    ```

    Este comando lista todas as subscrições associadas à sua conta do Azure. Localize o ID da subscrição que pretende utilizar. O ID de subscrição de escrita onde seja indicado no comando seguinte, em seguida, a definir a subscrição ativa executando o comando:

    ```
    az account set --subscription [subscription ID]
    ```

1. Os recursos do Azure criados neste exemplo são armazenados num grupo de recursos do Azure. Escolha um nome de grupo de recursos exclusivos e gravá-lo sempre que for indicado, em seguida, execute os dois comandos para criar o grupo de recursos do Azure:

    ```
    set AZURE_RESOURCE_GROUP=[resource group name]
    az group create --location eastus --name %AZURE_RESOURCE_GROUP%
    ```

#### <a name="create-the-storage-account"></a>Criar a conta de armazenamento

Agora, vamos criar a conta de armazenamento que anfitriões projeto ficheiros que devem ser acessados pelo Spark do HDInsight.

1. Escolha um nome de conta de armazenamento exclusivo e gravá-lo sempre que for indicado a seguir `set` de comando, em seguida, criar uma conta de armazenamento do Azure ao executar os dois comandos:

    ```
    set STORAGE_ACCOUNT_NAME=[storage account name]
    az storage account create --name %STORAGE_ACCOUNT_NAME% --resource-group %AZURE_RESOURCE_GROUP% --sku Standard_LRS
    ```

1. Liste as chaves de conta de armazenamento ao executar o seguinte comando:

    ```
    az storage account keys list --resource-group %AZURE_RESOURCE_GROUP% --account-name %STORAGE_ACCOUNT_NAME%
    ```

    O valor de `key1` como a chave de armazenamento no comando seguinte, em seguida, execute o comando para armazenar o valor.
    ```
    set STORAGE_ACCOUNT_KEY=[storage account key]
    ```
1. Criar uma partilha de ficheiros com o nome `baitshare` na sua conta de armazenamento com o seguinte comando:

    ```
    az storage share create --account-name %STORAGE_ACCOUNT_NAME% --account-key %STORAGE_ACCOUNT_KEY% --name baitshare
    ```
1. No seu editor de texto favorito, carregar o `settings.cfg` de ficheiros de subdiretório de "Código" do projeto do Azure Machine Learning Workbench e insira o nome da conta de armazenamento e a chave, conforme indicado. Guarde e feche o `settings.cfg` ficheiro.
1. Se ainda não o fez, transfira e instale o [AzCopy](https://aka.ms/downloadazcopy) utilitário. Certifique-se de que o executável de AzCopy está no caminho do sistema digitando "AzCopy" e pressionar Enter para mostrar a sua documentação.
1. Emita os seguintes comandos para copiar todos os dados de exemplo, pré-preparadas com modelos e scripts de preparação do modelo para os locais apropriados na sua conta de armazenamento:

    ```
    AzCopy /Source:https://mawahsparktutorial.blob.core.windows.net/test /SourceSAS:"?sv=2017-04-17&ss=bf&srt=sco&sp=rwl&se=2037-08-25T22:02:55Z&st=2017-08-25T14:02:55Z&spr=https,http&sig=yyO6fyanu9ilAeW7TpkgbAqeTnrPR%2BpP1eh9TcpIXWw%3D" /Dest:https://%STORAGE_ACCOUNT_NAME%.blob.core.windows.net/test /DestKey:%STORAGE_ACCOUNT_KEY% /S
    AzCopy /Source:https://mawahsparktutorial.blob.core.windows.net/train /SourceSAS:"?sv=2017-04-17&ss=bf&srt=sco&sp=rwl&se=2037-08-25T22:02:55Z&st=2017-08-25T14:02:55Z&spr=https,http&sig=yyO6fyanu9ilAeW7TpkgbAqeTnrPR%2BpP1eh9TcpIXWw%3D" /Dest:https://%STORAGE_ACCOUNT_NAME%.blob.core.windows.net/train /DestKey:%STORAGE_ACCOUNT_KEY% /S
    AzCopy /Source:https://mawahsparktutorial.blob.core.windows.net/middlesexma2016 /SourceSAS:"?sv=2017-04-17&ss=bf&srt=sco&sp=rwl&se=2037-08-25T22:02:55Z&st=2017-08-25T14:02:55Z&spr=https,http&sig=yyO6fyanu9ilAeW7TpkgbAqeTnrPR%2BpP1eh9TcpIXWw%3D" /Dest:https://%STORAGE_ACCOUNT_NAME%.blob.core.windows.net/middlesexma2016 /DestKey:%STORAGE_ACCOUNT_KEY% /S
    AzCopy /Source:https://mawahsparktutorial.blob.core.windows.net/pretrainedmodels /SourceSAS:"?sv=2017-04-17&ss=bf&srt=sco&sp=rwl&se=2037-08-25T22:02:55Z&st=2017-08-25T14:02:55Z&spr=https,http&sig=yyO6fyanu9ilAeW7TpkgbAqeTnrPR%2BpP1eh9TcpIXWw%3D" /Dest:https://%STORAGE_ACCOUNT_NAME%.blob.core.windows.net/pretrainedmodels /DestKey:%STORAGE_ACCOUNT_KEY% /S
    AzCopy /Source:https://mawahsparktutorial.blob.core.windows.net/pretrainedmodels /SourceSAS:"?sv=2017-04-17&ss=bf&srt=sco&sp=rwl&se=2037-08-25T22:02:55Z&st=2017-08-25T14:02:55Z&spr=https,http&sig=yyO6fyanu9ilAeW7TpkgbAqeTnrPR%2BpP1eh9TcpIXWw%3D" /Dest:https://%STORAGE_ACCOUNT_NAME%.file.core.windows.net/baitshare/pretrainedmodels /DestKey:%STORAGE_ACCOUNT_KEY% /S
    AzCopy /Source:https://mawahsparktutorial.blob.core.windows.net/scripts /SourceSAS:"?sv=2017-04-17&ss=bf&srt=sco&sp=rwl&se=2037-08-25T22:02:55Z&st=2017-08-25T14:02:55Z&spr=https,http&sig=yyO6fyanu9ilAeW7TpkgbAqeTnrPR%2BpP1eh9TcpIXWw%3D" /Dest:https://%STORAGE_ACCOUNT_NAME%.file.core.windows.net/baitshare/scripts /DestKey:%STORAGE_ACCOUNT_KEY% /S
    ```

    Espere a transferência de ficheiros para demorar aproximadamente uma hora. Enquanto espera, pode avançar para a secção seguinte: poderá ter de abrir outra Interface de linha de comando por meio da bancada de trabalho e redefinir as variáveis temporárias lá.

#### <a name="create-the-hdinsight-spark-cluster"></a>Criar o cluster do HDInsight Spark

Nosso método recomendado para criar um cluster do HDInsight utiliza o HDInsight Spark cluster modelo do resource manager incluído na subpasta "Code\01_Data_Acquisition_and_Understanding\01_HDInsight_Spark_Provisioning" deste projeto.

1. O modelo de cluster do Spark do HDInsight é o arquivo de "Template" na subpasta "Code\01_Data_Acquisition_and_Understanding\01_HDInsight_Spark_Provisioning" deste projeto. Por predefinição, o modelo cria um cluster do Spark com 40 nós de trabalho. Se deve ajustar esse número, abrir o modelo no seu editor de texto favorito e substituir quaisquer instâncias de "40" com o número de nós de trabalho da sua preferência.
    - Pode encontrar erros de insuficiência de memória mais tarde, se o número de nós de trabalho que escolher for menor. Para combater os erros de memória, poderá executar os scripts de treinamento e operacionalização num subconjunto de dados disponíveis conforme descrito posteriormente neste documento.
2. Escolha um nome exclusivo e a palavra-passe para o HDInsight cluster e escrevê-los onde seja indicado no seguinte comando: em seguida, criar o cluster emitindo comandos:

    ```
    set HDINSIGHT_CLUSTER_NAME=[HDInsight cluster name]
    set HDINSIGHT_CLUSTER_PASSWORD=[HDInsight cluster password]
    az group deployment create --resource-group %AZURE_RESOURCE_GROUP% --name hdispark --template-file "Code\01_Data_Acquisition_and_Understanding\01_HDInsight_Spark_Provisioning\template.json" --parameters storageAccountName=%STORAGE_ACCOUNT_NAME%.blob.core.windows.net storageAccountKey=%STORAGE_ACCOUNT_KEY% clusterName=%HDINSIGHT_CLUSTER_NAME% clusterLoginPassword=%HDINSIGHT_CLUSTER_PASSWORD%
    ```

Implementação do seu cluster pode demorar até 30 minutos (incluindo a execução da ação de aprovisionamento e o script).

### <a name="set-up-batch-ai-resources"></a>Configurar recursos do Batch AI

Enquanto espera pela sua transferência de ficheiros de conta de armazenamento e a implementação de cluster do Spark concluir, pode preparar o cluster do Batch AI servidor NFS (Network File) e a GPU. Abra uma linha de comandos da CLI do Azure e execute o seguinte comando:

```
az --version 
```

Confirme que `batchai` está listado entre os módulos instalados. Se não existir, pode estar a utilizar uma Interface de linha de comando diferentes (por exemplo, um aberta por meio da bancada de trabalho).

Em seguida, verifique que o registo do fornecedor foi concluída com êxito. (O registo do fornecedor demora até quinze minutos e ainda pode estar em curso se concluiu recentemente a [instruções de configuração do Batch AI](https://github.com/Azure/BatchAI/tree/master/recipes).) Certifique-se "Batch" e "Microsoft.BatchAI" aparecem com o estado "Registado" na saída do comando seguinte:

```
az provider list --query "[].{Provider:namespace, Status:registrationState}" --out table
```

Caso contrário, execute o seguinte fornecedor comandos de registo e Aguarde aproximadamente 15 minutos para o registo concluir.
```
az provider register --namespace Microsoft.Batch
az provider register --namespace Microsoft.BatchAI
```

Modificar os seguintes comandos para substituir as expressões entre parênteses, com os valores que utilizou anteriormente durante a criação de conta de armazenamento e de grupo de recursos. Em seguida, armazene os valores como variáveis, executando os comandos:
```
az account set --subscription [subscription ID]
set AZURE_RESOURCE_GROUP=[resource group name]
set STORAGE_ACCOUNT_NAME=[storage account name]
set STORAGE_ACCOUNT_KEY=[storage account key]
az configure --defaults location=eastus
az configure --defaults group=%AZURE_RESOURCE_GROUP%
```

Identificar a pasta que contém seu projeto do Azure Machine Learning (por exemplo, `C:\Users\<your username>\AzureML\aerialimageclassification`). Substitua o valor entre parênteses pelo caminho do ficheiro da pasta (com nenhuma barra invertida da direita) e execute o comando:
```
set PATH_TO_PROJECT=[The filepath of your project's root directory]
```
Agora está pronto para criar os recursos do Batch AI necessários para este tutorial.

#### <a name="prepare-the-batch-ai-network-file-server"></a>Preparar o servidor de ficheiros de rede de IA do Batch

O cluster do Batch AI acessa os dados de treinamento num servidor de ficheiros de rede. Acesso a dados pode ser several-fold mais rapidamente ao acessar arquivos de NFS versus uma partilha de ficheiros do Azure ou o armazenamento de Blobs do Azure.

1. Emita o comando seguinte para criar um servidor de ficheiros de rede:

    ```
    az batchai file-server create -n landuseclassifier -u demoUser -p "Dem0Pa$$w0rd" --vm-size Standard_DS2_V2 --disk-count 1 --disk-size 1000 --storage-sku Premium_LRS
    ```

1. Verifique o estado de aprovisionamento do seu servidor de ficheiros de rede com o seguinte comando:

    ```
    az batchai file-server list
    ```

    Quando o "provisioningState" do servidor de ficheiros de rede com o nome "landuseclassifier" é "concluído com êxito", está pronto para ser utilizado. Espera-se de aprovisionamento a demorar cerca de cinco minutos.
1. Encontre o endereço IP do seu NFS na saída do comando anterior (a propriedade de "fileServerPublicIp" em "mountSettings"). Escreva o IP de endereço onde seja indicado no comando seguinte, em seguida, armazenar o valor ao executar o comando:

    ```
    set AZURE_BATCH_AI_TRAINING_NFS_IP=[your NFS IP address]
    ```

1. Usando sua ferramenta SSH favorita (o seguinte comando usos de exemplo [PuTTY](http://www.putty.org/)), executar este projeto `prep_nfs.sh` script no NFS para transferir a imagem de treinamento e validação define lá.

    ```
    putty -ssh demoUser@%AZURE_BATCH_AI_TRAINING_NFS_IP% -pw Dem0Pa$$w0rd -m %PATH_TO_PROJECT%\Code\01_Data_Acquisition_and_Understanding\02_Batch_AI_Training_Provisioning\prep_nfs.sh
    ```

    Não se preocupar se as atualizações de progresso de transferência e extração de dados atravessam a janela do shell tão rapidamente que estão ilegível.

Se assim o desejar, pode confirmar que a transferência de dados prosseguiu conforme planejado, iniciando sessão no servidor de ficheiros com a sua ferramenta preferida de SSH e verificar o `/mnt/data` conteúdo do diretório. Deve encontrar duas pastas, training_images e validation_images, cada um contendo com subpastas nomeadas de acordo com o ' s land utilizar categorias.  Os conjuntos de formação e validação devem conter ~ 44 k e imagens de k de ~ 11, respectivamente.

#### <a name="create-a-batch-ai-cluster"></a>Criar um cluster do Batch AI

1. Crie o cluster emitindo o comando seguinte:

    ```
    az batchai cluster create -n landuseclassifier2 -u demoUser -p "Dem0Pa$$w0rd" --afs-name baitshare --nfs landuseclassifier --image UbuntuDSVM --vm-size STANDARD_NC6 --max 2 --min 2 --storage-account-name %STORAGE_ACCOUNT_NAME% 
    ```

1. Utilize o seguinte comando para verificar o que seu cluster do Estado de aprovisionamento:

    ```
    az batchai cluster list
    ```

    Quando o estado de alocação para o cluster com o nome "landuseclassifier" alterações de redimensionamento para estável, é possível submeter tarefas. No entanto, tarefas não começam em execução até que todas as VMs no cluster tenham deixado o estado de "preparação". Se a propriedade de "erros" do cluster não for nula, Ocorreu um erro durante a criação do cluster e não deve ser utilizado.

#### <a name="record-batch-ai-training-credentials"></a>Credenciais do Batch AI Training registos

Enquanto espera pela alocação de cluster a concluir, abra o `settings.cfg` ficheiro a partir do subdiretório de "Código" deste projeto no editor de texto à sua escolha. Atualize as seguintes variáveis com suas próprias credenciais:
- `bait_subscription_id` (o seu ID de subscrição do Azure de 36 carateres)
- `bait_aad_client_id` (o ID de aplicação/cliente de Azure Active Directory mencionado na secção "Pré-requisitos")
- `bait_aad_secret` (o segredo de aplicação de Azure Active Directory mencionado na secção "Pré-requisitos")
- `bait_aad_tenant` (o ID de inquilino de Azure Active Directory mencionado na secção "Pré-requisitos")
- `bait_region` (no momento da elaboração deste artigo, a única opção é: eastus)
- `bait_resource_group_name` (o grupo de recursos que selecionou anteriormente)

Assim que tiver atribuído estes valores, linhas modificadas do seu ficheiro settings.cfg devem assemelhar-se o seguinte texto:

```
[Settings]
    # Credentials for the Azure Storage account
    storage_account_name = yoursaname
    storage_account_key = kpIXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXQ==
    
    # Batch AI training credentials
    bait_subscription_id = 0caXXXXX-XXXX-XXXX-XXXX-XXXXXXXXX9c3
    bait_aad_client_id = d0aXXXXX-XXXX-XXXX-XXXX-XXXXXXXXX7f8
    bait_aad_secret = ygSXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX6I=
    bait_aad_tenant = 72fXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXb47
    bait_region = eastus
    bait_resource_group_name = yourrgname
```

Guarde e feche `settings.cfg`.

Já pode fechar a janela da CLI, onde é executada os comandos de criação de recursos do Batch AI. Todos os passos adicionais neste tutorial, utilize uma CLI iniciada a partir do Azure Machine Learning Workbench.

### <a name="prepare-the-azure-machine-learning-workbench-execution-environment"></a>Preparar o ambiente de execução do Azure Machine Learning Workbench

#### <a name="register-the-hdinsight-cluster-as-an-azure-machine-learning-workbench-compute-target"></a>Registe-se o cluster de HDInsight como um destino de computação do Azure Machine Learning Workbench

Depois de concluída a criação de clusters do HDInsight, registe-se o cluster como um destino de computação para o seu projeto da seguinte forma:

1.  Emita o comando seguinte a partir da Interface de linha de comandos do Azure Machine Learning:

    ```
    az ml computetarget attach cluster --name myhdi --address %HDINSIGHT_CLUSTER_NAME%-ssh.azurehdinsight.net --username sshuser --password %HDINSIGHT_CLUSTER_PASSWORD%
    ```

    Este comando adiciona dois ficheiros, `myhdi.runconfig` e `myhdi.compute`, ao seu projeto `aml_config` pasta.

1. Abra o `myhdi.compute` ficheiro no seu editor de texto favorito. Modificar a `yarnDeployMode: cluster` linha ler `yarnDeployMode: client`, em seguida, guarde e feche o ficheiro.
1. Execute o seguinte comando para preparar o ambiente do HDInsight para uso:
   ```
   az ml experiment prepare -c myhdi
   ```

#### <a name="install-local-dependencies"></a>Instalar dependências locais

Abra uma CLI do Azure Machine Learning Workbench e instalar dependências necessárias para execução local, emitindo o comando seguinte:

```
pip install matplotlib azure-storage==0.36.0 pillow scikit-learn azure-mgmt-batchai
```

## <a name="data-acquisition-and-understanding"></a>Aquisição e compreensão de dados

Este cenário utiliza dados de imagens aéreas publicamente disponíveis dos [programa de imagens de agricultura nacional](https://www.fsa.usda.gov/programs-and-services/aerial-photography/imagery-programs/naip-imagery/) uma resolução de medidor de 1. Podemos ter gerado conjuntos de 224 pixel x 224 pixel ficheiros PNG recortadas dos dados NAIP originais e classificados de acordo com as etiquetas de utilização de ' s land do [abrangem ' s Land de nacional o banco de dados](https://www.mrlc.gov/nlcd2011.php). Uma imagem de exemplo com a etiqueta "Developed" é apresentada com tamanho máximo:

![Um mosaico de exemplo da terra desenvolvido](media/scenario-aerial-image-classification/sample-tile-developed.png)

Conjuntos com balanceamento de classe de ~ 44 k e imagens de k 11 são usadas para preparação de modelos e validação, respectivamente. Vamos demonstrar a implementação do modelo numa imagem de k de ~ 67 definir tiling Middlesex County, MA – página inicial da Microsoft Research Nova Inglaterra e Centro de desenvolvimento (SUPLENTES). Para obter mais informações sobre como estes conjuntos de imagem foram construídos, consulte a [repositório de git de classificação de imagens Constrangedoramente paralelas](https://github.com/Azure/Embarrassingly-Parallel-Image-Classification).

![Localização do condado Middlesex, Massachusetts](media/scenario-aerial-image-classification/middlesex-ma.png)

Durante a configuração, os conjuntos de imagens aéreas utilizados neste exemplo foram transferidos para a conta de armazenamento que criou. O treinamento, validação e imagens de operacionalização são todos os ficheiros PNG de pixel de 224 pixel x 224 a uma resolução de um pixel por medidor quadrado. As imagens de formação e a validação tiveram sido organizadas em subpastas com base na respetiva etiqueta de utilização de ' s land. (As etiquetas de utilização de ' s land das imagens operacionalização são desconhecidas e em muitos casos ambíguos; alguns destas imagens contêm vários tipos de ' s land.) Para obter mais informações sobre como estes conjuntos de imagem foram construídos, consulte a [repositório de git de classificação de imagens Constrangedoramente paralelas](https://github.com/Azure/Embarrassingly-Parallel-Image-Classification).

Para ver o exemplo de imagens no armazenamento do Azure da conta (opcionais):
1. Inicie sessão no [Portal do Azure](https://portal.azure.com).
1. Procure o nome da conta de armazenamento na barra de pesquisa na parte superior do ecrã. Clique na sua conta de armazenamento nos resultados da pesquisa.
2. Clique na ligação "Blobs" no painel de principal da conta de armazenamento.
3. Clique no contentor com o nome "train." Deverá ver uma lista de diretórios nomeado de acordo com o ' s land utilizar.
4. Clique em qualquer um desses diretórios para carregar a lista de imagens que nele contidos.
5. Clique em qualquer imagem e transferi-lo para ver a imagem.
6. Se assim o desejar, clique nos contentores com o nome "teste" e "middlesexma2016" para ver o seu conteúdo também.

## <a name="modeling"></a>Modelação

### <a name="training-models-with-azure-batch-ai"></a>Modelos de treinamento com o Azure Batch AI

O `run_batch_ai.py` script na subpasta "Code\02_Modeling" do projeto Workbench é utilizado para emitir uma tarefa de preparação de IA do Batch. Esta tarefa retrains um classificador de imagem DNN escolhido pelo utilizador (AlexNet ou 18 de utilizar o ResNet pré-preparadas com ImageNet). A profundidade da reparametrização também pode ser especificada: reparametrização apenas a camada final da rede poderá reduzir overfitting quando alguns exemplos de treinamento estão disponíveis, ao ajustar a toda a rede (ou, para AlexNet, as camadas totalmente conectadas) pode levar a maior do modelo desempenho quando o conjunto de treinamento é suficientemente grande.

Quando a tarefa de preparação estiver concluída, este script salva o modelo (juntamente com um arquivo que descreve o mapeamento entre a saída de número inteiro do modelo e as etiquetas de cadeia de caracteres) e as previsões para o armazenamento de Blobs. Ficheiro de registo da tarefa BAIT é analisado para extrair o timecourse de melhoria de taxa de erro em relação as epochs de treinamento. O timecourse de melhoria de taxa de erro é registado a funcionalidade de histórico de execuções da bancada de trabalho de AML para posterior visualização.

Selecione um nome para o seu modelo preparado, um tipo de modelo pré-preparadas com e uma profundidade de reparametrização. Escreva que as suas seleções onde seja indicado no comando seguinte, em seguida, começam a reparametrização ao executar o comando a partir de um Azure ML Command Line Interface:

```
az ml experiment submit -c local Code\02_Modeling\run_batch_ai.py --config_filename Code/settings.cfg --output_model_name [unique model name, alphanumeric characters only] --pretrained_model_type {alexnet,resnet18} --retraining_type {last_only,fully_connected,all} --num_epochs 10
```

Esperar que o Azure Machine Learning executada para a conclusão demorar cerca de meia hora. Recomendamos que execute alguns comandos semelhantes (variando o nome do modelo de saída, o tipo de modelo pré-preparadas com e a profundidade de reparametrização) para que pode comparar o desempenho dos modelos com base com métodos diferentes.

### <a name="training-models-with-mmlspark"></a>Modelos de treinamento com MMLSpark

O `run_mmlspark.py` script na subpasta "Code\02_Modeling" do projeto Workbench é utilizado para formar uma [MMLSpark](https://github.com/Azure/mmlspark) modelos de classificação de imagens. O featurizes primeiro script de preparação definido imagens usando um classificador de imagem que DNN pré-preparadas no conjunto de dados ImageNet (AlexNet ou utilizar o ResNet um camada de 18). O script, em seguida, utiliza as imagens de caracterizadas para preparar um modelo de MMLSpark (uma floresta aleatória ou um modelo de regressão logística) para classificar as imagens. O conjunto de imagem de teste é, em seguida, caracterizadas e pontuadas com o modelo preparado. A precisão das previsões de indisponibilidade do modelo de conjunto de teste é calculada e iniciou a funcionalidade do Azure Machine Learning Workbench histórico de execuções. Por fim, o modelo de MMLSpark preparado e respetivas previsões de indisponibilidade do conjunto de teste são guardados no armazenamento de Blobs.

Selecione um nome de modelo exclusivo de saída para o seu modelo preparado, um tipo de pré-preparadas com o modelo e um tipo de modelo MMLSpark. Escreva que as suas seleções onde seja indicado no modelo do seguinte comando, em seguida, começam a reparametrização ao executar o comando a partir de um Azure ML Command Line Interface:

```
az ml experiment submit -c myhdi Code\02_Modeling\run_mmlspark.py --config_filename Code/settings.cfg --output_model_name [unique model name, alphanumeric characters only] --pretrained_model_type {alexnet,resnet18} --mmlspark_model_type {randomforest,logisticregression}
```

Mais `--sample_frac` parâmetro pode ser usado para dar formação e testar o modelo com um subconjunto de dados disponíveis. Utilizar uma fração de pequena amostra reduz os requisitos de memória e tempo de execução em detrimento da precisão do modelo preparado. (Por exemplo, uma execução com `--sample_frac 0.1` demore aproximadamente 20 minutos.) Para obter mais informações sobre este e outros parâmetros, execute `python Code\02_Modeling\run_mmlspark.py -h`.

Os utilizadores são encorajados a para executar este script várias vezes com diferentes parâmetros de entrada. O desempenho dos modelos de resultantes pode ser comparado na funcionalidade do Azure Machine Learning Workbench histórico de execuções.

### <a name="comparing-model-performance-using-the-workbench-run-history-feature"></a>Comparando o desempenho do modelo a utilizar a funcionalidade de histórico de execuções da bancada de trabalho

Depois de ter executado duas ou mais treinamento é executado de qualquer tipo, navegue para a funcionalidade de histórico de execuções no Workbench ao clicar no ícone de relógio ao longo da barra de menu do lado esquerdo. Selecione `run_mmlspark.py` na lista de scripts à esquerda. Um painel carrega comparando a precisão do conjunto de teste para todas as execuções. Para ver mais detalhes, desloque para baixo e clique no nome de uma execução individual.

## <a name="deployment"></a>Implementação

Para aplicar um dos seus modelos de formação para imagens aéreas lado a lado com a execução remota no HDInsight com Middlesex County, MA, inserir o nome do modelo pretendido para o seguinte comando e executá-lo:

```
az ml experiment submit -c myhdi Code\03_Deployment\batch_score_spark.py --config_filename Code/settings.cfg --output_model_name [trained model name chosen earlier]
```

Mais `--sample_frac` parâmetro pode ser utilizado para operacionalizar o modelo com um subconjunto de dados disponíveis. Utilizar uma fração de pequena amostra reduz os requisitos de memória e tempo de execução às custas de integridade de predição. Para obter mais informações sobre este e outros parâmetros, execute `python Code\03_Deployment\batch_score_spark -h`.

Este script escreve previsões do modelo na sua conta de armazenamento. As previsões podem ser examinadas conforme descrito na secção seguinte.

## <a name="visualization"></a>Visualização

O bloco de notas do Jupyter "Análise de previsão de modelo" na subpasta "Code\04_Result_Analysis" do projeto Bancada de trabalho visualiza previsões de um modelo. Carregar e executar o bloco de notas da seguinte forma:
1. Abra o projeto no Workbench e clique na pasta de ícone ("ficheiros") ao longo do menu do lado esquerdo, para carregar a listagem de diretórios.
2. Navegue para a subpasta do "Code\04_Result_Analysis" e clique em do bloco de notas com o nome "Análise de previsão de modelo". Um processamento de pré-visualização do bloco de notas deve ser apresentado.
3. Clique em "Iniciar bloco de notas do servidor" para carregar o bloco de notas.
4. A primeira célula, introduza o nome do modelo cujos resultados gostaria de analisar sempre que for indicado.
5. Clique em "célula -> executar todos os" para executar todas as células no bloco de notas.
6. Leia juntamente com o bloco de notas para saber mais sobre as análises e visualizações, que ele apresenta.

## <a name="cleanup"></a>Limpeza
Quando tiver concluído o exemplo, recomendamos que elimine todos os recursos que criou ao executar o seguinte comando da Interface de linha de comandos do Azure:

  ```
  az group delete --name %AZURE_RESOURCE_GROUP%
  ```

## <a name="references"></a>Referências

- [O repositório de classificação de imagens Constrangedoramente paralelas](https://github.com/Azure/Embarrassingly-Parallel-Image-Classification)
   - Descreve a construção de conjunto de dados de imagens disponíveis gratuitamente e etiquetas
- [MMLSpark](https://github.com/Azure/mmlspark) repositório do GitHub
   - Contém exemplos adicionais de preparação de modelos e de avaliação com o MMLSpark

## <a name="conclusions"></a>Conclusões

O Azure Machine Learning Workbench ajuda os cientistas de dados implemente facilmente seu código destinos de computação remota. Neste exemplo, código de treinamento de MMLSpark local tiver sido implementado durante a execução remota num cluster do HDInsight e um script local iniciou uma tarefa de preparação num cluster GPU de IA do Azure Batch. Funcionalidade de histórico de execuções do Azure Machine Learning Workbench controlados o desempenho de vários modelos e ajudou-na identificar o modelo mais exato. Funcionalidade de blocos de notas do Jupyter do Workbench ajudou a visualizar predições dos nossos modelos num ambiente interativo e gráfica.

## <a name="next-steps"></a>Passos Seguintes
Para aprofundar-se para este exemplo:
- No recurso do Azure Machine Learning Workbench histórico de execuções, clique nos símbolos de engrenagem para selecionar quais os gráficos e as métricas são apresentadas.
- Examine os scripts de exemplo para instruções ao chamar o `run_logger`. Verifique se compreender como cada métrica está a ser registada.
- Examine os scripts de exemplo para instruções ao chamar o `blob_service`. Verificação de que compreende como preparação de modelos e as previsões são armazenados e recuperados a partir da cloud.
- Explore o conteúdo dos contentores criados na sua conta de armazenamento de Blobs. Certifique-se de que compreender qual script ou comando é responsável pela criação de cada grupo de ficheiros.
- Modificar o script de treinamento para preparar um tipo de modelo MMLSpark diferente ou para alterar os hiperparâmetros do modelo. Utilize a funcionalidade de histórico de execução para determinar se as suas alterações aumentaram ou diminuíram a precisão do modelo.
