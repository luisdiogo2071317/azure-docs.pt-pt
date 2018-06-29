---
title: Referência de domínio prebuilt - Azure | Microsoft Docs
titleSuffix: Azure
description: Referência para os domínios prebuilt, que são prebuilt coleções de entidades do idioma compreender inteligente serviços (LUIS) e pendentes.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/20/2018
ms.author: v-geberr
ms.openlocfilehash: 14c53bd25913922a0cd7cc438ad0fbe8b4663dd1
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37061969"
---
# <a name="prebuilt-domain-reference"></a>Referência de domínio pré-concebida
Esta referência fornece informações sobre o [domínios prebuilt](luis-how-to-use-prebuilt-domains.md), que são prebuilt coleções de entidades que oferece LUIS e pendentes.

[Domínios personalizados](create-new-app.md), por outro lado, iniciar sem pendentes e modelos. Pode adicionar qualquer pendentes domínio prebuilt e entidades para um modelo personalizado.

## <a name="list-of-prebuilt-domains"></a>Lista de domínios prebuilt
LUIS oferece 20 domínios prebuilt. 

| Domínio prebuilt | Descrição | Idiomas Suportados |
| ---------------- |-----------------------|:------:|
| Calendário | O domínio de calendário fornece intenção e entidades para adicionar, eliminar, ou uma sessão de edição, a verificação de disponibilidade de participantes e localizar informações sobre um evento de calendário.| pt-PT<br/> zh-CN |
| Câmara | O domínio câmara fornece pendentes e entidades para tirar fotografias, vídeos de gravação e as vídeo de difusão para uma aplicação.| pt-PT |
| Comunicação | Envio de mensagens e efetuar chamadas telefónicas.| pt-PT <br/> zh-CN |
| Entretenimento  | Processamento de consultas relacionadas com a música, filmes e TV.| pt-PT |
| Eventos | Booking permissões para concerts, festivals, jogos desporto e comedy mostra.| pt-PT |
| Adequação | Processar os pedidos relacionados com atividades de adequação de controlo.| pt-PT |
| Jogos | Processar os pedidos relacionados com uma parte num jogos multijogador jogo.| pt-PT |
| HomeAutomation | Controlar dispositivos raiz inteligentes como lights e aplicações.| pt-PT<br/> zh-CN |
| MovieTickets | Booking bilhetes de filmes em theater um filme.| pt-PT |
| Música | Reprodução de música num leitor de música.| pt-PT<br/> zh-CN |
| Nota | O domínio de nota fornece entidades relacionadas com a criação, edição e localizar notas e pendentes.| pt-PT<br/> zh-CN |
| OnDevice | O domínio OnDevice fornece pendentes e entidades relacionadas com a controlar o dispositivo.| pt-PT<br/> zh-CN |
| Locais  | Processamento de consultas relacionadas com locais como empresas, instituições, restaurants, espaços públicos e endereços.| pt-PT<br/> zh-CN |
| Lembrete | Processar os pedidos relacionados com a criação, edição e localizar lembretes.| pt-PT<br/> zh-CN |
| RestaurantReservation | Processamento de pedidos para gerir reservas restaurante.| pt-PT<br/> zh-CN |
| Taxi | A processar bookings para um taxi.| pt-PT<br/> zh-CN |
| Traduzir | Traduzir texto para um idioma de destino.| pt-PT<br/> zh-CN |
| TV | Controlar TVs.| pt-PT |
| Serviços Públicos  | Pedidos de processamento que são comuns em vários domínios, como "ajuda", "Repetir", "começar de novo."| pt-PT |
| Meteorologia | Obter relatórios meteorologia e previsões.| pt-PT<br/> zh-CN |
| Web | Navegar para um Web site.| pt-PT<br/> zh-CN |

Para mais detalhes sobre cada domínio, consulte as secções que se seguem.

## <a name="calendar"></a>Calendário 

O domínio de calendário fornece entidades relacionadas com entradas de calendário e pendentes. Os pendentes de calendário incluem adicionar, eliminar ou editar uma sessão, a verificação de disponibilidade e localizar informações sobre uma entrada de calendário ou a sessão.

### <a name="intents"></a>Intenções
| Nome da intenção | Descrição | Exemplos |
| ---------------- |-----------------------|----|
| Adicionar | Adicione um novo item de uma única para o calendário.| Se uma sessão com Lisa em 2 pm no Domingo <br/><br/>Pretender agendar uma reunião<br/><br/>Preciso de configurar uma reunião|
| CheckAvailability | Localize a disponibilidade de uma sessão ou reunião num calendário do utilizador ou o calendário de outra pessoa.| Quando está disponível para satisfazer Jim? <br/><br/>Mostrar quando Camila está disponível amanhã<br/><br/>É gratuito Chris no Sábado?|
| Eliminar | O pedido para eliminar uma entrada de calendário.| Cancele a minha sessão com Camila. <br/><br/>Eliminar o meu reunião 09: 00<br/>|
| Editar | O pedido para alterar uma reunião existente ou a entrada de calendário.| Mova o meu reunião 09: 00 para 10 am.<br/><br/>Pretende atualizar a minha agenda.<br/><br/>Reschdule meu reunião com Ryan.|
| Localizar | Apresente o meu calendário semanal.| Localizar o dentist rever sessão. <br/><br/>Mostrar o meu calendário<br/>|

### <a name="entities"></a>Entidades
| Nome da entidade | Descrição | Exemplos |
| ---------------- |-----------------------|----|
| Localização | Localização do item de calendário, cumprindo ou sessão. Os endereços, cidades e regiões são bons exemplos de localizações.| 209 Nashville Gym <br/><br/>Próxima de pancake 897<br/><br/>Garage|
| Assunto | O título de uma reunião ou a sessão.| Sessão do Dentist <br/><br/>Lunch com Leonor<br/><br/>Sessão do Doctor|

## <a name="camera"></a>Câmara 
O domínio câmara fornece entidades relacionadas com a utilização de uma câmara e pendentes. Os pendentes abrangem capturar uma fotografia, selfie, captura de ecrã ou vídeo e difundir as vídeo para uma aplicação.

