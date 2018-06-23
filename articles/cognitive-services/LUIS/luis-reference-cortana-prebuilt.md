---
title: Referência da aplicação prebuilt Cortana | Microsoft Docs
description: Referência para o Assistente de pessoal de Cortana, uma aplicação prebuilt do idioma compreender inteligente serviços (LUIS).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 12/13/2017
ms.author: v-geberr
ms.openlocfilehash: cd808c30a6781fc0252992c13ba247486e35ad44
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351925"
---
# <a name="cortana-prebuilt-app-reference"></a>Referência da aplicação prebuilt Cortana
Esta referência lista as entidades que a aplicação prebuilt Cortana reconhece e pendentes.

## <a name="cortana-prebuilt-intents"></a>Cortana prebuilt pendentes
A aplicação de Assistente de pessoal prebuilt pode identificar os pendentes seguintes:

| Objetivo | Utterances de exemplo |
|--------| ------------------|
| Builtin.Intent.Alarm.alarm_other|atualizar os meus 7:30 alarme ser oito o'clock <br/>alterar os meus alarme 8 am para 09: 00 |
| Builtin.Intent.Alarm.delete_alarm|eliminar um alarme <br/>eliminar o meu alarme "reativar"|
| Builtin.Intent.Alarm.find_alarm|altura meu alarme de reativação está definido para? <br/> é o meu alarme de reativação? |
| Builtin.Intent.Alarm.set_alarm|Ativar o meu alarme de reativação<br/>Pode definir um alarme pela 12 chamado demorar antibiotics?|
| Builtin.Intent.Alarm.Snooze|suspensão de alarme para 5 minutos<br/>suspensão alarme|
| Builtin.Intent.Alarm.time_remaining| quanto já ter até "reativação"? <br/> quanto tempo até que a minha alarme seguinte?|
| Builtin.Intent.Alarm.turn_off_alarm | desativar o alarme meu 7 am <br/> desativar a minha alarme de reativação |
| Builtin.Intent.Calendar.change_calendar_entry| alterar uma sessão<br/>mover o meu reunião de hoje para amanhã|
|Builtin.Intent.Calendar.check_availability|é tim ocupado sexta-feira?<br/>é brian livre no Sábado|
|Builtin.Intent.Calendar.connect_to_meeting|ligar a uma reunião<br/>associar o reunião online|
|Builtin.Intent.Calendar.create_calendar_entry|Preciso de agendar uma sessão com Diogo para sexta-feira<br/>Se uma sessão com lisa em 2 pm no Domingo|
|Builtin.Intent.Calendar.delete_calendar_entry|eliminar a minha sessão<br/>remover reunião em 3 pm amanhã meu calendário|
|Builtin.Intent.Calendar.find_calendar_entry|Mostrar o meu calendário<br/>apresentar o meu calendário semanal|
|Builtin.Intent.Calendar.find_calendar_when|Quando está a minha seguinte cumprir com jon?<br/>tempo de que é a minha sessão com larry na segunda-feira?|
|Builtin.Intent.Calendar.find_calendar_where|Mostrar a localização do meu reunião 6 o'clock<br/>onde é que reunião com jon?|
|Builtin.Intent.Calendar.find_calendar_who|quem é esta reunião com?<br/>quem mais é convidou?|
|Builtin.Intent.Calendar.find_calendar_why|quais são os detalhes do meu reunião 11 o'clock?<br/>o que é que reunião com jon sobre?|
|Builtin.Intent.Calendar.find_duration|quanto tempo é meu reunião seguinte<br/>o número de minutos longo é meu reunião este afternoon|
|Builtin.Intent.Calendar.time_remaining|quanto tempo até que a minha sessão seguinte?<br/>Quanto mais tempo ter antes da reunião?|
|Builtin.Intent.Communication.add_contact|Guardar este número e coloque o nome do blogue<br/>colocar jason na minha lista de contactos|
|Builtin.Intent.Communication.answer_phone|resposta<br/>a chamada de resposta|
|Builtin.Intent.Communication.assign_nickname|Editar alcunha para nickolas<br/>Adicionar uma alcunha para João Silva|
|Builtin.Intent.Communication.call_voice_mail|correio de voz<br/>chamada de correio de voz|
|Builtin.Intent.Communication.find_contact|Mostrar os meus contactos<br/>localizar contactos|
|Builtin.Intent.Communication.forwarding_off|Pare de reencaminhar minhas chamadas<br/>desactivar a chamada de reencaminhamento|
|Builtin.Intent.Communication.forwarding_on|definir o reencaminhamento de chamada para enviar chamadas de telefone de casa<br/>reencaminhar chamadas para o telefone de casa|
|Builtin.Intent.Communication.ignore_incoming|não responder a essa chamada<br/>não agora, estou ocupado|
|Builtin.Intent.Communication.ignore_with_message|não responder nesse chamada mas em vez disso, a enviar uma mensagem<br/>Ignorar e enviar um texto novamente|
|Builtin.Intent.Communication.make_call|Bernardo chamada e João<br/>invocar mom e dad|
|Builtin.Intent.Communication.press_key|Estrela de acesso telefónico<br/>Prima 3 de 2 1|
|Builtin.Intent.Communication.read_aloud|texto de leitura<br/>o que ela diga na mensagem|
|Builtin.Intent.Communication.Redial|Redial meu última chamada<br/>Redial|
|Builtin.Intent.Communication.send_email|mensagem de e-mail para mike waters mike que foi splendid dinner última semana<br/>envie um e-mail para bob|
|Builtin.Intent.Communication.send_text|Enviar texto para Bernardo e João<br/>message|
|Builtin.Intent.Communication.speakerphone_off|tirar-me desativar orador<br/>Desativar speakerphone|
|Builtin.Intent.Communication.speakerphone_on|modo de Speakerphone<br/>colocar em pausa speakerphone|
|Builtin.Intent.mystuff.find_attachment|localizar que o João documento enviado por e-mail-me ontem <br/>localizar o documento a partir de João|
|Builtin.Intent.mystuff.find_my_stuff|Pretender editar os meus lista compras de ontem<br/>localizar os meus notas chemistry a partir de Setembro
|Builtin.Intent.mystuff.search_messages|Abra a mensagem <br/>mensagens|
|Builtin.Intent.mystuff.transform_my_stuff|partilhar a minha compras lista com os meus husband<br/>eliminar o meu lista compras|
|Builtin.Intent.ondevice.open_setting|Abra as definições de cortana <br/>Ir para as notificações|
|Builtin.Intent.ondevice.Pause|Desativar música<br/>música desativado|
|Builtin.Intent.ondevice.play_music|Pretender ouvir proprietário de um heart lonely<br/>reproduzir algumas música gospel|
|Builtin.Intent.ondevice.recognize_song|informar-me Novidades este song<br/>analisar e obter o título do song|
|Builtin.Intent.ondevice.Repeat|Repita este controlar<br/>reproduzir este song novamente|
|Builtin.Intent.ondevice.resume|reiniciar música<br/>Inicie novamente a música|
|Builtin.Intent.ondevice.skip_back|um registo de segurança<br/>song anterior|
|Builtin.Intent.ondevice.skip_forward|song seguinte<br/>Avançar diretamente um registo|
|Builtin.Intent.ondevice.turn_off_setting|Wi-Fi desativado<br/>desativar o modo de avião|
|Builtin.Intent.ondevice.turn_on_setting|Wi-Fi no<br/>Ativar o bluetooth|
|Builtin.Intent.Places.add_favorite_place|Adicionar este endereço aos meus Favoritos<br/>Guardar esta localização para os meus Favoritos|
|Builtin.Intent.Places.book_public_transportation|comprar uma permissão de formação<br/>livro uma passagem de tram|
|Builtin.Intent.Places.book_taxi|Pode encontrar-me um substituir nesta hora?<br/>localizar um taxi|
|Builtin.Intent.Places.check_area_traffic|o que é o tráfego, como no 520<br/>como é o tráfego no i-5|
|Builtin.Intent.Places.check_into_place|Verifique para joya<br/>Verifique aqui|
|Builtin.Intent.Places.check_route_traffic|Mostrar o tráfego de forma kirkland<br/>como é o tráfego para seattle?|
|Builtin.Intent.Places.find_place|onde live <br/>Atribuir-me as restaurants japonês 3 principais|
|Builtin.Intent.Places.get_address|Mostrar o endereço do guu no robson completo<br/>o que é o endereço do mais próximo starbucks?|
|Builtin.Intent.Places.get_coupon|localizar lida no tvs na minha área<br/>descontos na vista das regiões montanhosas|
|Builtin.Intent.Places.get_distance|como até que ponto ausente é inn de férias?<br/>o que é a distância à tahoe|
|Builtin.Intent.Places.get_hours|quais são barra horas da del corso no segundas?<br/>Quando a biblioteca está aberta?|
|Builtin.Intent.Places.get_menu|Mostrar menu do applebee<br/>Novidades no menu à sizzler|
|Builtin.Intent.Places.get_phone_number|Atribua o número de depósito inicial<br/>o que é o número de telefone a starbucks mais próximo?|
|Builtin.Intent.Places.get_price_range|qual é o dinner custo lobster vermelho<br/>como dispendiosa é café roxa|
|Builtin.Intent.Places.get_reviews|Mostrar as revisões para os arquivos de hardware locais<br/>Quero ver revê restaurante|
|Builtin.Intent.Places.get_route|Atribuir-me indicações para|é possível obter a hut pizza nos requisitos de memória|
|Builtin.Intent.Places.get_star_rating|quantos estrelas tem o laundry francês?<br/>é aconselhável aquarium no monterrey?|
|Builtin.Intent.Places.get_transportation_schedule|o tempo de frente ferry deixe?<br/>é quando o comboio sai mais recente para seattle?|
|Builtin.Intent.Places.get_travel_time|o que é o tempo despertar para denver de SF<br/>quanto tempo necessário para aceder ao frente aqui|
|Builtin.Intent.Places.make_call|chamada Silva de Dr. no bellevue<br/>chamada de depósito raiz na partir do dia 1 Rua|
|Builtin.Intent.Places.rate_place|Atribua uma classificação para este restaurante<br/>taxa il fornaio 5 estrelas na yelp|
|Builtin.Intent.Places.show_map|o que é o meu localização atual <br/>o que é o meu localização|
|Builtin.Intent.Places.takes_reservations|é possível efetuar uma reserva do ambiente olive<br/>Galeria art aceita reservas|
|Builtin.Intent.Reminder.change_reminder|alterar um lembrete<br/>Mover para cima meu lembrete de dia de imagem 30 minutos|
|Builtin.Intent.Reminder.create_single_reminder|Notificar-me a reativação às 7 am<br/>Notificar-me ir truque ou encará-los com luca em 4:40 pm|
|Builtin.Intent.Reminder.delete_reminder|eliminar este lembrete<br/>eliminar o meu lembrete de imagem|
|Builtin.Intent.Reminder.find_reminder|é necessário qualquer lembretes configurou para hoje<br/>é necessário um lembrete definido para terceiros do luca|
|Builtin.Intent.Reminder.read_aloud|ler lembrete terminar loud<br/>Esse lembrete de leitura|
|Builtin.Intent.Reminder.Snooze|Snooze esse lembrete até amanhã<br/>Snooze este lembrete|
|Builtin.Intent.Reminder.turn_off_reminder|Desativar lembrete<br/>dispensar lembrete de recolha de airport|
|Builtin.Intent.weather.change_temperature_unit|alterar fahrenheit para kelvin<br/>alterar fahrenheit para celsius|
|Builtin.Intent.weather.check_weather|Mostrar a previsão para o meu viagem futura para seattle<br/>como a Meteorologia será este fim de semana|
|Builtin.Intent.weather.check_weather_facts|o que é a meteorologia, como no hawaii no Dezembro?<br/>qual era a temperatura de momento último ano?|
|Builtin.Intent.weather.compare_weather|Atribuir-me uma comparação entre a temperatura elevada e lows de dallas e houston, tx<br/>como a meteorologia does comparar slc e nyc|
|Builtin.Intent.weather.get_frequent_locations|fornecer-me meu localização mais frequente<br/>deixa de mostrar mais frequentemente|
|Builtin.Intent.weather.get_weather_advisory|Meteorologia avisos<br/>Mostrar Meteorologia Consultivo de alterações para sacramento|
|Builtin.Intent.weather.get_weather_maps|apresentar um mapa de Meteorologia<br/>Mostrar Meteorologia maps para África|
|Builtin.Intent.weather.question_weather|poderá ter foggy amanhã manhã?<br/>será necessário shovel snow este fim de semana?|
|Builtin.Intent.weather.show_weather_progression|Mostrar radar Meteorologia local<br/>Iniciar radar|
|Builtin.Intent.None|como é a sua<br/>Abra a câmara|

