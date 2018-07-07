---
title: Ler e escrever ficheiros de dados grandes | Documentos da Microsoft
description: Ler e escrever ficheiros grandes em experiências do Azure Machine Learning.
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 09/10/2017
ms.openlocfilehash: 5a772f8792c02139e45977e207b5be4bebc63a9c
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/07/2018
ms.locfileid: "37908239"
---
# <a name="persisting-changes-and-working-with-large-files"></a>Manter as alterações e trabalhar com ficheiros grandes
Com o serviço de experimentação do Azure Machine Learning, pode configurar uma variedade de destinos de execução. Alguns destinos são locais, como um computador local ou um contentor do Docker num computador local. Outros recursos são remotos, como o contentor do Docker num computador remoto ou um cluster do HDInsight. Para obter mais informações, consulte [descrição geral do Azure Machine Learning experimentar o serviço de execução](experimentation-service-configuration.md). 

Pode executar num destino, tem de copiar a pasta do projeto para o destino de computação. Deve fazer por isso, mesmo com uma execução local que usa uma pasta temporária do local para esse fim. 

## <a name="execution-isolation-portability-and-reproducibility"></a>Isolamento de execução, a portabilidade e a capacidade de reprodução
O objetivo deste design é garantir o isolamento, a capacidade de reprodução e a portabilidade da execução. Se executar o mesmo script duas vezes, sobre o mesmo ou outro destino de computação, receberá os mesmos resultados. As alterações feitas durante a primeira execução não devem afetar aqueles na segunda execução. Com esta estrutura, pode tratar destinos de computação como recursos de computação sem monitoração de estado, contendo cada sem afinidade para as tarefas que são executadas depois de concluir o procedimento.

## <a name="challenges"></a>Desafios
Mesmo que esse design fornece os benefícios de portabilidade e a capacidade de repetição, também traz alguns desafios únicos.

### <a name="persisting-state-changes"></a>Mantendo alterações de estado
Se o seu script modifica o estado do contexto de computação, as alterações não são mantidas para sua execução seguinte, e eles não estiverem propagados de volta para o computador cliente automaticamente. 

Mais especificamente, se o script cria uma subpasta ou escreve um ficheiro, esse ficheiro ou pasta não estarão presente no seu diretório do projeto após a execução. Os ficheiros são armazenados numa pasta temporária no ambiente de destino de computação. Pode usá-los para fins de depuração, mas não pode contar com sua existência permanente.

### <a name="working-with-large-files-in-the-project-folder"></a>Trabalhar com ficheiros grandes em que a pasta do projeto

Se a pasta do projeto contém todos os ficheiros grandes, pode incorrer latência quando a pasta é copiada para o ambiente de computação de destino no início de uma execução. Mesmo que a execução ocorre localmente, existe tráfego de disco ainda desnecessárias para evitar. Por esse motivo, atualmente, limite o tamanho máximo de projeto com 25 MB.

## <a name="option-1-use-the-outputs-folder"></a>Opção 1: Utilizar o *produz* pasta
Esta opção é preferível se todas as condições seguintes se aplicar:
* O script produz os ficheiros.
* Espera que os ficheiros para alterar com cada experimentação.
* Pretende manter um histórico desses arquivos. 

Os casos de utilização comuns são:
* Preparar um modelo
* Criar um conjunto de dados
* Desenhar um gráfico como um ficheiro de imagem como parte da sua execução de preparação de modelos 

Além disso, deseja comparar as saídas nas execuções, selecionadas um ficheiro de saída (por exemplo, um modelo) que foi produzido pelas anterior executar e, em seguida, utilizá-lo para uma tarefa subseqüente (por exemplo, classificação).

Pode escrever ficheiros para uma pasta denominada *produz* que é relativo ao diretório de raiz. A pasta recebe o tratamento especial pelo serviço de experimentação. Nada criado pelo script na pasta durante a execução, tais como um ficheiro de modelo, o ficheiro de dados ou o ficheiro de imagem plotada (coletivamente conhecido como _artefactos_), é copiado para a conta de armazenamento de Blobs do Azure que está associada a conta de experimentação após a execução estiver concluída. Os ficheiros se tornar parte do seu registo de histórico de execuções.

Eis um breve exemplo de código para armazenar um modelo no *produz* pasta:
```python
import os
import pickle

# m is a scikit-learn model. 
# we serialize it into a mode.plk file under the ./outputs folder.
with open(os.path.join('.', 'outputs', 'model.pkl'), 'wb') as f:    
    pickle.dump(m, f)
```
Pode transferir quaisquer artefactos ao navegar para o **ficheiros de saída** secção da página de detalhes de uma determinada execução no Azure Machine Learning Workbench. Basta selecionar a execução e, em seguida, selecione o **transferir** botão. Em alternativa, pode introduzir o `az ml asset download` comando na janela da interface de linha de comandos (CLI).