### <a name="intents"></a>Intenções
| Nome da intenção | Descrição | Exemplos |
| ---------------- |-----------------------|----|
| CapturePhoto| Capture uma fotografia.| Tirar uma fotografia<br/><br/>captura|
| CaptureScreenshot | Capture uma captura de ecrã.| Demorar captura de ecrã.<br/><br/>captura de ecrã.|
| CaptureSelfie | Capture uma selfie.| Tirar um selfie <br/><br/>tirar uma fotografia-me |
| CaptureVideo | Inicie a gravação de vídeo.| Iniciar gravação <br/><br/>Iniciar a gravação|
| StartBroadcasting| Inicie as vídeo de difusão.| Iniciar a difusão para Facebook|
| StopBroadcasting| Pare as vídeo de difusão.| Parar difusão|
| StopVideoRecording| Pare a gravação um vídeo.| Que é suficiente<br/><br/>parar a gravação|

### <a name="entities"></a>Entidades
| Nome da entidade | Descrição | Exemplos |
| ---------------- |-----------------------|----|
| AppName | O nome de difusão de vídeo para uma aplicação.| OneNote<br/><br/>Facebook<br/><br/>Skype|


## <a name="communication"></a>Comunicação 
O domínio de comunicação fornece pendentes e entidades relacionados com correio eletrónico, mensagens e chamadas telefónicas.

### <a name="intents"></a>Intenções
| Nome da intenção | Descrição | Exemplos |
| ---------------- |-----------------------|----|
| AddContact| Adicione um novo contacto a lista de contactos do utilizador.|Adicionar o novo contacto <br/><br/>Guardar este número e coloque o nome como Camila|
| AddMore| Adicione mais um e-mail ou o texto, como parte de uma composição step-wise de texto ou e-mail.|Adicionar mais para texto <br/><br/>Adicionar mais para o corpo do correio eletrónico|
| Resposta| Responda a uma chamada telefónica.|a chamada de resposta <br/><br/>Escolha se a cópia de segurança|
| AssignContactNickname| Atribua uma alcunha para um contacto.|Alterar Isaac ao teu <br/>Editar alcunha de Jim<br/>Adicionar alcunha para Patti Owens|
| CallVoiceMail| Ligar para o correio de voz do utilizador.|Ligar-me à minha caixa de correio de voz <br/>correio de voz<br/>chamada de correio de voz|
| CheckIMStatus| Verifique o estado de um contacto Skype.|É o estado online de Jim definido como ausente? <br/>Camila está disponível para falar no chat com?|
| Confirmar| Certifique-se uma ação.|Sim<br/>Okay<br/>Todos os direito<br/>Confirmo que pretender enviar este e-mail.<br/>|
| acesso telefónico| Efetuar uma chamada telefónica.|Chamada Jim<br/>Marcar 311<br/>|
| FindContact| Localize as informações de contacto por nome.|Localizar o número de Camila<br/>Mostrar o número de Camila<br/>|
| FindSpeedDial| Determinar o número de speedial que um número de telefone está definido para e vice-versa.|O que é o meu número de acesso telefónico 5?<br/>É necessário velocidade marcar conjunto?<br/>O que é o número de marcação para 941-5555-333?|
| GetForwardingsStatus| Obter o estado atual da chamada de reencaminhamento.|A minha reencaminhamento chamada está ativado?<br/>Informar-me se a minha estado de chamada é ligado ou desligado<br/>|
| GoBack| Volte ao passo anterior.|Volte ao twitter<br/>Voltar atrás de um passo<br/>Voltar atrás|
| Ignorar| Ignore uma chamada.|Não responder<br/>Ignorar chamada|
| IgnoreWithMessage| Ignorar uma chamada e responder com texto em vez disso.|Não responder nesse chamada mas em vez disso, a enviar uma mensagem.<br/>Ignorar e enviar um texto novamente.|
| PressKey| Prima uma botão ou um número no teclado.|Estrela de marcação.<br/>Prima 1 2 3.|
| ReadAloud| Ler uma mensagem ou do e-mail ao utilizador.|Leia o texto.<br/>O que ela diga na mensagem?|
| TurnForwardingOff| Efetuar uma chamada telefónica.|<br/><br/>|
| Redial| Redial ou chamar novamente um número.|Redial.<br/>Redial meu última chamada.|
| Rejeitar| Rejeite uma chamada.|Rejeitar chamada<br/>Não pode responder agora<br/>Não está disponível de momento e irá chamar novamente mais tarde.|
| SendEmail| Envie uma mensagem de e-mail. Este objetivo aplica-se ao e-mail, mas não as mensagens de texto.|Mensagem de e-mail para Mike Waters: Mike, que foi splendid dinner última semana.<br/>Enviar um e-mail para Bernardo<br/>|
| SendMessage| Envie uma mensagem de texto ou uma mensagem instantânea.|Enviar texto para Chris e Camila|
| SetSpeedDial| Defina um atalho de marcação de velocidade para o número de telefone de um contacto.|Defina a marcação de velocidade um para Camila.<br/>Configure a marcação de velocidade para mom.|
| ShowNext| Consulte o seguinte item, por exemplo, uma lista de mensagens de texto ou mensagens de correio eletrónico.|Mostra um seguinte.<br/>Aceda à página seguinte.|
| ShowPrevious| Consulte o item anterior, por exemplo, uma lista de mensagens de texto ou mensagens de correio eletrónico.|Mostra anterior.<br/>Anterior<br/>Aceda à anterior.|
| StartOver| Iniciar o sistema através de ou iniciar uma sessão de novo.|Recomeçar<br/>Nova sessão<br/>restart|
| TurnForwardingOff| Desative o reencaminhamento de chamada.|Pare de reencaminhar minhas chamadas<br/>desactivar a chamada de reencaminhamento|
| TurnForwardingOn| Desative o telefone de orador.|Reencaminhamento minhas chamadas para 3333<br/>Comutador no reencaminhamento de chamada para 3333|
| TurnSpeakerOff| Desative o telefone de orador.|Demorar-me desativar orador.<br/>Desative speakerphone.<br/>|
| TurnSpeakerOn| Ative o telefone de orador.|Modo de Speakerphone.<br/>Coloque speakerphone no.<br/>|

### <a name="entities"></a>Entidades
| Nome da entidade | Descrição | Exemplos |
| ---------------- |-----------------------|----|
| AudioDeviceType | Tipo de dispositivo de áudio (orador, headset, microfone, etc.).| Orador<br/>Mãos livres<br/>Bluetooth|
| Categoria | A categoria de uma mensagem ou e-mail.| Importante<br/>Alta prioridade|
| ContactAttribute | Um atributo de contacto que do utilizador inquires sobre.| Aniversários<br/>Endereço<br/>Número de telefone|
| ContactName | O nome de um destinatário de contacto ou a mensagem.| Camila<br/>Jim<br/>Chris|
| EmailSubject | O texto utilizado como a linha de assunto para mensagens de correio eletrónico.| RE: interessantes bloco|
| Linha | A linha do utilizador pretende utilizar para efetuar uma chamada ou enviar um texto/e-mail.| Linha de trabalho<br/>Célula British<br/>Skype|
| Mensagem | A mensagem será enviada como texto ou e-mail.| Foi excelente que responde hoje. Consulte o artigo é novamente em breve!|
| MessageType | O nome de um destinatário de contacto ou a mensagem.| Texto<br/>Email|
| OrderReference | A posição ordinal ou relativa numa lista, que identifica um item a obter. Por exemplo, "último" ou "recentes" em "Que foi a última mensagem enviado posso?"| Último<br/>Recente|
| SenderName | O nome do remetente.| Patti Owens|

