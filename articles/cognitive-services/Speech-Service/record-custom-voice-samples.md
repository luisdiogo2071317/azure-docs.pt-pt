---
title: Exemplos de como grave voz para a criação de uma voz personalizada | Documentos da Microsoft
titleSuffix: Microsoft Cognitive Services
description: Torne uma voz costum de qualidade de produção ao preparar um script robusto, contratação de talento de voz boa e gravar profissionalmente.
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 07/5/2018
ms.author: v-jerkin
ms.openlocfilehash: 7b58dc6e9bbfbf69358b30d29d93da6f6d6dc899
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/09/2018
ms.locfileid: "37922578"
---
# <a name="how-to-record-voice-samples-for-a-custom-voice"></a>Como exemplos de registos de voz para uma voz personalizada

Criar uma voz personalizados de alta qualidade de produção do zero não é uma empreitada casual. O componente central de uma voz personalizada é uma grande coleção de exemplos de áudio de voz humana. É fundamental que estas gravações de áudio ser de alta qualidade. Escolha o talento de voz com experiência fazer esse tipo de gravações e tê-los registado por um engenheiro de gravação competente com equipamento profissional.

Antes de poder fazer essas gravações, no entanto, é necessário um script: as palavras que serão dito pelo seu talento de voz para criar os exemplos de áudio. Para obter melhores resultados, o script tem de ter boa cobertura fonética e variedade suficiente para preparar o modelo de voz personalizada.

Muitos ingredientes entram numa boa voz personalizada. Este guia serve como uma introdução ao processo, com foco particular em problemas que tem probabilidade de encontrar.

> [!TIP]
> Para obter os resultados de qualidade mais altos, considere como utilizar o Microsoft para ajudar a desenvolver a sua voz personalizada. A Microsoft tem ampla experiência em produzir vozes de alta qualidade para seus próprio produtos, incluindo o Cortana e o Office.

## <a name="voice-recording-roles"></a>Funções de gravação de voz

Existem quatro funções básicas num projeto de gravação de voz personalizada.

Função|Objetivo
-|-
Talentos de voz        |A pessoa cujo voz formará a base de voz personalizada.
Engenheiro de gravação  |A pessoa que supervisiona os aspectos técnicos da gravação e opera o equipamento de gravação de mensagens em fila.
Diretor            |Prepara o script e coaches desempenho o talento de voz.
Editor              |Finaliza os arquivos de áudio e prepara-os para carregamento para o portal de voz personalizada.

Um indivíduo pode preencher a mais de uma função. Este guia assume que o que irá ser principalmente a preencher a função de diretor e contratação de talento e engenheiro de gravação de voz. Há algumas informações sobre a função de engenheiro de gravação no caso de pretender disponibilizar as gravações por conta própria.

## <a name="choosing-voice-talent"></a>Escolher o talento de voz

Atores com experiência no trabalho de caráter voiceover ou voz certifique talentos de voz personalizada boa. Muitas vezes, pode encontrar talento adequado entre announcers e newsreaders.

Escolha o talento de voz cujo natural de voz, como. É possível criar vozes exclusivo "character", mas é muito mais difícil para a maioria dos talentos de executá-los de forma consistente e o esforço pode fazer com que a sobrecarga de voz.

> [!TIP]
> Em geral, evitar o uso de vozes reconhecíveis para criar uma voz personalizada, a menos que, certamente, seu objetivo é produzir uma voz de celebridade. Vozes de menos conhecidos são, normalmente, menos distração aos utilizadores.

O único fator mais importante para a escolha de talentos de voz é a consistência. Idealmente, todas as gravações devem parecer pareçam feitas no mesmo dia na mesma sala. Pode abordar esse ideal através de práticas de gravação de bons e engenharia. 

Seu talento de voz é a outra metade da equação. Ele deve poder falar com taxa consistente, nível de volume, pitch e tom. Limpar diction é essencial. Seu talento também tem de ser capaz de controlar rigorosamente seu variação pitch emocional efeito e mannerisms de voz.

