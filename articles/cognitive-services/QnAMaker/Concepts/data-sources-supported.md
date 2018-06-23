---
title: As origens de dados suportadas - serviços cognitivos Microsoft | Microsoft Docs
titleSuffix: Azure
description: Origens de dados suportadas
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 04/21/2018
ms.author: saneppal
ms.openlocfilehash: b888846056fd60f37cdb1da85904fa14ffe79a39
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354103"
---
# <a name="data-sources"></a>Origens de dados 
Maker de QnA automaticamente pode extrair pares de resposta de pergunta a partir dos formatos de conteúdo semiestruturados comuns, tais como o perguntas mais frequentes e como produto. Também pode ser adicionado a conteúdo para a base de dados de conhecimento dos ficheiros structured.

## <a name="plain-faq-pages"></a>Páginas de FAQ simples
Este é o tipo mais comuns da página de FAQ, em que as respostas de seguir de imediato perguntas na mesma página. 

![Página de FAQ simples](../media/qnamaker-concepts-datasources/plain-faq.png) 

 

## <a name="faq-pages-with-section-links"></a>Páginas de FAQ com ligações de secção 
Este tipo de página de FAQ, as perguntas são agregadas em conjunto e estão ligadas a respostas são nas secções diferentes na mesma página.

 ![Página de FAQ de ligação da secção](../media/qnamaker-concepts-datasources/sectionlink-faq.png) 


## <a name="faq-pages-with-links-to-different-pages"></a>Páginas de FAQ com ligações a diversas páginas 
Este tipo de página de FAQ é semelhante a uma página de FAQ secção ligada, exceto que as ligações redirecionar para uma página diferente. Maker de QnA pesquisa todas as páginas ligadas para extrair as respostas correspondentes.

 ![Página de FAQ de ligação avançada](../media/qnamaker-concepts-datasources/deeplink-faq.png) 


## <a name="product-manuals"></a>Como produto

Uma manual é, geralmente, material de documentação de orientação que acompanha um produto. Ajuda o utilizador para configurar, utilizar, manter e resolver problemas relacionados com o produto. Quando Maker de QnA processa uma manual do utilizador, extrai os cabeçalhos e subheadings como perguntas e os conteúdos subsequentes como respostas. Ver um exemplo [aqui](http://download.microsoft.com/download/2/9/B/29B20383-302C-4517-A006-B0186F04BE28/surface-pro-4-user-guide-EN.pdf).

> [!NOTE]
> Extração funciona melhor no como ter uma tabela de conteúdo e/ou uma página de índice e uma estrutura simples com cabeçalhos hierárquicas.


## <a name="structured-data-format-through-file-upload"></a>Formato de dados estruturados através do carregamento de ficheiros

Ficheiros estruturados como .tsv, ou com colunas formatadas podem também ser também carregados para Maker de QnA durante a criação da base de dados de conhecimento. Também pode carregar ficheiros a partir de **definições** separador de uma base de dados de conhecimento

| Pergunta  | Resposta  | Metadados                |
|-----------|---------|-------------------------|
| Question1 | Answer1 | `Key1:Value1\|Key2:Value2` |
| Question2 | Answer2 |      `Key:Value`           |
Quaisquer colunas adicionais no ficheiro de origem são ignoradas.

## <a name="structured-data-format-through-import"></a>Formato de dados estruturados através da importação
Importar uma base de dados de conhecimento substitui o conteúdo da base de dados de conhecimento existente. Importação requer um ficheiro de .tsv estruturados que contém informações de origem de dados. Estas informações ajudam Maker de QnA agrupar os pares de resposta de pergunta e atributo-los para uma determinada origem de dados.

| Pergunta  | Resposta  | Origem| Metadados                |
|-----------|---------|----|---------------------|
| Question1 | Answer1 | Url1|`Key1:Value1\|Key2:Value2` |
| Question2 | Answer2 | Editorial|    `Key:Value`       |

## <a name="editorial"></a>Editorial
Se não tiver conteúdo pré-existente para povoar a base de dados de conhecimento, pode também adicionar editorially nos conhecimentos do Maker de QnA base. Saiba como atualizar a base de dados de conhecimento [aqui](../How-To/edit-knowledge-base.md).

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Configurar um serviço de QnA Maker](../How-To/set-up-qnamaker-service-azure.md)

## <a name="see-also"></a>Consulte também 

[Descrição geral de QnA Maker](../Overview/overview.md)