## <a name="entertainment"></a>Entretenimento  
Mostra o entretenimento domínio fornece entidades relacionadas com a pesquisa de filmes, música, jogos e TV e pendentes.

### <a name="intents"></a>Intenções
| Nome da intenção | Descrição | Exemplos |
| ---------------- |-----------------------|----|
| Pesquisa| Mostra a pesquisa de filmes, música, aplicações, jogos e TV.|Procure na loja Halo.<br/>Procure Avatar.|

### <a name="entities"></a>Entidades
| Nome de entidades | Descrição | Exemplos |
| ---------------- |-----------------------|----|
| ContentRating | Classificação como G ou R para filmes de conteúdo de suporte de dados.|Vídeo de menores.<br/>PG classificados.|
| Género | O género de um filme, jogos, aplicações ou song.|Comedies<br/>Dramas<br/>Funny|
| Palavra-chave| Uma palavra-chave de pesquisa genérico especificando um atributo de não existe nas ranhuras de suporte de dados mais específicas.|Soundtracks<br/>Moon River<br/>Amelia Earhart|
| Idioma | Classificação como G ou R para filmes de conteúdo de suporte de dados.|Francês<br/>Português<br/>Coreano|
| MediaFormat | Adicional tipo técnico especial em que está formatado corretamente o suporte de dados.|HD filmes<br/>Filmes 3D<br/>Transferível|
| MediaSource | O arquivo ou marketplace para adquirir o suporte de dados.|Netflix<br/>Prime|
| MediaSubTypes| Tipos de suportes de dados mais pequenos do que filmes e jogos.|Demonstrações<br/>DLC<br/>Trailers|
| Nationality| O país/região onde foi criado um filme, mostrar ou song.|Francês<br/>Alemão<br/>Coreano|
| Pessoa| O ator, director, produtor, músico ou artista preferido associado um filme, aplicações, jogos ou programa de TV.|Madonna<br/>Stanley Kubrick|
| Função| Função reproduzida por uma pessoa na criação do suporte de dados.|Sings<br/>Indicado pelo<br/>Por|
| Cargo| O nome de um filme, aplicações, jogos, de TV ou song.|Amigos<br/>Minecraft|
| Tipo| O formato de tipo ou de suportes de dados de um filme, aplicações, jogos, de TV ou song.|Música<br/>MovieTV <br/>Mostra|
| UserRating| Estrela de utilizador ou thumbs de classificação.|5 estrelas<br/>3 estrelas<br/>4 estrelas|

## <a name="events"></a>Eventos 
O domínio de eventos fornece pendentes e entidades relacionadas com pedidos de suporte de booking eventos como concerts, festivals, jogos desporto e comedy mostra.

### <a name="intents"></a>Intenções
| Nome da intenção | Descrição | Exemplos |
| ---------------- |-----------------------|----|
| No livro| Comprar permissões a um evento.|Gostaria de comprar um pedido de suporte para o symphony este fim de semana.|


### <a name="entities"></a>Entidades
| Nome de entidades | Descrição | Exemplos |
| ---------------- |-----------------------|----|
| Endereço | Localização de evento ou endereço. |Da Palo Alto<br/>300 112th Ave zar <br/> Seattle |
| Nome | O nome de um evento.|Shakespeare no Park|
| PlaceName| O nome de localização de eventos.|Louvre<br/>Opera próxima<br/>Broadway|
| PlaceType | O tipo de localização de evento será retido no.|Cafe<br/>Theatre<br/>Biblioteca|
| Tipo | O tipo de um evento.|Concert<br/>Jogos de desporto|

## <a name="fitness"></a>Adequação 
O domínio de adequação a fornece entidades relacionadas com controlo atividades adequação e pendentes. Os pendentes incluem guardar notas, o restante período ou distância ou guardar resultados de atividade.

### <a name="intents"></a>Intenções
| Nome da intenção | Descrição | Exemplos |
| ---------------- |-----------------------|----|
| AddNote| Adiciona notas suplementares para uma atividade controlada.|Dificuldade de execução era 6/10<br/>Terrain que estou em execução é asphalt<br/>Estou a utilizar uma bicicleta 3 velocidade|
|GetRemaining| Obtém o restante período ou distância para uma atividade.|Quanto tempo até a próxima lap?<br/>Quantos quilómetros são restante no meu executar? Quanto tempo para a divisão?|
| LogActivity| Guardar ou iniciar sessão resultados de atividade foi concluída.|Guardar as minha última execução<br/>Inicie o meu percurso de manhã Sábado<br/>armazenar os meus swim anterior|
| LogWeight| Guardar ou iniciar sessão ponderação atual do utilizador.|Guardar as minha ponderação atual<br/>Inicie o meu ponderação agora<br/>armazenar os meus ponderação corpo atual|

### <a name="entities"></a>Entidades
| Nome de entidades | Descrição | Exemplos |
| ---------------- |-----------------------|----|
| ActivityType | O tipo de atividade para controlar. |Executar<br/>Percurso<br/>Swim<br/>Ciclo de |
| Comida | Um tipo de prato controlar numa aplicação adequação. |Banana<br/>Salmon<br/>Protein abanar|
| MealType| O tipo de meal para controlar a um Estado de funcionamento ou adequação a uma aplicação.|Breakfast<br/>Dinner<br/>Lunch<br/>Supper|
| Medida| Um tipo de valores de tempo, distância ou ponderação, para utilização numa aplicação adequação ou estado de funcionamento.|Kilometers<br/>Quilómetros<br/>Minutos<br/>Kilograms|
| Number | Uma quantidade numérico para utilização numa aplicação adequação ou estado de funcionamento.|19<br/>três<br/>200<br/>um|
| StatType | Um tipo de estatística nos dados agregados, para utilização numa aplicação adequação ou estado de funcionamento.|Soma<br/>Média<br/>Máximo<br/>Mínimo|

