---
title: Começar com o serviço de exploração de dados de conhecimento | Microsoft Docs
description: Utilize o serviço de exploração de dados de conhecimento (KES) para criar um motor para uma experiência de pesquisa interativo em publicações académicas nos serviços cognitivos da Microsoft.
services: cognitive-services
author: bojunehsu
manager: stesp
ms.service: cognitive-services
ms.component: knowledge-exploration
ms.topic: article
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: 02dc9368eef02d6fa507335ef3171e923412acca
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352513"
---
<a name="getting-started"></a>
# <a name="get-started-with-the-knowledge-exploration-service"></a>Começar com o serviço de exploração de dados de conhecimento
Esta explicação passo a passo, utiliza o serviço de exploração de dados de conhecimento (KES) para criar o motor de uma experiência de pesquisa interativa para publicações académicas. Pode instalar a ferramenta de linha de comandos, [ `kes.exe` ](CommandLine.md)e todos os ficheiros de exemplo do [SDK do serviço de exploração de dados de conhecimento](https://www.microsoft.com/en-us/download/details.aspx?id=51488).

O exemplo de publicações académico contém uma amostra de papers académico 1000 publicados pelos investigadores da Microsoft.  Cada documento está associado um título, ano de publicação, autores e palavras-chave. Cada autor é representada por um ID, o nome e a afiliação no momento da publicação. Cada palavra-chave pode ser associado um conjunto de sinónimos (por exemplo, a palavra-chave "suporte máquina de vetor" pode ser associada com o sinónimo "svm").

<a name="defining-schema"></a>
## <a name="define-the-schema"></a>Definir o esquema
O esquema descreve a estrutura de atributo dos objetos no domínio. Especifica o nome e tipo de dados para cada atributo num formato de ficheiro JSON. O exemplo a seguir está o conteúdo do ficheiro *Academic.schema*.

```json
{
  "attributes":[
    {"name":"Title", "type":"String"},
    {"name":"Year", "type":"Int32"},
    {"name":"Author", "type":"Composite"},
    {"name":"Author.Id", "type":"Int64", "operations":["equals"]},
    {"name":"Author.Name", "type":"String"},
    {"name":"Author.Affiliation", "type":"String"},
    {"name":"Keyword", "type":"String", "synonyms":"Keyword.syn"}
  ]
}
```

Aqui, definir *título*, *ano*, e *palavra-chave* como uma cadeia, número inteiro e uma cadeia de atributo, respetivamente. Porque os autores são representados por ID, o nome e afiliação, definir *autor* como um atributo composto com três atributos secundárias: *Author.Id*, *Author.Name*, e *Author.Affiliation*.

Por predefinição, os atributos suportam todas as operações disponíveis para o respetivo tipo de dados, incluindo *é igual a*, *starts_with*, e *is_between*. Porque o ID do autor só é utilizada internamente como um identificador, substituir a predefinição e especifique *é igual a* como o único indexada operação.

Para o *palavra-chave* atributo, permita automaticamente sinónimos à correspondem aos valores canónicos palavra-chave, especificando o ficheiro sinónimo *Keyword.syn* na definição do atributo. Este ficheiro contém uma lista de canónico e pares de valor sinónimo:

```json
...
["support vector machine","support vector machines"]
["support vector machine","support vector networks"]
["support vector machine","support vector regression"]
["support vector machine","support vector"]
["support vector machine","svm machine learning"]
["support vector machine","svm"]
["support vector machine","svms"]
["support vector machine","vector machine"]
...
```

Para obter informações adicionais sobre a definição de esquema, consulte [esquema formato](SchemaFormat.md).

<a name="generating-data"></a>
## <a name="generate-data"></a>Gerar dados
O ficheiro de dados descreve a lista de publicações para indexar, com cada linha a especificar os valores de atributo de um documento no [formato JSON](http://json.org/).  O exemplo seguinte é uma única linha do ficheiro de dados *Academic.data*, formatado para legibilidade:

```
...
{
    "logprob": -16.714,
    "Title": "the world wide telescope",
    "Year": 2001,
    "Author": [
        {
            "Id": 717694024,
            "Name": "alexander s szalay",
            "Affiliation": "microsoft"
        },
        {
            "Id": 2131537204,
            "Name": "jim gray",
            "Affiliation": "microsoft"
        }
    ]
}
...
```

Este fragmento, pode especificar o *título* e *ano* atributo o documento como uma cadeia JSON e o número, respetivamente. Vários valores são representados por utilizar matrizes JSON. Porque *autor* é um atributo composto, cada valor é representado utilizando um objeto JSON constituídas pelos respetivos atributos secundárias. Atributos com valores em falta, tais como *palavra-chave* neste caso, podem ser excluídos da representação JSON.

Para diferenciar a probabilidade de papers diferentes, especifique a probabilidade de registo relativo utilizando incorporada *logprob* atributo. Dada uma probabilidade *p* entre 0 e 1, é a probabilidade de registo como registo de computação (*p*), onde log() é a função de registo natural.

Para obter mais informações, consulte [formato de dados](DataFormat.md).

<a name="building-index"></a>
## <a name="build-a-compressed-binary-index"></a>Criar um índice de binário comprimido
Depois de ter um ficheiro de esquema e um ficheiro de dados, pode criar um índice de binário comprimido dos objetos de dados utilizando [ `kes.exe build_index` ](CommandLine.md#build_index-command). Neste exemplo, criar o ficheiro de índice *Academic.index* do ficheiro de esquema da entrada *Academic.schema* e ficheiro de dados *Academic.data*. Utilize o seguinte comando:

`kes.exe build_index Academic.schema Academic.data Academic.index`

Para fazer o protótipo rápido fora do Azure, [ `kes.exe build_index` ](CommandLine.md#build_index-command) podem criar índices pequenas localmente, dos ficheiros de dados que contêm objetos de até 10 000. Para ficheiros de dados maiores, pode executar o comando a partir de um [VM do Windows no Azure](../../../articles/virtual-machines/windows/quick-create-portal.md), ou executar uma compilação remota no Azure. Para obter mais informações, consulte [como aumentar verticalmente](#scaling-up).

<a name="authoring-grammar"></a>
## <a name="use-an-xml-grammar-specification"></a>Utilize uma especificação de gramática XML
A gramática Especifica o conjunto de consultas de linguagem natural a que o serviço pode interpretar, bem como a forma como estas consultas de linguagem natural são convertidas em expressões de consulta de semântica. Neste exemplo, utilizar a gramática especificada no *academic.xml*:

```xml
<grammar root="GetPapers">

  <!-- Import academic data schema-->
  <import schema="Academic.schema" name="academic"/>

  <!-- Define root rule-->
  <rule id="GetPapers">
    <example>papers about machine learning by michael jordan</example>

    papers
    <tag>
      yearOnce = false;
      isBeyondEndOfQuery = false;
      query = All();
    </tag>

    <item repeat="1-" repeat-logprob="-10">
      <!-- Do not complete additional attributes beyond end of query -->
      <tag>AssertEquals(isBeyondEndOfQuery, false);</tag>

      <one-of>
        <!-- about <keyword> -->
        <item logprob="-0.5">
          about <attrref uri="academic#Keyword" name="keyword"/>
          <tag>query = And(query, keyword);</tag>
        </item>

        <!-- by <authorName> [while at <authorAffiliation>] -->
        <item logprob="-1">
          by <attrref uri="academic#Author.Name" name="authorName"/>
          <tag>authorQuery = authorName;</tag>
          <item repeat="0-1" repeat-logprob="-1.5">
            while at <attrref uri="academic#Author.Affiliation" name="authorAffiliation"/>
            <tag>authorQuery = And(authorQuery, authorAffiliation);</tag>
          </item>
          <tag>
            authorQuery = Composite(authorQuery);
            query = And(query, authorQuery);
          </tag>
        </item>

        <!-- written (in|before|after) <year> -->
        <item logprob="-1.5">
          <!-- Allow this grammar path to be traversed only once -->
          <tag>
            AssertEquals(yearOnce, false);
            yearOnce = true;
          </tag>
          <ruleref uri="#GetPaperYear" name="year"/>
          <tag>query = And(query, year);</tag>
        </item>
      </one-of>

      <!-- Determine if current parse position is beyond end of query -->
      <tag>isBeyondEndOfQuery = GetVariable("IsBeyondEndOfQuery", "system");</tag>
    </item>
    <tag>out = query;</tag>
  </rule>

  <rule id="GetPaperYear">
    <tag>year = All();</tag>
    written
    <one-of>
      <item>
        in <attrref uri="academic#Year" name="year"/>
      </item>
      <item>
        before
        <one-of>
          <item>[year]</item>
          <item>
            <attrref uri="academic#Year" op="lt" name="year"/>
          </item>
        </one-of>
      </item>
      <item>
        after
        <one-of>
          <item>[year]</item>
          <item>
            <attrref uri="academic#Year" op="gt" name="year"/>
          </item>
        </one-of>
      </item>
    </one-of>
    <tag>out = year;</tag>
  </rule>
</grammar>
```

Para obter mais informações sobre a sintaxe de especificação de gramática, consulte [formato de gramática](GrammarFormat.md).

<a name="compiling-grammar"></a>
## <a name="compile-the-grammar"></a>A gramática de compilação
Depois de ter uma especificação de gramática XML, pode compilá-la para uma gramática de binária utilizando [ `kes.exe build_grammar` ](CommandLine.md#build_grammar-command). Tenha em atenção que se a gramática importa um esquema, o ficheiro de esquema tem de estar localizado no mesmo caminho como a gramática XML. Neste exemplo, criar o ficheiro de gramática de binários *Academic.grammar* do ficheiro de gramática XML de entrada *Academic.xml*. Utilize o seguinte comando:

`kes.exe build_grammar Academic.xml Academic.grammar`

<a name="hosting-index"></a>
## <a name="host-the-grammar-and-index-in-a-web-service"></a>A gramática e o índice de um serviço web do anfitrião
Para fazer o protótipo rápido, pode alojar a gramática e índice num serviço web no computador local, utilizando [ `kes.exe host_service` ](CommandLine.md#host_service-command). Em seguida, pode aceder ao serviço através de [web APIs](WebAPI.md) para validar a estrutura de dados estão corretas e de gramática. Neste exemplo, alojar o ficheiro de gramática *Academic.grammar* e ficheiros de índice *Academic.index* em http://localhost:8000/. Utilize o seguinte comando:

`kes.exe host_service Academic.grammar Academic.index --port 8000`

Esta ação inicia uma instância local do serviço web. Pode testar o serviço de forma interativa, visitando `http::localhost:<port>` num browser. Para obter mais informações, consulte [testar serviço](#testing-service).

Pode também diretamente chamar vários [web APIs](WebAPI.md) para testar a interpretação de linguagem natural, conclusão de consulta, avaliação de consulta estruturada e cálculo de histograma. Para parar o serviço, introduza "Sair" para o `kes.exe host_service` linha de comandos ou prima Ctrl + C. Eis alguns exemplos:

* [http://localhost:8000/interpret?query=papers por susan t dumais](http://localhost:8000/interpret?query=papers%20by%20susan%20t%20dumais)
* [http://localhost:8000/interpret?query=papers por susan t d & concluída = 1](http://localhost:8000/interpret?query=papers%20by%20susan%20t%20d&complete=1)
* [http://localhost:8000/evaluate?expr=Composite(Author.Name=='susan t dumais') & attributes=Title,Year,Author.Name,Author.Id & contagem = 2](http://localhost:8000/evaluate?expr=Composite%28Author.Name==%27susan%20t%20dumais%27%29&attributes=Title,Year,Author.Name,Author.Id&count=2)
* [http://localhost:8000/calchistogram?expr=And(Composite(Author.Name=='susan t dumais'), ano > = 2013) & atributos = ano, a palavra-chave e a contagem = 4](http://localhost:8000/calchistogram?expr=And%28Composite%28Author.Name=='susan%20t%20dumais'%29,Year>=2013%29&attributes=Year,Keyword&count=4)

Fora do Azure, [ `kes.exe host_service` ](CommandLine.md#host_service-command) está limitado a índices de até 10 000 objetos. Outros limites incluem uma taxa de API de 10 pedidos por segundo e um total de pedidos de 1000 antes do processo termina automaticamente. Para ignorar estes restrições, execute o comando a partir de um [VM do Windows no Azure](../../../articles/virtual-machines/windows/quick-create-portal.md), ou implementar um serviço em nuvem do Azure utilizando o [ `kes.exe deploy_service` ](CommandLine.md#deploy_service-command) comando. Para obter mais informações, consulte [implementar serviço](#deploying-service).

<a name="scaling-up"></a>
## <a name="scale-up-to-host-larger-indices"></a>Aumentar verticalmente para índices maiores de anfitrião
Quando estiver a executar o `kes.exe` fora do Azure, o índice está limitado a 10 000 objetos. Pode criar e alojar índices maiores através da utilização do Azure. Inscrever-se um [avaliação gratuita](https://azure.microsoft.com/pricing/free-trial/). Em alternativa, se subscrever Visual Studio ou o MSDN, pode [ativar os benefícios de subscritor](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). Estes oferecem algumas créditos do Azure por mês.

Para permitir `kes.exe` acesso a uma conta do Azure, [transfira o ficheiro de definições de publicação do Azure](https://portal.azure.com/#blade/Microsoft_Azure_ClassicResources/PublishingProfileBlade) do portal do Azure. Se lhe for solicitado, inicie sessão para a conta do Azure pretendida. Guarde o ficheiro como *AzurePublishSettings.xml* no diretório de trabalho a partir de onde `kes.exe` é executado.

Existem duas formas para criar e alojar índices grandes. É o primeiro para preparar os ficheiros de esquema e dados de uma VM do Windows no Azure. Em seguida, execute [ `kes.exe build_index` ](#building-index) para criar o índice localmente na VM, sem quaisquer restrições de tamanho. O índice resultante pode ser alojado localmente na VM utilizando [ `kes.exe host_service` ](#hosting-service) para fazer o protótipo rápido, novamente sem restrições. Para obter passos detalhados, consulte o [tutorial da VM do Azure](../../../articles/virtual-machines/windows/quick-create-portal.md).

O segundo método consiste em efetuar uma compilação do Azure remota, utilizando [ `kes.exe build_index` ](CommandLine.md#build_index-command) com o `--remote` parâmetro. Esta ação Especifica um tamanho de VM do Azure. Quando o `--remote` parâmetro for especificado, o comando cria uma VM do Azure temporário desse tamanho. Em seguida, criar o índice na VM, carrega o índice para o armazenamento de BLOBs de destino e elimina a VM após a conclusão. A subscrição do Azure é cobrada o custo da VM, enquanto o índice está a ser criado.

Esta capacidade de compilação do Azure remoto permite [ `kes.exe build_index` ](CommandLine.md#build_index-command) para ser executado em qualquer ambiente. Quando estiver a efetuar uma compilação remota, os argumentos de esquema e os dados de entrada podem ser caminhos de ficheiros locais ou [blob storage do Azure](../../storage/blobs/storage-dotnet-how-to-use-blobs.md) URLs. O argumento de índice de saída tem de ser um URL de armazenamento de Blobs. Para criar uma conta de armazenamento do Azure, consulte [contas do storage do Azure sobre](../../storage/common/storage-create-storage-account.md). Para copiar ficheiros de forma eficiente para e do armazenamento de BLOBs, utilize o [AzCopy](../../storage/common/storage-use-azcopy.md) utilitário.

Neste exemplo, pode assumir que já foi criado o contentor de blob storage seguintes: http://&lt;*conta*&gt;.blob.core.windows.net/&lt;*contentor* &gt;/. Contém o esquema *Academic.schema*, o ficheiro sinónimo referenciado *Keywords.syn*e o ficheiro de dados aproximar *Academic.full.data*. Pode criar o índice completo remotamente utilizando o seguinte comando:

`kes.exe build_index http://<account>.blob.core.windows.net/<container>/Academic.schema http://<account>.blob.core.windows.net/<container>/Academic.full.data http://<account>.blob.core.windows.net/<container>/Academic.full.index --remote <vm_size>`

Tenha em atenção que poderá demorar 5-10 minutos para aprovisionar um temporay VM para criar o índice. Para fazer o protótipo rápido, pode:
- Desenvolva com um conjunto de dados mais pequeno localmente em qualquer máquina.
- Manualmente [criar uma VM do Azure](../../../articles/virtual-machines/windows/quick-create-portal.md), [ligar ao mesmo](../../../articles/virtual-machines/windows/quick-create-portal.md#connect-to-virtual-machine) através do ambiente de trabalho remoto, instale o [SDK do serviço de exploração de dados de conhecimento](https://www.microsoft.com/en-us/download/details.aspx?id=51488)e execute [ `kes.exe` ](CommandLine.md) de dentro da VM.

A paginação atrasar o processo de compilação. Para evitar a paginação, utilize uma VM com três vezes a quantidade de RAM como o tamanho do ficheiro de dados de entrada para a criação do índice. Utilize uma VM com mais RAM superior ao tamanho de índice para o alojamento de 1 GB. Para obter uma lista de tamanhos VM disponíveis, consulte [tamanhos das virtual machines](../../../articles/virtual-machines/virtual-machines-windows-sizes.md).

<a name="deploying-service"></a>
## <a name="deploy-the-service"></a>Implementar o serviço
Depois de ter uma gramática e um índice, está pronto para implementar o serviço para um serviço em nuvem do Azure. Para criar um novo serviço em nuvem do Azure, consulte [como criar e implementar um serviço em nuvem](../../../articles/cloud-services/cloud-services-how-to-create-deploy-portal.md). Não especifique um pacote de implementação neste momento.  

Quando criou o serviço em nuvem, pode utilizar [ `kes.exe deploy_service` ](CommandLine.md#deploy_service-command) para implementar o serviço. Um serviço em nuvem do Azure tem dois ranhuras de implementação: produção e teste. Para um serviço que recebe o tráfego do utilizador em direto, inicialmente deve implementar para o bloco de transição. Aguarde que o serviço para arrancar e inicializar a próprio. Em seguida, pode enviar alguns pedidos para validar a implementação e certifique-se de que tenha sido enviado testes básicos.

[Comutação](../../../articles/cloud-services/cloud-services-nodejs-stage-application.md) o conteúdo da transição espaço com a ranhura de produção, para que o tráfego em direto agora é direcionado para o serviço recentemente implementado. Pode repetir este processo quando implementar uma versão atualizada do serviço com novos dados. Como com todos os outros serviços em nuvem do Azure, opcionalmente, pode utilizar o portal do Azure para configurar [dimensionamento automático](../../../articles/cloud-services/cloud-services-how-to-scale-portal.md).

Neste exemplo, implementar o *académico* índice para o bloco de transição de um serviço em nuvem existente com *< vm_size >* VMs. Utilize o seguinte comando:

`kes.exe deploy_service http://<account>.blob.core.windows.net/<container>/Academic.grammar http://<account>.blob.core.windows.net/<container>/Academic.index <serviceName> <vm_size> --slot Staging`

Para obter uma lista de tamanhos VM disponíveis, consulte [tamanhos das virtual machines](../../../articles/virtual-machines/virtual-machines-windows-sizes.md).

Depois de implementar o serviço, pode chamar a vários [web APIs](WebAPI.md) para testar a interpretação de linguagem natural, conclusão de consulta, avaliação de consulta estruturada e cálculo de histograma.  

<a name="testing-service"></a>
## <a name="test-the-service"></a>O serviço de teste
Para depurar um serviço em direto, navegue para a máquina do anfitrião de um browser. Para implementar um serviço local através de [host_service](#hosting-service), visite `http://localhost:<port>/`.  Para do Azure implementado através do serviço em nuvem [deploy_service](#deploying-service), visite `http://<serviceName>.cloudapp.net/`.

Esta página contém ligações para informações sobre estatísticas básicas de chamada API, bem como a gramática e índice alojadas neste serviço. Esta página também contém uma interface de pesquisa interativas que demonstra a utilização de APIs da web. Introduza as consultas na caixa de pesquisa para ver os resultados do [interpretar](interpretMethod.md), [avaliar](evaluateMethod.md), e [calchistogram](calchistogramMethod.md) chamadas da API. A origem de HTML subjacente desta página também serve como um exemplo de como integrar a APIs web numa aplicação, para criar uma experiência de pesquisa avançada, interativa.