Para obter um exemplo mais completo, consulte a `iris_sklearn.py` script de Python no _Classifying Iris_ projeto de exemplo.

## <a name="option-2-use-the-shared-folder"></a>Opção 2: Utilizar a pasta compartilhada
Se não precisar de manter um histórico de ficheiros de cada execução, através de uma pasta compartilhada que pode ser acessada através de execuções independentes podem ser uma excelente opção para os seguintes cenários: 
- O script tem de carregar dados de ficheiros locais, como ficheiros. csv ou um diretório de ficheiros de texto ou imagem, como dados de treinamento ou de teste. 
- O script processa dados não processados e escreve os resultados intermediários, como os dados de treinamento de caracterizadas do ficheiros de texto ou imagem, que são utilizados num treinamento subsequente executar. 
- O script produz um modelo e seu script de classificação subsequente tem de escolher o modelo e utilizá-lo para avaliação. 

É importante observar que a pasta partilhada reside localmente no seu destino de computação escolhido. Por conseguinte, esta opção só funciona se todas as execuções de script que fazem referência a essa pasta compartilhada são executadas no mesmo destino de computação e o destino de computação não é reciclado entre as execuções.

Ao tirar partido da funcionalidade de pasta partilhada, pode ler ou gravar para uma pasta especial que é identificada por uma variável de ambiente, `AZUREML_NATIVE_SHARE_DIRECTORY`. 

### <a name="example"></a>Exemplo
Eis alguns exemplos de código de Python para utilizar esta pasta de partilha para ler e gravar num arquivo de texto:
```python
import os

# write to the shared folder
with open(os.environ['AZUREML_NATIVE_SHARE_DIRECTORY'] + 'test.txt', "w") as f1:
    f1.write(“Hello World”)

# read from the shared folder
with open(os.environ['AZUREML_NATIVE_SHARE_DIRECTORY'] + 'test.txt', "r") as f2:
    text = f2.read()
```

Para obter um exemplo mais completo, consulte a *iris_sklearn_shared_folder.py* de ficheiros a _Classifying Iris_ projeto de exemplo.

Antes de poder utilizar esta funcionalidade, é necessário definir *.compute* algumas configurações simples que representam o contexto de execução visados de ficheiros a *aml_config* pasta. O caminho real para a pasta pode variar consoante o destino de computação que escolher e o valor que configurar.

### <a name="configure-local-compute-context"></a>Configurar o contexto de cálculo local

Para ativar esta funcionalidade num contexto de cálculo local, basta adicionar ao seu *.compute* ficheiro a linha seguinte, que representa o _local_ ambiente (normalmente denominados *local.compute*).
```
# local.runconfig
...
nativeSharedDirectory: ~/.azureml/share
...
```

O caminho ~/.azureml/share é o caminho de pasta base do padrão. Pode alterá-lo para qualquer caminho absoluto local que seja acessível pelo script ser executado. O nome da conta de experimentação, o nome de área de trabalho e o nome de projeto são automaticamente acrescentados ao nome do diretório base, o que se torna o caminho completo do diretório partilhado. Por exemplo, os ficheiros podem ser gravados (e obtidos a partir do) o seguinte caminho se usar anterior de valor predefinido:

```
# on Windows
C:\users\<username>\.azureml\share\<exp_acct_name>\<workspace_name>\<proj_name>\

# on macOS
/Users/<username>/.azureml/share/<exp_acct_name>/<workspace_name>/<proj_name>/
```

### <a name="configure-the-docker-compute-context-local-or-remote"></a>Configurar o contexto de cálculo do Docker (local ou remoto)
Para ativar esta funcionalidade num contexto de cálculo do Docker, tem de adicionar as duas linhas seguintes ao seu local ou remoto Docker *.compute* ficheiro.

```
# docker.compute
...
sharedVolumes: true
nativeSharedDirectory: ~/.azureml/share
...
```
>[!IMPORTANT]
>O **sharedVolumes** propriedade tem de ser definida como *true* quando utiliza o `AZUREML_NATIVE_SHARE_DIRECTORY` variável de ambiente para aceder à pasta partilhada. Caso contrário, a execução falhará.

