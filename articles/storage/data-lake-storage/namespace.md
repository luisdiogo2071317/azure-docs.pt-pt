---
title: Espaço de nomes no armazenamento Lake Gen2 pré-visualização hierárquica dados do Azure
description: Descreve o conceito de espaço de nomes hierárquico para pré-visualização do Azure Data Lake armazenamento Gen2
services: storage
author: jamesbak
manager: twooley
ms.service: storage
ms.topic: article
ms.date: 06/27/2018
ms.author: jamesbak
ms.component: data-lake-storage-gen2
ms.openlocfilehash: 9b41ca1eedcf69b23557c079e018d69de9fb907c
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37114372"
---
# <a name="azure-data-lake-storage-gen2-preview-hierarchical-namespace"></a>Espaço de nomes hierárquico pré-visualização do Data Lake armazenamento Gen2 do Azure

Um mecanismo de chave que permite que o Azure Data Lake armazenamento Gen2 Preview fornecer o desempenho do sistema de ficheiros na escala de armazenamento de objeto e os preços consiste na adição de um **espaço de nomes hierárquico**. Isto permite que a coleção de objetos/ficheiros dentro de uma conta para ser organizadas para uma hierarquia de diretórios e subdiretórios aninhados da mesma forma que o sistema de ficheiros no seu computador está organizado. Com o espaço de nomes hierárquico ativado, Gen2 de armazenamento do Data Lake fornece a escalabilidade e a eficácia de custos de armazenamento de objeto com semântica de sistema de ficheiros que está familiarizado motores de análise e estruturas.

## <a name="the-benefits-of-the-hierarchical-namespace"></a>As vantagens do espaço de nomes hierárquica

> [!NOTE]
> Durante a pré-visualização pública do Azure Data Lake armazenamento Gen2, algumas das funcionalidades listadas abaixo podem variar na respetiva disponibilidade. Como novas funcionalidades e regiões são lançadas durante o programa de pré-visualização, estas informações serão comunicadas através do nosso grupo Yammer dedicado.  

As seguintes vantagens estão associadas a sistemas de ficheiros que implementa um espaço de nomes hierárquico através de dados de BLOBs:

- **Manipulação de diretório atómica:** arquivos de objeto aproximada uma hierarquia de diretório por adotar uma convenção de incorporar barras (/) no nome do objeto para denotar segmentos de caminho. Enquanto esta Convenção funciona para organizar os objetos, a Convenção oferece sem assistência para ações como mover, mudar o nome ou eliminar diretórios. Sem diretórios reais, aplicações tem de processar, potencialmente, milhões de blobs individuais para conseguir o nível do diretório de tarefas. Por outro lado, o espaço de nomes hierárquico processa estas tarefas, atualizando uma única entrada (o diretório principal). 

    Esta otimização enormes é especialmente significativa para muitas estruturas de análise de macrodados. Ferramentas como o Hive, Spark, etc., muitas vezes, escrever saída localizações temporárias e, em seguida, mude o nome a localização na conclusão da tarefa. Sem espaço de nomes hierárquico, este mude o nome, muitas vezes, pode demorar mais do que a análise de processar si próprio. Menor latência de tarefa é igual a menor custo total de propriedade (TCO) para cargas de trabalho de análise.

- **Estilo de Interface familiar:** sistemas de ficheiros estão compreendidos por programadores e os utilizadores igual. Não é necessário para obter uma novo paradigma de armazenamento quando mover para a nuvem como a interface de sistema de ficheiros exposta pelo Gen2 de armazenamento do Data Lake é o mesmo paradigma utilizado por computadores, grande e em pequenos.

Uma das razões que arquivos de objeto tem não historicamente suportada espaços de nomes hierárquicos é que os espaços de nomes hierárquicos limitada escala. No entanto, o espaço de nomes hierárquico Gen2 de armazenamento do Data Lake é dimensionado linearmente e não diminuir a capacidade de dados ou o desempenho.

## <a name="when-to-enable-the-hierarchical-namespace"></a>Quando ativar o espaço de nomes hierárquico

Ativar o espaço de nomes hierárquico é recomendada para cargas de trabalho de armazenamento que foram concebidas para sistemas de ficheiros que manipular diretórios. Isto inclui todas as cargas de trabalho que são principalmente para processamento de análise. Conjuntos de dados que requerem um nível elevado da organização irão beneficiar também Ativando o espaço de nomes hierárquico.

As razões para ativar o espaço de nomes hierárquico são determinadas por uma análise do TCO. Um modo geral, melhoramentos na latência de carga de trabalho devido a aceleração de armazenamento serão necessários recursos de computação para menos tempo. Latência para muitas cargas de trabalho pode ser melhorada devido a manipulação de diretório atómica que está ativada por espaço de nomes hierárquico. Em muitas cargas de trabalho, o recurso de computação representa > 85% do total de custos e por isso mesmo uma redução modest da latência de carga de trabalho equivale a uma quantidade significativa de poupança de TCO. Mesmo nos casos em que a ativar o espaço de nomes hierárquico aumenta os custos de armazenamento, o TCO ainda é lowered devido a custos de computação reduzida.

## <a name="when-to-disable-the-hierarchical-namespace"></a>Quando desativar o espaço de nomes hierárquico

Algumas cargas de trabalho do arquivo de objeto não podem obter qualquer benefício em Ativando o espaço de nomes hierárquico. Exemplos destas cargas de trabalho incluem as cópias de segurança, armazenamento de imagem e outras aplicações em que organização de objeto é armazenada separadamente dos próprios objetos (*por exemplo,*, numa base de dados separado).

> [!NOTE]
> Com a versão de pré-visualização, se ativar o espaço de nomes hierárquico, não há nenhum interoperabilidade de dados ou operações entre Blob e APIs de REST do Data Lake armazenamento Gen2. Esta funcionalidade será adicionada durante a pré-visualização.

## <a name="next-steps"></a>Passos Seguintes

- [Criar uma conta de armazenamento](./quickstart-create-account.md)