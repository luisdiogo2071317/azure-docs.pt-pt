---
title: Ambientes de desenvolvimento do Azure Machine Learning | Documentos da Microsoft
description: Uma visão geral dos ambientes de desenvolvimento, que pode utilizar com o serviço Azure Machine Learning. Python 3 é o único requisito para o seu ambiente de desenvolvimento, mas recomendamos que utilize também Conda ambientes. Para o desenvolvimento de ferramentas, recomendamos que blocos de notas do Jupyter, blocos de notas do Azure e editores de IDE/código.
services: machine-learning
author: rastala
ms.author: roastala
manager: cgronlun
ms.service: machine-learning
ms.component: core
ms.reviewer: larryfr
ms.topic: conceptual
ms.date: 9/24/2018
ms.openlocfilehash: 4d25e147044053aa76afe2da482b71c24efc2325
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51242879"
---
# <a name="development-environment-for-azure-machine-learning"></a>Ambiente de desenvolvimento do Azure Machine Learning 

Saiba mais sobre os ambientes de desenvolvimento que pode usar com o serviço Azure Machine Learning. 

O serviço Azure Machine Learning é independente de plataforma e não requer um ambiente de desenvolvimento específico. Ele requer __Python 3__, e recomendamos que utilize __Conda__ para criar ambientes isolados. __Se já tiver um ambiente de desenvolvimento que atenda a essas demandas__, pode ignorar este documento e vá para o [configurar o ambiente de desenvolvimento](how-to-configure-environment.md) documento.

O restante deste documento aborda os ambientes de desenvolvimento que recomendamos que:

* __Blocos de Notas do Jupyter__
* __Blocos de notas do Azure__
* __Integrado (IDE) de ambientes de desenvolvimento e editores de códigos__
* __Máquina de Virtual de ciência de dados__

Uma comparação entre esses ambientes é difícil, como blocos de notas e IDEs podem ser estendidos. Por exemplo, pode servir-se alguns IDEs como clientes para blocos de notas do Jupyter. Em termos gerais, __blocos de notas__ foram concebidos para __interativa experimentação__ e __visualização__. __IDEs e editores de código__ fornecem ferramentas para __melhorar a qualidade do código__ e __integrar com sistemas externos__ , como controle de versão.

> [!TIP]
> Usar um ambiente não bloquear a sua utilização utilizando o outro. Blocos de notas e IDEs são apenas as ferramentas e podem ser misturadas conforme necessário. Por exemplo, poderá começar a fazer experimentações num bloco de notas, em seguida, exportar para um script de python para edição e depuração num IDE.
>
> É por isso a máquina de Virtual de ciência de dados fornece os blocos de notas do Jupyter e vários IDEs de Python populares.

## <a name="jupyter-notebooks"></a>Jupyter Notebooks

Blocos de notas do Jupyter faz parte do [Jupyter projeto](https://jupyter.org/). Eles estão focalizados no fornecimento de uma experiência interativa de codificação onde criar documentos que misturam código em direto com texto narrativo e gráficos. Pode instalar o Jupyter Notebooks numa variedade de plataformas.

Ter sua própria instalação do bloco de notas do Jupyter permite-lhe instalar e configurar o ambiente, conforme necessário. No entanto, é responsáveis por manter o sistema.

## <a name="azure-notebooks"></a>Azure Notebooks

[Blocos de notas do Azure](https://notebooks.azure.com) (pré-visualização) é um ambiente de blocos de notas na cloud do Azure. Baseia-se no Jupyter e fornece um ambiente que tenha sido previamente carregado com bibliotecas populares. O SDK do Azure Machine Learning já está instalado em blocos de notas do Azure, para que possa começar a fazer experiências com quase nenhuma configuração.

Blocos de notas do Azure também simplifica o processo de compartilhamento de seus blocos de notas. Pode partilhar um URL para seus blocos de notas, tornar sua biblioteca pública ou partilhar nas redes sociais a partir da interface de blocos de notas do Azure.

A desvantagem de blocos de notas do Azure é que não tem controle completo sobre o ambiente e poderá não conseguir instalar software personalizado que precisa. Também é um ambiente compartilhado, para que seus blocos de notas podem executar mais lento do que numa instalação de bloco de notas do Jupyter dedicada.

## <a name="ides-and-code-editors"></a>IDEs e editores de código

Existem vários IDEs e editores de código que funcionam com o Azure Machine Learning. O requisito de software apenas é o SDK do Azure Machine Learning, que pode ser instalado utilizando o `pip` utilitário.

Recomendamos [Visual Studio Code](https://code.visualstudio.com/), pois ele fornece ferramentas para trabalhar com a linguagem Python e o Azure Machine Learning. Está disponível para plataformas Linux, macOS e Windows.

## <a name="data-science-virtual-machine"></a>Máquina Virtual de Ciência de Dados

A Máquina Virtual de ciência de dados (DSVM) é uma combinação de ambientes de anteriores. É uma VM na plataforma do Azure com o Jupyter Notebooks, Visual Studio Code e o SDK do Azure Machine Learning pré-instalado. Uma vez que o software necessário está pré-instalado na imagem VM, pode começar a experimentar com o Azure Machine Learning rapidamente quando a VM tiver sido criada.

A DSVM permite-lhe selecionar os recursos de computação que necessários, como a CPU, GPU e memória. Ele também é previamente instalado com outros editores como PyCharm, bem como software, como o TensorFlow, Keras e PyTorch popular de aprendizagem. Se o software que necessário não estiver instalado, pode instalá-lo por conta própria.

Para obter mais informações sobre o que é previamente instalado, consulte a [o que é a máquina de Virtual de ciência de dados](../data-science-virtual-machine/overview.md) documento.

## <a name="next-steps"></a>Passos Seguintes

Agora que aprendeu sobre os ambientes de desenvolvimento, saiba [como configurar um ambiente de desenvolvimento](how-to-configure-environment.md).

