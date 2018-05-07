---
title: Carregar ficheiros para uma conta de Media Services do Azure utilizando REST | Microsoft Docs
description: "Saiba como obter o conteúdo do suporte de dados para os serviços de suporte de dados através da criação e carregar recursos."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: juliako
ms.openlocfilehash: 4ba6fdcec8d71326b02d71dbad429be8c2052171
ms.sourcegitcommit: 71fa59e97b01b65f25bcae318d834358fea5224a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/11/2018
---
# <a name="upload-files-into-a-media-services-account-using-rest"></a>Carregar ficheiros para uma conta de Media Services utilizando REST
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-upload-files.md)
> * [REST](media-services-rest-upload-files.md)
> * [Portal](media-services-portal-upload-files.md)
> 

Nos Serviços de Multimédia, os ficheiros digitais são carregados para um elemento. O [Asset](https://docs.microsoft.com/rest/api/media/operations/asset) entidade pode conter vídeo, áudio, imagens, coleções de miniaturas, texto controla e legendas ficheiros (e os metadados sobre estes ficheiros.)  Depois dos ficheiros são carregados para o elemento, o conteúdo estiver armazenado em segurança na nuvem para processamento adicional e a transmissão em fluxo. 

Neste tutorial, irá aprender a carregar um ficheiro e outra operação associado:

> [!div class="checklist"]
> * Configurar o Postman para todas as operações de carregamento
> * Ligar aos Media Services 
> * Criar uma política de acesso com permissão de escrita
> * Criar um recurso
> * Criar um localizador SAS e crie o URL de carregamento
> * Carregar um ficheiro para o blob storage utilizando o URL de carregamento
> * Criar um metadados no recurso para o ficheiro de suporte de dados que carregou

## <a name="prerequisites"></a>Pré-requisitos

- Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de começar.
- [Criar uma conta de Media Services do Azure no portal do Azure](media-services-portal-create-account.md).
- Reveja o [aceder à API do Azure suporte de dados de serviços com descrição geral da autenticação do AAD](media-services-use-aad-auth-to-access-ams-api.md) artigo.
- Configurar **Postman** conforme descrito em [chama de configurar o Postman para API de REST dos serviços de suporte de dados](media-rest-apis-with-postman.md).

## <a name="considerations"></a>Considerações

Aplicam as seguintes considerações ao utilizar a API de REST dos serviços de suporte de dados:
 
* Ao aceder a entidades utilizando a API de REST dos serviços de suporte de dados, tem de definir campos de cabeçalho específicos e os valores no seus pedidos HTTP. Para obter mais informações, consulte [programa de configuração para o desenvolvimento de API de REST de serviços de suporte de dados](media-services-rest-how-to-use.md). <br/>A coleção de Postman utilizada neste tutorial encarrega-se de definir os cabeçalhos necessários.
* Os Media Services utiliza o valor da propriedade IAssetFile.Name ao criar os URLs para os conteúdos de transmissão em fluxo (por exemplo, http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Por este motivo, por cento de codificação não é permitida. O valor da **nome** propriedade não pode ter qualquer um dos seguintes [por cento codificação-reservados carateres](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters):! *' ();: @& = + $, /? % # [] ". Além disso, só pode existir um '.' para a extensão de nome de ficheiro.
* O comprimento do nome não deve ser superior a 260 carateres.
* Existe um limite para o tamanho máximo dos ficheiros suportado para processamento nos Serviços de Multimédia. Consulte [isto](media-services-quotas-and-limitations.md) artigo para obter detalhes sobre a limitação de tamanho de ficheiro.

## <a name="set-up-postman"></a>Configurar o Postman

Para obter os passos sobre como configurar o Postman para este tutorial, consulte [configurar Postman](media-rest-apis-with-postman.md).

## <a name="connect-to-media-services"></a>Ligar aos Media Services

1. Adicione valores de ligação ao seu ambiente. 

    Algumas variáveis que fazem parte do **MediaServices** [ambiente](postman-environment.md) tem de ser definida manualmente antes de começar a executar operações definidas no [coleção](postman-collection.md).

    Para obter os valores para as variáveis de cinco pela primeira vez, consulte [aceder à API de serviços de suporte de dados do Azure com a autenticação do Azure AD](media-services-use-aad-auth-to-access-ams-api.md). 

    ![Carregar um ficheiro](./media/media-services-rest-upload-files/postman-import-env.png)
2. Especifique o valor para o **MediaFileName** variável de ambiente.

    Especifique o nome de ficheiro do suporte de dados que estiver a planear para carregar. Neste exemplo, vamos para carregar o BigBuckBunny.mp4. 
3. Examine o **AzureMediaServices.postman_environment.json** ficheiro. Verá que quase todas as operações na coleção de executar um script de "teste". Os scripts demorar alguns valores devolvidos pelo resposta e definir variáveis de ambiente adequado.

    Por exemplo, a primeira operação obtém um token de acesso e defina-o no **AccessToken** variável de ambiente que é utilizado em todas as outras operações.

    ```    
    "listen": "test",
    "script": {
        "type": "text/javascript",
        "exec": [
            "var json = JSON.parse(responseBody);",
            "postman.setEnvironmentVariable(\"AccessToken\", json.access_token);"
        ]
    }
    ```
4. À esquerda do **Postman** janela, clique em **1. Obtenha o token de autenticação do AAD** -> **obter o Azure AD Token para o Principal de serviço**.

    A parte do URL é preenchida com o **AzureADSTSEndpoint** variável de ambiente (o valor que configurou anteriormente no tutorial).
    
5. Prima **Enviar**.

    ![Carregar um ficheiro](./media/media-services-rest-upload-files/postment-get-token.png)

    Pode ver a resposta que contenha "access_token". O script de "teste" utiliza este valor e define o **AccessToken** variável de ambiente (conforme descrito acima). Se examinar as variáveis de ambiente, verá que esta variável contém agora o valor do token (token de portador) de acesso que é utilizado o resto das operações. 

    Se o token expira percorrer novamente o passo "Obter o Azure AD Token para o serviço Principal". 

## <a name="create-an-access-policy-with-write-permission"></a>Criar uma política de acesso com permissão de escrita

### <a name="overview"></a>Descrição geral 

>[!NOTE]
>Existe um limite de 1,000,000 políticas para diferentes políticas do AMS (por exemplo, para a política Locator ou ContentKeyAuthorizationPolicy). Deve utilizar o mesmo ID de política se estiver a utilizar sempre os mesmas permissões de dias/acesso, por exemplo, políticas para localizadores que pretendam permanecem no local durante muito tempo (políticas de não carregamento). Para obter mais informações, veja [este](media-services-dotnet-manage-entities.md#limit-access-policies) artigo.

Antes de carregar ficheiros para o armazenamento de BLOBs, defina o acesso de direitos de política para escrever um recurso. Para tal, PUBLIQUE um pedido HTTP para o conjunto de entidades AccessPolicies. Definir um valor de DurationInMinutes após a criação ou receber uma mensagem de erro de servidor interno 500 na resposta. Para obter mais informações sobre AccessPolicies, consulte [AccessPolicy](https://docs.microsoft.com/rest/api/media/operations/accesspolicy).

### <a name="create-an-access-policy"></a>Criar uma política de acesso

1. Selecione **AccessPolicy** -> **criar AccessPolicy para carregamento**.
2. Prima **Enviar**.

    ![Carregar um ficheiro](./media/media-services-rest-upload-files/postman-access-policy.png)

    O script de "teste" obtém o Id de AccessPolicy e define a variável de ambiente adequado.

## <a name="create-an-asset"></a>Criar um recurso

### <a name="overview"></a>Descrição geral

Um [asset](https://docs.microsoft.com/rest/api/media/operations/asset) é um contentor para vários tipos ou conjuntos de objetos nos Media Services, incluindo as vídeo, áudio, imagens, coleções de miniaturas, controla de texto e legendas ficheiros. Na REST API, criar um recurso necessita de enviar o pedido POST para serviços de suporte de dados e posicionar quaisquer informações de propriedade sobre o elemento no corpo do pedido.

Uma das propriedades que pode adicionar ao criar um recurso é **opções**. Pode especificar uma das seguintes opções de encriptação: **nenhum** (o predefinido, sem encriptação é utilizado), **StorageEncrypted** (para conteúdo que foi previamente encriptado com encriptação de armazenamento do lado do cliente), **CommonEncryptionProtected**, ou **EnvelopeEncryptionProtected**. Quando tiver um recurso encriptado, terá de configurar uma política de entrega. Para obter mais informações, consulte [configurar políticas de entrega de elemento](media-services-rest-configure-asset-delivery-policy.md).

Se o seu elemento é encriptado, tem de criar um **ContentKey** e ligá-lo ao seu elemento, conforme descrito no seguinte artigo: [como criar um ContentKey](media-services-rest-create-contentkey.md). Depois de carregar os ficheiros no elemento, tem de atualizar as propriedades de encriptação no **AssetFile** entidade com os valores recebeu durante o **Asset** encriptação. Fazê-lo utilizando o **intercalar** pedido de HTTP. 

Neste exemplo, estamos a criar um recurso não encriptado. 

### <a name="create-an-asset"></a>Criar um recurso

1. Selecione **ativos** -> **criar elemento**.
2. Prima **Enviar**.

    ![Carregar um ficheiro](./media/media-services-rest-upload-files/postman-create-asset.png)

    O script de "teste" obtém o Id de recurso e define a variável de ambiente adequado.

## <a name="create-a-sas-locator-and-create-the-upload-url"></a>Criar um localizador SAS e crie o URL de carregar

### <a name="overview"></a>Descrição geral

Assim que tiver o AccessPolicy e localizador definido, o ficheiro propriamente dito é carregado para um contentor de Blob Storage do Azure com as APIs de REST de armazenamento do Azure. Tem de carregar os ficheiros como blobs de blocos. Os blobs de página não são suportados pelos Media Services do Azure.  

Para obter mais informações sobre como trabalhar com blobs de armazenamento do Azure, consulte [API de REST do serviço Blob](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API).

Para receber o URL de carregamento real, crie um localizador SAS (mostrado abaixo). Os localizadores definem a hora de início e o tipo de ponto final de ligação para os clientes que pretendem aceder a ficheiros de um recurso. Pode criar várias entidades de localizador para um determinado par de recursos e AccessPolicy processar pedidos de cliente diferentes e as suas necessidades. Cada um destes localizadores utiliza o valor de StartTime e o valor de DurationInMinutes do AccessPolicy para determinar o período de tempo que pode ser utilizado um URL. Para obter mais informações, consulte [localizador](https://docs.microsoft.com/rest/api/media/operations/locator).

Um URL SAS tem o seguinte formato:

    {https://myaccount.blob.core.windows.net}/{asset name}/{video file name}?{SAS signature}

### <a name="considerations"></a>Considerações

São aplicáveis algumas considerações:

* Não pode ter mais de cinco localizadores exclusivos associados um determinado elemento de uma só vez. Para obter mais informações, consulte localizador.
* Se precisar de carregar os ficheiros imediatamente, deve definir o valor de StartTime para cinco minutos antes da hora atual. Isto acontece porque poderá haver relógio dissimetrias entre o computador cliente e os Media Services. Além disso, o valor de StartTime tem de estar no seguinte formato DateTime: aaaa-MM-aaaathh (por exemplo, "2014-05-23T17:53:50Z").    
* Poderá existir um segundo 30-40 atrasar depois de é criado um localizador para quando está disponível para utilização.

### <a name="create-a-sas-locator"></a>Criar um localizador SAS

1. Selecione **localizador** -> **criar localizador SAS**.
2. Prima **Enviar**.

    O script de "teste" cria o "URL de carregar" com base no nome de ficheiro do suporte de dados que especificou e informações de localizador SAS e define a variável de ambiente adequado.

    ![Carregar um ficheiro](./media/media-services-rest-upload-files/postman-create-sas-locator.png)

## <a name="upload-a-file-to-blob-storage-using-the-upload-url"></a>Carregar um ficheiro para o blob storage utilizando o URL de carregamento

### <a name="overview"></a>Descrição geral

Agora que tem o URL de carregamento, terá de escrever alguns códigos utilizando as APIs de Blob do Azure diretamente ao carregar o ficheiro para o contentor SAS. Para obter mais informações, veja os artigos seguintes:

- [Utilizar o armazenamento do Azure REST API](https://docs.microsoft.com/azure/storage/common/storage-rest-api-auth?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
- [COLOCAR Blob](https://docs.microsoft.com/rest/api/storageservices/put-blob)
- [Carregue os blobs no armazenamento de BLOBs](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy#upload-blobs-to-blob-storage)

### <a name="upload-a-file-with-postman"></a>Carregar um ficheiro com o Postman

Por exemplo, utilizamos Postman para carregar um ficheiro mp4 pequeno. Pode haver um limite de tamanho de ficheiro no carregamento binário através do Postman.

O pedido de carregamento não é parte do **AzureMedia** coleção. 

Criar e configurar um novo pedido:
1. Prima  **+** , para criar um novo separador do pedido.
2. Selecione **colocar** operação e colar **{{UploadURL}}** no URL.
2. Deixe **autorização** separador, é (não definido-lo para o **tokens de portador**).
3. No **cabeçalhos** separador, especifique: **chave**: "x-ms--tipo de blob" e **valor**: "BlockBlob".
2. No **corpo** separador, clique em **binário**.
4. Escolher o ficheiro com o nome que especificou no **MediaFileName** variável de ambiente.
5. Prima **Enviar**.

    ![Carregar um ficheiro](./media/media-services-rest-upload-files/postman-upload-file.png)

##  <a name="create-a-metadata-in-the-asset"></a>Criar um metadados no elemento

Depois do ficheiro foi carregado, tem de criar um metadados no recurso para o ficheiro de suporte de dados que carregou para o armazenamento de BLOBs associado com o seu elemento.

1. Selecione **AssetFiles** -> **CreateFileInfos**.
2. Prima **Enviar**.

    ![Carregar um ficheiro](./media/media-services-rest-upload-files/postman-create-file-info.png)

O ficheiro deve ser carregado e definir os seus metadados.

## <a name="validate"></a>Validação

Para validar que o ficheiro foi carregado com êxito, é aconselhável consultar o [AssetFile](https://docs.microsoft.com/rest/api/media/operations/assetfile) e compare o **ContentFileSize** (ou outros detalhes) para que o que esperava ver no recurso de novo. 

Por exemplo, o seguinte **obter** operação coloca os dados de ficheiro para o ficheiro de recurso (em ou caso, o ficheiro BigBuckBunny.mp4). A consulta está a utilizar o [variáveis de ambiente](postman-environment.md) que configurou anteriormente.

    {{RESTAPIEndpoint}}/Assets('{{LastAssetId}}')/Files

Resposta irá conter o tamanho, o nome e outras informações.

    "Id": "nb:cid:UUID:69e72ede-2886-4f2a-8d36-80a59da09913",
    "Name": "BigBuckBunny.mp4",
    "ContentFileSize": "3186542",
    "ParentAssetId": "nb:cid:UUID:0b8f3b04-72fb-4f38-8e7b-d7dd78888938",
            
## <a name="next-steps"></a>Passos Seguintes

Agora, pode codificar os elementos que carregar. Para obter mais informações, veja [Codificar elementos](media-services-portal-encode.md)

Também pode utilizar as Funções do Azure para acionar uma tarefa de codificação num ficheiro que esteja a chegar ao contentor configurado. Para obter mais informações, veja [este exemplo](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ).

