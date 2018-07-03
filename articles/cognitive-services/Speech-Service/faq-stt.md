---
title: Perguntas mais frequentes sobre conversão de voz em texto de serviço no Azure | Documentos da Microsoft
description: Seguem-se as respostas às perguntas mais populares sobre a conversão de voz em texto.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 06/11/2018
ms.author: panosper
ms.openlocfilehash: db57e0bd0f7afcfa836ef6446cdfd74e5d61c440
ms.sourcegitcommit: 756f866be058a8223332d91c86139eb7edea80cc
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/02/2018
ms.locfileid: "37345265"
---
# <a name="custom-speech-service-frequently-asked-questions"></a>Perguntas mais frequentes sobre o serviço de voz personalizada

Se não conseguir encontrar respostas para suas perguntas neste FAQ, experimente perguntar à Comunidade de serviço de voz personalizada no [Stack Overflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) e [UserVoice](https://cognitive.uservoice.com/)

## <a name="general"></a>Geral

**Pergunta**: qual é a diferença entre a linha de base e de voz personalizada para modelos de texto?

**Resposta**: modelos de linha de base foram treinados com a Microsoft dados de propriedade e já estão implementados na nuvem. Modelos personalizados que o utilizador para se adaptar um modelo para se adequar melhor um ambiente particular com ruído do específico ou linguagem. Chão de fábrica, carros, streets ruidosos exigiria um modelo acústico adaptado ao passo que os tópicos específicos, como a biologia, física, radiology, nomes de produtos e acrônimos personalizados requer um modelo de idioma adaptado.

**Pergunta**: por onde começar se eu quiser usar um modelo de linha de base?

**Resposta**: primeiro tem de obter um [chave de subscrição](get-started.md). Se quiser fazer chamadas REST para os modelos de linha de base predeployed, consulte a [detalhes aqui](rest-apis.md). Se quiser usar o download de WebSockets o [SDK](speech-sdk.md)

**Pergunta**: faço sempre tem de criar um modelo de voz personalizada?

**Resposta**: não, se seu aplicativo está usando linguagem genérica do dia a dia, em seguida, não é necessário personalizar um modelo. Além disso, se seu aplicativo é usado num ambiente em que há pouco ou nenhum barulho de fundo, em seguida, não é necessário personalizar qualquer um. O portal permite aos utilizadores implementar a linha de base e modelos personalizados e executar testes de precisão contra eles. Os utilizadores podem utilizar esta funcionalidade para medir a precisão do vs linha de base de um modelo personalizado.

**Pergunta**: como posso saber quando o processamento de meu conjunto de dados ou o modelo for concluído?

**Resposta**: atualmente, o estado do modelo ou do conjunto de dados na tabela é o só quer saber.
Quando o processamento estiver concluído, o estado irá ser "com êxito".

**Pergunta**: pode criar mais de um modelo?

**Resposta**: não há limite para quantas modelos estão em sua coleção.

**Pergunta**: percebi que cometi um erro. Como cancelar a minha importação de dados ou modelo de criação está em curso? 

**Resposta**: atualmente, não é possível reverter um processo de adaptação acústicos ou linguagem. Modelos e dados importados podem ser eliminados após eles estão num Estado terminal.

**Pergunta**: qual é a diferença entre a pesquisa e modelos de ditado e os modelos de conversação?

**Resposta**: há mais do que um modelos de linha de base à sua escolha no serviço de voz. O modelo de conversação é adequado para reconhecimento de Voz falada num estilo de conversação. Esse modelo seria o ideal para chamadas de trnascribing durante a pesquisa e ditado é ideal para aplicações de acionadas por voz. Universal é um novo modelo que pretende resolver ambos os cenários.

**Pergunta**: posso atualizar o meu modelo existente (empilhamento de modelo)?

**Resposta**: não não possível atualizar modelos existentes. Como solução alternativa combinar o conjunto de dados antigo com o novo e readapt.

Os conjuntos de dados antigos e novos deve ser combinados num único. zip (se for dados acústicos) ou um arquivo. txt, caso se trate de dados de idioma. Uma vez adaptação é feita as necessidades de modelo atualizado novo ser anular implementado para obter um novo ponto final

**Pergunta**: E se eu precisar uma simultaneidade mais elevada para meu modelo de deloyed que o que está disponível no portal. 

**Resposta**: pode aumentar verticalmente o seu modelo em incrementos de 20 pedidos simultâneos. 

Contacte-nos se necessitar de uma escala maior.

**Pergunta**: Posso transferir o meu modelo e executá-lo localmente?

**Resposta**: modelos não podem ser baixados e executados localmente.

**Pergunta**: são os meus pedidos com sessão iniciados?

**Resposta**: tem uma opção durante a criação de uma implementação de optar por desativar o rastreio, altura em que não existem áudio ou transcrições serão registadas. Caso contrário, pedidos, normalmente, são registados no Azure no armazenamento seguro. Se tiver mais questões de privacidade que proíbem a utilização do serviço de voz personalizada, entre em contato com um dos canais de suporte.

## <a name="importing-data"></a>Importar dados

**Pergunta**: o que é o limite no tamanho do conjunto de dados? Porquê? 

**Resposta**: O limite atual para um conjunto de dados é de 2 GB, devido à restrição no tamanho de um ficheiro para o upload HTTP. 

**Pergunta**: pode zip meus arquivos de texto para carregar um ficheiro de texto maior? 

**Resposta**: não, atualmente só os ficheiros de texto não comprimidos são permitidos.

**Pergunta**: O relatório de dados diz havia expressões com falha. O que é o problema?

**Resposta**: Falha ao carregar a 100% das expressões num arquivo não é um problema.
Se a grande maioria das expressões num dados acústicos ou idioma definido (por exemplo, > 95%) com êxito são importados, o conjunto de dados pode ser utilizado. No entanto, é recomendável que tente compreender por que as expressões com falha e corrigir os problemas. Problemas mais comuns, como formatação de erros, são fáceis de corrigir. 

## <a name="creating-am"></a>Criação de AM

**Pergunta**: A quantidade de dados acústico é necessário?

**Resposta**: Recomendamos que comece com 30 minutos a uma hora de dados acústicos

**Pergunta**: os dados que posso coletar?

**Resposta**: recolher dados que é o mais próximo o cenário de aplicação e utilizar as maiúsculas e minúsculas possível.
A recolha de dados deve corresponder a aplicação de destino e os utilizadores em termos de dispositivo ou dispositivos, ambientes e tipos de oradores. Em geral, deve recolher dados a partir de mais amplo um intervalo de oradores possível. 

**Pergunta**: como deve recolher? 

**Resposta**: pode criar uma aplicação de recolha de dados autónoma ou usar alguns desativar o software de gravação de áudio de prateleira.
Também pode criar uma versão do seu aplicativo que registra os dados de áudio e usa. 

**Pergunta**: É necessário de dados de adaptação de transcrever eu mesmo? 

**Resposta**: Sim! Pode transcrever por conta própria ou utilizar um serviço de transcrição profissional. Alguns usuários prefiram transcribers profissionais, enquanto outros utilizam crowdsourcing ou fazer as transcrições propriamente ditas.

## <a name="accuracy-testing"></a>Testes de precisão

**Pergunta**: Posso executar testes offline de meu modelo acústico personalizado usando um modelo de idioma personalizado?

**Resposta**: Sim, basta selecionar o modelo de idioma personalizado na lista suspensa quando configurou o teste offline

**Pergunta**: Posso executar testes offline de meu modelo de idioma personalizado usando um modelo acústico personalizado?

**Resposta**: Sim, basta selecionar o modelo acústico personalizado no menu pendente quando configurou o teste offline.

**Pergunta**: o que é a taxa de erros do Word e como é calculada?

**Resposta**: taxa de erros do Word é a métrica de avaliação de reconhecimento de fala. A VM é contabilizada como o número total de erros, que inclui inserções, eliminações e substituições, divididas pelo número total de palavras na transcrição de referência. Detalhes de modo [aqui](https://en.wikipedia.org/wiki/Word_error_rate)

**Pergunta**: como posso determinar se os resultados de um teste de precisão é bom?

**Resposta**: os resultados mostram uma comparação entre o modelo de linha de base e a personalizou.
Tente atacar o modelo de linha de base para fazer a personalização que vale a pena

**Pergunta**: como posso calcular o WER dos modelos de bases, para que eu possa ver se havia melhoria? 

**Resposta**: os resultados do teste offline mostrarem a precisão de precisão de linha de base do modelo personalizado com a melhoria ao longo da linha de base

## <a name="creating-lm"></a>Criação de LM

**Pergunta**: quantos dados de texto é necessário carregar?

**Resposta**: depende de como diferente o vocabulário e expressões usados em seu aplicativo são provenientes os modelos de linguagem de partida. Para todas as novas palavras, é útil fornecer exemplos tantos possível da utilização dessas palavras. Para expressões comuns que são utilizados na sua aplicação, incluindo expressões nos dados de idioma também é útil à medida que informa ao sistema para escutar também estes termos. É comum ter pelo menos um hudrend e, normalmente, várias centenas expressões com o conjunto de dados de idioma ou mais. Também se determinados tipos de consultas devem ser mais comuns do que outras pessoas, pode inserir várias cópias das consultas comuns no conjunto de dados.

**Pergunta**: Posso apenas carregar uma lista de palavras?

**Resposta**: carregar uma lista de palavras irá obter as palavras em para vocabulário mas ensina o sistema como as palavras são normalmente utilizadas.
Ao fornecer expressões com total ou parciais (frases ou expressões de coisas que os usuários são provavelmente diria), o modelo de idioma pode aprender novas palavras e como, elas são usadas. O modelo de idioma personalizado é bom, não apenas para a introdução de novas palavras no sistema, mas também para ajustar a probabilidade de palavras conhecidas para a sua aplicação. Fornecimento de expressões completo com ajuda o sistema saiba a melhor. 

## <a name="next-steps"></a>Passos Seguintes

* [Resolução de problemas](troubleshooting.md)
* [Notas de versão](releasenotes.md)
