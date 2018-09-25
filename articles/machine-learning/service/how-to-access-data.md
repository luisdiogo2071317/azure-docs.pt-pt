---
title: Utilizar arquivos de dados no Azure Machine Learning para aceder a dados
description: Como utilizar arquivos de dados para o armazenamento de dados de acesso durante o treinamento
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: minxia
author: mx-iao
ms.reviewer: sgilley
ms.date: 09/24/2018
ms.openlocfilehash: 615ab592c040eedf7d31e3a3036f558ea6c09740
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46990812"
---
# <a name="how-to-access-data-during-training"></a>Como acessar dados durante o treinamento
Nos serviços do Azure Machine Learning, um arquivo de dados é uma abstração sobre [armazenamento do Azure](https://docs.microsoft.com/azure/storage/common/storage-introduction). O arquivo de dados pode fazer referência a um [BLOBs do Azure](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction) contentor ou [partilha de ficheiros do Azure](https://docs.microsoft.com/azure/storage/files/storage-files-introduction) como o armazenamento subjacente. Arquivos de dados permitem-lhe aceder e interagir facilmente com o armazenamento de dados durante a seus fluxos de trabalho do Azure Machine Learning através do SDK de Python ou a CLI.

## <a name="create-a-datastore"></a>Criar um arquivo de dados
Para poder usar arquivos de dados, precisará primeiro uma [área de trabalho](concept-azure-machine-learning-architecture.md#workspace). Pode criar uma nova área de trabalho ou obter um já existente:

```Python
import azureml.core
from azureml.core import Workspace

ws = Workspace.from_config()
```

### <a name="use-the-default-datastore"></a>Utilizar o arquivo de dados padrão
Cada área de trabalho tem um predefinição arquivo de dados que pode começar a utilizar de imediato, que lhe permite poupar o trabalho de criação e configuração de suas próprias contas de armazenamento.

Para obter o arquivo de dados do espaço de trabalho predefinido:
```Python
ds = ws.get_default_datastore()
```

### <a name="register-a-datastore"></a>Registe-se de um arquivo de dados
Se já tiver o armazenamento do Azure existente, pode registá-lo como um arquivo de dados na sua área de trabalho. O Azure Machine Learning fornece a funcionalidade para registrar um contentor de Blobs do Azure ou a partilha de ficheiros do Azure como um arquivo de dados. Todos os métodos de Registro são sobre o `Datastore` classe e têm a forma `register_azure_*`.

#### <a name="azure-blob-container-datastore"></a>Arquivo de dados de contentor de Blobs do Azure
Para registar um arquivo de dados do contentor de Blobs do Azure:

```Python
ds = Datastore.register_azure_blob_container(workspace=ws, 
                                             datastore_name='your datastore name', 
                                             container_name='your azure blob container name',
                                             account_name='your storage account name', 
                                             account_key='your storage account key',
                                             create_if_not_exists=True)
```

#### <a name="azure-file-share-datastore"></a>Arquivo de dados de partilha de ficheiros do Azure
Para registar um arquivo de dados de partilha de ficheiros do Azure:

```Python
ds = Datastore.register_azure_file_share(workspace=ws, 
                                         datastore_name='your datastore name', 
                                         container_name='your file share name',
                                         account_name='your storage account name', 
                                         account_key='your storage account key',
                                         create_if_not_exists=True)
```

### <a name="get-an-existing-datastore"></a>Obter um arquivo de dados existente
Para consultar um arquivo de dados registado pelo nome:
```Python
ds = Datastore.get(ws, datastore_name='your datastore name')
```

Também pode obter todos os arquivos de dados para uma área de trabalho:
```Python
datastores = ws.datastores()
for name, ds in datastores.items(),
    print(name, ds.datastore_type)"
```

Para sua comodidade, se quiser definir um dos seus arquivos de dados registados como o arquivo de dados predefinido para a área de trabalho, poderá fazer isso da seguinte forma:
```Python
ws.set_default_datastore('your datastore name')
```

## <a name="upload-and-download-data"></a>Carregar e transferir dados
### <a name="upload"></a>Carregar
Pode carregar um diretório ou arquivos individuais para o arquivo de dados com o SDK de Python.

Para carregar um diretório para um arquivo de dados `ds`:
```Python
ds.upload(src_dir='your source directory',
          target_path='your target path',
          overwrite=True,
          show_progress=True)
```
`target_path` Especifica a localização na partilha de ficheiros (ou contentor de BLOBs) para carregar. Assume como predefinição `None`, caso em que os dados são carregados para a raiz. `overwrite=True` irá substituir quaisquer dados existentes em `target_path`.

Em alternativa pode carregar uma lista de arquivos individuais para o arquivo de dados por meio do arquivo de dados `upload_files()` método.

### <a name="download"></a>Transferência
Da mesma forma, pode transferir dados de um arquivo de dados ao seu sistema de ficheiros local.

```Python
ds.download(target_path='your target path',
            prefix='your prefix',
            show_progress=True)
```
`target_path` é a localização do diretório local para transferir os dados. Para especificar um caminho para a pasta na partilha de ficheiros (ou contentor de BLOBs) para transferir, fornecer esse caminho em `prefix`. Se `prefix` é `None`, todo o conteúdo da sua partilha de ficheiros (ou o contentor de BLOBs) que irá ser descarregado.

## <a name="access-datastores-for-training"></a>Acesso a arquivos de dados de treinamento
Pode acessar um arquivo de dados durante um treinamento ser executado (por exemplo, para dados de treinamento ou validação) num destino de computação remota através do SDK de Python. 

Existem duas formas de suporte para tornar o seu arquivo de dados disponíveis na computação remota:
* **Montagem**  
`ds.as_mount()`: ao especificar este modo de montagem, o arquivo de dados irá ser montado para na computação remota. 
* **Carregamento/transferência**  
    * `ds.as_download(path_on_compute='your path on compute')`: com este modo de transferência, os dados irão obter transferidos a partir de seu arquivo de dados para a computação remota para a localização especificada pela `path_on_compute`.
    * Por outro lado, também pode carregar os dados que foi produzidos do seu treinamento executar cópia de segurança para um arquivo de dados. Por exemplo, se o seu script de treinamento cria um `foo.pkl` de ficheiros no diretório de trabalho atual na computação remoto, pode especificar para o mesmo para obter carregado para o arquivo de dados depois de executar o script: `ds.as_upload(path_on_compute='./foo.pkl')`. Isso irá carregar o ficheiro para a raiz do seu arquivo de dados.
    
Se quiser referenciar um ficheiro no seu arquivo de dados ou a pasta específica, pode usar o arquivo de dados **`path`** função. Por exemplo, se seu arquivo de dados tem um diretório com o caminho relativo `./bar`e apenas pretender transferir o conteúdo dessa pasta para o destino de computação, pode fazê-lo da seguinte forma: `ds.path('./bar').as_download()`

Qualquer `ds` ou `ds.path` objeto é resolvido para um nome de variável de ambiente do formato `"$AZUREML_DATAREFERENCE_XXXX"` cujo valor representa o caminho de montagem/transferir na computação remoto. O caminho de arquivo de dados na computação remoto não pode ser o mesmo que o caminho de execução para o script.

Para aceder ao seu arquivo de dados durante o treinamento, pode passá-lo para o seu script de treinamento como um argumento da linha de comandos por meio de `script_params`:

```Python
from azureml.train.estimator import Estimator

script_params = {
    '--data_dir': ds.as_mount()
}

est = Estimator(source_directory='your code directory',
                script_params=script_params,
                compute_target=compute_target,
                entry_script='train.py')
```
`as_mount()` é o modo predefinido para um arquivo de dados, para que pode também diretamente passar `ds` para o `'--data_dir'` argumento.

Em alternativa, pode passar uma lista de arquivos de dados para o `inputs` parâmetro do construtor Estimator montar ou copiar de/para o seu datastore(s):

```Python
est = Estimator(source_directory='your code directory',
                compute_target=compute_target,
                entry_script='train.py',
                inputs=[ds1.as_download(), ds2.path('./foo').as_download(), ds3.as_upload(path_on_compute='./bar.pkl')])
```
O código acima irá:
* transferir todo o conteúdo no arquivo de dados `ds1` para a computação remota antes do script de treinamento `train.py` é executado
* Transfira a pasta `'./foo'` no arquivo de dados `ds2` para a computação remota antes de `train.py` é executado
* Carregue o ficheiro `'./bar.pkl'` de computação remota até o arquivo de dados `d3` após a execução do seu script

## <a name="next-steps"></a>Passos Seguintes
* [Preparar um modelo](how-to-train-ml-models.md)
