---
title: Procurar dados não estruturados no armazenamento na cloud do Azure
description: Pesquisar dados não estruturados com a pesquisa do Azure.
author: roygara
services: storage
ms.service: storage
ms.topic: tutorial
ms.date: 10/12/2017
ms.author: rogarana
ms.custom: mvc
ms.openlocfilehash: eba2ef280e60693cfd4402348fe61b122cdccdf6
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/01/2018
ms.locfileid: "39399861"
---
# <a name="search-unstructured-data-in-cloud-storage"></a>Procurar dados não estruturados no armazenamento na cloud

Neste tutorial, vai aprender a pesquisar dados não estruturados com o [Azure Search](../../search/search-what-is-azure-search.md), através dos dados armazenados em blobs do Azure. Os dados não estruturados são os dados que não estão organizados de maneira predefinida ou não têm um modelo de dados. Um exemplo seria um ficheiro .txt.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Criar um grupo de recursos
> * Criar uma conta de armazenamento
> * Criar um contentor
> * Carregar dados para o contentor
> * Criar um serviço de pesquisa através do portal
> * Utilizar o serviço de pesquisa para pesquisar o seu contentor

## <a name="download-the-sample"></a>Transferir o exemplo

Um conjunto de dados de exemplo foi preparado para si. **Transfira [clinical-trials.zip](https://github.com/Azure-Samples/storage-blob-integration-with-cdn-search-hdi/raw/master/clinical-trials.zip)** e deszipe-o para a própria pasta.

O exemplo consiste em ficheiros de texto obtidos a partir de [clinicaltrials.gov](https://clinicaltrials.gov/ct2/results). Pode utilizá-los como ficheiros de texto de exemplo para pesquisar com o Azure.

## <a name="log-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão no [Portal do Azure](http://portal.azure.com).

## <a name="create-a-storage-account"></a>Criar uma conta de armazenamento

Uma conta de armazenamento fornece uma localização exclusiva para armazenar e aceder aos objetos de dados de Armazenamento do Azure.

Atualmente, existem dois tipos de contas de armazenamento: **Blob** e para **Fins gerais**. Neste tutorial, vai criar uma conta de armazenamento para **Fins gerais**.

Se não estiver familiarizado com o processo de criação de uma conta de armazenamento para fins gerais, eis como criar uma:

1. No menu à esquerda, selecione **Contas de Armazenamento** e selecione **Adicionar**.

2. Introduza um nome único para a conta de armazenamento. 

3. Selecione **Resource Manager** para o **Modelo de implementação** e selecione **Fins gerais** na lista pendente **Tipo de conta**.

4. Selecione **Armazenamento localmente redundante (LRS)** na lista pendente **Replicação**.

5. Em **Grupo de recursos**, selecione **Criar Novo** e introduza um nome exclusivo.

6. Selecione uma subscrição adequada.

7. Escolha uma Localização e selecione **Criar.**

  ![Pesquisa não estruturada](media/storage-unstructured-search/storagepanes2.png)

## <a name="create-a-container"></a>Criar um contentor

Os contentores são semelhantes às pastas e são utilizados para armazenar blobs.

Neste tutorial, vai utilizar um único contentor para armazenar os ficheiros de texto obtidos do clinicaltrials.gov.

1. Navegue para a sua conta de armazenamento no portal do Azure.

2. Selecione **Procurar blobs** em **Serviço Blob.**

3. Adicione um novo contentor.

4. Dê ao contentor o nome "data" e selecione **Contentor** para o nível de acesso público.

5. Selecione **OK** para criar o contentor. 

  ![Pesquisa não estruturada](media/storage-unstructured-search/storageactinfo.png)

## <a name="upload-the-example-data"></a>Carregar os dados de exemplo

Agora que tem um contentor, pode carregar os dados de exemplo para o mesmo.

1. Selecione o contentor e selecione **Carregar**.

2. Selecione o ícone de pasta azul junto ao campo Ficheiros e navegue até a pasta local de onde extraiu os dados de exemplo.

3. Selecione todos os ficheiros extraídos e selecione **Abrir**

4. Selecione **Carregar** para iniciar o processo de carregamento.

  ![Pesquisa não estruturada](media/storage-unstructured-search/upload.png)

O processo de carregamento pode demorar um pouco.

Depois de terminar, navegue novamente para o contentor de dados para confirmar os ficheiros de texto carregados.

  ![Pesquisa não estruturada](media/storage-unstructured-search/clinicalfolder.png)

## <a name="create-a-search-service"></a>Criar um serviço de pesquisa

O Azure Search é uma solução cloud de pesquisa como serviço que dá aos programadores APIs e ferramentas para apresentar uma rica experiência de pesquisa aos seus dados nas aplicações Web, móveis e empresariais.

Se não estiver familiarizado com o processo de criação de um serviço de pesquisa, eis como criar um:

1. Navegue para a sua conta de armazenamento no portal do Azure.

2. Desloque-se para baixo e clique em **Ligar ao Azure Search** em **SERVIÇO BLOB**.

3. Em **Importar Dados**, selecione **Escolha o seu serviço**.

4. Selecione **Clique aqui para criar um novo serviço de pesquisa**.

5. Dentro de **Novo Serviço de Pesquisa**, introduza um nome exclusivo para o seu serviço de pesquisa no campo **URL**.

6. Em **Grupo de recursos**, selecione **Utilizar existente** e escolha o grupo de recursos que criou anteriormente.

7. Em **Escalão de preço**, selecione o escalão **Gratuito** e clique em **Selecionar**.

8. Selecione **Criar** para criar o serviço de pesquisa.

  ![Pesquisa não estruturada](media/storage-unstructured-search/createsearch2.png)

## <a name="connect-your-search-service-to-your-container"></a>Ligar o serviço de pesquisa ao contentor

Agora que tem um serviço de pesquisa, pode anexá-lo ao armazenamento de blobs. Esta secção descreve o processo de escolha de uma origem de dados, criação de um índice e criação de um indexador.

1. Navegue até à sua conta de armazenamento.

2. Selecione **Ligar ao Azure Search** em **SERVIÇO BLOB.**

3. Selecione **Search Service** dentro de **Importar dados** e, em seguida, clique no serviço de pesquisa que criou na secção anterior. Esta ação abre **Nova origem de dados**.

### <a name="new-data-source"></a>Nova origem de dados

  Uma origem de dados especifica quais os dados a indexar e como aceder aos dados. Uma origem de dados pode ser utilizada várias vezes pelo mesmo serviço de pesquisa.

1. Introduza um nome para a origem de dados. Em **Dados a extrair**, selecione **Conteúdo e Metadados**. A origem de dados especifica as partes do blob que estão indexadas.
    
    a. Nos seus cenários futuros, também pode selecionar **Apenas metadados de armazenamento**. Faria essa seleção se pretendesse limitar os dados que são indexados a propriedades de blob padrão ou propriedades definidas pelo utilizador.
    
    b. Também pode escolher **Todos os metadados** para obter as duas propriedades de blob padrão e *todos* os metadados específicos do tipo de conteúdo. 

2. Uma vez que os blobs que está a utilizar são ficheiros de texto, defina o **Modo de Análise** para **Texto**.
    
    a. Nos seus cenários futuros, poderá selecionar [outros modos de análise](../../search/search-howto-indexing-azure-blob-storage.md), consoante o conteúdo dos blobs.

  ![Pesquisa não estruturada](media/storage-unstructured-search/datasources.png)

3. Selecione **Contentor de Armazenamento** para listar as contas de armazenamento disponíveis.

4. Selecione a sua conta de armazenamento e selecione o contentor que criou anteriormente.

5. Clique em **Selecionar** para regressar a **Nova origem de dados** e selecione **OK** para continuar.

  ![Pesquisa não estruturada](media/storage-unstructured-search/datacontainer.png)

### <a name="configure-the-index"></a>Configurar o índice

  Um índice é uma coleção de campos da sua origem de dados que podem ser pesquisados. É através do índice que o serviço de pesquisa sabe de que forma os seus dados devem ser pesquisados.

1. Em **Importar dados**, selecione **Personalizar o índice de destino**.
 
2. Introduza um nome para o índice no campo **Nome do índice**.

3. Selecione a caixa de verificação do atributo **Recuperável** em **metadata_storage_name**.

  ![Pesquisa não estruturada](media/storage-unstructured-search/valuestoselect.png)

4. Clique em **OK**, o que apresenta **Criar um Indexador**.

Os parâmetros do seu índice e os atributos que fornecer a esses parâmetros são importantes. Os parâmetros especificam *quais* os dados a armazenar, os atributos especificam *como* armazenar esses dados.

A coluna **NOME DO CAMPO** contém os parâmetros. A tabela seguinte fornece uma listagem dos atributos disponíveis e as respetivas descrições.

### <a name="field-attributes"></a>Atributos do campo
| Atributo | Descrição |
| --- | --- |
| *Chave* |Uma cadeia que fornece o ID único de cada documento, utilizada para a procura de documentos. Todos os índices devem ter uma chave. Apenas um campo pode ser uma chave e o tipo deve ser definido para Edm.String. |
| *Recuperável* |Especifica se um campo pode ser devolvido num resultado da pesquisa. |
| *Filtrável* |Permite que o campo seja utilizado nas consultas de filtro.  |
| *Ordenável* |Permite a uma consulta ordenar os resultados da pesquisa através deste campo. |
| *Facetável* |Permite a um campo ser utilizado numa estrutura de navegação por facetas para filtragem do utilizador auto-direcionada. Geralmente os campos com valores repetitivos que pode utilizar para agrupar vários documentos (por exemplo, vários documentos que se inserem numa única marca ou categoria de serviço) funcionam melhor como facetas. |
| *Pesquisável* |Marca o campo como pesquisável no texto completo. |


### <a name="create-an-indexer"></a>Criar um indexador
    
  Um indexador liga uma origem de dados a um índice de pesquisa e fornece uma agenda para voltar a indexar os seus dados.

1. Introduza um nome no campo **Nome** e selecione **OK**.

  ![Pesquisa não estruturada](media/storage-unstructured-search/exindexer.png)

2. Volta para **Importar Dados**, selecione **OK** para concluir o processo de ligação.

Ligou com êxito o blob ao serviço de pesquisa. Demora alguns minutos para o portal mostrar que o índice foi preenchido. No entanto, o serviço de pesquisa começa a indexação imediatamente, para que possa começar a procurar imediatamente.

## <a name="search-your-text-files"></a>Pesquisar os ficheiros de texto

Para pesquisar os ficheiros, abra o explorador de procura dentro do índice do seu serviço de pesquisa criado recentemente.

Os passos seguintes mostram-lhe onde encontrar o explorador de procura e fornecem-lhe algumas consultas de exemplo:

1. Navegue para todos os recursos e localize o serviço de pesquisa criado recentemente.

  ![Pesquisa não estruturada](media/storage-unstructured-search/exampleurl.png)

3. Selecione o seu índice para abri-lo. 

  ![Pesquisa não estruturada](media/storage-unstructured-search/overview.png)

4. Selecione **Explorador de Procura** para abrir o explorador de procura, onde pode fazer consultas em direto nos seus dados.

  ![Pesquisa não estruturada](media/storage-unstructured-search/indexespane.png)

5. Selecione **Pesquisar** enquanto o campo de cadeia de consulta está vazio. Uma consulta vazia devolve *todos* os dados dos seus blobs.

  ![Pesquisa não estruturada](media/storage-unstructured-search/emptySearch.png)

### <a name="full-text-search"></a>Pesquisa em texto completo 

Introduza "Miopia" no campo **Cadeia de consulta** e selecione **Pesquisar**. Iniciar uma pesquisa de conteúdo dos ficheiros e devolver um subconjunto dos mesmos, que contém a palavra "Miopia".

  ![Pesquisa não estruturada](media/storage-unstructured-search/secondMyopia.png) 

### <a name="system-properties-search"></a>Pesquisa das propriedades do sistema

Também pode criar consultas que procuram propriedades do sistema, com o parâmetro `$select`. Introduza `$select=metadata_storage_name` na cadeia de consulta e prima enter, devolvendo apenas esse campo específico.
    
A cadeia de consulta está a modificar diretamente o URL, pelo que não são permitidos espaços. Para pesquisar vários campos, utilize uma vírgula, como em: `$select=metadata_storage_name,metadata_storage_path`
    
O parâmetro `$select` só pode ser utilizado em campos que foram marcados como recuperáveis ao definir o seu índice.

  ![Pesquisa não estruturada](media/storage-unstructured-search/metadatasearchunstructured.png) 

Concluiu este tutorial e tem um conjunto pesquisável de dados não estruturados.

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, aprendeu a pesquisar dados não estruturados com o Azure Search, como por exemplo, a:

> [!div class="checklist"]
> * Criar um grupo de recursos
> * Criar uma conta de armazenamento
> * Criar um contentor
> * Carregar dados para o seu contentor
> * Criar um Search Service
> * Utilizar o Search Service para pesquisar o seu contentor

Siga esta ligação para saber mais sobre como indexar documentos com o Azure Search.

> [!div class="nextstepaction"]
> [Indexar Documentos no Armazenamento de Blobs do Azure com o Azure Search](../../search/search-howto-indexing-azure-blob-storage.md)