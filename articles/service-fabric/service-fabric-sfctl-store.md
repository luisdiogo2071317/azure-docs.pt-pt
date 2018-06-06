---
title: Azure Service Fabric CLI sfctl arquivo | Microsoft Docs
description: Descreve os comandos de arquivo de sfctl CLI de recursos de infraestrutura de serviço.
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
ms.date: 05/23/2018
ms.author: bikang
ms.openlocfilehash: 39ecf568c5c41c0007b358670af755be1dd5d99e
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/05/2018
ms.locfileid: "34763243"
---
# <a name="sfctl-store"></a>sfctl store
Efetue operações de nível de ficheiro básico no arquivo de imagens de cluster.

## <a name="commands"></a>Comandos

|Comando|Descrição|
| --- | --- |
| eliminar | Elimina existente imagem armazena conteúdo. |
| informações de raiz | Obtém as informações dos conteúdos na raiz do arquivo de imagens. |
| STAT | Obtém as informações de conteúdo de arquivo de imagem. |

## <a name="sfctl-store-delete"></a>eliminação de arquivo sfctl
Elimina existente imagem armazena conteúdo.

Elimina existente que está a ser encontrado a imagem de determinado arquivo de conteúdo da imagem armazena caminho relativo. Este comando pode ser utilizado para eliminar pacotes de aplicações carregado depois de terem sido aprovisionados.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| -caminho de conteúdo [necessário] | Caminho relativo ao ficheiro ou pasta no arquivo de imagem de raiz. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-store-root-info"></a>informações de raiz do arquivo sfctl
Obtém as informações dos conteúdos na raiz do arquivo de imagens.

Devolve as informações sobre o arquivo de imagens conteúdo na raiz do arquivo de imagens.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-store-stat"></a>sfctl arquivo stat
Obtém as informações de conteúdo de arquivo de imagem.

Devolve as informações sobre o conteúdo da loja de imagem no contentPath especificado. O contentPath é relativo à raiz do arquivo de imagens.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| -caminho de conteúdo [necessário] | Caminho relativo ao ficheiro ou pasta no arquivo de imagem de raiz. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="next-steps"></a>Passos Seguintes
- [A configuração](service-fabric-cli.md) os recursos de infraestrutura do serviço CLI.
- Saiba como utilizar a CLI de recursos de infraestrutura de serviço utilizando o [scripts de exemplo](/azure/service-fabric/scripts/sfctl-upgrade-application).