Gravação de exemplos de voz personalizada pode ser mais fatiguing que outros tipos de trabalho de voz. A maioria dos talentos de voz podem gravar para dois ou três horas por dia. Limite sessões para três ou quatro uma semana, com um dia desativar entre se possível.

Gravações efetuadas para um modelo de voz devem ser emocionalmente neutras. Ou seja, uma expressão de sua decisão não deve ser lidos de forma a sua decisão. Tentarei pode ser adicionado para a fala sintetizada mais tarde. Trabalhar com o seu talento de voz para desenvolver uma "pessoa" que define o tom geral som e emocional de voz personalizada. No processo, vai identificar quais "neutral" parece ser para essa pessoa.

Uma pessoa pode ter, por exemplo, uma personalidade upbeat naturalmente. Portanto, a voz pode trazer uma nota de optimism, mesmo quando falando neutrally. No entanto, uma característica de personalidade desse tipo deve ser sutil e consistente. Ouça a alguns vozes existentes para ter uma idéia de que o alvo é.

### <a name="legalities"></a>Serem

Normalmente, desejará ter as gravações de voz que fizer. Seu talento de voz deve ser acessível a um contrato de trabalho de contratação para o projeto.

## <a name="creating-a-script"></a>Criar um script

O ponto de partida de qualquer sessão de gravação de voz personalizada é o script, que contém as expressões a ser falado por seu talento de voz. (O termo "expressões com" abrange as sentenças completas e frases mais curtos.)

