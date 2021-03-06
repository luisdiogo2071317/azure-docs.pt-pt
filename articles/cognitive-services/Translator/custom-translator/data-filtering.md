---
title: Filtragem de dados - Translator personalizado
titleSuffix: Azure Cognitive Services
description: Quando submete documentos a ser utilizado para um sistema personalizado de formação, os documentos passar por uma série de passos para preparar para treinamento de filtragem e de processamento.
author: jann-skotdal
manager: christw
ms.service: cognitive-services
ms.subservice: custom-translator
ms.date: 12/18/2018
ms.author: v-jansko
ms.topic: article
ms.openlocfilehash: dadcf228d2a633426fc759ca6914b8b894ea8c3d
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55226320"
---
# <a name="data-filtering"></a>Filtragem de dados 

Quando submete documentos a ser utilizado para um sistema personalizado de formação, os documentos passar por uma série de passos para preparar para treinamento de filtragem e de processamento. Estes passos são explicados a seguir. O conhecimento de como a filtragem pode ajudá-lo a compreender a contagem de sentença apresentada no Microsoft translator personalizado, bem como os passos que pode seguir para preparar os documentos para treinamento tradutor personalizado. 

## <a name="sentence-alignment"></a>Alinhamento de frases 
Se o seu documento não está no formato XLIFF, TMX ou ALIGN, Translator personalizado se Alinha as frases de seus documentos de origem e destino entre si, frase por frase. Tradutor não executa o alinhamento de documento – ela segue a nomenclatura dos documentos para o documento correspondente do outro idioma. Dentro do documento, personalizado Translator tenta encontrar a frase correspondente no outro idioma. Ele usa o documento de etiquetas de marcação como HTML incorporado para ajudar com o alinhamento.  

Se vir uma grande discrepância entre o número de frases na origem e documentos de lado de destino, o documento pode não ter sido paralelo em primeiro lugar, ou por outros motivos alignable não bem. O documento combinada com uma grande diferença (> 10%) das sentenças em cada lado garante uma olhada segundo para se certificar de que eles são realmente paralelos. Tradutor personalizada mostra um aviso junto ao documento se a contagem de frase diferente com o nome.  


## <a name="deduplication"></a>Eliminação de duplicados 
Tradutor personalizado remove as frases que estão presentes no ajuste de documentos a partir de dados de treinamento e teste. A remoção dinamicamente acontece dentro de treinamento ser executado, não no passo de processamento de dados. Tradutor personalizado comunica a contagem de frase para si na descrição geral do projeto antes dessa remoção.  

## <a name="length-filter"></a>Filtro de comprimento 
* Remova as frases com apenas uma palavra em ambos os lados. 
* Remova as frases com mais de 100 palavras em ambos os lados.  Chinês, japonês, coreano são excluídos. 
* Remova as frases com menos de 3 carateres. Chinês, japonês, coreano são excluídos. 
* Remova as frases com mais de 2000 caracteres para chinês, japonês, coreano. 
* Remova as frases com menos de 1% carateres alfa. 
* Remova entradas de dicionário que contém mais de 50 palavras. 

 
## <a name="white-space"></a>Espaço em branco 
* Substitua qualquer seqüência de caracteres de espaço em branco, incluindo guias e sequências de CR/LF com um caráter de espaço único. 
* Remover o espaço em branco à esquerda nem à direita da sentença 


## <a name="sentence-end-punctuation"></a>Pontuação de final da sentença 
Substitua os vários carateres de pontuação de final da sentença numa única instância.  

 
## <a name="japanese-character-normalization"></a>Normalização de caracteres japoneses 
Normalizar os caracteres japoneses duplicados: Converta meia largura para caracteres de largura total. 

 
## <a name="unescaped-xml-tags"></a>Invalidada marcas XML 
Filtragem de etiquetas de invalidada de transformações em etiquetas com caráter de escape: 
* `&lt;` torna-se `&amp;lt;` 
* `&gt;` torna-se `&amp;gt;` 
* `&amp;` torna-se `&amp;amp;` 

 
## <a name="invalid-characters"></a>Carateres inválidos 
Tradutor personalizado remove as frases que contenha o caráter de Unicode U + FFFD. O caractere U + FFFD indica uma falha na conversão de codificação. 

## <a name="next-steps"></a>Passos Seguintes

- [Preparar um modelo](how-to-train-model.md) no Translator personalizado.
