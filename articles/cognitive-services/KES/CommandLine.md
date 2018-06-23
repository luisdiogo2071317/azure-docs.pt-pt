---
title: Interface de linha de comandos do serviço de exploração de dados de conhecimento | Microsoft Docs
description: Utilize a interface de linha de comandos KES para criar ficheiros de índice e a gramática de dados estruturados e, em seguida, implementá-los como serviços web dos serviços do Microsoft cognitivos.
services: cognitive-services
author: bojunehsu
manager: stesp
ms.service: cognitive-services
ms.component: knowledge-exploration
ms.topic: article
ms.date: 03/24/2016
ms.author: paulhsu
ms.openlocfilehash: ffa42ac73b42a8271004d2d45d7a80f3307ef059
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351800"
---
# <a name="command-line-interface"></a>Interface de Linha de Comandos
A interface de linha de comandos KES fornece a capacidade para criar ficheiros de índice e a gramática de dados estruturados e implementá-las como serviços web.  Utiliza a sintaxe geral: `kes.exe <command> <required_args> [<optional_args>]`.  Pode executar `kes.exe` sem argumentos para apresentar uma lista de comandos, ou `kes.exe <command>` para apresentar uma lista de argumentos disponíveis para o comando especificado.  Segue-se uma lista de comandos disponíveis:
* build_index
* build_grammar
* host_service
* deploy_service
* describe_index
* describe_grammar

<a name="build_index-command"></a>
## <a name="buildindex-command"></a>build_index comando
O **build_index** comando cria um ficheiro de índice binário de um ficheiro de definição de esquema e um ficheiro de dados de objetos a ser indexados.  O ficheiro de índice resultante pode ser utilizado para avaliar as expressões de consulta estruturada ou para gerar interpretações de linguagem natural consultas em conjunto com um ficheiro de gramática compilados.

`kes.exe build_index <schemaFile> <dataFile> <indexFile> [options]`

| Parâmetro      | Descrição               |
|----------------|---------------------------|
| `<schemaFile>` | Caminho de esquema de entrada |
| `<dataFile>`   | Caminho de dados de entrada   |
| `<indexFile>`  | Caminho de saída de índice |
| `--description <description>` | Cadeia de descrição |
| `--remote <vmSize>`           | Tamanho da VM para a compilação remoto |

Estes ficheiros podem ser especificados pela caminhos de ficheiro local ou URL para blobs do Azure.  O ficheiro de esquema descreve a estrutura dos objetos que está a ser indexada, bem como as operações para ser suportado (consulte [esquema formato](SchemaFormat.md)).  O ficheiro de dados enumera os objetos e os valores de atributo ser indexados (consulte [formato de dados](DataFormat.md)).  Quando a compilação for bem sucedida, o ficheiro de índice de saída contém uma representação comprimida dos dados de entrada que suporta as operações pretendidas.  

Uma cadeia de descrição pode ser especificada, opcionalmente, para identificar subsequentemente um índice binário, utilizando o **describe_index** comando.  

Por predefinição, o índice é criado no computador local.  Fora do ambiente do Azure, compilações locais estão limitadas aos ficheiros de dados que contêm objetos de até 10 000.  Quando - remoto sinalizador for especificado, o índice será criado numa VM do Azure criado temporariamente o tamanho especificado.  Isto permite que os índices de grandes dimensões de ser criada de forma eficiente com as VMs do Azure com mais memória.  Para evitar a paginação que atrasar o processo de compilação, recomendamos que utilize uma VM com 3 vezes a quantidade de RAM, como o tamanho do ficheiro de dados de entrada.  Para obter uma lista de tamanhos VM disponíveis, consulte [tamanhos das virtual machines](../../../articles/virtual-machines/virtual-machines-windows-sizes.md).

> [!TIP] 
> Para mais rápidas compilações, presort os objetos no ficheiro de dados, diminuindo a probabilidade.

<a name="build_grammar-command"></a>
## <a name="buildgrammar-command"></a>build_grammar comando
O **build_grammar** comando compila diretamente uma gramática especificada em secções XML para um ficheiro de gramática de binários.  O ficheiro de gramática resultante pode ser utilizado em conjunto com um ficheiro de índice para gerar interpretações de consultas de linguagem natural.

`kes.exe build_grammar <xmlFile> <grammarFile>`

| Parâmetro       | Descrição               |
|-----------------|---------------------------|
| `<xmlFile>`     | Caminho de especificação de gramática XML entrado |
| `<grammarFile>` | Caminho de saída de gramática compilados         |