As expressões no seu script podem vir de qualquer lugar: fiction, não fiction, transcrições de seus discursos, notícias de relatórios e tudo o resto disponíveis no formulário de impressos. Se pretender certificar-se de que sua voz faz bem em tipos específicos de palavras (por exemplo, a terminologia médica ou gíria, programação), pode querer incluir frases de investigadores papers ou documentos técnicos. (No entanto, consulte [serem](#legalities) abaixo.) Também pode escrever seu próprio texto.

Suas expressões com não precisa provenientes da mesma origem ou o mesmo tipo de origem. Eles nem precisa ter algo a ver com uns dos outros. No entanto, se quiser utilizar definir frases (por exemplo, "iniciaram sessão com êxito") na sua aplicação de voz, certifique-se para incluí-los no seu script. Este irá dar a sua voz personalizada uma chance maior de pronouncing essas frases bem. E se deve optar por utilizar uma gravação em vez de fala sintetizada, já terá-na voz mesmo como sua fala sintetizada.

Enquanto a consistência é fundamental na escolha de talentos de voz, a variedade é a marca de um script boa. O script deve incluir muitas diferentes palavras e frases com uma variedade de comprimentos de sentença, estruturas e reagem. Cada som no idioma deve ser representada várias vezes e em vários contextos (chamado *cobertura fonética).* 

Além disso, o texto deve incorporar todas as formas que um determinado som podem ser representados por escrito e colocar cada som em diferentes locais nas frases. Frases declarativas e perguntas devem ser incluídas e ler com intonation apropriado.

É difícil escrever um script que fornece *suficiente* dados para permitir ao portal de voz personalizada criar uma voz boa. Na prática, a forma mais simples de fazer um script que alcança cobertura fonética robusta é incluir um grande número de amostras. Vozes de padrão da Microsoft foram criados a partir de dezenas de milhares de expressões. Deve estar preparado para gravar alguns a vários milhares discursos para criar uma voz personalizada de qualidade de produção.

Verifique o script cuidadosamente quanto a erros. Se possível, ter alguém verificá-lo demasiado. Ao executar o script com o seu talento, provavelmente irá capturar alguns erros mais.

### <a name="script-format"></a>Formato de script

Pode escrever o seu script no Microsoft Word. O script é para utilização durante a sessão de gravação, para que possa definir a segurança de qualquer forma, que encontrar fácil trabalhar com. Crie o ficheiro de texto necessário pelo portal de voz personalizada em separado.

Um formato de script básico inclui três colunas:

* O número de expressão, começando em 1. Numeração irá facilitar para todos os utilizadores no studio para fazer referência a uma expressão específica ("Vamos tentar número 356 novamente"). Pode usar o parágrafo do Word numeração funcionalidade numerar as linhas da tabela automaticamente.
* Uma coluna em branco, onde irá escrever o código de tempo de cada ocorrência de pronunciação: a hora em que descobrirá que é no arquivo de áudio gravado.
* O texto da expressão em si.

![Script de exemplo](media/custom-voice/script.png)

Deixe espaço suficiente após cada linha escrever notas. Certifique-se de que nenhuma expressão é dividida entre páginas. Número de páginas e imprimir o seu script no lado do documento.

Três cópias do script de impressão: um para o talento, um para o engenheiro e outro para o diretor (). Utilizar um clip de papel em vez de staples: um artista de voz experientes irá separar as páginas para evitar agitando como as páginas são ativadas.

### <a name="legalities"></a>Serem

Nos termos da legislação dos direitos autorais, leitura de um ator de texto protegido por direitos autorais pode ser um para o qual o autor do trabalho deve ser compensado de desempenho. Este desempenho não possa ser reconhecido no produto final, a voz personalizada. Mesmo assim, não é bem estabelecida legalidade da utilização de um trabalho protegido por direitos autorais para esta finalidade. A Microsoft não é possível fornecer aconselhamento legal sobre esse problema; consulte o seu Conselho.

Felizmente, é fácil evitar esses problemas inteiramente. Existem várias fontes de texto que pode utilizar sem licença ou permissão.

|||
|-|-|
|[Corpus CMU Arctic](http://festvox.org/cmu_arctic/)|Aproximadamente 1100 frases selecionadas a partir de fora de copyright funciona especificamente para uso em projetos de síntese de fala. Um ponto de partida excelente.|
|Já não funciona<br>autorais|Normalmente funciona publicado antes 1923. Para inglês, o Project Gutenberg oferece dezenas de milhares de tal funciona. Poderá pretender focar-se no mais recente funciona, como o idioma será mais de perto como inglês modernos.|
|Governo&nbsp;funciona|Funciona criada pelo Governo dos Estados Unidos não é protegidos por direitos autorais nos Estados Unidos, embora o Governo poderá solicitar copyright em outros países.|
|Domínio público|Funciona para os quais direitos de autor tenham sido explicitamente isenta ou que tem sido dedicada para o domínio público.|
|Funciona permissively licenciado|Funciona distribuída sob uma licença, como Creative Commons ou a GNU documentação uma licença gratuita. Wikipedia utiliza o GFDL. Algumas licenças, no entanto, podem impor restrições sobre o desempenho do conteúdo licenciado que possam afetar a criação de um modelo de voz personalizadas, por isso, leia a licença com atenção.|

## <a name="recording-your-script"></a>Gravar o seu script

De preferência, deve ecord seu script num estúdio de profissional gravar especializada em trabalho voiceover. Eles terão um stand de gravação, os equipamentos certo para a tarefa e as pessoas certas para trabalhar com ele. Vale a pena não tentados na gravação.

Discuta o seu projeto com o engenheiro de gravação do studio e ouça seu Conselho. A gravação deve ter pouco ou nenhum compressão de intervalo dinâmico (máximo de 4:1). É fundamental que o áudio têm volume consistente e uma alta taxa de sinal e ruído, e está livre de sons indesejados.

### <a name="doing-it-yourself"></a>Fazê-lo mesmo

Se quiser fazer a gravação por conta própria, em vez de entrar num estúdio de gravação, aqui está uma cartilha curta. Graças ao aumento de gravação doméstica e podcasts, é mais fácil do que nunca encontrar conselhos de gravação de bons e recursos online.

Seu estande"gravação" deve ser uma única sala sem eco perceptível ou "tom sala" e que é o mais silencioso e soundproof quanto possível. Drapes nas paredes podem ser utilizados para reduzir o eco e neutralizá ou "deaden" o som da sala.

Use um microfone de condenser ("mic") de alta qualidade studio destinado a gravação de voz. Sennheiser AKG e até mesmo mais recente microfones de Zoom podem produzem bons resultados. Pode comprar o mic ou alugar um a partir de uma empresa de aluguel audiovisuais local. Procure um com uma interface USB. Este tipo de mic convenientemente combina o elemento de microfone, preamp e conversor analógico para o digital num pacote, simplificando a conexão.

Também pode usar um microfone analógico. Muitos alugueres casas oferecem microfones "capacidade", que são renomados para respetiva caractere de voz. Tenha em atenção que profissionais engrenagem analógica utiliza equilibradas XLR conectores, em vez de 1/4" conectar utilizadas em equipamentos de consumidor. Se for analógica, também terá uma preamp e uma interface de áudio do computador com estes conectores.

Instalar o microfone num site ou boom e utilize um filtro de pop-frot do microfone para eliminar a interferência dos sons "plosive" como "p" e "b". Alguns microfones são fornecidos com uma montagem de suspensão os isola da vibrations em espera, o que é útil.

Os talentos de voz devem manter-se a uma distância consistente do microfone. Utilize a banda no chão para marcar onde deve funcionar. Se prefere o talento sentar-se, tomar cuidado especial para monitorizar a distância de mic e evitar o ruído de cadeira.

Utilize um para manter o script. Evite angling a espera para que ela pode refletir o som no microfone.

A pessoa responsável, na verdade, a gravação — o engenheiro — deve estar num ambiente separado de talento, com alguma forma para comunicar com o talento no estande da gravação (um "circuito de talkback").

A gravação deve conter como ruído pequeno quanto possível, com o objetivo de um rácio de sinal e ruído db 80 ou superior.

Ouça diretamente a uma gravação de silêncio no seu "portagens," descobrir onde é proveniente de qualquer ruído e eliminar a causa. Origens comuns de ruído são eventos de ar, fluorescente ballasts leves, o tráfego nas proximidades de estradas e os nossos fãs de equipamentos (até mesmo bloco de notas PCs poderá ter de fãs). Microfones e cabos podem pegar elétrica interferência dos próximos conectando de AC, normalmente, um hum ou o buzz.

> [!TIP]
> Em alguns casos, poderá usar um equalizer ou um software de redução de ruído Plug-in para o ajudar a remover barulho do seu gravações, embora é sempre melhor interrompê-lo na origem sempre que possível.

Níveis devem ser definidos, para que a maior parte do intervalo dinâmico disponível de gravação de digital é utilizado sem overdriving em distorção. Isso significa que voz alta, mas não tão voz alta que distorçam o áudio. Segue-se um exemplo de uma forma de onda de uma gravação de bons.

![gravação de boa forma de onda](media/custom-voice/good-recording.png)

Pode ver que a maior parte do intervalo (altura) é utilizado, mas os mais elevados picos de sinal de não chegam a parte superior ou inferior da tela. Também pode ver que o silêncio na gravação aproxima-se uma linha horizontal dinâmico, que indica um andar de baixo de ruído. Esta gravação tem o intervalo aceitável de dinâmico e a proporção de sinal e ruído.

Registo diretamente no computador usando uma interface de áudio de alta qualidade ou uma porta USB, dependendo do tipo de mic está a utilizar. Simplificar a cadeia de áudio: mic, preamp, interface de áudio, computador. Ambos [ávido ferramentas Pro](http://www.avid.com/en/pro-tools) e [Adobe Audition](https://www.adobe.com/products/audition.html) podem ser licenciados mensalmente um custo razoável. Se o orçamento é extremamente curto, experimente gratuitamente [Audacity](https://www.audacityteam.org/).

Registe 44.1 KHz 16 bits monophonic (CD qualidade) ou melhor. Atual-de-modernas são 48 KHz 24 bits, se seu equipamento de oferecer suporte a ele. Será downsample o áudio a 16 KHz 16 bits antes de enviá-lo para o portal de voz personalizada. Ainda assim, vale a pena ter uma alta qualidade original gravar no caso de edições são necessários.

O ideal é que pessoas diferentes servir nas funções de diretor, engenheiro e talento. Não tente fazê-lo a todos os sozinho! Numa situação de emergência, o diretor e o engenheiro podem ser uma única pessoa.

### <a name="before-the-session"></a>Antes da sessão

Para evitar perder tempo studio, execute o script com o seu talento de voz antes da sessão de gravação. Como o talento de voz se torna familiar com o texto, pode clarificar a pronúncia de quaisquer palavras familiarizadas.

> [!NOTE]
> A maioria dos estúdios de gravação oferecem apresentar eletrónica de scripts no stand a gravação. Neste caso, digite suas anotações de orientação sobre diretamente no documento o script. Continuará QUERENDO uma cópia do documento para tomar notas durante a sessão, no entanto. A maioria dos engenheiros desejará um hardcopy demasiado. Nesta fase, pode editar pequeno sons indesejável que tenha perdido durante a gravação — desde que eles não se sobrepõem a qualquer voz real.

Se não é possível corrigir um arquivo, removê-lo do seu conjunto de dados totalmente, fazer uma observação que tenha feito. Downsample cada ficheiro 16 KHz e de 16 bits antes de guardar e, se de que registou no alto, remova o segundo canal. Guarde a cada ficheiro no formato WAV. Arquive o original gravar num local seguro, caso seja necessário voltar a ele mais tarde.

Preserva o seu script e notas, demasiado. Está pronto para carregar as gravações e criar a sua voz personalizada! Sons não devem ser omitidos ou em conjunto, o que é comum na conversão de voz casual, slurred *, a menos que tenha sido escritos dessa forma no script.*

|Texto escrito|Pronúncia casual indesejada|
|-|-|
|nunca irão lhe para dar cópia de segurança|nunca irão lhe para dar cópia de segurança|
|Existem quatro luzes|lá está quatro luzes|
|como é a meteorologia hoje em dia|como é th "sobreviver aos hoje mesmo|
|meu pequeno pony|meu lil' pony|

Deve talento *não* adicionar interrupções distintas entre as palavras. A frase ainda deve fluir naturalmente, mesmo durante o soar formal um pouco. Essa distinção bem pode demorar alguns prática de solucionar.

### <a name="the-recording-session"></a>A sessão de gravação

Criar uma referência de gravação no início de sessão de uma expressão comum e reproduzi-lo para o talento de voz regularmente para os ajudar a manter o seu desempenho consistente. O engenheiro pode utilizá-lo como uma referência para níveis e a consistência geral de som. Isto é especialmente importante quando a retoma gravação, após uma interrupção ou em outro dia.

Orientá-seu talento para tirar um respire fundo e colocar em pausa por um momento antes de cada ocorrência de pronunciação. Registe a alguns segundos de silêncio entre expressões com. Ritmo de processamento deve ser consistente; um metronome jogado por meio de auscultadores o talento pode ser útil se estiver a ter problemas. Palavras devem ser pronuncia-se da mesma forma cada vez que aparecem.

Registe um bom cinco segundos, de silêncio antes da gravação de primeira para capturar o "tom sala." Isto ajuda ao portal de voz personalizada compensar qualquer ruído restante nas gravações.

> [!TIP]
> Tudo que realmente precisa de registar é o talento de voz, para que não há problema em fazer uma gravação (canal de único) monophonic de apenas linhas. No entanto, se gravar no alto, pode utilizar o segundo canal para registrar o chatter numa sala de controlo. Muitas vezes é útil fazer referência a este mais tarde. Remova este Roteiro da versão carregada para o portal de voz personalizada.

Ouça com atenção, utilizar auscultadores, ao desempenho o talento de voz. Está à procura de diction boa mas natural, pronúncia correta e a falta de sons indesejados. Não hesite em fazer o seu talento para registrar novamente uma expressão que não cumpram estas normas. 

> [!TIP] 
> Ao gravar um grande volume de expressões, perda de uma única expressão não afetem a voz resultante de qualquer forma considerável. Por isso, poderá ser mais vantajoso simplesmente observar qualquer expressão que têm problemas, excluí-los a partir do seu conjunto de dados e veja como a voz personalizada acontece. Pode sempre voltar ao studio e registe os exemplos em falta mais tarde.

A maioria dos estúdios de ter um grande vídeo "código de tempo" que indica a hora atual na gravação. Tome nota do tempo no seu script para cada ocorrência de pronunciação. Peça o engenheiro se eles podem marcar cada ocorrência de pronunciação a gravação metadados ou indicação folha também.

Tire quebras regulares para permitir que o seu talento de voz, manter a sua voz em bom estado. Forneça o talento algo a bebida para manter seus throat da obtenção de dry.

### <a name="after-the-session"></a>Após a sessão

Os estúdios de gravação modernos executam em computadores. No final da sessão, em seguida, recebe uma ou mais arquivos de áudio, não uma banda. Estes ficheiros serão provavelmente WAV ou AIFF formato na qualidade de CD (44.1 KHz 16-bit) ou melhor. 48 kHz 24 bits é comum e desejável. Em geral, as taxas de amostragem mais elevadas, por exemplo, 96 KHz, não são necessários.

O studio provavelmente irá fornecer um ou mais arquivos de áudio que contém expressões com muitos. Para carregar as gravações para o portal de voz personalizada, cada expressão tem de ser em seu próprio arquivo. O engenheiro de gravação pode ter inserido um marcador no arquivo (ou fornecida uma lista de indicação separado) para indicar onde começa cada ocorrência de pronunciação. Estes metadados podem ser utilizado para extrair as expressões.

Precisará percorrer a gravação completa e um arquivo WAV para cada ocorrência de pronunciação de fazer. Utilizar suas anotações para localizar as expressões exatas de que pretende e utiliza um som, tais como o utilitário de edição [ávido ferramentas profissionais](http://www.avid.com/en/pro-tools), [Adobe Audition](https://www.adobe.com/products/audition.html), ou o software gratuito [Audacity](https://www.audacityteam.org/) para copiar cada um para um novo ficheiro.

Deixe apenas cerca de 0,2 segundos de silêncio no início e no final de cada clip, exceto o primeiro. Esse arquivo deve começar com um completo cinco segundos de silêncio. Não utilize o editor de áudio a partes de silenciosa "zero de saída" do ficheiro. Incluindo o tom"espaço" ajudará a voz personalizada algoritmos compensar nenhum barulho de fundo residual.

Ouça a cada ficheiro com cuidado. Nesta fase, pode editar pequeno sons indesejável que tenha perdido durante a gravação — desde que eles não se sobrepõem a qualquer voz real. Se não é possível corrigir um arquivo, removê-lo do seu conjunto de dados totalmente, fazer uma observação que tenha feito.

Downsample cada ficheiro 16 KHz e de 16 bits antes de guardar e, se de que registou no alto, remova o segundo canal. Guarde a cada ficheiro no formato WAV.

Arquive o original gravar num local seguro, caso seja necessário voltar a ele mais tarde. Preserva o seu script e notas, demasiado.

## <a name="next-steps"></a>Passos Seguintes

Está pronto para carregar as gravações e criar a sua voz personalizada!

> [!div class="nextstepaction"]
> [Criar tipos de voz personalizada](how-to-customize-voice-font.md)
