---
title: Perguntas mais frequentes para reconhecimento de voz ao serviço de texto no Azure | Microsoft Docs
description: Seguem-se as respostas às perguntas sobre o reconhecimento de voz para texto mais populares.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 06/11/2018
ms.author: panosper
ms.openlocfilehash: 64e505889ef9472603471d67a961985c1290663a
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37045848"
---
# <a name="custom-speech-service-frequently-asked-questions"></a>Serviço de reconhecimento de voz personalizadas perguntas mais frequentes

Se não conseguir encontrar respostas às suas perguntas neste FAQ, tente pedir a Comunidade do serviço de reconhecimento de voz personalizadas no [StackOverflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) e [UserVoice](https://cognitive.uservoice.com/)

## <a name="general"></a>Geral

**Pergunta**: qual é a diferença entre a linha de base e de reconhecimento de voz personalizadas para modelos de texto?

**Resposta**: já estão implementados na nuvem e ter sido preparados com a propriedade de dados do Microsoft de modelos de linha de base. Modelos personalizados permitem ao utilizador para se adaptar a um modelo para melhor satisfazer um ambiente específico com ruído ambiente específico ou o idioma. Floors de fábrica, carros, streets desnecessárias, seriam necessários adaptado modelo acústica enquanto tópicos específicos, tais como biology, physics, radiology, nomes de produtos e utilização de acrónimos personalizada precisaria que um modelo de linguagem.

**Pergunta**: onde começar se pretender utilizar um modelo de linha de base?

**Resposta**: tem primeiro de obter um [chave de subscrição](get-started.md). Se pretender efetuar chamadas REST para os modelos de linha de base predeployed, consulte o [os detalhes aqui](rest-apis.md). Se pretender utilizar a transferência de WebSockets o [SDK](speech-sdk.md)

**Pergunta**: fazer I sempre tem de criar um modelo de reconhecimento de voz personalizadas?

**Resposta**: não, se a aplicação estiver a utilizar o genérico diário idioma sem vocabulário personalizado ou -lo que, em seguida, raro maybe não é necessário personalizar um modelo. Além disso, se a aplicação está a ser utilizada num ambiente onde há pouca ou nenhuma ruído de fundo, em seguida, que não necessita de personalizar um. O portal permite aos utilizadores implementar a linha de base e modelos personalizados e executar testes de precisão contra eles. Os utilizadores podem utilizar esta funcionalidade para medir a precisão das vs linha de base de um modelo personalizado.

**Pergunta**: como posso saber quando o processamento do meu conjunto de dados ou o modelo estiver concluído?

**Resposta**: atualmente, o estado do modelo ou do conjunto de dados na tabela é pretender apenas de conhecer.
Quando o processamento estiver concluído, o estado será "Pronto".

**Pergunta**: pode criar mais de um modelo num momento?

**Resposta**: não existe nenhum limite para quantas modelos são na sua coleção mas só pode ser criado numa altura em cada página.
Por exemplo, não é possível iniciar um processo de criação de modelo de linguagem se não existir atualmente um modelo de idioma na fase do processo.
No entanto, pode ter um modelo acústica e um modelo de linguagem processar ao mesmo tempo. 

**Pergunta**: Posso realizados posso efetuadas um erro. Como cancelar a minha importação de dados ou que está em curso a criação do modelo? 

**Resposta**: atualmente, não é possível reverter um processo de adaptation acústica ou idioma.
Dados importados podem ser eliminados após a importação foi concluída

**Pergunta**: qual é a diferença entre a pesquisa & ditado modelos e os modelos Conversational?

**Resposta**: Existem dois Base acústica & idioma modelos à sua escolha no serviço de reconhecimento de voz personalizadas.
consultas de pesquisa ou ditado. O Conversational Microsoft AM é adequado para reconhecer o reconhecimento de voz ditas um estilo conversational.
Este tipo de reconhecimento de voz, normalmente, é direcionado em outra pessoa, tal como em centros de chamada ou reuniões.

**Pergunta**: posso atualizar o meu modelo existente (empilhamento de modelo)?

**Resposta**: não não possível atualizar os modelos existentes. Como um trabalho em torno de combinar o conjunto de dados antigo com o novo e readapt.

Os conjuntos de dados antigos e novos tem de ser combinados num único. zip (se for dados acústica) ou um ficheiro. txt se se tratar de dados de idioma. Uma vez adaptation é feito as necessidades de modelo atualizado novo ser anular implementado para obter um novo ponto final

**Pergunta**: E se necessário simultaneidade superior que o valor predefinido ou o que é fornecido no portal. 

**Resposta**: pode dimensionar o seu modelo em incrementos de 20 pedidos em simultâneo. 

Contacte-nos se necessitar de uma escala maior.

**Pergunta**: Posso transferir o meu modelo e executá-la localmente?

**Resposta**: modelos não podem ser transferidos e executados localmente.

**Pergunta**: são os meus pedidos registados?

**Resposta**: têm uma opção durante a criação de uma implementação para desactivar o rastreio, altura em que nenhum áudio ou transcriptions serão registados. Caso contrário pedidos normalmente iniciados no Azure no armazenamento seguro. Se ainda tiver problemas de privacidade proíbem tem de utilizar o serviço de reconhecimento de voz personalizadas, contacte um dos canais de suporte.

## <a name="importing-data"></a>Importar dados

**Pergunta**: o que é o limite no tamanho do conjunto de dados? Porquê? 

**Resposta**: O limite atual de um conjunto de dados é de 2 GB, devido à lista de restrições no tamanho de um ficheiro para o carregamento HTTP. 

**Pergunta**: pode zip os meus ficheiros de texto para carregar um ficheiro de texto maior? 

**Resposta**: não, atualmente só os ficheiros de texto descomprimidos são permitidos.

**Pergunta**: O relatório de dados indica que ocorreram utterances falhadas. O que é o problema?

**Resposta**: Falha ao carregar a 100% de utterances num ficheiro não é um problema.
Se a vasta maioria das utterances nos dados de um idioma ou acústica definido (por exemplo, > 95%) com êxito são importados, o conjunto de dados pode ser utilizado. No entanto, é recomendado que tentar compreender por que motivo os utterances falha e corrigir os problemas. Problemas mais comuns, tais como de erros, de formatação são fáceis de corrigir. 

## <a name="creating-am"></a>Criar AM

**Pergunta**: quantidade de dados acústica preciso?

**Resposta**: Recomendamos que comece com 30 minutos para uma hora dos dados acústica

**Pergunta**: os dados que posso recolher?

**Resposta**: recolher dados que são tão próximos o cenário de aplicação e utilizar as maiúsculas e minúsculas quanto possível.
A recolha de dados deve corresponder à aplicação de destino e os utilizadores em termos de dispositivo ou dispositivos, ambientes e tipos de speakers. Em geral, deve recolher os dados de abrangentes como um intervalo de speakers quanto possível. 

**Pergunta**: como deve recolher? 

**Resposta**: pode criar uma aplicação de recolha de dados autónoma ou utilizar algumas desativar o software de gravação de áudio shelf.
Também pode criar uma versão da aplicação que regista os dados de áudio e que utiliza. 

**Pergunta**: É necessário transcribe dados adaptation myself? 

**Resposta**: os dados têm de ser transcribed. Pode transcribe por si ou utilizar um serviço transcription profissionais. Alguns destes transcribers profissionais de utilização e outros utilizam crowdsourcing.

**Pergunta**: quanto tempo demora para criar um modelo de acústica personalizado?

**Resposta**: é o tempo de processamento para criar um modelo de acústica personalizado sobre o mesmo que o comprimento do conjunto de dados acústica.
Por isso, um modelo acústica personalizado criado a partir de um conjunto de dados de cinco horas irá demorar cerca de cinco horas ao processo. 

## <a name="offline-testing"></a>Testes Offline

**Pergunta**: posso efetuar testes offline do meu modelo acústica personalizado utilizando um modelo de idioma personalizado?

**Resposta**: Sim, apenas selecionar o modelo de idioma personalizado na lista pendente quando configurou o teste offline

**Pergunta**: posso efetuar testes offline do meu modelo de idioma personalizado utilizando um modelo de acústica personalizado?

**Resposta**: Sim, apenas selecionar o modelo acústica personalizado no menu pendente ao configurar o teste offline.

**Pergunta**: qual é a taxa de erros do Word e como é calculada?

**Resposta**: taxa de erros do Word é a métrica de avaliação de reconhecimento de voz. Este é contabilizado como o número total de erros, que inclui inserções, eliminações e substituições, divididas pelo número total de palavras no transcription de referência.

**Pergunta**: como posso determinar se os resultados de um teste de precisão é boa?

**Resposta**: os resultados mostram uma comparação entre o modelo de linha de base e aquele que personalizada.
Deve procure de beat o modelo de linha de base para fazer a personalização valer a pena

**Pergunta**: como posso descobrir o WER dos modelos de base, para que posso ver se ocorreu melhoramento? 

**Resposta**: os resultados do teste offline mostram precisão de precisão de linha de base do modelo personalizado bem como a melhoria através da linha de base

## <a name="creating-lm"></a>Criar LM

**Pergunta**: quanto dados de texto se é necessário carregar?

**Resposta**: depende de forma diferente vocabulário e expressões utilizados na sua aplicação são de modelos de idioma de partida. Para todas as palavras novo, é útil fornecer exemplos tantas quanto possível da utilização desses palavras. Para as expressões comuns que são utilizadas na sua aplicação, incluindo expressões nos dados de idioma também é útil como indica o sistema para escutar bem estes termos. É comum para 100, pelo menos, um e normalmente várias centenas utterances em conjunto de dados de idioma ou mais. Também se determinados tipos de consultas são esperados para ser mais comuns a outras pessoas, pode inserir várias cópias das consultas comuns no conjunto de dados.

**Pergunta**: pode posso apenas carregar uma lista das palavras?

**Resposta**: carregamento de uma lista das palavras irá obter as palavras para vocabulário, mas não ensinar o sistema como as palavras são normalmente utilizadas.
Fornecendo utterances total ou parciais (frases ou expressões das ações que os utilizadores se prevê a dizer), o modelo de linguagem pode saber as novas palavras e como, são utilizados. O modelo de idioma personalizado é boa não apenas para obter novas palavras no sistema, mas também para ajustar a probabilidade de palavras conhecidas para a sua aplicação. Fornecer utterances completas ajuda-o sistema saiba melhor. 

## <a name="next-steps"></a>Passos Seguintes

* [Resolução de problemas](troubleshooting.md)
* [Notas de versão](releasenotes.md)