## <a name="gaming"></a>Jogos 
O domínio de jogos fornece entidades relacionadas com a gestão de uma parte num jogos multijogador jogo e pendentes.

### <a name="intents"></a>Intenções
| Nome da intenção | Descrição | Exemplos |
| ---------------- |-----------------------|----|
| InviteParty| Convide um contacto para associar uma parte de jogos.|Convidar este leitor para a minha terceiros<br/>São fornecidos para os meus terceiros<br/>Associar o meu clan|
|LeaveParty| Obtém o restante período ou distância para uma atividade.|Sou enviados<br/>Posso estou abandonar o fileparser este terceiros para outra<br/>Posso estou quitting|
| StartParty| Inicie uma parte de jogos um jogos multijogador.|Membro de TI vamos iniciar uma entidade<br/>iniciar uma entidade<br/>deve iniciar uma clan tonight|

### <a name="entities"></a>Entidades
| Nome da entidade | Descrição | Exemplos |
| ---------------- |-----------------------|----|
| Contacto| Um nome de contacto para utilizar um jogos multijogador.|Camila<br/>Jim|


## <a name="homeautomation"></a>HomeAutomation 
O domínio HomeAutomation fornece pendentes e entidades relacionadas com a controlar dispositivos raiz inteligentes como lights e aplicações.

### <a name="intents"></a>Intenções
| Nome da intenção | Descrição | Exemplos |
| ---------------- |-----------------------|----|
| TurnOff| Desativar, fechar ou desbloquear um dispositivo.|Desativar os lights<br/>Parar maker o café<br/>Porta garage fechar|
|TurnOn| Ativar um dispositivo ou defina o dispositivo para a definição específica ou modo.|Ativar o meu maker café<br/>Pode ativar no meu maker café?<br/>Defina o thermostat 72 graus.|


### <a name="entities"></a>Entidades
| Nome da entidade | Descrição | Exemplos |
| ---------------- |-----------------------|----|
| Dispositivo | Um tipo de dispositivo que pode ser ativado ou desativado.|maker café<br/>Thermostat<br/>Lights|
| Operação | O estado a definir do dispositivo.|bloqueio<br/>Abrir<br/>em<br/>desativado|
| Sala | A localização ou o espaço que o dispositivo estiver no.|sala de maior duração<br/>bedroom<br/>cozinha|

## <a name="movietickets"></a>MovieTickets 
O domínio MovieTickets fornece entidades relacionadas com pedidos de suporte de booking para filmes em theater um filme e pendentes.

### <a name="examples"></a>Exemplos
```
Book me two tickets for Captain Omar and the two Musketeers
Cancel tickets
When is Captain Omar showing?
```

### <a name="intents"></a>Intenções
| Nome da intenção | Descrição | Exemplos |
| ---------------- |-----------------------|----|
| No livro | Adquira os pedidos de suporte de filmes.|Livro-me dois bilhetes para Captain Omar e as duas musketeers<br/>Pretendo comprar uma permissão de filmes do amanhã<br/>Pretendo que um pedido de suporte para Captian Omar parte 2 seguinte quarta-feira|
|GetShowTime| Obter o showtime de um filme.|Quando é apresentada Captain Omar?|


### <a name="entities"></a>Entidades
| Nome da entidade | Descrição | Exemplos |
| ---------------- |-----------------------|----|
| Endereço | O endereço de um theater de filmes.|Da Palo Alto<br/>300 112th Ave zar<br/>Seattle|
| MovieTitle | O título de um filme.|Duração do instalador de plataforma<br/>Hunger jogos<br/>Inception|
| PlaceName | O nome de um theater de filmes ou cinema.|Cinema Amir<br/>Alexandria Theatre<br/>Nova Iorque Theater|
| PlaceType | O tipo de localização de que um filme está a ser mostrada no.|cinema<br/>Theater<br/>IMAX cinema|

## <a name="music"></a>Música 
O domínio de música fornece entidades relacionadas com a reprodução de música num leitor de música e pendentes.

### <a name="examples"></a>Exemplos
```
play Beethoven
Increase track volume
Skip to the next song
```

### <a name="intents"></a>Intenções
| Nome da intenção | Descrição | Exemplos |
| ---------------- |-----------------------|----|
| DecreaseVolume | Diminua o volume do dispositivo.|reduzir o volume de registo<br/>volume para baixo|
| IncreaseVolume | Aumente o volume do dispositivo.|Aumente o volume de registo<br/>volume de cópia de segurança|
| Desativar som |Mute as músicas playing.|Mute song<br/>Colocar a controlar no desativar som<br/>Música Mute |
| Colocar em pausa | Colocar em pausa as músicas playing.|Colocar em pausa<br/>Música pausa<br/>Controlar pausa|
| PlayMusic | Reproduza música num dispositivo.|reproduzir Kevin Durant<br/>reproduzir Paradise por Coldplay<br/>reproduzir Olá por Adele|
| Repetir |Repita a playing música.|Repetir song<br/>Reproduzir o ganho controlar<br/>Música repetida|
| Retomar | Retome as músicas playing.|Retomar song<br/>Inicie novamente a música<br/>Unpause|
| SkipBack | Efeitos de ignorar um registo.|Avance para o seguinte song<br/>Reproduzir o song seguinte|
| SkipForward |Ignore reencaminhar um registo.|Reproduzir o song anterior<br/>Voltar para o registo anterior |
| Parar | Pare uma ação de reprodução de música. |Pare esta album de desempenho de funções.|
| Unmute | Unmute um dispositivo de reprodução de música.| Unmute.|

### <a name="entities"></a>Entidades
| Nome da entidade | Descrição | Exemplos |
| ---------------- |-----------------------|----|
| ArtistName | O ator, director, produtor, escritor, músico ou artista preferido associado suporte para reproduzir num dispositivo.|Elvis Presley<br/>Taylor Swift<br/>Adele<br/>Mozart|
| Género | O género de música que está a ser solicitada.|Músicas de país<br/>Broadway classics<br/>Reproduzir os meus música classical do período Baroque|

## <a name="note"></a>Nota 
O domínio de nota fornece entidades relacionadas com a criação, edição e localizar notas e pendentes.

### <a name="examples"></a>Exemplos
```
Add to my groceries note lettuce tomato bread coffee
Check off bananas from my grocery list
Remove all items from my vacation list
```

