---
title: Expanda a transformação de JSON com o Azure Machine Learning Workbench
description: O documento de referência para a transformação de "Expandir JSON"
services: machine-learning
author: ranvijaykumar
ms.author: ranku
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: mvc, reference
ms.topic: article
ms.date: 09/14/2017
ms.openlocfilehash: dbda4b7b6d82e8cf1e89dc78ce82efbac08b9933
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/12/2018
ms.locfileid: "35650344"
---
# <a name="expand-json-transformation"></a>Expanda a transformação de JSON
O **expanda JSON** transformação permite aos utilizadores expandir uma coluna existente que contém o texto JSON válido em várias colunas.

## <a name="how-to-perform-this-transformation"></a>Como realizar esta transformação

Siga estas etapas para executar essa transformação:
1. Selecione a coluna de origem que contém o texto JSON.
2. Selecione **expanda JSON** partir do **transforma** menu. Em alternativa, clique com o botão direito do rato no cabeçalho da coluna de origem e selecione **expanda JSON** no menu de contexto. 
3. Clique em **OK**. 
 
Junto a coluna de origem, são adicionadas colunas novas. Estas colunas contêm propriedades do próximo nível de hierarquia no texto JSON. Propriedade de chave, se estiver presente, é utilizada para criar o nome da coluna correspondente. Propriedades aninhadas são preservadas como texto JSON que o utilizador pode expandir iterativamente ou descartar conforme necessário.

## <a name="examples"></a>Exemplos

A origem columnn *Customer* é expandido em duas colunas *Customer.Name* e *Customer.Phone*.

| Cliente                                                | Customer.Name   | Customer.Phone |
|---------------------------------------------------------|-----------------|----------------|
| {"Name": "Carrie Dodson", "Phone": "123-4567-890"}   | Carrie Dodson   | 123-4567-890   |
| {"Name": "Leonard Robledo", "Phone": "456-7890-123"} | Leonard Robledo | 456-7890-123   |

