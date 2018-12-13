---
title: Azure Service Fabric CLI sfctl loja | Documentos da Microsoft
description: Descreve os comandos do CLI do Service Fabric sfctl store.
services: service-fabric
documentationcenter: na
author: Christina-Kang
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: cli
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 12/06/2018
ms.author: bikang
ms.openlocfilehash: 034ae29b5fabae15aa2b6b96e7fefaef23c1c5a1
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53275232"
---
# <a name="sfctl-store"></a>sfctl store
Efetue operações de nível de arquivo básico no armazenamento de imagens do cluster.

## <a name="commands"></a>Comandos

|Comando|Descrição|
| --- | --- |
| delete | Elimina o conteúdo do arquivo de imagem de existente. |
| informações de raiz | Obtém as informações dos conteúdos na raiz do arquivo de imagem. |
| STAT | Obtém as informações de conteúdo de arquivo de imagem. |

## <a name="sfctl-store-delete"></a>o sfctl store delete
Elimina o conteúdo do arquivo de imagem de existente.

Eliminações existente conteúdo de arquivo de imagem que está a ser encontrado dentro da imagem de determinado armazenam o caminho relativo. Este comando pode ser utilizado para eliminar pacotes de aplicações carregadas depois de terem sido aprovisionados.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| – caminho de conteúdo [necessário] | Caminho relativo para o ficheiro ou pasta no arquivo de imagem de sua raiz. |
| – tempo limite -t | Tempo limite do servidor em segundos.  Predefinido\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| – depuração | Aumenta a verbosidade de registo para mostrar que todos os registos de depuração. |
| – ajudar -h | Mostre esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinido\: json. |
| – consulta | Cadeia de consulta do JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumenta a verbosidade do registo. Utilize--debug para os registos de depuração completa. |

## <a name="sfctl-store-root-info"></a>informações de raiz do arquivo sfctl
Obtém as informações dos conteúdos na raiz do arquivo de imagem.

Devolve as informações sobre armazenamento de imagens do conteúdo na raiz do arquivo de imagem.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| – tempo limite -t | Tempo limite do servidor em segundos.  Predefinido\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| – depuração | Aumenta a verbosidade de registo para mostrar que todos os registos de depuração. |
| – ajudar -h | Mostre esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinido\: json. |
| – consulta | Cadeia de consulta do JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumenta a verbosidade do registo. Utilize--debug para os registos de depuração completa. |

## <a name="sfctl-store-stat"></a>o sfctl store stat
Obtém as informações de conteúdo de arquivo de imagem.

Devolve as informações sobre o conteúdo do arquivo de imagem no contentPath especificado. O contentPath é relativo à raiz do arquivo de imagem.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| – caminho de conteúdo [necessário] | Caminho relativo para o ficheiro ou pasta no arquivo de imagem de sua raiz. |
| – tempo limite -t | Tempo limite do servidor em segundos.  Predefinido\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| – depuração | Aumenta a verbosidade de registo para mostrar que todos os registos de depuração. |
| – ajudar -h | Mostre esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinido\: json. |
| – consulta | Cadeia de consulta do JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumenta a verbosidade do registo. Utilize--debug para os registos de depuração completa. |


## <a name="next-steps"></a>Passos Seguintes
- [Configuração](service-fabric-cli.md) a CLI do Service Fabric.
- Saiba como utilizar a CLI do Service Fabric utilizando o [scripts de exemplo](/azure/service-fabric/scripts/sfctl-upgrade-application).