### <a name="intents"></a>Intenções
| Nome da intenção | Descrição | Exemplos |
| ---------------- |-----------------------|----|
| AddToNote | Adicione informações a nota.|Adicionar à minha groceries nota lettuce tomato bread café<br/>Adicionar à minha lista de tarefas<br/>Adicionar cupcakes à minha Wunderlist|
| CheckOffItem | Certifique-se desativar os itens de nota já existente.|Verificação terminada bananas na minha lista grocery<br/>Marcar cheese cake no meu feriado compras lista como feito|
| Limpar | Limpe todos os itens de nota já existente.|Remova todos os itens da minha lista de férias<br/>Limpar todos os meus lista de leitura|
| Confirmar | Certifique-se uma ação de nota.|Pode por mim<br/>sim<br/>Posso estou confirmar manter todos os itens na lista|
| Criar | Crie uma nova nota. | Criar uma lista<br/>Tenha em atenção para lembrar que Jason se encontra no town primeira semana do Maio|
| Eliminar | Elimine uma nota de toda. |Eliminar o meu lista de férias <br/>eliminar o meu nota groceries|
| DeleteNoteItem | Elimine itens de nota já existente.| Eliminar chips do meu lista grocery<br/>Remover o meu escola compras lista pens|
| ReadAloud | Ler uma lista loud terminar.|Leia-me primeiro<br/>Leia-me os detalhes|
| ShowNext | Consulte o seguinte item de uma lista de notas.|Mostrar um seguinte<br/>Página seguinte<br/>Seguinte|

### <a name="entities"></a>Entidades
| Nome da entidade | Descrição | Exemplos |
| ---------------- |-----------------------|----|
| AppName | O nome da aplicação Nota Colocar.|Wunderlist<br/>OneNote|
| ContactName | O nome de um contacto nota.|Camila<br/>Jim<br/>Chris|
| Origem de dados | Localização de notas.|OneDrive<br/>Google docs<br/>o meu computador|
| Tipo de dados | O tipo de ficheiro ou documentos, normalmente associadas a programas de software específico.|Slides<br/>Folha de cálculo<br/>Folha de Cálculo|
| Texto | O texto de uma nota ou lembrete.|Stretch antes o nosso percurso<br/>execução de longos amanhã|
| Cargo | Título de nota.|groceries<br/>pessoas para chamar<br/>tarefas|

## <a name="ondevice"></a>OnDevice 
O domínio OnDevice fornece pendentes e entidades relacionadas com a controlar o dispositivo.

### <a name="examples"></a>Exemplos
```
Close video player
Cancel playback
Can you make the screen brighter?
```

### <a name="intents"></a>Intenções
| Nome da intenção | Descrição | Exemplos |
| ---------------- |-----------------------|----|
| AreYouListening | Pergunte se o dispositivo está a escutar.|Este é no?<br/>são escuta?|
|CloseApplication|Feche a aplicação de dispositivo.|Feche o leitor de vídeo|
|FileBug|Ficheiro de erros no dispositivo.|ficheiro de erros.<br/>-Ficheiro de erros para mim?<br/>Deixar-me comunique este erro|
|GoBack|Pedir para voltar atrás de um passo ou voltar para o passo anterior.|Voltar atrás.<br/>Aceda ao ecrã anterior<br/>Aceda novamente parar à escuta|
|Ajuda| Pedir ajuda.|Ajudar.<br/>Olá<br/>O que fazer?<br/>Preciso de ajuda| 
|LocateDevice|Localize o dispositivo.|Pode localizar o meu telefone<br/>Encontrar iphone do personalizada<br/>Encontrar o meu telefone|
|Início de sessão|Início de sessão a um serviço utilizando o dispositivo.|Início de sessão.<br/>Início de sessão do Facebook<br/>Iniciar sessão LinkedIn|
|Terminar sessão|Inicie sessão fora de um serviço utilizando o dispositivo.|Terminar a sessão meu telefone<br/>Inicie sessão no twitter<br/>Terminar sessão|
|MainMenu|Visualize o menu principal de um dispositivo.|Menu Ver.|
|OpenApplication|Abra uma aplicação no dispositivo.|Abra o alarme.<br/>Ativar câmara<br/>Inicie o calendário|
|OpenSetting|Abra uma definição no dispositivo.|Abra as definições de rede.|
|PairDevice|Par o dispositivo.|Pode ajudar-me no emparelhamento sinal de Bluetooth para o telefone<br/>Ative o bluetooth e associe-portátil<br/>Sinal de Bluetooth par ao meu portátil|
|Desligado | Desative o dispositivo.|Pode encerrar o meu computador<br/>Encerrar<br/>Desativar a minha mobile|
|QueryBattery|Obter informações sobre a vida bateria.|Mostra vida bateria.<br/>O que é o meu estado bateria<br/>Quantidade de bateria restante agora?<br/>Mostrar bateria|
|QueryWifi|Obter informações sobre Wi-Fi.|Obter as informações de Wi-Fi.|
|Reiniciar|Reinicie o dispositivo.|Reinicie.|
|RingDevice| Colocar o dispositivo toque, para encontrá-lo quando ter perdido.|Anel meu telefone.| 
|SetBrightness|Defina o brightness do dispositivo.|Conjunto brightness como médios<br/>Conjunto brightness para alta<br/>Conjunto brightness baixa|
|SetupDevice|Inicie a configuração de dispositivo.|Pretender instalar o programa de configuração do SO<br/>A configuração.<br/>Efetue a configuração para mim|
|ShowAppBar|Mostra uma barra de aplicações.|Mostrar a barra de aplicações<br/>Volte da barra de aplicações<br/>Deixar-me, consulte a barra de aplicações|
|ShowContextMenu|Mostra menu de contexto.|Deixar-me, consulte o menu de contexto<br/>Menu de contexto.<br/>Pode a Mostrar menu de contexto|
|Modo de suspensão|Colocar o dispositivo em modo de suspensão.|Aceda a suspensão<br/>Modo de suspensão<br/>O meu computador suspenso|
|SwitchApplication|Mude a aplicação para utilizar no dispositivo.|Mudar para a minha leitor de suporte de dados.|
|TurnDownBrightness|Ative baixo brightness do dispositivo.|Dim o ecrã.|
|TurnOffSetting|Desative uma definição de dispositivo.|Desativar o Bluetooth<br/>Desativar dados<br/>Desligar bluetooth|
|TurnOnSetting|Ative uma definição de dispositivo.|Ativado <br/> Desativado|
|TurnUpBrightness|Ative segurança brightness do dispositivo.|Pode fazer com que o ecrã brighter?|