## <a name="prebuilt-entities"></a>Entidades prebuilt 

Seguem-se alguns exemplos de entidades que a aplicação de Assistente de pessoal prebuilt pode identificar:

|Entidade |Exemplo de entidade em utterance |
|-------|------------------|
|Builtin.Alarm.alarm_state | Ativar `off` meu alarme de reativação    <br/> é o meu alarme de reativação `on`  | 
|Builtin.Alarm.DURATION |suspensão para `10 minutes`    <br/> suspensão de alarme para `5 minutes`  |
|Builtin.Alarm.start_date | definir um alarme para `monday` às 7 am   <br/> definir um alarme para `tomorrow` , ao meio-dia   |
|Builtin.Alarm.start_time | criar um alarme para `30 minutes`    <br/> definir o alarme para ir `in 20 minutes`   |
|Builtin.Alarm.title | é meu `wake up` alarme no <br/> Pode definir um alarme para trimestre até à 12 segunda a sexta-chamado `take antibiotics` |
|Builtin.Calendar.absolute_location | criar uma sessão para amanhã em `123 main street`   <br/> a reunião ocorrerá `cincinnati` no 5th de Junho    |
|Builtin.Calendar.contact_name|colocar uma reunião marketing no meu calendário e ser Certifique-se que `joe` existe <br/>Pretender configurar um lunch em il fornaio e convidar `paul` |   
|Builtin.Calendar.destination_calendar|Esta opção para adicionar os meus `work` agenda   <br/>Coloque isto meu `personal` calendário |
|Builtin.Calendar.DURATION| configurar uma sessão para `an hour` em 6 tonight <br/>  livro um `2 hour` reunião com joe |  
|Builtin.Calendar.end_date | Crie uma entrada de calendário chamada férias de amanhã até `next monday` <br/>    bloquear o meu tempo ocupado até como `monday, october 5th` | 
|Builtin.Calendar.end_time | as extremidades de reunião no `5:30 PM` <br/> agendá-la a partir de 11 para `noon`  |   
|Builtin.Calendar.implicit_location | Cancelar a sessão no dmv <br/> alterar a localização de nascimento dos quilómetros para `poppy restaurant` |    
|Builtin.Calendar.move_earlier_time | Push de reunião reencaminhar `an hour` <br/> sessão a dentist mover para cima `30 minutes`       |
|Builtin.Calendar.move_later_time | mover a minha sessão dentist `30 minutes`    <br/> Push de reunião `an hour` |  
|Builtin.Calendar.original_start_date | Reprogramar a minha sessão em barber de "Terça-feira", a quarta-feira <br/> mover o meu reunião com o Manuel de `monday` para Terça-feira |
|Builtin.Calendar.original_start_time | Reprogramar a minha reunião de `2:00` para 3  <br/> alterar a minha sessão dentist de `3:30` a 4 |  
|Builtin.Calendar.start_date | altura o meu terceiros iniciar no `flag day` <br/> agendar lunch para o `friday after next` , ao meio-dia 
|Builtin.Calendar.start_time | Pretender agendá-la para `this morning` <br/> Pretender agendá-la no `morning` |  
|Builtin.Calendar.title | `vet appointment`  <br/> `dentist` Terça-feira |
|Builtin.Communication.audio_device_type | certifique-o através de chamada `bluetooth`  <br/> chamar com os meus `headset` | 
|Builtin.Communication.contact_name | texto `bob jones`  <br/> | chamada `sarah`|
|Builtin.Communication.destination_platform | chamada dave em `london` <br/> a chamada `work line` |    
|Builtin.Communication.from_relationship_name | Mostrar chamadas a partir do meu `daughter` <br/> ler o e-mail da `mom`   |   
|Builtin.Communication.key | acesso telefónico `star` <br/>  Prima a `hash` chave    |
|Builtin.Communication.Message | e-mail carly diga `i'm running late` <br/> texto Santos angus `good luck on your exam` |    
|Builtin.Communication.message_category | novo e-mail marcado para `follow up`  <br/> novo e-mail marcado `high priority` |    
|Builtin.Communication.message_type | enviar um `email`   <br/> my leitura `text` mensagens em voz alta |
|Builtin.Communication.phone_number | Pretender marcar `1-800-328-9459` <br/>     chamada `555-555-5555` |   
|Builtin.Communication.relationship_name | texto a minha `husband` <br/>  correio eletrónico `family`| 
|Builtin.Communication.slot_attribute | alterar o `recipient` <br/>    alterar o `text` | 
|Builtin.comminication.source_platform | chamada-lo a partir de `skype` <br/> chamada-lo a partir do meu `personal line` |
|Builtin.mystuff.Attachment| com documentos `attached` <br/> localizar o e-mail `attachment` Bernardo enviados |
|Builtin.mystuff.contact_name| localizar a folha de cálculo que lisa enviado para `me` <br/> em que é o documento i enviado para `susan` noite última |
|Builtin.mystuff.data_source | `c:\dev\` <br/> a minha `desktop` <br/> |`"resolution": { "resolution_type": "metadataItems", "metadataType": CanonicalEntity", "value": "desktop" }`|
|Builtin.mystuff.DATA_TYPE | Localize o `document` i trabalhados noite última <br/> |`"resolution": {"resolution_type": "metadataItems", "metadataType": "CanonicalEntity", "value":Document" }` <br/> Trazer do mike `visio diagram`  |
|Builtin.mystuff.end_date| Mostrar os docs i trabalhado `between yesterday and today`   <br/> localizar o documento i estava a funcionar no `before thursday the 31st` |
|Builtin.mystuff.end_time| localizar os ficheiros guardados i `before noon` <br/> localizar o documento i estava a funcionar no `before 4pm`|      
|Builtin.mystuff.file_action| Abra a folha de cálculo posso `saved` ontem <br/> localizar a folha de cálculo kevin `created`| 
|Builtin.mystuff.from_contact_name | localizar a proposta `jason` enviar-me <br/> abrir `isaac` da última e-mail |
|Builtin.mystuff.Keyword | Mostrar o `french conjugation` ficheiros <br/> localizar o `marketing plan` i drafted ontem |
|Builtin.mystuff.location | o documento i editada em `work` <br/> fotografias i demorou `paris` |
|Builtin.mystuff.message_category | Procure os meus `new` mensagens de correio eletrónico <br/> Procure os meus `high priority` e-mail |
|Builtin.mystuff.message_type | verificar a minha `email` <br/>  Mostrar os meus `text` mensagens  |
|Builtin.mystuff.source_platform | pesquisa meu `hotmail` e-mail de correio eletrónico do João    <br/> localizar que o documento i enviados a partir de `work` |
|Builtin.mystuff.start_date | localizar notas do `january` <br/> localizar o envio de correio eletrónico i rob `after january 1st` |
|Builtin.mystuff.start_time | localizar esse e-mail i enviados rob algum tempo antes de 2 pm mas `after noon`? <br/> localizar a folha de cálculo que kristin enviar-me que posso editado `last night` | 
|Builtin.mystuff.title | `c:\dev\mystuff.txt` <br/> `*.txt` |
|Builtin.mystuff.transform_action | `download` o João ficheiro enviar-me <br/> `open` Os meus documentos diretrizes de anotação |
|Builtin.Note.note_text | criar uma lista de grocery incluindo `pork chops, applesauce and milk` <br/> Tome nota para `buy milk` |
|Builtin.Note.title | Tome nota chamada `grocery list` <br/> Tome nota chamada `people to call` |
|Builtin.ondevice.music_artist_name | reproduzir tudo por `rufus wainwright` <br/> reproduzir `garth brooks` música |   
|Builtin.ondevice.music_genre | Mostrar `classic rock` músicas <br/> reproduzir os meus `classical` música do período baroque |
|Builtin.ondevice.music_playlist | shuffle todos os britney spears de `workout` listas de reprodução <br/> reproduzir `breakup` listas de reprodução
|Builtin.ondevice.music_song_name | Play `summertime` <br/> Play `me and bobby mcgee` | 
|Builtin.ondevice.setting_type | `quiet hours` <br/> `airplane mode` |
|Builtin.Places.absolute_location | colocar-me para a intersecção de `5th and pike` <br/> Não, pretendo que as direções para `1 microsoft way redmond wa 98052` |
|Builtin.Places.Atmosphere | Procure `interesting` locais ir    <br/> onde posso encontrar um `casual` restaurante |  
|Builtin.Places.audio_device_type |chamar o escritório de post `hands free` <br/> do chamada papa João com `speakerphone` |    
|Builtin.Places.avoid_route | evitar a `toll road` <br/> obter-me para a frente evitando o `construction on 101` |  
|Builtin.Places.cuisine | `halal` Deli quase das regiões montanhosas vista   <br/> `kosher` Ajustar jantar no peninsula |
|Builtin.Places.Date | fazer uma reserva para `next friday the 12th` <br/>  é mashiko aberta no `mondays` ? |
|Builtin.Places.discount_type | localizar um `coupon` para de macy <br/> localizar-me um `coupon` |
|Builtin.Places.Distance | Existe uma boa diner `within 5 miles` de aqui <br/> localizar aqueles `within 15 miles` |   
|Builtin.Places.from_absolute_location | instruções da `45 elm street` à home page <br/> obter-me instruções da `san francisco` da palo alto  |
|Builtin.Places.from_place_name | a ocasionar a partir de `post office` para Rua 56 center <br/> obter-me instruções da `home depot` para lowes |
|Builtin.Places.from_place_type | instruções para downtown do `work` <br/>  obter-me instruções do `drug store` à home page |
|Builtin.Places.meal_type | próximos em locais `dinner` <br/> localizar um bom local para uma empresa `lunch` | 
|Builtin.Places.nearby | Mostrar algumas shops frios `near` -me  <br/> existem quaisquer restaurants Lebanese boas `around` aqui? |
|Builtin.Places.open_status | Quando estiver a mall `closed` <br/> obter-me a `opening` horas do arquivo | 
|Builtin.Places.place_name | tirar-me para `central park` <br/> procurar o `eiffel tower` |   
|Builtin.Places.place_type | `atms` <br/> `post office` |
|Builtin.Places.prefer_route | Mostrar as direções pelo `shortest` rota <br/> tirar o `fastest` rota | 
|Builtin.Places.price_range| Atribuir-me locais que são `moderately affordable` <br/>  Pretendo um `expensive` um   |
|Builtin.Places.Product | onde posso obter `fresh fish` em torno aqui  <br/> onde em torno sells aqui `bare minerals` |
|Builtin.Places.public_transportation_route | agenda de barramento para o `m2` bus <br/> rota de barramento `3x` |  
|Builtin.Places.rating | Mostrar `3 star` restaurants <br/> Mostrar resultados que são `3 stars or higher`  |
|Builtin.Places.reservation_number | livro uma tabela para `seven` pessoas <br/>  fazer uma reserva para `two` em il fornaio |
|Builtin.Places.results_number | Mostrar o `10` cafés mais próximo para aqui <br/> Mostrar parte superior `3` aquariums  
|Builtin.Places.service_provided | onde podem posso aceder a `whale watch` pelo barramento? <br/> Preciso de um mechanic para `fix my brakes` |    
|Builtin.Places.Time | Pretendo locais abertos no sábados às `8 am`| é aberto o mashiko `now` |
|Builtin.Places.transportation_company | agendas de formação para `new jersey transit` <br/> posso obter não existe `bart` | 
|Builtin.Places.transportation_type | em que é um arquivo de música i pode aceder à `on foot`? <br/>  Atribuir-me `biking` indicações para mashiko|
|Builtin.Places.travel_time | Pretender ser capaz de unidade `less than 15 minutes` <br/>   Pretendo algures que posso pode obter no `under 15 minutes` |   
|Builtin.Reminder.absolute_location | Avisar-me para chamar o meu dad quando i encaminhado para depois `chicago` <br/> Quando posso voltar à `seattle` lembrar obter gás |
|Builtin.Reminder.contact_name | Quando `bob` chamadas, avisar-me dizê-lo a joke depois <br/> criar um lembrete para mencionar o barramento de profissional quando posso falar para `arthur` |
|Builtin.Reminder.leaving_absolute_location | o lembrete de recolher craig ao sair `1200 main` |
|Builtin.Reminder.leaving_implicit_location | Avisar-me para obter gás quando posso deixe depois `work`  |
|Builtin.Reminder.original_start_date | alterar o lembrete sobre lawn de `saturday` para Domingo <br/> mover o meu lembrete sobre profissional da `monday` para Terça-feira   |
|Builtin.Reminder.relationship_name | Quando os meus `husband` chamadas, avisar-me dizê-lo sobre a reunião pta depois <br/> Avisar-me depois novamente quando `mom` chamadas|
|Builtin.Reminder.reminder_text | Pode, avisar-me depois para `bring up my small spot of patchy skin` quando dr smith'calls  <br/> Avisar-me para depois `pick up dry cleaning` em 4:40   |
|Builtin.Reminder.start_date | Avisar-me Depois de `thursday after next` em 8 pm  <br/> Avisar-me depois `next thursday the 18th` em 8 pm    |
|Builtin.Reminder.start_time | criar um lembrete `in 30 minutes` <br/> criar um lembrete para water fábricas `this evening at 7` |  
|Builtin.weather.absolute_location | será rain no `boston` <br/> o que é a previsão de `seattle`?  |
|Builtin.weather.date_range | Meteorologia no nyc `this weekend` <br/>   procurar o `five day` previsão no hollywood na florida |
|Builtin.weather.suitable_for | Pode aceder `hiking` no shorts este fim de semana?   <br/> irá ser nice para `walk` para hoje em dia no jogo? | 
|Builtin.weather.temperature_unit | o que é a temperatura hoje em dia no `kelvin`   <br/> Mostrar temps no `celsius` |  
|Builtin.weather.time_range | Procure, tal como será snow `tonight`? <br/> é direito windy `now`?  |
|Builtin.weather.weather_condition | Mostrar `precipitation` <br/> como espesso é o `snow` em lake tahoe agora?  |

