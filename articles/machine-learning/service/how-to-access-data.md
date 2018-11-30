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
ms.openlocfilehash: ef183397cf7ca3f1f561f6951290b722df31e5ed
ms.sourcegitcommit: eba6841a8b8c3cb78c94afe703d4f83bf0dcab13
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/29/2018
ms.locfileid: "52620205"
---
# <a name="how-to-access-data-during-training"></a>Como acessar dados durante o treinamento
Utilize um arquivo de dados para aceder e interagir com os seus dados em fluxos de trabalho do Azure Machine Learning.

No serviço do Azure Machine Learning, o arquivo de dados é uma abstração sobre [armazenamento do Azure](https://docs.microsoft.com/azure/storage/common/storage-introduction). O arquivo de dados pode fazer referência a um [BLOBs do Azure](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction) contentor ou [partilha de ficheiros do Azure](https://docs.microsoft.com/azure/storage/files/storage-files-introduction) como o armazenamento subjacente. 

## <a name="create-a-datastore"></a>Criar um arquivo de dados
Para utilizar arquivos de dados, primeiro tem um [área de trabalho](concept-azure-machine-learning-architecture.md#workspace). Comece por qualquer um [criar uma nova área de trabalho](quickstart-create-workspace-with-python.md) ou obtenção de um já existente:

```Python
import azureml.core
from azureml.core import Workspace

ws = Workspace.from_config()
```

### <a name="use-the-default-datastore"></a>Utilizar o arquivo de dados padrão
Não é necessário para criar ou configurar uma conta de armazenamento.  Cada área de trabalho tem um arquivo de dados padrão que pode começar a utilizar imediatamente.

Para obter o arquivo de dados do espaço de trabalho predefinido:
```Python
ds = ws.get_default_datastore()
```

### <a name="register-a-datastore"></a>Registe-se de um arquivo de dados
Se tiver de armazenamento do Azure existente, pode registá-lo como um arquivo de dados na sua área de trabalho. Pode registrar um contentor de Blobs do Azure ou a partilha de ficheiros do Azure como um arquivo de dados. Todos os métodos de Registro são sobre o `Datastore` classe e têm a forma `register_azure_*`.

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
    print(name, ds.datastore_type)
```

Para sua comodidade, defina um dos seus arquivos de dados registados, como o arquivo de dados predefinido para a área de trabalho:
```Python
ws.set_default_datastore('your datastore name')
```

## <a name="upload-and-download-data"></a>Carregar e transferir dados
### <a name="upload"></a>Carregar
Carregar um diretório ou arquivos individuais para o arquivo de dados com o SDK de Python.

Para carregar um diretório para um arquivo de dados `ds`:
```Python
ds.upload(src_dir='your source directory',
          target_path='your target path',
          overwrite=True,
          show_progress=True)
```
`target_path` Especifica a localização na partilha de ficheiros (ou contentor de BLOBs) para carregar. Assume como predefinição `None`, caso em que os dados são carregados para a raiz. `overwrite=True` irá substituir quaisquer dados existentes em `target_path`.

Ou carregue uma lista de arquivos individuais para o arquivo de dados por meio do arquivo de dados `upload_files()` método.

### <a name="download"></a>Transferência
Da mesma forma, transferir dados de um arquivo de dados para o sistema de ficheiros local.

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
    * `ds.as_download(path_on_compute='your path on compute')` transfere dados a partir do seu arquivo de dados para a computação remota para a localização especificada pela `path_on_compute`.
    * `ds.as_upload(path_on_compute='yourfilename'` carrega dados para o arquivo de dados.  Suponha que o script de treinamento cria um `foo.pkl` ficheiro no diretório de trabalho atual na computação remoto. Carregar o ficheiro para o arquivo de dados com `ds.as_upload(path_on_compute='./foo.pkl')` depois do script cria o ficheiro. O ficheiro é carregado para a raiz do seu arquivo de dados.
    
Para fazer referência a uma pasta específica ou um ficheiro no seu arquivo de dados, utilize o arquivo de dados **`path`** função. Por exemplo, para transferir o conteúdo do `./bar` diretório a partir do arquivo de dados para o destino de computação, utilize `ds.path('./bar').as_download()`.

Qualquer `ds` ou `ds.path` objeto é resolvido para um nome de variável de ambiente do formato `"$AZUREML_DATAREFERENCE_XXXX"` cujo valor representa o caminho de montagem/transferir na computação remoto. O caminho de arquivo de dados na computação remoto não pode ser o mesmo que o caminho de execução para o script.

Para aceder ao seu arquivo de dados durante o treinamento, transmita-o para o script de treinamento como um argumento da linha de comandos por meio de `script_params`:

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

Ou passar uma lista de arquivos de dados para o construtor de Calculadora `inputs` parâmetro montar ou copiar de/para o seu datastore(s):

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