### <a name="entities"></a>Entidades
| Nome da entidade | Descrição | Exemplos |
| ---------------- |-----------------------|----|
| AppName | Nome de uma aplicação no dispositivo.|SoundCloud<br/>YouTube|
| BrightnessLevel | Defina o nível de brightness no dispositivo.|Percentagem de uma centena<br/>Cinquenta<br/>40%|
| ContactName | O nome de um contacto no dispositivo.|Paul<br/>Máx. de Marlen|
| DeviceType | O tipo de dispositivo. |Telefone<br/>Kindle<br/>Computador portátil|
| Tipo de suporte | O tipo de suporte de dados processado pelo dispositivo.|Música<br/>Filme<br/>Mostra de TV|
| SettingType | Um tipo de definição ou painel de definições que o utilizador pretende editar.|Wi-Fi<br/>Rede sem fios<br/>Esquema de cores<br/>Centro de notificação|

## <a name="places"></a>Locais  
O domínio de locais fornece pendentes de processamento de consultas relacionadas com locais como empresas, instituição, restaurants, espaços públicos e endereços.

### <a name="examples"></a>Exemplos
```
Save this location to my favorites
How far away is Holiday Inn?
At what time does Safeway close?
```

### <a name="intents"></a>Intenções
| Nome da intenção | Descrição | Exemplos |
| ---------------- |-----------------------|----|
| AddFavoritePlace | Adicione uma localização para a lista de Favoritos do utilizador.|Guardar esta localização para os meus Favoritos<br/>Adicionar este endereço aos meus Favoritos|
|CheckAccident|Pergunte se houver um acidente uma viagem especificada.|Existe um acidente no 880?<br/>Mostrar informações de acidentalmente|
|CheckAreaTraffic|Verifique o tráfego para uma área geral ou autoestrada, não numa rota especificada.|Tráfego em Seattle<br/>O que é o tráfego, como em Seattle?|
|CheckIntoPlace|Certifique-se local através de redes sociais.|Verificar-me Foursquare<br/>Verifique aqui|
|CheckRouteTraffic| Verifique o tráfego de uma rota específica especificado pelo utilizador.|Como é o tráfego para Mashiko?<br/>Mostrar traffice para Kirkland<br/>Como é o tráfego para Seattle?| 
|Confirmar|Certifique-se uma ação de um local.|Confirme os meus reserva restaurante.|
|Sair|Ação a sair de uma tarefa de um local.|Sair.<br/>Fornecer-me indicações de sair|
|FindPlace|Procure um local (empresas, instituição, restaurante, espaço público, endereço).|Onde está a mais próximo biblioteca?<br/>Localizar-me um restaurante italiano boa na vista das regiões montanhosas|
|GetAddress| Peça para o endereço de um local.|Mostrar o endereço do Guu Robson Rua<br/>O que é o endereço do mais próximo Starbucks?| 
|GetDistance|Colocar sobre distância para um local específica.|Como até que ponto ausente é Inn de férias?<br/>como extremidade é a Bellevue quadrado aqui<br/>o que é a distância à Tahoe|
|GetHours|Colocar sobre as horas de sistema operativos para um local.|Em que altura Safeway fechar?<br/>Quais são as horas para a home page depósito?<br/>É aberto o Starbucks?|
|GetMenu|Peça para os itens de menu para um restaurante.|Zucca serve nada vegan?<br/>Novidades no menu à Sizzler<br/>Mostrar menu do Applebee|
|GetPhoneNumber| Solicite ao número de telefone de um local.|O que é o número de telefone a Starbucks mais próximo?<br/>Atribua o número de depósito home page| 
|GetPriceRange| Pede-lhe para o intervalo de preço de um local.|É Zucca cheap?<br/>É o preço de meio Cineplex no Wednesdays?<br/>Qual é o um custo de dinner lobster todo em Sizzler?|
|GetReviews|Pedir revisões de um local.|Mostrar revisões da fábrica de Cheesecase<br/>Leitura Cineplex revê no Yelp|
|GetRoute|Pedir indicações para um local.|Como a guiá-lo para Bellevue quadrado<br/>Mostrar a forma mais curta 8th e 59th a partir daqui<br/>Obter-me indicações para a vista das regiões montanhosas AC|
|GetStarRating|Peça a classificação em estrelas de um local.|Como é classificado Zucca, de acordo com Yelp?<br/>Quantos estrelas tem o Laundry francês?<br/>É aconselhável aquarium no Monterrey?|
|GetTransportationSchedule|Obter a agenda de barramento para um local.|O tempo é a seguinte bus downtown?<br/>Mostrar os barramentos no King County|
|GetTravelTime|Pedir o tempo de levar para um destino especificado.|Quanto tempo necessário para aceder ao frente aqui<br/>O que é o tempo despertar para Denver de SF|
|MakeCall|Efetue uma chamada telefónica para um local.|Chamada mom<br/>Gostaria de colocar uma chamada do Skype para Anna<br/>Chamada Jim|
|MakeReservation|Pedir uma reserva para um restaurante ou outras empresas.|Reservar em Zucca para dois para tonight<br/>Uma tabela para amanhã de livro<br/>Tabela para 3 da Palo Alto no 8|
|MapQuestions|Solicitar informações sobre as direções ou se um viagem especificada passa para um destino.|13 passar downtown?<br/>Pode demorar 880 para Oakland?|
|Classificação|Obter a descrição de classificação de um restaurante ou local.|Quantos estrelas a Inn a Contoso tem?|
|ReadAloud|Ler uma lista de locais loud terminar.|Leia-me primeiro<br/>Leia-me os detalhes|
|SelectItem|Escolha um item de uma lista de opções de um local ou locais.|Escolha a segunda<br/>Selecione o primeiro|
|ShowMap|Mostra um mapa de uma área.|Mostrar um mapa para o segundo<br/>Mostrar mapa<br/>Localizar frente no mapa|
|ShowNext|Mostra o seguinte item de uma série.|Mostrar um seguinte<br/>Ir para a página seguinte|
|ShowPrevious|Mostra item anterior de uma série.|Mostrar anterior<br/>anterior<br/>Aceda ao anterior|
|StartOver|Reinicie a aplicação ou iniciar uma sessão de novo.|Recomeçar<br/>Nova sessão<br/>
restart|
|TakesReservations|Pergunte se um local aceita reservas.|Galeria art aceita reservas<br/>É possível efetuar uma reserva no ambiente a Olive

