---
title: Guia detalhado sobre como utilizar as preparações de dados do Azure Machine Learning | Documentos da Microsoft
description: Este documento fornece uma descrição geral e detalhes sobre como resolver problemas de dados com preparações de dados do Azure Machine Learning
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: ''
ms.devlang: ''
ms.topic: article
ms.date: 02/01/2018
ROBOTS: NOINDEX
ms.openlocfilehash: 7536e67d0ae4973008c8acc91a99a7d0d286f9b8
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46988738"
---
# <a name="data-preparations-user-guide"></a>Guia de utilizador de preparações de dados 

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 


A experiência de preparações de dados do Azure Machine Learning oferece muita funcionalidade avançada. Este artigo documenta as partes mais profundo da experiência.

### <a name="step-execution-history-and-caching"></a>Execução do passo, histórico e colocação em cache 
Histórico de etapa de preparações de dados mantém uma série de caches por motivos de desempenho. Se selecionar uma etapa e chegar a uma cache, não executar novamente. Se tiver um bloco de escrita no final do histórico de passo e Inverte frente e para trás nos passos, mas não efetuar alterações, a operação de escrita não é acionada após a primeira vez. Uma gravação de novos ocorre e substitui aquele anterior, se:

- Faça as alterações para o bloco de escrita.
- Adicionar um novo bloco de transformação e mova-a acima o bloco de escrita, o que gera uma invalidação de cache.
- Altere as propriedades de um bloco acima o bloco de escrita, o que gera uma invalidação de cache.
- Selecione a atualização de uma amostra (portanto, invalidar todos os caches).

### <a name="error-values"></a>Valores de erro

Transformações de dados poderão falhar para um valor de entrada, porque esse valor não pode ser tratado adequadamente. Por exemplo, no caso de operações de coerção de tipo, a coerção falha se o valor de cadeia de entrada não pode ser convertido no tipo de destino especificado. Uma operação de coerção de tipo pode ser uma coluna do tipo de cadeia a converter para um tipo numérico ou booleano ou tentando duplicar uma coluna que não existe. (Esta falha ocorre como resultado de mover o *eliminar coluna X* operação antes do *colunas duplicados X* operação.)

Nestes casos, preparações de dados produz um valor de erro como o resultado. Valores de erro indicam que uma operação anterior falhou para o valor indicado. Internamente, eles são tratados como um tipo de valor de primeira classe, mas sua presença não altera o tipo subjacente de uma coluna, mesmo que uma coluna consiste inteiramente em valores de erro.

Valores de erro são fáceis de identificar. Estão realçadas em vermelho e ler "Erro". Para determinar o motivo do erro, coloque o cursor sobre um valor de erro para ver uma descrição de texto da falha.

Propagação de valores de erro. Após um erro valor ocorre, ele propaga na maioria dos casos, como um erro por meio da maioria das operações. Existem três formas de substituir ou removê-los:

* Substituir
    -  Uma coluna com o botão direito e selecione **substituir valores de erro**. Em seguida, pode escolher um valor de substituição para cada valor de erro encontrado na coluna.

* Remover
    - Preparações de dados inclui filtros interativos para preservar ou remover valores de erro.
    - Uma coluna com o botão direito e selecione **filtrar coluna**. Para preservar ou remover valores de erro, crie uma condicional com a condição *"é o erro"* ou *"não é erro."*

* Use uma expressão de Python para condicionalmente operam em valores de erro. Para obter mais informações, consulte a [secção sobre extensões Python](data-prep-python-extensibility-overview.md).

### <a name="sampling"></a>Amostragem
Um ficheiro de origens de dados aceita dados não processados de um ou mais origens, a partir do sistema de arquivos local ou uma localização remota. O bloco de exemplo permite-lhe especificar se pretende trabalhar com um subconjunto dos dados através da geração de exemplos. Operação de uma amostra de dados em vez de um conjunto de dados grandes, muitas vezes, leva a melhor desempenho quando realizar operações em passos posteriores.

Para cada ficheiro de origens de dados, vários exemplos podem ser gerados e armazenados. No entanto, apenas um exemplo pode ser definido como o exemplo de Active Directory. Pode criar, editar ou eliminar os exemplos no Assistente de origem de dados ou ao editar o bloco de exemplo. Quaisquer ficheiros de preparações de dados que fazem referência a uma origem de dados inerentemente utilizam o exemplo especificado no ficheiro de origens de dados.

Existem várias estratégias de amostragem disponíveis, cada uma com diferentes parâmetros configuráveis.

#### <a name="top"></a>Superior
Esta estratégia pode ser aplicada a arquivos locais ou remotos. As primeiras linhas de N (especificadas por contagem) são necessários para a origem de dados.