Estes ficheiros podem ser especificados pela caminhos de ficheiro local ou URL para blobs do Azure.  A especificação de gramática descreve o conjunto de expressões de linguagem natural a ponderada e os respetivos interpretações semânticos (consulte [formato de gramática](GrammarFormat.md)).  Quando a compilação for bem sucedida, o ficheiro de gramática de saída contém uma representação binária da especificação de gramática para ativar a descodificar rápida.

<a name="host_service-command"/>
## <a name="hostservice-command"></a>host_service comando
O **host_service** comando aloja uma instância do serviço KES no computador local.

`kes.exe host_service <grammarFile> <indexFile> [options]`

| Parâmetro       | Descrição                |
|-----------------|----------------------------|
| `<grammarFile>` | Caminho de gramática de binários de entrada         |
| `<indexFile>`   | Caminho de entrada de índice binário           |
| `--port <port>` | Número de porta local.  Predefinição: 8000 |

Estes ficheiros podem ser especificados pela caminhos de ficheiro local ou URL para blobs do Azure.  Um serviço web será alojado no http://localhost:&lt; porta&gt;/.  Consulte [APIs da Web](WebAPI.md) para uma lista de operações suportadas.

Fora do Azure o ambiente, localmente serviços alojados estão limitados a índice ficheiros até 1 MB de tamanho, 10 pedidos por segundo e chamadas de total de 1000.  Para ultrapassar estas limitações, execute **host_service** dentro de uma VM do Azure, ou implementar um serviço em nuvem do Azure a utilizar **deploy_service**.

<a name="deploy_service-command"/>
## <a name="deployservice-command"></a>deploy_service comando
O **deploy_service** comando implementa uma instância do serviço KES num serviço em nuvem do Azure.

`kes.exe deploy_service <grammarFile> <indexFile> <serviceName> <vmSize>[options]`

| Parâmetro       | Descrição                  |
|-----------------|------------------------------|
| `<grammarFile>` | Caminho de gramática de binários de entrada           |
| `<indexFile>`   | Caminho de entrada de índice binário             |
| `<serviceName>` | Nome do serviço de nuvem de destino |
| `<vmSize>`      | Tamanho da VM de serviço de nuvem     |
| `--slot <slot>` | Ranhura de serviço de nuvem: "transição" (predefinição), "production" |

Estes ficheiros podem ser especificados pela caminhos de ficheiro local ou URL para blobs do Azure.  Nome do serviço Especifica um serviço em nuvem do Azure pré-configurada (consulte [como criar e implementar um serviço em nuvem](../../../articles/cloud-services/cloud-services-how-to-create-deploy-portal.md)).  O comando irão implementar automaticamente o serviço KES ao serviço em nuvem do Azure especificado, utilizando as VMs do tamanho especificado.  Para evitar a paginação que diminui significativamente o desempenho, recomendamos que utilize uma VM com 1 GB mais RAM ao tamanho do ficheiro de entrada de índice.  Para obter uma lista de tamanhos VM disponíveis, consulte [tamanhos para serviços em nuvem](../../../articles/cloud-services/cloud-services-sizes-specs.md).

Por predefinição, o serviço está implementado para o ambiente de teste, opcionalmente substituída através do – parâmetro ranhura.  Consulte [APIs da Web](WebAPI.md) para uma lista de operações suportadas.

<a name="describe_index-command"/>
## <a name="describeindex-command"></a>comando describe_index
O **describe_index** comando devolve informações sobre um ficheiro de índice, incluindo o esquema e a descrição.

`kes.exe describe_index <indexFile>`

| Parâmetro     | Descrição      |
|---------------|------------------|
| `<indexFile>` | Caminho de entrada de índice |

Este ficheiro pode ser especificado um caminho de ficheiro local ou um caminho de URL para um blob do Azure.  A cadeia de descrição de saída pode ser especificada utilizando o - parâmetro de descrição do **build_index** comando.

<a name="describe_grammar-command"/>
## <a name="describegrammar-command"></a>comando describe_grammar
O **describe_grammar** comando produz a especificação de gramática original utilizada para criar a gramática de binária.

`kes.exe describe_grammar <grammarFile>`

| Parâmetro       | Descrição      |
|-----------------|------------------|
| `<grammarFile>` | Caminho de gramática de entrada |

Este ficheiro pode ser especificado um caminho de ficheiro local ou um caminho de URL para um blob do Azure.