### <a name="entities"></a>Entidades
| Nome da entidade | Descrição | Exemplos |
| ---------------- |-----------------------|----|
| AbsoluteLocation | A localização ou o endereço de um local.|Da Palo Alto<br/>300 112th Ave zar<br/>Seattle|
| Amenities | As objetivo características/vantagens de um local.|menores eat livres<br/>marítima de são Francisco<br/>parking livre|
| Atmosphere | Atmosphere de um local.|compatível com KID<br/>casuais restaurante<br/>sporty|
| Cuisine | Cuisine de um local. |Mediterranean<br/>Italiano<br/>Britânico|
| DestinationAddress| Uma localização de destino ou um endereço.|Da Palo Alto<br/>300 112th Ave zar<br/>Seattle|
| DestinationPlaceName| O nome de um destino que seja um negócio, restaurante, attraction público ou instituição.|Central park<br/>safeway<br/>walmart|
| DestinationPlaceType | O tipo de destino que seja um negócio local, restaurante, attraction público ou instituição. |Restaurante<br/>Opera<br/>cinema|
| Distância | A distância para um local.|quilómetros 15<br/>quilómetros 5<br/>10 quilómetros ausente|
| MealType | Tipo de meal como breakfast ou lunch. |Breakfast<br/>Dinner<br/>Lunch<br/>Supper|
| OpenStatus | Indica se um local está aberta ou fechada.|Aberto<br/>fechado<br/>abrir|
| PlaceName | O nome de um local.|Fábrica de cheesecake|
| PlaceType | O tipo de um local.|Cafe<br/>Theatre<br/>Biblioteca|
| PreferredRoute | A rota preferencial especificada pelo utilizador. | 101 <br/>202 <br/>Rota 401|
| Produto | O produto oferecido por um local. | Clothes<br/>Câmaras digitais ASR<br/>Peixe raiz | 
| PublicTransportationRoute | O nome da rota transportes pública que o utilizador está a procurar. | Formação corridor Nordeste<br/>Rota de barramento 3 X |
| Classificação | A classificação de um local. | 5 estrelas<br/>3 estrelas<br/>4 estrelas|
| RouteAvoidanceCriteria | Critérios para evitar rotas específicas, como evitando podem ocorrer acidentes que, construções ou tolls | Tolls <br/>Construções<br/>Rota 11|
| ServiceProvided | Este é o serviço fornecido uma empresa ou local como haircut, snow plowing, landscaping. | haircut<br/>mechanic<br/>plumber|
| TransportationCompany | O nome de um fornecedor de transporte.|Amtrak<br/>Acela<br/>Greyhound|
| TransportationType | O tipo de transportes.|Barramento<br/>Preparar<br/>Driving|

## <a name="reminder"></a>Lembrete 
O domínio lembrete fornece pendentes e entidades para criar, editar e localizar lembretes.

### <a name="examples"></a>Exemplos
```
Change my interview to 9 am tomorrow
Remind me to buy milk on my way back home
Can you check if I have a reminder about Christine's birthday?
```

### <a name="intents"></a>Intenções
| Nome da intenção | Descrição | Exemplos |
| ---------------- |-----------------------|----|
| Alterar| Altere um lembrete.|Altere o meu interview para 09: 00 amanhã<br/>Mover o meu lembrete de atribuição para amanhã|
| Criar| Crie um lembrete de novo.|criar um lembrete<br/>Avisar-me comprar milk depois<br/>Pretendo não se esqueça de chamar Rebecca quando estou em casa|
| Eliminar | Elimine um lembrete.|eliminar o meu lembrete de imagem<br/>eliminar este lembrete|
| Localizar | Localize um lembrete.|É necessário um lembrete sobre a minha aniversário da?<br/>Pode verificar se tiver um lembrete sobre aniversário de Christine?|

### <a name="entities"></a>Entidades
| Nome da entidade | Descrição | Exemplos |
| ---------------- |-----------------------|----|
| Texto | A descrição de um lembrete do texto.|recolherá dry limpeza<br/>remover o meu carro no Centro de serviço|

## <a name="restaurantreservation"></a>RestaurantReservation 
O domínio RestaurantReservation fornece entidades relacionadas com a gestão reservas restaurante e pendentes.

### <a name="examples"></a>Exemplos
```
Reserve at Zucca for two for tonight
Book a table at BJ's for tomorrow
Table for 3 in Palo Alto at 7
```

### <a name="intents"></a>Intenções
| Nome da intenção | Descrição | Exemplos |
| ---------------- |-----------------------|----|
| Reserva | Pedir uma reserva para um restaurante. |Reservar em Zucca para dois para tonight<br/>Uma tabela para amanhã de livro<br/>Tabela para 3 da Palo Alto em 7|

### <a name="entities"></a>Entidades
| Nome da entidade | Descrição | Exemplos |
| ---------------- |-----------------------|----|
| Endereço| Uma localização de eventos ou uma reserva de endereço.|Da Palo Alto<br/>300 112th Ave zar<br/>Seattle|
| Amenities | Um atributo que descrevem os amenities de um local.|Vista de Ocean<br/>não smoking|
| AppName | O nome de uma aplicação para efetuar as reservas.|OpenTable<br/>Yelp<br/>TripAdvisor|
| Atmosphere | Uma descrição da atmosphere de um restaurante ou noutro local.|romantic<br/>casuais<br/>boa para grupos|
| Cuisine | Um tipo de nationality prato, cuisine ou cuisine. |Chinês<br/>Italiano<br/>Mexican|
| MealType | Um tipo de meal associado a uma reserva.|Breakfast<br/>Dinner<br/>Lunch<br/>Supper|
| PlaceName | O nome de um negócio local, restaurante, da atração pública ou instituição.|IHOP<br/>Fábrica de cheesecake<br/>Louvre|
| PlaceType | O tipo de um negócio local, restaurante, da atração pública ou instituição.|restaurante<br/>opera<br/>cinema|
| Classificação | A classificação de um local ou restaurante.|5 estrelas<br/>3 estrelas<br/>4 estrelas|

## <a name="taxi"></a>Taxi 
 
O domínio Taxi fornece entidades para criar e gerir taxi bookings e pendentes.

### <a name="examples"></a>Exemplos
```
Get me a cab at 3 pm
How much longer do I have to wait for my taxi?
Cancel my Uber
```

### <a name="intents"></a>Intenções
| Nome da intenção | Descrição | Exemplos |
| ---------------- |-----------------------|----|
| No livro | Chame uma taxi. |Obter-me um ficheiro cab<br/>localizar um taxi<br/>Livro-me um uber x|
| Cancelar | Cancele uma ação de booking um taxi.|Cancelar a minha taxi<br/>Cancelar a minha Uber|
| Registar | Controle uma rota taxi.|Quanto já tem de aguardar a minha taxi?<br/>Onde está o meu Uber?|