#### <a name="random-n"></a>N aleatório 
Esta estratégia pode ser aplicada apenas a arquivos locais. Demora aleatórias linhas de N (especificadas por contagem) na origem de dados. Pode fornecer uma semente específica para garantir que é gerado o mesmo exemplo, desde que a contagem é também o mesmo.

#### <a name="random-"></a>% Aleatório 
Esta estratégia pode ser aplicada a arquivos locais ou remotos. Em ambos os casos, uma probabilidade e uma semente tem de ser fornecido, semelhante à estratégia N aleatório.

Para exemplos de ficheiros remotos, é precisam ser fornecidos parâmetros adicionais:

- Gerador de exemplo 
  - Selecione um cluster do Spark ou destino a ser utilizado para a geração de exemplo de computação do Docker remoto. O destino de computação tem de ser criado para o projeto com antecedência para que ele for apresentada nesta lista. Siga os passos na secção "criam um novo destino de computação" na [como utilizar a GPU no Azure Machine Learning](how-to-use-gpu.md) para criar destinos de computação.
- Armazenamento de exemplo 
  - Forneça uma localização de armazenamento intermediário para armazenar o exemplo remoto. Este caminho tem de ser um diretório diferente da localização de ficheiro de entrada.

#### <a name="full-file"></a>Ficheiro completo 
Esta estratégia pode ser aplicada apenas a arquivos locais, tendo o ficheiro completo na origem de dados. Se o ficheiro é demasiado grande, esta opção pode retardar a operações futuras na aplicação. Talvez seja mais adequado utilizar uma estratégia de amostragem diferentes.


### <a name="fork-merge-and-append"></a>Bifurcar, intercalar e acrescentar

Quando aplicar um filtro ao longo de um conjunto de dados, a operação divide os dados em dois conjuntos de resultados: um conjunto representa registos que tenha êxito no filtro e outro conjunto é para os registos que falharam. Em ambos os casos, o usuário pode escolher qual resultado definido para apresentar. O utilizador pode eliminar o outro conjunto de dados ou colocá-lo num novo fluxo de dados. A última opção é chamada para criar o fork.

Para bifurcar: 
1. Selecione uma coluna, botão direito do mouse e selecione o **filtro** coluna.

2. Sob **pretendo**, selecione **manter linhas** para apresentar o conjunto de resultados que passa o filtro.

3. Selecione **remover linhas** para apresentar o conjunto com falha.

4. Após **condições**, selecione **criar fluxo de dados que contém as linhas filtradas em** para bifurcar o resultado de exibição de não definido num novo fluxo de dados.


Esta prática é frequentemente utilizada para separar um conjunto de dados que necessita de preparação adicional. Depois que preparou o conjunto de dados bifurcado, é comum para intercalar os dados com o resultado definido no fluxo de dados original. Para executar uma intercalação (o inverso de uma operação do fork), utilize uma das seguintes ações:

- **Acrescentar linhas**. Intercalar dois ou mais fluxos de dados verticalmente (inter-relacionam). 
- **Acrescentar colunas**. Intercalar dois ou mais fluxos de dados horizontalmente (column-wise).


>[!NOTE]
>Se ocorrer uma colisão de coluna de acréscimo falha de colunas.


Após uma operação de intercalação, um ou mais fluxos de dados são referenciados por um fluxo de dados de origem. Preparações de dados notifica-o com uma notificação no canto inferior direito da aplicação, abaixo da lista de passos.


Qualquer operação no fluxo de dados referenciada requer que o fluxo de dados principal para atualizar o exemplo utilizado a partir do fluxo de dados referenciada. De eventos, uma caixa de diálogo de confirmação substitui a notificação de referência do fluxo de dados no canto inferior direito. Essa caixa de diálogo confirma que tem de atualizar o fluxo de dados para sincronizar alterações para quaisquer fluxos de dados de dependência.

### <a name="list-of-appendices"></a>Lista de apêndices 
* [Origens de dados suportadas](data-prep-appendix2-supported-data-sources.md)  
* [Transformações suportadas](data-prep-appendix3-supported-transforms.md)  
* [Inspetores suportados](data-prep-appendix4-supported-inspectors.md)  
* [Destinos suportados](data-prep-appendix5-supported-destinations.md)  
* [Expressões de filtro de exemplo em Python](data-prep-appendix6-sample-filter-expressions-python.md)  
* [Expressões do fluxo de dados de transformação do exemplo em Python](data-prep-appendix7-sample-transform-data-flow-python.md)  
* [Origens de dados de exemplo em Python](data-prep-appendix8-sample-source-connections-python.md)  
* [Ligações de destino de exemplo em Python](data-prep-appendix9-sample-destination-connections-python.md)  
* [Coluna de exemplo se transforma em Python](data-prep-appendix10-sample-custom-column-transforms-python.md)  
