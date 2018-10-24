---
title: Armazenamento de Blobs do Azure em dispositivos do Azure IoT Edge | Documentos da Microsoft
description: Implemente um módulo de armazenamento de Blobs do Azure para o seu dispositivo IoT Edge para armazenar dados na periferia.
author: kgremban
manager: timlt
ms.author: kgremban
ms.reviewer: arduppal
ms.date: 10/03/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 4b86f73302d9f5d07cd1e6e8c7801de56a988cc7
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/24/2018
ms.locfileid: "49955291"
---
# <a name="store-data-at-the-edge-with-azure-blob-storage-on-iot-edge-preview"></a>Store dados na periferia com o armazenamento de Blobs do Azure no IoT Edge (pré-visualização)

Armazenamento de Blobs do Azure no IoT Edge fornece uma [blob de blocos](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-block-blobs) solução de armazenamento na periferia. Um módulo de armazenamento de BLOBs no seu dispositivo IoT Edge se comporta como um serviço de BLOBs de bloco do Azure, mas os blobs de bloco são armazenados localmente no seu dispositivo IoT Edge. Pode aceder a blobs com os mesmos métodos SDK de armazenamento do Azure ou bloquear chamadas de API de BLOBs que já está habituado a. 

Cenários onde os dados, como vídeos, imagens, dados financeiros, dados de hospital ou quaisquer dados que tem de ser armazenada localmente, mais tarde que poderiam ser processados localmente ou transferidos para a cloud são bons exemplos para utilizar este módulo.

Este artigo fornece instruções para implementar um armazenamento de Blobs do Azure no contêiner do IoT Edge que executa um serviço de BLOBs no seu dispositivo IoT Edge. 

>[!NOTE]
>Armazenamento de Blobs do Azure no IoT Edge está em [pré-visualização pública](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). 

## <a name="prerequisites"></a>Pré-requisitos

Um dispositivo Azure IoT Edge:

* Pode seguir os passos no início rápido para [dispositivos Linux](quickstart-linux.md) ou [Windows](quickstart.md) para utilizar o seu computador de desenvolvimento ou uma máquina virtual como um dispositivo Edge.
* Armazenamento de Blobs do Azure no módulo do IoT Edge suporta as seguintes configurações de dispositivo:

   | Sistema operativo | Arquitetura |
   | ---------------- | ------------ |
   | Ubuntu Server 16.04 | AMD64 |
   | Ubuntu Server 18.04 | AMD64 |
   | Windows 10 IoT Core (atualização de Outubro) | AMD64 |
   | Windows 10 IoT Enterprise (atualização de Outubro) | AMD64 |
   | Windows Server de 2019 | AMD64 |
   | Raspbian Stretch | ARM32 |

Recursos da cloud:

* Um [Hub IoT](../iot-hub/iot-hub-create-through-portal.md) no escalão standard no Azure. 


## <a name="deploy-blob-storage-to-your-device"></a>Implementar o armazenamento de BLOBs para o seu dispositivo

Existem várias formas de implementar módulos para um dispositivo IoT Edge e todos eles funcionam para armazenamento de Blobs do Azure em módulos do IoT Edge. Os dois métodos mais simples estão a utilizar os modelos de código do Visual Studio ou o portal do Azure. 

### <a name="azure-portal"></a>Portal do Azure

#### <a name="find-the-module"></a>Encontrar o módulo

Escolha uma das duas formas de localizar o módulo de armazenamento de BLOBs:

1. Na pesquisa do Portal do Azure para o "Azure Blob de armazenamento no IoT Edge". E **selecione** o item de resultado de pesquisa
2. Ir para o Marketplace no Portal do Azure, em seguida, clique em "Internet das coisas". Na secção "IoT Edge módulos" selecione "Azure Blob de armazenamento no IoT Edge". E clique em **Create**

#### <a name="steps-to-deploy"></a>Passos para implementar

**Dispositivos de destino para o módulo do IoT Edge**

1. Selecione a subscrição"" em que o seu IoT Hub é implementado.
2. Selecione "IoT Hub".
3. Forneça o "IoT Edge nome do dispositivo" onde pretende implementar este módulo. Pode optar por utilizar "Localizar dispositivo" para localizar o seu dispositivo.
4. Clique em **Criar**.

