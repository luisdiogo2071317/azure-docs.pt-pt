---
title: Origens de dados suportadas - QnA Maker
titleSuffix: Azure Cognitive Services
description: A ferramenta QnA Maker possível extrair automaticamente pares de perguntas respostas de formatos de conteúdo semiestruturados comuns, como o perguntas frequentes e manuais de produto. Conteúdo também pode ser adicionado para a base de dados de conhecimento de ficheiros estruturados.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/24/2018
ms.author: tulasim
ms.openlocfilehash: b9214d44061edad967212a1f904c2cdb6687dba2
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47039059"
---
# <a name="data-sources"></a>Origens de dados 
A ferramenta QnA Maker possível extrair automaticamente pares de perguntas respostas de formatos de conteúdo semiestruturados comuns, como o perguntas frequentes e manuais de produto. Conteúdo também pode ser adicionado para a base de dados de conhecimento de ficheiros estruturados.

## <a name="plain-faq-pages"></a>Páginas de FAQ simples
Este é o tipo mais comum da página de FAQ, em que as respostas siga imediatamente as perguntas na mesma página. 

![Página de perguntas frequentes simples](../media/qnamaker-concepts-datasources/plain-faq.png) 

 

## <a name="faq-pages-with-section-links"></a>Páginas de FAQ com Links de secção 
Este tipo de página de perguntas frequentes, as perguntas são agregadas em conjunto e são associadas a respostas que estão nas seções diferentes na mesma página.

 ![Página de FAQ de ligação da secção](../media/qnamaker-concepts-datasources/sectionlink-faq.png) 


## <a name="faq-pages-with-links-to-different-pages"></a>Páginas de FAQ com links para páginas diferentes 
Este tipo de página de perguntas frequentes é semelhante a uma página de FAQ vinculados a seção, exceto pelo fato das ligações redirecionar para uma página diferente. A ferramenta QnA Maker rastreia todas as páginas ligadas para extrair as respostas correspondentes.

 ![Página de perguntas frequentes sobre a ligação avançada](../media/qnamaker-concepts-datasources/deeplink-faq.png) 


## <a name="product-manuals"></a>Manuais de produto

Um manual é, normalmente, material de orientação que acompanha um produto. Ele ajuda o utilizador para configurar, utilizar, manter e resolver problemas relacionados com o produto. Quando o QnA Maker processa um manual, extrai os títulos e subtítulos como perguntas e o conteúdo subsequente como respostas. Ver um exemplo [aqui](http://download.microsoft.com/download/2/9/B/29B20383-302C-4517-A006-B0186F04BE28/surface-pro-4-user-guide-EN.pdf).

> [!NOTE]
> Extração funciona melhor em manuais que tenham uma tabela de conteúdos e/ou uma página de índice e uma estrutura clara com cabeçalhos hierárquicas.


## <a name="structured-data-format-through-file-upload"></a>Formato de dados estruturados através de carregamento de ficheiros

Ficheiros estruturados, como. tsv,. xlsx com colunas formatadas também podem ser carregados para o QnA Maker durante a criação da base de dados de conhecimento. Também pode carregar ficheiros a partir da **definições** separador de uma base de dados de conhecimento

| Pergunta  | Resposta  | Metadados                |
|-----------|---------|-------------------------|
| Question1 | Answer1 | "Key1:Value1|Key2:Value2' |
| Question2 | Answer2 |      `Key:Value`           |
Quaisquer colunas adicionais no arquivo de origem são ignoradas.

## <a name="structured-data-format-through-import"></a>Formato de dados estruturados através de importação
Importar uma base de dados de conhecimento substitui o conteúdo da base de dados de conhecimento existente. Importação requer um ficheiro. tsv estruturados que contém informações de origem de dados. Estas informações ajudam o QnA Maker agrupar os pares de resposta de pergunta e atributo-los para uma determinada origem de dados.

| Pergunta  | Resposta  | Origem| Metadados                |
|-----------|---------|----|---------------------|
| Question1 | Answer1 | Url1|"Key1:Value1|Key2:Value2' |
| Question2 | Answer2 | Editorial|    `Key:Value`       |

## <a name="editorial"></a>Editorial
Se não tiver conteúdo já existente para preencher a base de dados de conhecimento, também pode adicioná-los forma editorial nos conhecimentos do QnA Maker base. Saiba como atualizar a sua base de dados de conhecimento [aqui](../How-To/edit-knowledge-base.md).

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Adicionar personalidade para respostas com pessoas chit-bate-papo](../How-To/chit-chat-knowledge-base.md)

## <a name="see-also"></a>Consulte também 

[Descrição geral do Criador de FAQ](../Overview/overview.md)
