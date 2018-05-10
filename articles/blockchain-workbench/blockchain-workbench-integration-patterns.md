---
title: Inteligentes padrões de integração de contrato no Azure Blockchain Workbench
description: Descrição geral de padrões de integração de contrato inteligente no Azure Blockchain Workbench.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 5/1/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: mmercuri
manager: femila
ms.openlocfilehash: a6a44e30fe58617b43c5491a72fc882015bc9591
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/08/2018
---
# <a name="smart-contract-integration-patterns"></a>Padrões de integração do contrato inteligente

Contratos inteligentes frequentemente irão representar um fluxo de trabalho de negócio que precisa para integrar com sistemas externos e de dispositivos.

Os requisitos destes fluxos de trabalho incluem a necessidade de iniciar a transações num ledger distribuída que incluem dados a partir de um dispositivo, serviço ou sistema externo. Também têm de ter os sistemas externos reagir a eventos provenientes inteligentes contratos numa ledger distribuída.

A REST API e integração mensagens fornecem a capacidade dos enviar transações de sistemas externos para contratos inteligentes incluídos numa aplicação Azure Blockchain Workbench, bem como enviar notificações de eventos para sistemas externos com base nas alterações que efetuar Coloque dentro de uma aplicação.

Para cenários de integração de dados, o Azure Blockchain Workbench inclui um conjunto de vistas de base de dados que uma combinação de dados transacionais do blockchain e meta-data sobre as aplicações e contratos inteligentes de intercalação.

Além disso, alguns cenários, como os relacionados com a cadeia ou suportes de dados, de fornecer também podem exigir a integração de documentos. Enquanto o Azure Blockchain Workbench fornecer chamadas à API para processamento de documentos diretamente, documentos podem ser incorporados numa aplicação de Blockchain do Azure. Esta secção também inclui esse padrão.

Esta secção inclui os padrões identificados para implementar cada um destes tipos de integrações nas suas soluções de ponto a ponto.

## <a name="rest-api-based-integration"></a>Integração de API de REST

Capacidades da aplicação web do Azure Blockchain Workbench gerado são expostas através da API REST. Isto inclui o Azure Blockchain Workbench carregamento, a configuração e a administração de aplicações, transações a enviar para um ledger distribuído e a consulta dos dados de metadados e ledger de aplicação.

A API REST é utilizada sobretudo para clientes interativos, tais como web, móveis e aplicações de bot.

Esta secção observa padrões concentra-se nos aspetos da REST API que enviam transações para um ledger distribuída e os que consultar dados sobre as transações do Workbench do Azure Blockchain *desativado cadeia* base de dados SQL.

### <a name="sending-transactions-to-a-distributed-ledger-from-an-external-system"></a>Transações de envio para um distribuída ledger de um sistema externo

A API de REST do Azure Blockchain Workbench fornece a capacidade para enviar pedidos autenticados para executar as transações num ledger distribuída.

![Transações a enviar para um ledger distribuída](media/blockchain-workbench-integration-patterns/send-transactions-ledger.png)

Isto ocorre com o processo descrito acima, em que:

-   A aplicação externa autentica para o Azure Active Directory aprovisionado como parte da implementação do Azure Blockchain Workbench.
-   Os utilizadores autorizados recebem um token de portador que pode ser enviado com pedidos para a API.
-   Aplicações externas efetuar chamadas à REST API utilizando o token de portador.
-   A API REST do pedido como uma mensagem de pacotes e envia-a para o Service Bus. A partir daqui-será obtido, assinado e enviado para o ledger distribuída adequado.
-   A API REST faz um pedido para a BD de SQL do Azure Blockchain Workbench para registar o pedido e estabelecer o atual estado de aprovisionamento.
-   A BD do SQL Server devolve o estado de aprovisionamento e a chamada de API devolve o ID da aplicação externo que chamado.

### <a name="querying-blockchain-workbench-metadata-and-distributed-ledger-transactions"></a>Consultar os metadados do Blockchain Workbench e transações distribuídas ledger

A API de REST do Azure Blockchain Workbench fornece a capacidade para enviar pedidos autenticados para detalhes de consulta relacionadas com a execução de contrato inteligente num ledger distribuída.

![Consulta de metadados](media/blockchain-workbench-integration-patterns/querying-metadata.png)

Isto ocorre com o processo descrito acima, em que:

1. A aplicação externa autentica para o Azure Active Directory aprovisionado como parte da implementação do Azure Blockchain Workbench.
2. Os utilizadores autorizados recebem um token de portador que pode ser enviado com pedidos para a API.
3. Aplicações externas efetuar chamadas à REST API utilizando o token de portador.
4. A API REST consulta os dados para o pedido da base de dados SQL e devolve-lo para o cliente.

## <a name="messaging-integration"></a>Integração de mensagens

Integração de mensagens facilita a interação com sistemas, serviços e dispositivos em que um início de sessão interativo não é possível ou desejável. Integração de mensagens centra-se em dois tipos de mensagens em fila, os que pedir que transações ser executado num ledger distribuída e os eventos que estão expostos por que razão quando transações tem sido efetuada.

Integração de mensagens centra-se em execução e a monitorização de transações relacionadas com a criação do utilizador, a criação de contrato e a execução de transações em contratos é principalmente utilizado pelo *sem interface* sistemas de back-end.

Esta secção observa padrões concentra-se nos aspetos da API baseada em mensagens que enviam transações para um ledger distribuída e os que representam as mensagens de evento expostas de ledger distribuída subjacente.

### <a name="one-way-event-delivery-from-a-smart-contract-to-an-event-consumer"></a>Entrega de eventos unidirecional a partir de um contrato inteligente para um consumidor de eventos 

Neste cenário, ocorre um evento dentro de um contrato inteligente, por exemplo, uma alteração de estado ou a execução de um tipo específico de transação. Este evento é difusão através de uma grelha de eventos para consumidores a jusante e os consumidores, em seguida, tome as ações adequadas.

Um exemplo deste cenário é quando ocorre uma transação, um consumidor pretende ser alertado e pode demorar ação, tal como a gravar as informações na base de dados do SQL ou o serviço de dados comuns. Este é o mesmo padrão que se segue Workbench para preencher o *desativado cadeia* BD do SQL.

Outro seria se um contrato inteligente passa para um estado específico como, por exemplo, quando um contrato entra num *OutOfCompliance*. Quando esta alteração de estado acontece, poderia acionar um alerta para ser enviado para telemóvel um administrador.

![Entrega de eventos unidirecional](media/blockchain-workbench-integration-patterns/one-way-event-delivery.png)

Isto ocorre com o processo descrito acima, em que:

-   O contrato inteligente passa para um Estado de novo e envia um evento para o ledger.
-   O ledger recebe e entrega o evento para o Azure Blockchain Workbench.
-   Azure Blockchain Workbench está subscrito a eventos a partir do ledger e recebe o evento.
-   Azure Blockchain Workbench publica o evento para subscritores na grelha de eventos.
-   Sistemas externos subscritos à grelha de evento, consumir a mensagem e tome as ações adequadas.

## <a name="one-way-event-delivery-of-a-message-from-an-external-system-to-a-smart-contract"></a>Entrega de eventos unidirecional de uma mensagem de um sistema externo a um contrato inteligente

Também é um cenário que passa na direção oposta. Neste caso, um evento é gerado por um sensor ou um sistema externo e os dados de que o evento devem ser enviados para um contrato inteligente.

Um exemplo comum é a entrega de dados de mercados financeiros que, por exemplo, os preços de commodities, gráfico de cotações ou bonds, para um contrato inteligente.

### <a name="direct-delivery-of-an-azure-blockchain-workbench-in-the-expected-format"></a>Entrega direta de um Workbench de Blockchain do Azure no formato esperado

Algumas aplicações incorporadas para integrar com o Azure Blockchain Workbench diretamente gera e enviar mensagens nos formatos esperados.

![Entrega direta](media/blockchain-workbench-integration-patterns/direct-delivery.png)

Isto ocorre com o processo descrito acima, em que:

-   Ocorre um evento no sistema externo que aciona a criação de uma mensagem para o Azure Blockchain Workbench.
-   O sistema externo tem de código escrito para criar esta mensagem num formato conhecido e envia esta diretamente para o Service Bus.
-   Azure Blockchain Workbench está subscrito a eventos do Service Bus e obtém a mensagem.
-   Azure Blockchain Workbench inicia uma chamada para ledger, envio de dados de sistema externo para um contrato específico.
-   Após a receção da mensagem, o contrato passa para um Estado de novo.

### <a name="delivery-of-a-message-in-a-format-unknown-to-azure-blockchain-workbench"></a>Entrega de uma mensagem num formato desconhecido para Azure Blockchain Workbench

Alguns sistemas não podem ser modificados para entregar mensagens nos formatos padrão utilizados pelo Azure Blockchain Workbench. Estes casos, a mecanismos existentes e a mensagem formatos partir desses sistemas, muitas vezes, podem ser utilizados. Especificamente, os tipos de mensagem nativa destes sistemas podem ser transformados com Logic Apps, as funções do Azure ou outro código personalizado para mapear para um padrão esperados de formatos de mensagens.

![Formato de mensagem desconhecida](media/blockchain-workbench-integration-patterns/unknown-message-format.png)

Isto ocorre com o processo descrito acima, em que:

-   Ocorre um evento no sistema externo que aciona a criação de uma mensagem.
-   Uma aplicação lógica ou código personalizado é utilizado para receber essa mensagem e transformá-los a uma mensagem com formato de Azure Blockchain Workbench padrão.
-   A aplicação lógica envia a mensagem transformada diretamente para o Service Bus.
-   Azure Blockchain Workbench está subscrito a eventos do Service Bus e obtém a mensagem.
-   Azure Blockchain Workbench inicia uma chamada para ledger, envio de dados de sistema externo para uma função específica no contrato.
-   A função executa e normalmente modifica o estado. A alteração do Estado de reencaminhar move o fluxo de trabalho do negócio refletido no contrato inteligente, ativar a outras funções agora ser executada conforme apropriado.

### <a name="transitioning-control-to-an-external-process-and-await-completion"></a>Transição de controlo para um externo processar e await conclusão

Existem cenários onde um contrato inteligente tem de parar a execução interna e mão desativado para o processo externo. Que processo externo, em seguida, seria concluída, envie uma mensagem para o contrato inteligente e execução, em seguida, continuaria dentro do contrato inteligente.

#### <a name="transition-to-the-external-process"></a>Transição para o processo externo

Este padrão normalmente é implementado utilizando a seguinte abordagem:

-   Transições de contrato inteligente para um estado específico. Neste estado, qualquer ou um número limitado de funções pode ser executado enquanto um sistema externo executa uma ação pretendida.
-   A alteração de estado é anexada como um evento a um consumidor a jusante.
-   O consumidor a jusante recebe o evento e aciona a execução de código externo.

![Controlo de transição para o processo externo](media/blockchain-workbench-integration-patterns/transition-external-process.png)

#### <a name="return-of-control-from-the-smart-contract"></a>Retorno do controlo de contrato inteligente

Consoante a capacidade para personalizar o sistema externo pode ou não conseguir entregar mensagens dos formatos padrão que espera Azure Blockchain Workbench. Com base na capacidade de sistemas externos gerar uma destas mensagens irá determinar que os seguintes dois caminhos de retorno serão executadas.

##### <a name="direct-delivery-of-an-azure-blockchain-workbench-in-the-expected-format"></a>Entrega direta de um Workbench de Blockchain do Azure no formato esperado

![](media/blockchain-workbench-integration-patterns/direct-delivery.png)

Neste modelo, a comunicação com o contrato e alteração de estado subsequentes ocorre seguindo o procedimento acima processar where -

-   Após a alcançar a conclusão ou uma referência específica na execução de código externo, é enviado um evento para o barramento de serviço ligado ao Azure Blockchain Workbench.

-   Para os sistemas que não podem ser diretamente adaptada descritos para escrever uma mensagem que está em conformidade com as expectativas da API, irá ser transformado.

-   O conteúdo da mensagem é embalado cópias de segurança e enviado para uma função específica no contrato inteligente. Isto é feito em nome do utilizador associado com o sistema externo.

-   A função executa e, normalmente, irá modificar o estado. A alteração do Estado de reencaminhar move o fluxo de trabalho do negócio refletido no contrato inteligente, ativar a outras funções agora ser executada conforme apropriado.

### 

### <a name="delivery-of-a-message-in-a-format-unknown-to-azure-blockchain-workbench"></a>Entrega de uma mensagem num formato desconhecido para Azure Blockchain Workbench

![Formato de mensagem desconhecida](media/blockchain-workbench-integration-patterns/unknown-message-format.png)

Neste modelo onde enviada uma mensagem num formato padrão não pode ser diretamente, a comunicação com o contrato e alteração de estado subsequentes ocorre seguindo o procedimento acima processar onde:

1.  Após a alcançar a conclusão ou uma referência específica na execução de código externo, é enviado um evento para o barramento de serviço ligado ao Azure Blockchain Workbench.
2.  Uma aplicação lógica ou código personalizado é utilizado para receber essa mensagem e transformá-los a uma mensagem com formato de Azure Blockchain Workbench padrão.
3.  A aplicação lógica envia a mensagem transformada diretamente para o Service Bus.
4.  Azure Blockchain Workbench está subscrito a eventos do Service Bus e obtém a mensagem.
5.  Azure Blockchain Workbench inicia uma chamada para ledger, envio de dados de sistema externo para um contrato específico.
6. O conteúdo da mensagem é embalado cópias de segurança e enviado para uma função específica no contrato inteligente. Isto é feito em nome do utilizador associado com o sistema externo.
7.  A função executa e, normalmente, irá modificar o estado. A alteração do Estado de reencaminhar move o fluxo de trabalho do negócio refletido no contrato inteligente, ativar a outras funções agora ser executada conforme apropriado.

## <a name="iot-integration"></a>Integração de IoT

Um cenário de integração comum é a inclusão de dados de telemetria obtidas sensores num contrato inteligente. Com base nos dados fornecidos por sensores, contratos inteligentes foi possível executar ações informadas e alterar o estado do contrato.

Por exemplo, se um camião entrega medicine tivesse a temperatura soar para 110 graus,-pode afetar a eficácia do medicine e pode provocar um problema de segurança público se não for detetado e removidas da cadeia de fornecimento. Se um controlador acelerados STA carro para 100 quilómetros por hora, informações de sensores de resultante poderia acionar um cancelamento de insurance pelo seu fornecedor insurance. Se o automóvel era um carro rental, dados GPS foi indicam quando o controlador correu fora uma geografia abrangida pelo seu contrato rental e cobram uma penalidade.

O desafio é que estes sensores podem ser entrega de dados numa base constante e não é adequado enviar todos estes dados para um contrato inteligente. Uma abordagem comum é para limitar o número de mensagens enviadas para o blockchain ao fornecimento de todas as mensagens para um arquivo secundário. Por exemplo, entrega de mensagens recebidas em apenas intervalo fixo, por exemplo, uma vez por hora e quando um valor contido está fora de um definido após o intervalo de um contrato inteligente. A verificar os valores que se inserem fora as tolerâncias, garante que os dados relevantes para a lógica de negócio contratos recebidos e executados. Verificar o valor no intervalo confirma que o sensor ainda está a comunicar. Todos os dados são enviados para um arquivo de relatórios secundário para ativar a análise de relatórios, mais abrangente e a aprendizagem. Por exemplo, ao obter as leituras dos sensores para GPS não pode ser necessário cada minuto para um contrato inteligente, conseguiu fornecer dados interessantes para ser utilizado em relatórios ou rotas de mapeamento.

Na plataforma do Azure, é geralmente feita integração com dispositivos com o IoT Hub. IoT Hub fornece a capacidade para encaminhar mensagens com base no conteúdo e permite que o tipo de funcionalidade descrito acima.

![Mensagens de IoT](media/blockchain-workbench-integration-patterns/iot.png)

O processo acima ilustra um padrão para isto é implementado:

-   Um dispositivo comunica diretamente ou através de um gateway de campo ao IoT Hub.
-   IoT Hub recebe as mensagens e avalia as mensagens de rotas estabelecidas que verifique o conteúdo da mensagem, por exemplo. *O sensor comunica uma temperatura maior do que 50 graus?*
-   O IoT Hub envia mensagens que cumprem os critérios para um barramento de serviço definido para a rota.
-   Uma aplicação lógica ou outro código de escuta para o Service Bus do IoT Hub estabelecida para a rota.
-   A aplicação lógica ou outro código obtém e a mensagem num formato conhecidos de transformação.
-   A mensagem transformada, agora num formato padrão, é enviada para o Service Bus para Azure Blockchain Workbench.
-   Azure Blockchain Workbench está subscrito a eventos do Service Bus e obtém a mensagem.
-   Azure Blockchain Workbench inicia uma chamada para ledger, envio de dados de sistema externo para um contrato específico.
-   Após a receção da mensagem, o contrato avalia os dados e pode alterar o estado com base no resultado nessa avaliação, por exemplo, para uma elevada temperatura, altere o estado para *fora de conformidade*.

## <a name="data-integration"></a>Integração de dados

Além de REST e com base na mensagem de API, Azure Blockchain Workbench também fornece acesso a uma base de dados do SQL preenchida com meta-data de aplicação e o contrato, bem como os dados transacionais do ledgers distribuídas.

![Integração de dados](media/blockchain-workbench-integration-patterns/data-integration.png)

A integração de dados é bem conhecida:

-   Azure Blockchain Workbench armazena os metadados sobre aplicações, os fluxos de trabalho, contratos e transações como parte do respetivo comportamento funcionamento normal.
-   Sistemas externos ou ferramentas de fornecem um ou mais caixas de diálogo para facilitar a recolha de informações sobre a base de dados, tais como o nome do servidor de base de dados, o nome de base de dados, o tipo de autenticação, as credenciais de início de sessão e vistas de base de dados para utilizar.
-   As consultas são escritas em relação a vistas de base de dados SQL para facilitar a jusante consumo por sistemas externos, serviços, relatórios, ferramentas de programador e ferramentas de produtividade da empresa.

## <a name="storage-integration"></a>Integração de armazenamento

Muitos cenários podem exigir a necessidade para incorporar attestable ficheiros. Por vários motivos, será inadequada colocar ficheiros num blockchain. Em vez disso, uma abordagem comum é efetuar um hash criptográfico (por exemplo, SHA-256) em relação a um ficheiro e partilhar esse hash num ledger distribuída. Efetuar o hash novamente em qualquer altura futura deverá devolver o mesmo resultado. Se o ficheiro é modificado, mesmo que apenas um pixel está modificada uma imagem, o hash irá devolver um valor diferente.

![Integração de armazenamento](media/blockchain-workbench-integration-patterns/storage-integration.png)

O padrão pode ser implementado em que:

-   Um sistema externo mantém um ficheiro de um mecanismo de armazenamento, como o Storage do Azure.
-   Um hash é gerado com o ficheiro ou o ficheiro e metadados associados, como um identificador para o proprietário, o URL onde o ficheiro está localizado, etc.
-   O hash e quaisquer metadados são enviadas para uma função de um contrato inteligente, tais como *FileAdded*
-   No futuro, o ficheiro de metadados e podem ser protegido por hash novamente e comparado com os valores armazenados de ledger.

## <a name="prerequisites-for-implementing-integration-patterns-using-the-rest-and-message-apis"></a>Pré-requisitos para implementar padrões de integração com o resto e APIs de mensagens

Para facilitar a capacidade para um dispositivo ou sistema externo interagir com o contrato inteligente através de REST ou de uma mensagem de API, deve ocorrer o seguinte-

1. No Azure Active Directory para o consortium, é criada uma conta que representa o sistema externo ou o dispositivo.
2. O contract(s) inteligente adequado para a sua aplicação Azure Blockchain Workbench tem funções definidas para aceitar os eventos do seu sistema externo ou o dispositivo.
3. O ficheiro de configuração de aplicação para o seu contrato inteligente contém a função que o sistema ou o dispositivo será atribuído.
4. O ficheiro de configuração de aplicação para o seu contrato inteligente identifica nas quais os Estados que esta função pode ser chamada pela função definida.
5. O ficheiro de configuração de aplicação e respetivos contratos inteligentes são carregados para o Azure Blockchain Workbench.

Assim que a aplicação foi carregada, a conta do Azure Active Directory para o sistema externo é atribuída para o contrato e a função associada.

## <a name="testing-external-system-integration-flows-prior-to-writing-integration-code"></a>Testar os fluxos de integração de sistema externo antes de escrever o código de integração 

Fornecer a capacidade para integrar com sistemas externos é um requisito de muitos cenários-chave. É necessário para conseguir validar a estrutura de contrato inteligente anterior ou em paralelo para o desenvolvimento de código para integrar com sistemas externos.

A utilização do Azure Active Directory (Azure AD) pode significativamente acelerar a produtividade do programador e o tempo para o valor. Especificamente, a integração de código com um sistema externo pode demorar uma quantidade de tempo não trivial. Ao utilizar o Azure AD e a geração automática do UX pelo Azure Blockchain Workbench, isto pode permitir que os programadores de início de sessão para o Workbench como desse sistema externo e preencher valores esperado a partir desse sistema externo através do UX. Isto permite rapidamente a desenvolver e a validação de ideias uma prova de conceito ambiente é anterior ou em paralelo com o código de integração a ser escrito para os sistemas externos.