O código em execução no contentor do Docker sempre vê essa pasta compartilhada como */azureml-share /*. O caminho da pasta, como visto pelo contentor do Docker, não é configurável. Não utilize este nome de pasta no seu código. Em vez disso, utilize sempre o nome da variável de ambiente `AZUREML_NATIVE_SHARE_DIRECTORY` para fazer referência a esta pasta. Este é mapeado para uma pasta local no anfitrião do Docker machine ou contexto de cálculo. O diretório base dessa pasta local é o valor configurável do `nativeSharedDirectory` definição *.compute* ficheiro. O caminho local da pasta partilhada no computador anfitrião, se usar o valor predefinido, é o seguinte:
```
# Windows
C:\users\<username>\.azureml\share\<exp_acct_name>\<workspace_name>\<proj_name>\

# macOS
/Users/<username>/.azureml/share/<exp_acct_name>/<workspace_name>/<proj_name>/

# Ubuntu Linux
/home/<username>/.azureml/share/<exp_acct_name>/<workspace_name>/<proj_name>/
```

>[!NOTE]
>O caminho da pasta partilhada no disco local é o mesmo se se trata de um contexto de cálculo local ou de um contexto de cálculo do Docker local. Isso significa que pode até compartilhar arquivos entre uma execução local e uma execução local do Docker.

Pode colocar dados de entrada diretamente nessas pastas e esperam que seus locais ou de execuções do Docker na máquina pegá-lo. Também pode gravar arquivos para esta pasta no seu local ou de execuções do Docker e esperar que ficheiros obterem persistentes nessa pasta, sobreviverem ao ciclo de vida de execução.

Para obter mais informações, consulte [ficheiros de configuração de execução do Azure Machine Learning Workbench](experimentation-service-configuration-reference.md).

>[!NOTE]
>O `AZUREML_NATIVE_SHARE_DIRECTORY` variável de ambiente não é suportada num contexto de computação do HDInsight. No entanto, é fácil alcançar o mesmo resultado usando explicitamente um caminho absoluto do armazenamento de Blobs do Azure para ler e gravar para o armazenamento de BLOBs anexado.

## <a name="option-3-use-external-durable-storage"></a>Opção 3: Utilizar o armazenamento durável externo

Pode utilizar o armazenamento durável externo para persistir o estado durante a execução. Esta opção é útil nos seguintes cenários:
- Os dados de entrada já estão armazenados no armazenamento durável, que é acessível a partir do ambiente de computação de destino.
- Os ficheiros não precisam de fazer parte dos registos do histórico de execuções.
- Os ficheiros devem ser compartilhados por execuções em vários ambientes de computação.
- Os ficheiros tem de ser capazes de resistir ao contexto de cálculo propriamente dito.

Uma boa abordagem é utilizar o armazenamento de Blobs do Azure a partir do código de Python ou PySpark. Eis um pequeno exemplo:

```python
from azure.storage.blob import BlockBlobService
from azure.storage.blob.models import PublicAccess
import glob
import os

ACCOUNT_NAME = "<your blob storage account name>"
ACCOUNT_KEY = "<account key>"
CONTAINER_NAME = "<container name>"

blob_service = BlockBlobService(account_name=ACCOUNT_NAME, account_key=ACCOUNT_KEY)

## Create a new container if necessary, or use an existing one
blob_service.create_container(CONTAINER_NAME, fail_on_exist=False, public_access=PublicAccess.Container)

# df is a pandas DataFrame
df.to_csv('mydata.csv', sep='\t', index=False)

# Export the mydata.csv file to Blob storage.
for name in glob.iglob('mydata.csv'):
    blob_service.create_blob_from_path(CONTAINER_NAME, 'single_file.csv', name)
```

Eis um pequeno exemplo para anexar qualquer armazenamento de Blobs do Azure arbitrário para um tempo de execução HDI Spark:
```python
def attach_storage_container(spark, account, key):
    config = spark._sc._jsc.hadoopConfiguration()
    setting = "fs.azure.account.key." + account + ".blob.core.windows.net"
    if not config.get(setting):
        config.set(setting, key)
 
attach_storage_container(spark, "<storage account name>", "<storage key>”)
```

## <a name="conclusion"></a>Conclusão
Porque o Azure Machine Learning executa scripts ao copiar a pasta de projeto inteiro para o contexto de cálculo de destino, tome cuidado especial com grande entrada, saída e arquivos de intermediários. Para as transações de arquivo grande, pode utilizar a pasta de saídas especial, a pasta partilhada que está acessível através do `AZUREML_NATIVE_SHARE_DIRECTORY` variável de ambiente ou armazenamento durável externo. 

## <a name="next-steps"></a>Passos Seguintes
- Reveja os [ficheiros de configuração de execução do Azure Machine Learning Workbench](experimentation-service-configuration-reference.md) artigo.
- Veja como o [Classifying Iris](tutorial-classifying-iris-part-1.md) projeto de tutorial utiliza a pasta de saídas para manter um modelo preparado.
