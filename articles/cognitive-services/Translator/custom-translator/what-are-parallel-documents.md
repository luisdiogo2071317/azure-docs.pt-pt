---
title: Quais são os documentos paralelos? -Tradutor personalizado
titleSuffix: Azure Cognitive Services
description: Documentos paralelos são pares de documentos em que uma é a tradução do outro. Um documento no par contém frases no idioma de origem e o outro documento contém essas frases traduzidas no idioma de destino.
author: rajdeep-in
manager: christw
ms.service: cognitive-services
ms.subservice: custom-translator
ms.date: 11/13/2018
ms.author: v-rada
ms.topic: article
ms.openlocfilehash: d7f479bbacef7270807d9292e7b91fe835485647
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55217378"
---
# <a name="what-are-parallel-documents"></a>Quais são os documentos paralelos?

Documentos paralelos são pares de documentos em que uma é a tradução do outro. Um documento no par contém frases no idioma de origem e o outro documento contém essas frases traduzidas no idioma de destino.
Não importa qual idioma está marcado como "origem" e o idioma que está marcado como "destino" – um documento paralelo pode ser usado para treinar um sistema de tradução em ambas as direções.

## <a name="use-of-parallel-documents"></a>Utilização de documentos paralelas

Documentos paralelos são usados pelo sistema:

1.  Para saber como o palavras, frases e frases, geralmente são mapeadas entre as duas linguagens.

2.  Para saber como processar o contexto apropriado, consoante as frases ao redor. Uma palavra pode sempre significa que a mesma palavra exata no outro idioma.

Como melhor prática, certifique-se de que existe uma correspondência de 1:1 frase entre a origem e de versões de idioma de destino dos documentos.

Documentos carregados são privados para cada área de trabalho e podem ser utilizados em tantos projetos ou treinamentos como desejar. Frases extraídas dos seus documentos são armazenadas separadamente no seu repositório, como arquivos de texto Unicode simples e estão disponíveis para eliminar. Não utilize o Translator personalizada como um repositório de documentos, não será capaz de transferir os documentos que carregou no formato que carregou-los.

## <a name="recommendations"></a>Recomendações

Se seu projeto de domínio (categoria) específico, os documentos devem ser consistentes na terminologia de dentro dessa categoria. A qualidade do sistema de tradução resultante depende do número de frases no seu conjunto de documentos e a qualidade das sentenças. Os exemplos mais os documentos contêm com diversas utilizações para uma palavra específica para a categoria do seu, fazer o trabalho melhor o sistema durante a tradução.

Precisará de um mínimo de 10 000 frases paralelas para treinar um sistema. Como melhor prática, pode adicionar continuamente mais conteúdo paralelo e voltar a preparar, para melhorar a qualidade do seu sistema de tradução.

A Microsoft requer que os documentos carregados para o tradutor personalizado não se viole propriedades de direitos de autor ou intelectual de terceiros. Para obter mais informações, consulte a [termos de utilização](https://azure.microsoft.com/support/legal/cognitive-services-terms/).
Carregar um documento com o portal não altera a propriedade da propriedade intelectual no próprio documento.

## <a name="next-steps"></a>Passos Seguintes

- Saiba como utilizar um [dicionário](what-is-dictionary.md) no Translator personalizado.