**Definir módulos**

1. Na secção "Adicionar módulos", sob "Módulos de implementação" encontrará que esse módulo já está listado com o nome começado por "AzureBlobStorageonIoTEdge". 
2. **Selecione** o módulo de armazenamento de BLOBs na lista de "Módulos de implementação". Aberto o painel do lado de "Módulos de personalizado do IoT Edge".
3. **Nome**: pode alterar o nome do módulo aqui
4. **URI da imagem**: substituir o URI para **mcr.microsoft.com/azure-blob-storage:latest**
5. **Opções de criar contentor**: editar o JSON abaixo com os seus valores e substituí-lo com o JSON na página do Portal:
   
   ```json
   {
       "Env":[
           "LOCAL_STORAGE_ACCOUNT_NAME=<your storage account name>",
           "LOCAL_STORAGE_ACCOUNT_KEY=<your storage account key>"
       ],
       "HostConfig":{
           "Binds":[
               "<storage directory bind>"
           ],
           "PortBindings":{
               "11002/tcp":[{"HostPort":"11002"}]
           }
       }
   }
   ```   
   
    * Atualização `<your storage account name>`. Nomes de contas devem ser três para vinte e quatro carateres com letras minúsculas e números.
    * Atualização `<your storage account key>` com uma chave de base64 de 64 bytes. Pode gerar uma chave com ferramentas como o [GeneratePlus](https://generate.plus/en/base64?gp_base64_base[length]=64). Irá utilizar estas credenciais para aceder ao armazenamento de BLOBs a partir de outros módulos.
    * Atualização `<storage directory bind>`. Consoante o sistema operativo do contentor. Forneça o nome de um [volume](https://docs.docker.com/storage/volumes/) ou o caminho absoluto para um diretório no seu dispositivo IoT Edge onde pretende que o módulo de BLOBs para armazenar os dados.  

       * Contentores do Linux:  **\<caminho de armazenamento >: / blobroot**. Por exemplo, / srv/containerdata: / blobroot. Ou, meu volume: / blobroot. 
       * Contentores do Windows:  **\<caminho de armazenamento >: C: / BlobRoot**. Por exemplo, c: / ContainerData:C: / BlobRoot. Ou, meu-volume: C: / blobroot.
   
   > [!CAUTION]
   > Não altere o "/ blobroot" para Linux e "C: / BlobRoot" para o Windows, para  **\<bind do diretório de armazenamento >** valores.

    ![Atualize os valores de módulo](./media/how-to-store-data-blob/edit-module.png)

6. **Guardar** os valores em "Módulos de personalizado do IoT Edge"
7. Clique em **seguinte** na secção "Definir módulos"
8. Clique em **seguinte** na seção "Especificar rotas"
9. Depois de rever, clique em **submeter** na seção "Implantação de revisão".
10. Certifique-se do seu IoT Hub que o dispositivo está a executar o módulo de armazenamento de BLOBs 

### <a name="visual-studio-code-templates"></a>Modelos do Visual Studio Code

O Azure IoT Edge fornece modelos no Visual Studio Code para ajudar a desenvolver soluções de ponta. Estes passos requerem que tenha [Visual Studio Code](https://code.visualstudio.com/) instalado no computador de desenvolvimento e configurado com o [extensão Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge).

Utilize os seguintes passos para criar uma nova solução de IoT Edge com um módulo de armazenamento de BLOBs e configure o manifesto de implantação. 

1. Selecione **View** > **paleta de comandos**. 

2. Na paleta de comandos, introduza e execute o comando **do Azure IoT Edge: nova solução do IoT Edge**. 

3. Siga as instruções para criar uma nova solução: 

   1. **Selecione a pasta** -procure a pasta onde pretende criar a nova solução.  
   
   2. **Forneça um nome de solução** – introduza um nome para a sua solução ou aceite a predefinição.
   
   3. **Modelo de módulo de seleção** -escolha **módulo existente (URL de imagem completa de Enter)**.
   
   4. **Forneça um nome de módulo** -introduza um nome reconhecível para seu módulo, como **azureBlobStorage**.
   
   5. **Forneça a imagem do Docker para o módulo** -forneça o URI da imagem: **mcr.microsoft.com/azure-blob-storage:latest**

Código VS usa as informações fornecidas, cria uma solução de IoT Edge e, em seguida, carrega-o numa nova janela. 

O modelo de solução cria um modelo de manifesto de implantação que inclua a sua imagem de módulo de armazenamento de BLOBs, mas tem de configurar opções de criação do módulo. 

1. Open **deployment.template.json** na sua nova área de trabalho da solução e localizar o **módulos** secção. 

2. Eliminar a **tempSensor** módulo, como ele não do necessário para esta implementação. 

3. Copie e cole o código seguinte para o **createOptions** campo do seu módulo de armazenamento de BLOBs: 

   ```json
   {\"Env\": [\"LOCAL_STORAGE_ACCOUNT_NAME=$STORAGE_ACCOUNT_NAME\",\" LOCAL_STORAGE_ACCOUNT_KEY=$STORAGE_ACCOUNT_KEY\"],\"HostConfig\": {\"Binds\": [\"<storage directory bind>\"],\"PortBindings\": {\"11002/tcp\": [{\"HostPort\":\"11002\"}]}}}
   ```

   ![Opções de criação do módulo de atualização](./media/how-to-store-data-blob/create-options.png)

4. As opções de criar, JSON, atualizar `<storage directory bind>` consoante o sistema operativo do contentor. Forneça o nome de um [volume](https://docs.docker.com/storage/volumes/) ou o caminho absoluto para um diretório no seu dispositivo IoT Edge onde pretende que o módulo de BLOBs para armazenar os dados.  

   * Contentores do Linux:  **\<caminho de armazenamento >: / blobroot**. Por exemplo, / srv/containerdata: / blobroot. Ou, meu volume: / blobroot.
   * Contentores do Windows:  **\<caminho de armazenamento >: C: / BlobRoot**. Por exemplo, c: / ContainerData:C: / BlobRoot. Ou, meu-volume: C: / blobroot.
   
   > [!CAUTION]
   > Não altere o "/ blobroot" para Linux e "C: / BlobRoot" para o Windows, para  **\<bind do diretório de armazenamento >** valores.

5. Guarde **deployment.template.json**.

6. Open **. env** na sua área de trabalho da solução. 

7. Não tem de introduzir os valores de registo de contentor para a imagem de armazenamento de BLOBs, uma vez que está disponível publicamente. Em vez disso, adicione duas novas variáveis de ambiente: 

   ```env
   STORAGE_ACCOUNT_NAME=
   STORAGE_ACCOUNT_KEY=
   ```

8. Forneça valor para `STORAGE_ACCOUNT_NAME`, nomes de contas devem ser três para vinte e quatro carateres com letras minúsculas e números. E fornecer uma chave de 64 bytes base64 para o `STORAGE_ACCOUNT_KEY`. Pode gerar uma chave com ferramentas como o [GeneratePlus](https://generate.plus/en/base64?gp_base64_base[length]=64). Irá utilizar estas credenciais para aceder ao armazenamento de BLOBs a partir de outros módulos. 

9. Guarde **. env**. 

10. Com o botão direito **deployment.template.json** e selecione **manifesto de implantação de gerar o IoT Edge**. 

Código do Visual Studio usa as informações que fornecidos no deployment.template.json e. env e utiliza-o para criar um novo arquivo de manifesto de implantação. O manifesto de implantação é criado numa nova **config** pasta na sua área de trabalho da solução. Assim que tiver esse ficheiro, pode seguir os passos em [módulos de implementar o Azure IoT Edge do Visual Studio Code](how-to-deploy-modules-vscode.md) ou [módulos de implementar o Azure IoT Edge com a CLI 2.0 do Azure](how-to-deploy-modules-cli.md).

## <a name="connect-to-your-blob-storage-module"></a>Ligar ao seu módulo de armazenamento de BLOBs

Pode utilizar o nome da conta e a chave de conta que configurou para o seu módulo aceder ao armazenamento de BLOBs no seu dispositivo IoT Edge. 

Especifique o seu dispositivo IoT Edge como o ponto final do blob para o armazenamento de quaisquer pedidos que fizer nele. Pode [criar uma cadeia de ligação para um ponto de final de armazenamento explícita](../storage/common/storage-configure-connection-string.md#create-a-connection-string-for-an-explicit-storage-endpoint) usando as informações do dispositivo IoT Edge e o nome da conta que configurou. 

1. Para os módulos que são implementados no mesmo dispositivo edge onde "Azure Blob de armazenamento no IoT Edge" está em execução, é o ponto final do blob: `http://<Module Name>:11002/<account name>`. 
2. Para módulos que são implementados no dispositivo do edge diferentes, que o dispositivo de limite em que está a executar o "Azure Blob de armazenamento no IoT Edge", então consoante a configuração do ponto final do blob é: `http://<device IP >:11002/<account name>` ou `http://<IoT Edge device hostname>:11002/<account name>` ou `http://<FQDN>:11002/<account name>`

## <a name="logs"></a>Registos

Os registos de dentro do contentor, pode ser encontrado em: 
* Para Linux: /blobroot/logs/platformblob.log

## <a name="deploy-multiple-instances"></a>Implementar várias instâncias

Se pretender implementar várias instâncias do armazenamento de Blobs do Azure no IoT Edge, apenas terá de alterar o HostPort que liga o módulo. Os módulos de armazenamento de BLOBs expõem sempre a porta 11002 no contentor, mas pode declarar que porta está vinculado a no anfitrião. 

Editar o módulo criar opções para alterar o valor de HostPort:

```json
\"PortBindings\": {\"11002/tcp\": [{\"HostPort\":\"<port number>\"}]}
```

Quando se liga aos módulos de armazenamento de BLOBs adicionais, altere o ponto final para apontar para a porta do anfitrião atualizado. 

### <a name="try-it-out"></a>Experimentar

A documentação do armazenamento de Blobs do Azure inclui guias de introdução que fornecem código de exemplo em várias linguagens. Pode executar estes exemplos para testar o armazenamento de Blobs do Azure no IoT Edge ao alterar o ponto final do blob para apontar para o módulo de armazenamento de Blobs.

Inícios rápidos utilizarem outros idiomas que também são suportados pela IoT Edge, para que pode implementá-los como módulos do IoT Edge, juntamente com o módulo de armazenamento de BLOBs:

* [.NET](../storage/blobs/storage-quickstart-blobs-dotnet.md)
* [Java](../storage/blobs/storage-quickstart-blobs-java.md)
* [Python](../storage/blobs/storage-quickstart-blobs-python.md)
* [Node.js](../storage/blobs/storage-quickstart-blobs-nodejs.md)

## <a name="supported-storage-operations"></a>Operações de armazenamento suportadas

Módulos de armazenamento de Blobs do IoT Edge utilizam o mesmo SDKs de armazenamento do Azure e são consistentes com a versão de 2018-03-28 da API de armazenamento do Azure para pontos finais de blob de bloco. Versões posteriores são dependentes as necessidades dos clientes. 

Nem todas as operações de armazenamento de Blobs do Azure são suportadas pelo armazenamento de Blobs do Azure no IoT Edge. Os seguintes detalhes de secções, as operações não são um são suportados. 

### <a name="account"></a>Conta

Suporte para: 
* Listar contentores

Não suportado: 
* Obter e definir as propriedades do serviço de BLOBs
* Solicitação de simulação de blob
* Obter estatísticas do serviço blob
* Obtenha informações de conta

### <a name="containers"></a>Contentores

Suporte para: 
* Criar e eliminar o contentor
* Obter metadados e propriedades do contentor
* Listar blobs

Não suportado: 
* Obter e definir o contentor de ACL
* Contentor de concessão
* Metadados do conjunto de contentor

### <a name="blobs"></a>Blobs

Suporte para: 
* Colocar, obter e eliminar BLOBs
* Obter e definir propriedades de blob
* Obter e definir os metadados do blob

Não suportado: 
* Blob de concessão
* Blob de instantâneo
* Copiar e anular o blob de cópia
* Anular a eliminação de BLOBs
* Definir camada de blob

### <a name="block-blobs"></a>Blobs de bloco

Suporte para: 
* Colocar o bloco:-tem de ser menor ou igual a 4 MB o bloco de tamanho
* Colocar e obter a lista de bloqueios

Não suportado:
* Colocar o bloco de URL

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre [armazenamento de Blobs do Azure](../storage/blobs/storage-blobs-introduction.md)

