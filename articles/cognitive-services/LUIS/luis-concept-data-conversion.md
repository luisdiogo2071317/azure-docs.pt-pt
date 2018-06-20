---
title: Compreender os conceitos de conversão de dados do LUIS - Azure | Microsoft Docs
description: Saiba como utterances podem ser alteradas antes predições na compreensão de idiomas (LUIS)
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr;
ms.openlocfilehash: 56de113b41be419a5e39d705a22466139c1c29ce
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36266109"
---
# <a name="data-conversion-concepts-in-luis"></a>Conceitos de conversão de dados no LUIS
LUIS fornece uma forma para converter utterances de utterances ditas utterances texto antes de predição. 

## <a name="speech-to-intent-conversion-concepts"></a>Reconhecimento de voz para conceitos de intenção de conversão
A conversão de reconhecimento de voz para texto LUIS permite-lhe enviar utterances ditas para um ponto final e receber uma resposta de predição LUIS. O processo é uma integração do [voz](https://docs.microsoft.com/azure/cognitive-services/Speech) serviço com LUIS. 

### <a name="key-requirements"></a>Requisitos de chaves
Não é necessário criar um **API de reconhecimento de voz do Bing** chaves para esta integração. A chave de LUIS funciona para esta integração.

### <a name="new-endpoint"></a>Novo ponto final 
Esta integração cria um novo ponto final e [preços](luis-boundaries.md#key-limits) modelo. O ponto final é capaz de receber ambos autenticação ditas e utterances de texto, permitindo-lhe para utilizá-lo como um único ponto final. 

### <a name="quota-usage"></a>Utilização da quota
Consulte [chave limites](luis-boundaries.md#key-limits) para obter informações. 

### <a name="data-retention"></a>Retenção de dados
Os dados enviados para o ponto final, independentemente se se tratar de reconhecimento de voz ou de texto, só são utilizados para melhorar o seu modelo de reconhecimento de voz. Não é utilizado para além do seu modelo para melhorar o reconhecimento de voz ou LUIS uma capacidade geral. Quando a aplicação de LUIS é eliminada, os dados retidos também são eliminados.

<!-- TBD: Machine translation conversion concepts -->

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Ortografia correto prende com este tutorial](luis-tutorial-bing-spellcheck.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions