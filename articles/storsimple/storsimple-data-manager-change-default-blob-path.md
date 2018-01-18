---
title: "Caminho de blob de alteração da predefinição | Microsoft Docs"
description: "Saiba como configurar uma função do Azure para mudar o nome de um caminho de ficheiro do blob"
services: storsimple
documentationcenter: NA
author: alkohli
manager: jeconnoc
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 01/16/2018
ms.author: alkohli
ms.openlocfilehash: f73d9dcedee5165af752b9e10fb70de860e8e98b
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/17/2018
---
# <a name="change-a-blob-path-from-the-default-path"></a>Alterar um caminho de blob do caminho predefinido

Quando o serviço StorSimple Manager de dados transforma os dados, por predefinição coloca os blobs transformados num contentor de armazenamento, conforme especificado durante a criação do repositório de destino. Chegarem blobs nesta localização, pode pretender mover estes blobs para uma localização alternativa. Este artigo descreve como configurar uma função do Azure para mudar o nome de um caminho de ficheiro predefinido de blob e, por conseguinte, mova os blobs para uma localização diferente.

## <a name="prerequisites"></a>Pré-requisitos

Certifique-se de que tem uma definição de tarefa corretamente configurada no serviço StorSimple Manager de dados.

## <a name="create-an-azure-function"></a>Criar uma função do Azure

Para criar uma função do Azure, execute os seguintes passos:

1. Aceda ao [Portal do Azure](http://portal.azure.com/).

2. Clique em **+ criar um recurso**. No **pesquisa** caixa, escreva **aplicação de função** e prima **Enter**. Selecione e clique em **aplicação de função** na lista de aplicações apresentada.

    ![Escreva "Aplicação de função" na caixa de pesquisa](./media/storsimple-data-manager-change-default-blob-path/search-function-app.png)

3. Clique em **Criar**.

    ![O botão de "Criar" da janela de aplicação de função](./media/storsimple-data-manager-change-default-blob-path/create-function-app.png)

4. No **aplicação de função** painel de configuração, execute os seguintes passos:

    1. Forneça um exclusivo **nome da aplicação**.
    2. Na lista pendente, selecione o **subscrição**. Esta subscrição deve ser idêntica à associados com o serviço StorSimple Manager de dados.
    3. Selecione **criar nova** grupo de recursos.
    4. Para o **alojamento planear** na lista pendente, selecione **consumo planear**.
    5. Especifique uma localização onde a sua função é executada. Pretende que a mesma região onde o serviço StorSimple Manager de dados e a conta de armazenamento associada a definição de tarefa, estão localizados.
    6. Selecione uma conta de armazenamento existente ou crie uma nova. Uma conta de armazenamento é utilizada internamente para a função.

        ![Introduza novos dados de configuração de aplicação de função](./media/storsimple-data-manager-change-default-blob-path/function-app-parameters.png)

    7. Clique em **Criar**. A aplicação de função é criada.
     
        ![Aplicação de função criada](./media/storsimple-data-manager-change-default-blob-path/function-app-created.png)

5. Selecione **funções**e clique em **+ nova função**.

    ![Clique em + nova função](./media/storsimple-data-manager-change-default-blob-path/create-new-function.png)

6. Selecione **c#** para o idioma. Na matriz de mosaicos do modelo, selecione **c#** no **QueueTrigger CSharp** mosaico.

7. No **acionador de fila**:

    1. Introduza um **nome** para a sua função.
    2. No **nome da fila** caixa, escreva o nome de definição de tarefa de transformação de dados.
    3. Em **ligação de conta de armazenamento**, clique em **novo**. Na lista de contas do storage, selecione a conta associada a definição da tarefa. Anote o nome da ligação (realçado). É necessário o nome mais tarde na função do Azure.

        ![Criar uma nova função de c#](./media/storsimple-data-manager-change-default-blob-path/new-function-parameters.png)

    4. Clique em **Criar**. O **função** é criado.

     
10. Na janela de função, execute _.csx_ ficheiro.

    ![Criar uma nova função de c#](./media/storsimple-data-manager-change-default-blob-path/new-function-run-csx.png)
    
    Execute os seguintes passos.

    1. Cole o seguinte código:

        ```
        using System;
        using System.Configuration;
        using Microsoft.WindowsAzure.Storage.Blob;
        using Microsoft.WindowsAzure.Storage.Queue;
        using Microsoft.WindowsAzure.Storage;
        using System.Collections.Generic;
        using System.Linq;

        public static void Run(QueueItem myQueueItem, TraceWriter log)
        {
            CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConfigurationManager.AppSettings["STORAGE_CONNECTIONNAME"]);

            string storageAccUriEndswith = "windows.net/";
            string uri = myQueueItem.TargetLocation.Replace("%20", " ");
            log.Info($"Blob Uri: {uri}");

            // Remove storage account uri string
            uri = uri.Substring(uri.IndexOf(storageAccUriEndswith) + storageAccUriEndswith.Length);

            string containerName = uri.Substring(0, uri.IndexOf("/")); 

            // Remove container name string
            uri = uri.Substring(containerName.Length + 1);

            // Current blob path
            string blobName = uri; 

            string volumeName = uri.Substring(containerName.Length + 1);
            volumeName = uri.Substring(0, uri.IndexOf("/"));

            // Remove volume name string
            uri = uri.Substring(volumeName.Length + 1);

            string newContainerName = uri.Substring(0, uri.IndexOf("/")).ToLower();
            string newBlobName = uri.Substring(newContainerName.Length + 1);

            log.Info($"Container name: {containerName}");
            log.Info($"Volume name: {volumeName}");
            log.Info($"New container name: {newContainerName}");

            log.Info($"Blob name: {blobName}");
            log.Info($"New blob name: {newBlobName}");

            // Create the blob client.
            CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

            // Container reference
            CloudBlobContainer container = blobClient.GetContainerReference(containerName);
            CloudBlobContainer newContainer = blobClient.GetContainerReference(newContainerName);
            newContainer.CreateIfNotExists();

            if(!container.Exists())
            {
                log.Info($"Container - {containerName} not exists");
                return;
            }

            if(!newContainer.Exists())
            {
                log.Info($"Container - {newContainerName} not exists");
                return;
            }

            CloudBlockBlob blob = container.GetBlockBlobReference(blobName);
            if (!blob.Exists())
            {
                // Skip to copy the blob to new container, if source blob doesn't exist
                log.Info($"The specified blob does not exist.");
                log.Info($"Blob Uri: {blob.Uri}");
                return;
            }

            CloudBlockBlob blobCopy = newContainer.GetBlockBlobReference(newBlobName);
            if (!blobCopy.Exists())
            {
                blobCopy.StartCopy(blob);
                // Delete old blob, after copy to new container
                blob.DeleteIfExists();
                log.Info($"Blob file path renamed completed successfully");
            }
            else
            {
                log.Info($"Blob file path renamed already done");
                // Delete old blob, if already exists.
                blob.DeleteIfExists();
            }
        }

        public class QueueItem
        {
            public string SourceLocation {get;set;}
            public long SizeInBytes {get;set;}
            public string Status {get;set;}
            public string JobID {get;set;}
            public string TargetLocation {get; set;}
        }

        ```

    2. Substitua **STORAGE_CONNECTIONNAME** na linha 11 com a ligação de conta de armazenamento (consulte o passo 7 c).

        ![Nome de ligação de armazenamento de cópia](./media/storsimple-data-manager-change-default-blob-path/new-function-storage-connection-name.png)

    3. **Guardar** a função.

        ![Guardar a função](./media/storsimple-data-manager-change-default-blob-path/save-function.png)

12. Para concluir a função, adicione um ficheiro mais efetuando os seguintes passos:

    1. Clique em **ver ficheiros**.

       ![A hiperligação "Ver ficheiros"](./media/storsimple-data-manager-change-default-blob-path/view-files.png)

    2. Clique em **+ adicionar**.
        
        ![A hiperligação "Ver ficheiros"](./media/storsimple-data-manager-change-default-blob-path/new-function-add-file.png)
    
    3. Tipo **project.json**e, em seguida, prima **Enter**. No **project.json** ficheiro, cole o seguinte código:

        ```
        {
        "frameworks": {
            "net46":{
            "dependencies": {
                "windowsazure.storage": "8.1.1"
            }
            }
        }
        }

        ```

    
    4. Clique em **Guardar**.

        ![A hiperligação "Ver ficheiros"](./media/storsimple-data-manager-change-default-blob-path/new-function-project-json.png)

Foi criada com uma função do Azure. Esta função é acionada sempre que é gerado um novo blob pela tarefa de transformação de dados.

## <a name="next-steps"></a>Passos Seguintes

[Utilize o Gestor de dados StorSimple IU para transformar os seus dados](storsimple-data-manager-ui.md)
