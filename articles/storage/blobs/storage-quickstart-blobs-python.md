---
title: "Guia de Introdução do Azure – Transferir objetos de/para o armazenamento de Blobs do Azure com o Python | Microsoft Docs"
description: Aprenda rapidamente a transferir objetos de/para o armazenamento de Blobs do Azure com o Python
services: storage
author: ruthogunnnaike
manager: jeconnoc
ms.service: storage
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: quickstart
ms.date: 10/12/2017
ms.author: v-ruogun
ms.openlocfilehash: 50f43e6ef9ee60cbf489bb8d0c1c64ca61a393e1
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/02/2018
---
#  <a name="transfer-objects-tofrom-azure-blob-storage-using-python"></a>Transferir objetos de/para o armazenamento de Blobs do Azure com o Python
Neste guia de introdução, vai aprender a utilizar o Python, para carregar, transferir e listar blobs de blocos num contentor no armazenamento de Blobs do Azure. 

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este guia de início rápido: 
* Instalar o [Python](https://www.python.org/downloads/)
* Transfira e instale o [SDK de Armazenamento do Azure para Python](storage-python-how-to-use-blob-storage.md#download-and-install-azure-storage-sdk-for-python). 

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [storage-quickstart-tutorial-create-account-portal](../../../includes/storage-quickstart-tutorial-create-account-portal.md)]

## <a name="download-the-sample-application"></a>Transferir a aplicação de exemplo
A [aplicação de exemplo](https://github.com/Azure-Samples/storage-blobs-python-quickstart.git) utilizada neste guia de introdução é uma aplicação de Python básica.  

Utilize o [git](https://git-scm.com/) para transferir uma cópia da aplicação para o seu ambiente de desenvolvimento. 

```bash
git clone https://github.com/Azure-Samples/storage-blobs-python-quickstart.git 
```

Este comando clona o repositório para a sua pasta local do git. Para abrir o programa de Python, procure a pasta storage-blobs-python-quickstart e o ficheiro .py de exemplo.  

## <a name="configure-your-storage-connection-string"></a>Configurar a cadeia de ligação de armazenamento
Na aplicação, tem de indicar o nome da conta de armazenamento e a chave de conta para criar um objeto `BlockBlobService`. Abra o ficheiro `example.py` no Explorador de Soluções no seu IDE. Substitua os valores **accountname** e **accountkey** pelo nome da conta e a chave. 

```python 
block_blob_service = BlockBlobService(account_name='accountname', account_key='accountkey') 
```

## <a name="run-the-sample"></a>Executar o exemplo
Este exemplo cria um ficheiro de teste na pasta "Documentos". O programa de exemplo carrega o ficheiro de teste para o armazenamento de Blobs, lista os blobs no contentor e transfere o ficheiro com um novo nome. 

Execute o exemplo. O resultado seguinte é um exemplo do resultado devolvido ao executar a aplicação:
  
```
Temp file = C:\Users\azureuser\Documents\QuickStart_9f4ed0f9-22d3-43e1-98d0-8b2c05c01078.txt

Uploading to Blob storage as blobQuickStart_9f4ed0f9-22d3-43e1-98d0-8b2c05c01078.txt

List blobs in the container
         Blob name: QuickStart_9f4ed0f9-22d3-43e1-98d0-8b2c05c01078.txt

Downloading blob to C:\Users\azureuser\Documents\QuickStart_9f4ed0f9-22d3-43e1-98d0-8b2c05c01078_DOWNLOADED.txt
```
Quando prime qualquer tecla para continuar, o programa de exemplo elimina o contentor de armazenamento e os ficheiros. Antes de continuar, procure os dois ficheiros na pasta "Documentos". Pode abrir e ver que são idênticos.

Também pode utilizar uma ferramenta como o [Explorador de Armazenamento do Azure](http://storageexplorer.com) para ver os ficheiros no armazenamento de Blobs. O Explorador de Armazenamento do Azure é uma ferramenta multiplataformas gratuita que lhe permite aceder às informações da sua conta de armazenamento. 

Depois de verificar os ficheiros, prima qualquer tecla para concluir a demonstração e eliminar os ficheiros de teste. Agora que sabe o que o exemplo faz, abra o ficheiro example.py para ver o código. 

## <a name="understand-the-sample-code"></a>Compreender o código de exemplo

Em seguida, vamos analisar o código de exemplo, para que saiba como funciona.

### <a name="get-references-to-the-storage-objects"></a>Obter referências para os objetos de armazenamento
A primeira coisa a fazer é criar as referências para os objetos utilizados para aceder e gerir ao armazenamento de Blobs. Estes objetos dependem uns dos outros e cada um é utilizado pelo que vem a seguir na lista.

* Instancie o objeto **BlockBlobService**, que aponta para o serviço Blob na sua conta de armazenamento. 

* Instancie o objeto **CloudBlobContainer**, que represente o contentor a que está aceder. Os contentores servem para organizar os blobs, da mesma forma como utiliza pastas para organizar os ficheiros.

Quando tiver o contentor de Blobs da Cloud, pode instanciar o objeto **CloudBlockBlob**, que aponte para o blob específico no qual está interessado e realizar operações, como carregamento, transferência e cópia.

> [!IMPORTANT]
> Os nomes dos contentores têm de estar em minúscula. Para obter informações sobre os nomes dos contentores e dos blobs, veja [Naming and Referencing Containers, Blobs, and Metadata (Dar Nomes e Referenciar Contentores, Blobs e Metadados)](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata).

Nesta secção, vai instanciar os objetos, criar um contentor novo e, em seguida, definir as permissões no contentor, para que os blobs sejam públicos. O contentor é designado **quickstartblobs**. 

```python 
# Create the BlockBlockService that is used to call the Blob service for the storage account
block_blob_service = BlockBlobService(account_name='accountname', account_key='accountkey') 
 
# Create a container called 'quickstartblobs'.
container_name ='quickstartblobs'
block_blob_service.create_container(container_name) 

# Set the permission so the blobs are public.
block_blob_service.set_container_acl(container_name, public_access=PublicAccess.Container)
```
### <a name="upload-blobs-to-the-container"></a>Carregar blobs para o contentor

O armazenamento de blobs suporta blobs de blocos, blobs de acréscimo e blobs de páginas. Os blobs de blocos são os mais utilizados e vamos utilizá-los neste guia de introdução.  

Para carregar um ficheiro para um blob, obtenha o caminho completo do ficheiro ao associar o nome de diretório e o nome de ficheiro no disco local. Em seguida, pode carregar o ficheiro para o caminho especificado, com método **create\_blob\_from\_path**. 

O código de exemplo cria um ficheiro local que vai servir para o carregamento e a transferência, armazenando o ficheiro a carregar como **file\_path\_to\_file** e o nome do blob como **local\_file\_name**. O exemplo seguinte carrega o ficheiro para o seu contentor com o nome **quickstartblobs**.

```python
# Create a file in Documents to test the upload and download.
local_path=os.path.expanduser("~\Documents")
local_file_name ="QuickStart_" + str(uuid.uuid4()) + ".txt"
full_path_to_file =os.path.join(local_path, local_file_name)

# Write text to the file.
file = open(full_path_to_file,  'w')
file.write("Hello, World!")
file.close()

print("Temp file = " + full_path_to_file)
print("\nUploading to Blob storage as blob" + local_file_name)

# Upload the created file, use local_file_name for the blob name
block_blob_service.create_blob_from_path(container_name, local_file_name, full_path_to_file)
```

Pode utilizar diversos métodos de carregamento com o armazenamento de Blobs. Por exemplo, se tiver um fluxo de memória, pode utilizar o método **create\_blob\_from\_stream** em vez de **create\_blob\_from\_path**. 

Os blobs de blocos podem ter no máximo 4,7 TB e podem ser qualquer coisa, desde folhas de cálculo do Excel a ficheiros grandes de vídeo. Os blobs de páginas utilizam-se principalmente para os ficheiros VHD utilizados para fazer cópias de VMs de IaaS. Os blobs de acréscimo servem para registo, como quando quer escrever num ficheiro e continuar a adicionar mais informações. A maioria dos objetos guardados no armazenamento de Blobs são blobs de blocos.

### <a name="list-the-blobs-in-a-container"></a>Listar os blobs num contentor

Obtenha uma lista de ficheiros no contentor com o método **list_blobs**. Este método devolve um gerador. O código seguinte obtém a lista de blobs, depois percorre-os e mostra os nomes dos blobs encontrados num contentor.  

```python
# List the blobs in the container
print("\nList blobs in the container")
    generator = block_blob_service.list_blobs(container_name)
    for blob in generator:
        print("\t Blob name: " + blob.name)
```

### <a name="download-the-blobs"></a>Transferir os blobs

Transfira blobs para o seu disco local com o método **get\_blob\_to\_path**. O código seguinte transfere o blob atualizado numa secção anterior. "_DOWNLOADED" é adicionado como um sufixo ao nome do blob, para que possa ver ambos os ficheiros no disco local. 

```python
# Download the blob(s).
# Add '_DOWNLOADED' as prefix to '.txt' so you can see both files in Documents.
full_path_to_file2 = os.path.join(local_path, string.replace(local_file_name ,'.txt', '_DOWNLOADED.txt'))
print("\nDownloading blob to " + full_path_to_file2)
block_blob_service.get_blob_to_path(container_name, local_file_name, full_path_to_file2)
```

### <a name="clean-up-resources"></a>Limpar recursos
Se já não precisa dos blobs carregados neste guia de introdução, pode eliminar o contentor inteiro com **delete\_container**. Se os ficheiros criados já não são necessários, utilize o método **delete\_blob** para eliminar os ficheiros.

```python
# Clean up resources. This includes the container and the temp files
block_blob_service.delete_container(container_name)
os.remove(full_path_to_file)
os.remove(full_path_to_file2)
```

## <a name="next-steps"></a>Passos seguintes
 
Neste guia de introdução, aprendeu a transferir ficheiros entre um disco local e o armazenamento de blobs do Azure com Python. Para saber mais sobre a utilização do armazenamento de blobs, avance para os Procedimentos do armazenamento de Blobs.

> [!div class="nextstepaction"]
> [Procedimentos de Operações de Armazenamento de Blobs](./storage-python-how-to-use-blob-storage.md)
 

Para obter mais informações sobre o Explorador de armazenamento e os Blobs, veja [Manage Azure Blob storage resources with Storage Explorer](../../vs-azure-tools-storage-explorer-blobs.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) (Gerir recursos do armazenamento de Blobs do Azure com o Explorador de Armazenamento).
