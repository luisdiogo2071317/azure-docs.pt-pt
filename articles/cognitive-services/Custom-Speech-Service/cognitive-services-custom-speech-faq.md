---
title: Perguntas mais frequentes sobre o serviço de voz personalizada no Azure | Documentos da Microsoft
description: Seguem-se as respostas às perguntas mais populares sobre o serviço de voz personalizada.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.subservice: custom-speech
ms.topic: article
ms.date: 11/21/2016
ms.author: panosper
ms.openlocfilehash: 88486ec9d1ca11d25ca31ca0abb4a34509d19a27
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55228039"
---
# <a name="custom-speech-service-frequently-asked-questions"></a>Perguntas mais frequentes sobre o serviço de voz personalizada

[!INCLUDE [Deprecation note](../../../includes/cognitive-services-custom-speech-deprecation-note.md)] 

Se não conseguir encontrar respostas para suas perguntas neste FAQ, experimente perguntar à Comunidade de serviço de voz personalizada no [Stack Overflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) e [UserVoice](https://cognitive.uservoice.com/)

## <a name="general"></a>Geral

**Pergunta**: Como posso saber quando o processamento de meu conjunto de dados ou o modelo for concluído?

**resposta**: Atualmente, o estado do modelo ou do conjunto de dados na tabela é o só quer saber.
Quando o processamento estiver concluído, o estado será "Pronto".
Estamos a trabalhar em métodos aperfeiçoados para processar o estado, por exemplo, a notificação por correio eletrónico de comunicação.

**Pergunta**: Pode criar mais de um modelo ao mesmo tempo?

**resposta**: Não há limite para quantas modelos estão em sua coleção, mas só pode ser criado cada vez em cada página.
Por exemplo, não é possível iniciar um processo de criação de modelo de idioma, se houver atualmente um modelo de idioma na fase do processo.
Pode, no entanto, ter um modelo acústico e um modelo de idioma de processamento ao mesmo tempo. 

**Pergunta**: Percebi que cometi um erro. Como cancelar a minha importação de dados ou modelo de criação está em curso? 

**resposta**: Atualmente não é possível reverter um processo de adaptação acústicos ou linguagem.
Os dados importados podem ser eliminados após a importação foi concluída

**Pergunta**: O que é a diferença entre a pesquisa e modelos de ditado e os modelos de conversação?

**resposta**: Existem dois Base acústico e modelos de linguagem à sua escolha no serviço de voz personalizada.
consultas de pesquisa ou ditado. O Conversational Microsoft AM é adequada para reconhecimento de Voz falada num estilo de conversação.
Este tipo de voz, normalmente, é direcionado em outra pessoa, tal como em centros de chamada ou reuniões.

**Pergunta**: Posso atualizar o meu modelo existente (empilhamento de modelo)?

**resposta**: Não oferecemos a capacidade de atualizar um modelo existente com novos dados.
Se tiver um novo conjunto de dados e pretender personalizar um modelo existente deve adaptá-lo novamente com os novos dados e o conjunto de dados antigo que utilizou.
Os conjuntos de dados antigos e novos deve ser combinados num. zip único (se for dados acústicos) ou um ficheiro. txt, caso se trate de uma vez a adaptação de dados de idioma é feito as necessidades de modelo atualizado novo ser anular implementado para obter um novo ponto final

**Pergunta**: E se eu precisar de simultaneidade mais elevada que o valor predefinido. 

**resposta**: Pode aumentar verticalmente o seu modelo em incrementos de 5 pedidos em simultâneo que chamamos de unidades de escala. Cada unidade de escala garante que seu modelo pode processar 5 transmissão de áudio em simultâneo. Pode comprar 100 unidades de escala (ou 500 pedidos simultâneos).

Entre em contato conosco se necessitar de mais do que isso.

**Pergunta**: Pode transferir o meu modelo e executá-lo localmente?

**resposta**: Não podemos ativar modelos para ser transferido e executado localmente.

**Pergunta**: Os meus pedidos são registrados?

**resposta**: Tem uma opção durante a criação de uma implementação de optar por desativar o rastreio, altura em que não serão registadas nenhum áudio ou transcrições. Caso contrário, pedidos, normalmente, são registados no Azure no armazenamento seguro. Se tiver mais questões de privacidade que proíbem a utilização do serviço de voz personalizada, contacte-nos.

## <a name="importing-data"></a>Importar dados

**Pergunta**: O que é o limite no tamanho do conjunto de dados? Porquê? 

**resposta**: O limite atual para um conjunto de dados é de 2 GB, devido à restrição no tamanho de um ficheiro para HTTP carregar. 

**Pergunta**: Pode zip meus arquivos de texto para carregar um ficheiro de texto maior? 

**resposta**: Não, os ficheiros de texto não comprimidos atualmente, apenas são permitidos.

**Pergunta**: O relatório de dados diz que havia expressões com falha. Este é um problema?

**resposta**: Se apenas algumas expressões de com falha ser importado com êxito, isso não é um problema.
Se a grande maioria das expressões num dados acústicos ou idioma definido (por exemplo, > 95%) com êxito são importados, o conjunto de dados pode ser utilizado. No entanto, é recomendável que tente compreender por que as expressões com falha e corrigir os problemas.
Problemas mais comuns, como formatação de erros, são fáceis de corrigir. 

## <a name="creating-am"></a>Criação de AM

**Pergunta**: A quantidade de dados acústico é necessário?

**resposta**: Recomendamos que comece com 30 minutos a uma hora de dados acústicos

**Pergunta**: Que tipo de dados posso coletar?

**resposta**: Deve recolher dados que é o mais próximo o cenário de aplicação e utilizar as maiúsculas e minúsculas possível.
Isso significa que a recolha de dados deve corresponder a aplicação de destino e os utilizadores em termos de dispositivo ou dispositivos, ambientes e tipos de oradores. Em geral, deve recolher dados a partir de mais amplo um intervalo de oradores possível. 

**Pergunta**: Como posso coletá-lo? 

**resposta**: Pode criar uma aplicação de recolha de dados autónoma ou usar alguns desativar o software de gravação de áudio de prateleira.
Também pode criar uma versão do seu aplicativo que registra os dados de áudio e usa. 

**Pergunta**: Preciso de dados de adaptação de transcrever eu mesmo? 

**resposta**: Os dados devem ser transcrito. Pode transcrever por conta própria ou utilizar um serviço de transcrição profissional. Algumas dessas transcribers de profissionais de utilização e outras pessoas utilizam crowdsourcing. Pode também recomendamos um serviço de transcrição mediante pedido.

**Pergunta**: Quanto tempo demora para criar um modelo acústico personalizado?

**resposta**: O tempo de processamento para a criação de um modelo acústico personalizado é sobre o mesmo que o comprimento do conjunto de dados acústico.
Portanto, um modelo acústico personalizado criado a partir de um conjunto de dados de hora cinco irá demorar cerca de cinco horas ao processo. 

## <a name="offline-testing"></a>Testes Offline

**Pergunta**: Posso executar testes offline de meu modelo acústico personalizado usando um modelo de idioma personalizado?

**resposta**: Sim, basta selecionar o modelo de idioma personalizado na lista pendente quando configura o teste offline

**Pergunta**: Posso executar testes offline de meu modelo de idioma personalizado usando um modelo acústico personalizado?

**resposta**: Sim, basta selecione o modelo acústico personalizado no menu pendente quando configurou o teste offline.

**Pergunta**: O que é a taxa de erros do Word e como é calculada?

**resposta**: Taxa de erros do Word é a métrica de avaliação de reconhecimento de fala. A VM é contabilizada como o número total de erros, que inclui inserções, eliminações e substituições, divididas pelo número total de palavras na transcrição de referência.

**Pergunta**: Agora posso saber os resultados do teste de meu modelo personalizado, este é um número de bom ou ruim?

**resposta**: Os resultados mostram uma comparação entre o modelo de linha de base e a personalizou.
Tente atacar o modelo de linha de base para fazer a personalização que vale a pena

**Pergunta**: Como posso calcular o WER dos modelos de bases, para que eu possa ver se havia melhoria? 

**resposta**: Os resultados do teste offline mostrarem a precisão de precisão de linha de base do modelo personalizado com a melhoria ao longo da linha de base

## <a name="creating-lm"></a>Criação de LM

**Pergunta**: Quantos dados de texto é necessário carregar?

**resposta**: Isso é uma pergunta difícil para dar uma resposta precisa para, pois dependem como diferentes o vocabulário e expressões usados em seu aplicativo a partir de modelos de idioma inicial. Para todas as novas palavras, é útil fornecer exemplos tantos possível da utilização dessas palavras. Para expressões comuns que são utilizados na sua aplicação, incluindo aquelas em que os dados de idioma também é útil à medida que informa ao sistema para escutar também estes termos.
É comum ter, pelo menos, uma centena e, normalmente, várias centenas expressões com o conjunto de dados de idioma ou mais.
Também se determinados tipos de * de consultas devem ser mais comuns do que outras pessoas, pode inserir várias cópias das consultas comuns no conjunto de dados.

**Pergunta**: Só posso carregar uma lista de palavras?

**resposta**: Carregar uma lista de palavras, irá obter as palavras em para vocabulário mas ensina o sistema como as palavras são normalmente utilizadas.
Fornecendo expressões com total ou parciais (frases ou expressões de coisas que os usuários são provavelmente diria), o modelo de idioma pode aprender novas palavras e como elas são usadas. O modelo de idioma personalizado é bom, não apenas para a introdução de novas palavras no sistema, mas também para ajustar a probabilidade de palavras conhecidas para a sua aplicação. Fornecimento de expressões completo com ajuda o sistema Aprenda isto. 

-----

 * [Descrição geral](cognitive-services-custom-speech-home.md)
 * [Iniciado](cognitive-services-custom-speech-get-started.md)
 * [Glossário](cognitive-services-custom-speech-glossary.md)