### <a name="entities"></a>Entidades
| Nome da entidade | Descrição | Exemplos |
| ---------------- |-----------------------|----|
| Endereço| O endereço associado booking um taxi. |Da Palo Alto<br/>300 112th Ave zar<br/>Seattle|
| DestinationAddress| Uma localização de destino ou um endereço. |Da Palo Alto<br/>300 112th Ave zar<br/>Seattle|
| DestinationPlaceName | O nome de um destino que seja um negócio local, restaurante, attraction público ou instituição. |Central Park<br/>Safeway<br/>Walmart|
| DestinationPlaceType | O tipo de destino que seja um negócio local, restaurante, attraction público ou instituição. |Restaurante<br/>Opera<br/>cinema|
| PlaceName | Nome do negócio local, restaurante, da atração pública ou instituição. |Central Park<br/>Safeway<br/>Walmart|
| PlaceType| O tipo do local num pedido para o livro um taxi.|Restaurante<br/>Opera<br/>cinema|
| TransportationCompany | O nome de um fornecedor de transporte.|Amtrak<br/>Acela<br/>Greyhound|
| TransportationType | O tipo de transportes.|Barramento<br/>Preparar<br/>Driving|

## <a name="translate"></a>Traduzir 
O domínio Translate fornece pendentes e entidades relacionadas com traduzir texto para um idioma de destino.

### <a name="examples"></a>Exemplos
```
Translate to French
Translate hello to German
Translate this sentence to English
```

### <a name="intents"></a>Intenções
| Nome da intenção | Descrição | Exemplos |
| ---------------- |-----------------------|----|
| Traduzir| Traduzir texto para outro idioma.|Converter para francês<br/>Traduzir Olá para alemão|


### <a name="entities"></a>Entidades
| Nome da entidade | Descrição | Exemplos |
| ---------------- |-----------------------|----|
| TargetLanguage | O idioma de destino de uma conversão.|Francês<br/>Alemão<br/>Coreano|
| Texto | O texto a traduzir.|Olá, Mundo<br/>Manhã boa<br/>Evening boa|

## <a name="tv"></a>TV 
 
O domínio de TV fornece pendentes e entidades para controlar TVs.

### <a name="examples"></a>Exemplos
```
Switch channel to BBC
Show TV guide
Watch National Geographic
```

### <a name="intents"></a>Intenções
| Nome da intenção | Descrição | Exemplos |
| ---------------- |-----------------------|----|
| ChangeChannel| Altere um canal num Televisor.|Canal de alteração para CNN<br/>Canal de comutador para BBC<br/>Vá para o canal 4|
| ShowGuide| Mostra o guia de TV.|Mostrar o guia de TV<br/>Novidades no canal de filmes agora?<br/>Mostrar os meus lista de programa|
| WatchTV| Peça a observar um canal de TV.|Pretender ver o canal Disney<br/>Aceda a TV.<br/>Veja National geográfica|

### <a name="entities"></a>Entidades
| Nome da entidade | Descrição | Exemplos |
| ---------------- |-----------------------|----|
| ChannelName | O nome de um canal de TV.|CNN<br/>BBC<br/>Canal de filmes|

## <a name="utilities"></a>Serviços Públicos  
O domínio de utilitários fornece pendentes para tarefas que são comuns a muitas tarefas, tais como saudações, cancelamento, confirmação, ajuda, repetição, navegação, iniciar e parar.

### <a name="examples"></a>Exemplos
```
Go back to Twitter
Please help
Repeat last question please
```

### <a name="intents"></a>Intenções
| Nome da intenção | Descrição | Exemplos |
| ---------------- |-----------------------|----|
| Cancelar | Cancele uma ação.|Cancelar a mensagem<br/>Não pretendo enviar o e-mail já|
| Confirmar | Certifique-se uma ação.|Sim esqueceu confirmo<br/>Boa que posso estou confirmar<br/>OK que posso estou confirmar|
| FinishTask | Conclua uma tarefa, o utilizador foi iniciado.|Concluído<br/>Terminei a<br/>É efetuada|
| GoBack | Voltar atrás de um passo ou regressar ao passo anterior.|Volte ao Twitter<br/>Voltar atrás de um passo<br/>Voltar atrás|
| Ajuda | Pedido para obter ajuda.|Ajudar.<br/>abrir ajuda<br/>ajuda|
| Repetir | Repetir uma ação.|Repita a última pergunta.<br/>Repita a última song|
| ShowNext | Mostra o seguinte item de uma série. |Mostrar um seguinte<br/>Ir para a página seguinte|
| ShowPrevious | Mostra item anterior de uma série.|Mostrar anterior|
| StartOver | Reinicie a aplicação ou iniciar uma sessão de novo.|Recomeçar<br/>Nova sessão<br/>restart|
| Parar | Pare uma ação.| Parar a indicar esta situação<br/>Encerrar a cópia de segurança<br/>Parar.|

## <a name="weather"></a>Meteorologia 
O domínio de Meteorologia fornece pendentes e entidades para obter relatórios meteorologia e previsões.

### <a name="examples"></a>Exemplos
```
weather in London in september
What?s the 10 day forecast?
What's the average temperature in India in september?
```

### <a name="intents"></a>Intenções
| Nome da intenção | Descrição | Exemplos |
| ---------------- |-----------------------|----|
| GetCondition | Obter factos históricos relacionados com Meteorologia. |Meteorologia em Londres Setembro<br/>O que é a temperatura média no Índia Setembro?|
| GetForecast | Obter a meteorologia atual e a prever seguinte alguns dias. |Como é a meteorologia hoje?<br/>O que é o dia 10 previsão?<br/>Como a Meteorologia será este fim de semana?|

### <a name="entities"></a>Entidades
| Nome da entidade | Descrição | Exemplos |
| ---------------- |-----------------------|----|
| Localização| A localização absoluta para um pedido de Meteorologia.|Seattle<br/>Paris<br/>Da Palo Alto|

## <a name="web"></a>Web 
O domínio da Web fornece um objetivo para navegar para um Web site.

### <a name="examples"></a>Exemplos
```
Navigate to facebook.com
Go to www.twitter.com
Navigate to www.bing.com
```

### <a name="intents"></a>Intenções
| Nome da intenção | Descrição | Exemplos |
| ---------------- |-----------------------|----|
| Navegar | Um pedido para navegar para um site especificado. |Navegue para facebook.com<br/>Aceda a www.twitter.com|

