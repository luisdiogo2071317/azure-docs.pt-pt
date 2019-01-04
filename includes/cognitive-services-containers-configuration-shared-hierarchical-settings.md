---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 01/02/2019
ms.openlocfilehash: e2c8b4e74933df593dbc023dcb886dff80554a1a
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/02/2019
ms.locfileid: "53977395"
---
Definições para o contentor são hierárquicas e todos os contentores no computador anfitrião utilizarem uma hierarquia partilhada.

Pode utilizar qualquer um dos seguintes procedimentos para especificar as definições:

* [Variáveis de ambiente](#environment-variable-settings)
* [Argumentos da linha de comandos](#command-line-argument-settings)

Valores de variáveis de ambiente substituem valores de argumento da linha de comandos, que por sua vez, substituir os valores predefinidos para a imagem de contentor. Se especificar valores diferentes numa variável de ambiente e um argumento da linha de comandos para a mesma definição de configuração, o valor na variável de ambiente é utilizado pelo contêiner instanciado.

|Precedência|Definir localização|
|--|--|
|1|Variável de ambiente| 
|2|Linha de comandos|
|3|Valor de padrão de imagem de contentor|

### <a name="environment-variable-settings"></a>Definições de variáveis de ambiente

Os benefícios do uso de variáveis de ambiente são:

* É possível configurar várias definições.
* Vários contentores podem utilizar as mesmas definições.

### <a name="command-line-argument-settings"></a>Definições de argumento da linha de comandos

A vantagem de usar os argumentos da linha de comandos é que cada contentor pode usar diferentes configurações.
