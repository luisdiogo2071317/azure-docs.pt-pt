---
title: Arquitetura de Blockchain Workbench do Azure
description: Descrição geral da arquitetura de Azure Blockchain Workbench e os respetivos componentes.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 4/20/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: d37913caa94dc4cf79aef9c2c12a7aacce7c03ce
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/12/2018
---
# <a name="azure-blockchain-workbench-architecture"></a>Arquitetura de Blockchain Workbench do Azure

Azure Blockchain Workbench simplifica o desenvolvimento de aplicações blockchain ao fornecer uma solução com vários componentes do Azure. Blockchain Workbench podem ser implementado através de um modelo de solução no Azure Marketplace. O modelo permite aos utilizadores escolher os módulos e componentes a implementar com Blockchain Workbench, tais como a pilha de blockchain, tipo de aplicação de cliente e suporte para a integração de IoT. Depois de implementada, o Blockchain Workbench fornece acesso a uma aplicação web, a aplicação iOS e a aplicação Android.

![Arquitetura do Blockchain Workbench](media/blockchain-workbench-architecture/architecture.png)

## <a name="identity-and-authentication"></a>Identidade e autenticação

Utilizar Blockchain Workbench, um consortium pode federar as respetivas identidades de empresa com o Azure Active Directory (Azure AD). Workbench gera novas contas de utilizador de identidades na cadeia com as identidades de empresa armazenadas no Azure AD. O mapeamento de identidades facilita o início de sessão autenticado às aplicações e APIs cliente e utiliza as políticas de autenticação das organizações. Workbench também fornece a capacidade de associar as identidades de empresa para funções específicas dentro de um determinado contrato inteligente. Além disso, Workbench também fornece um mecanismo para identificar as ações essas funções pode demorar e em que altura.

Depois de implementar Blockchain Workbench, os utilizadores interagem com Blockchain Workbench ou através de aplicações de cliente, o cliente baseado em REST API ou a API de mensagens. Em todos os casos, as interações tem de ser autenticadas, quer através do Azure Active Directory (Azure AD) ou credenciais específicas do dispositivo.

Os utilizadores federar as respetivas identidades para um consortium do Azure AD enviando um convite de correio eletrónico aos participantes no respetivo endereço de e-mail. Quando iniciar sessão, estes utilizadores são autenticados utilizando o nome, a palavra-passe e a políticas. Por exemplo, autenticação de dois fatores da respetiva organização.

Azure AD é utilizado para gerir todos os utilizadores que tenham acesso aos Blockchain Workbench. Cada dispositivo ligar a um contrato inteligente também se encontra associado com o Azure AD.

Azure AD também é utilizado para atribuir utilizadores a um grupo de administrador especiais. Os utilizadores associados ao grupo Administrador obtêm direitos de administrador/ações Workbench Blockchain, tais como implementação de contratos e conceder permissões a um utilizador para aceder a um contrato. Os utilizadores fora deste grupo não têm acesso para ações de administrador.

## <a name="client-applications"></a>Aplicações de cliente

Workbench fornece as aplicações de cliente gerado automaticamente para a web e móveis (iOS, Android), que podem ser utilizado para validar, testar e ver blockchain aplicações. A interface de aplicação é gerada dinamicamente com base nos metadados de contrato inteligente e pode acomodar a qualquer caso de utilização. As aplicações cliente fornecem um front-end de destinada ao utilizador para as aplicações de blockchain concluída geradas pelo Blockchain Workbench. As aplicações cliente autentiquem os utilizadores através do Azure Active Directory (Azure AD) e, em seguida, apresentam uma experiência de utilizador e adaptada para o contexto empresarial do contrato inteligente. A experiência de utilizador permite a criação de novas instâncias de contrato inteligente por pessoas autorizadas e, em seguida, apresenta a capacidade de executar determinados tipos de transações em pontos apropriadas no processo de negócio que representa o contrato inteligente.

Na aplicação web, os utilizadores autorizados podem aceder a consola do administrador. A consola está disponível para os utilizadores no grupo de administrador no Azure AD e fornece acesso à funcionalidade seguinte:

* Implemente o Microsoft fornecida contratos inteligentes para cenários mais populares. Por exemplo, um cenário de transferência ativo.
* Carregar e implementar os seus próprios contratos inteligentes.
* Atribua um acesso de utilizador para o contrato inteligente no contexto de uma função específica.

## <a name="gateway-service-api"></a>API do serviço de gateway

Blockchain Workbench inclui uma API do serviço de gateway baseado em REST. Quando escrever um blockchain, a API gera e entrega mensagens a um mediador de eventos. Quando são solicitados dados pela API, consultas são enviadas para a base de dados do SQL Server-se. A base de dados SQL contém uma réplica dos dados na cadeia e metadados que fornece informações de contexto e a configuração para contratos inteligentes suportados. As consultas devolvem os dados necessários da réplica-se num formato informado pelos metadados para o contrato.

Os programadores podem aceder a API do serviço de gateway para criar ou integrar soluções blockchain sem depender de aplicações de cliente Blockchain Workbench.

> [!NOTE]
> Para ativar o acesso autenticado à API, duas aplicações de cliente estão registadas no Azure Active Directory. Azure Active Directory requer registos de aplicação distintos cada tipo de aplicação (nativa e web). 

## <a name="message-broker-for-incoming-messages"></a>Mediador de mensagem para mensagens a receber

Os programadores que pretendem enviar mensagens diretamente para o Blockchain Workbench podem enviar mensagens diretamente para o Service Bus. Por exemplo, API de mensagens pode ser utilizado para integração do sistema de sistema ou de dispositivos de IoT.

## <a name="message-broker-for-downstream-consumers"></a>Mediador de mensagens para consumidores a jusante

Durante o ciclo de vida da aplicação, eventos ocorrem. Podem ser acionados eventos pela API do Gateway ou o ledger. As notificações de eventos podem iniciar a jusante código com base no evento.

Blockchain Workbench implementa automaticamente dois tipos de consumidores de eventos. Um é acionado pelos eventos de blockchain para preencher o arquivo SQL desativar cadeia. O outro é a captura de metadados para eventos gerados pela API relacionados com o carregamento e o armazenamento de documentos.

## <a name="message-consumers"></a>Consumidores de mensagens

 Consumidores de mensagem demorar mensagens do Service Bus. Permite que o modelo de eventos CCM subjacente para consumidores de mensagem para extensões de serviços adicionais e sistemas. Por exemplo, pode adicionar suporte para preencher CosmosDB ou avaliar mensagens através da análise de transmissão em fluxo do Azure. As secções seguintes descrevem os consumidores de mensagem incluídos na Blockchain Workbench.

### <a name="distributed-ledger-consumer"></a>Consumidor de ledger distribuída

Mensagens de tecnologia (DLT) de ledger distribuída contêm os metadados para transações de escrita para o blockchain. O consumidor obtém as mensagens e envia os dados para um construtor de transação, signatário e router.

### <a name="database-consumer"></a>Consumidor de base de dados

O consumidor da base de dados demora mensagens do Service Bus e envia os dados para uma base de dados ligado, tais como a base de dados SQL.

### <a name="storage-consumer"></a>Consumidor de armazenamento

O consumidor armazenamento demora mensagens do Service Bus e envia dados para um armazenamento ligado. Por exemplo, armazenamento de documentos com hash no armazenamento do Azure.

## <a name="transaction-builder-and-signer"></a>O construtor de transação e signatário inválido

Se uma mensagem durante o Mediador de mensagens de entrada tem de ser escritos para o blockchain, serão processado pelo consumidor DLT. O consumidor DLT é um serviço, que obtém a mensagem que contém os metadados para uma transação pretendido para executar e, em seguida, envia as informações para o *builder de transação e signatário*. O *builder de transação e signatário* monta uma transação de blockchain com base nos dados e o destino de blockchain pretendido. Depois de assembled, a transação está assinada. As chaves privadas são armazenadas no Cofre de chaves do Azure.

 Blockchain Workbench obtém a chave privada adequada a partir do Cofre de chaves e inicia a transação fora do Cofre de chaves. Depois de iniciar, a transação é enviada para os routers de transação e ledgers.

## <a name="transaction-routers-and-ledgers"></a>Routers de transação e ledgers

Routers de transação e ledgers demorar transações assinadas e encaminhá-las para o blockchain adequado. Atualmente, o Blockchain Workbench suporta Ethereum como respetivo blockchain de destino.

## <a name="dlt-watcher"></a>Observador DLT

Um observador de tecnologia (DLT) ledger distribuída monitoriza eventos ocorridos em cadeias de blocos ligadas ao Blockchain Workbench.
Eventos refletem informações relevantes para indivíduos e sistemas. Por exemplo, a criação de novas instâncias de contrato, execução de transações e alterações de estado. Os eventos são capturados e enviados para o Mediador de mensagens de saída, pelo que pode ser utilizadas pelos consumidores a jusante.

Por exemplo, o consumidor SQL monitoriza os eventos, consome-los e preenche a base de dados SQL com os valores incluídos. A cópia ativa recriação de uma réplica dos dados na cadeia de um arquivo de desativar cadeia.

## <a name="azure-sql-database"></a>Base de dados SQL do Azure

A base de dados SQL do Azure ligado ao Blockchain Workbench armazena as definições de contrato, metadados de configuração e uma réplica dos dados armazenados no blockchain acessível para o SQL Server. Estes dados podem facilmente ser consultados, visualizados ou analisados pelo aceder diretamente a base de dados. Os programadores e outros utilizadores podem utilizar a base de dados para relatórios, análise ou outras integrações centrada em dados. Por exemplo, os utilizadores podem visualizar dados de transação através do Power BI.

Este armazenamento desativar cadeia fornece a capacidade para organizações empresariais de consultar os dados no SQL Server em vez de num blockchain ledger. Além disso, ao uniformizar num esquema padrão que é agnóstico em termos de pilhas de tecnologia de blockchain, o armazenamento de desativar cadeia permite a reutilização de relatórios e outros artefactos em projetos, cenários e organizações.

## <a name="azure-storage"></a>Storage do Azure

Armazenamento do Azure é utilizado para armazenar contratos e metadados associados à contratos.

As ordens de compra e lading de faturas, para imagens utilizadas na notícias e imagery médicas, para vídeo provenientes de uma continuum incluindo câmaras de corpo de política e principais de movimento de imagens, documentos desempenham uma função em vários cenários blockchain centrada. Documentos não são adequados colocar diretamente no blockchain.

Blockchain Workbench suporta a capacidade de adicionar documentos ou outro conteúdo do suporte de dados com blockchain lógica de negócio. Um hash do conteúdo de documentos ou suportes de dados é armazenado no blockchain e o documento real ou o conteúdo do suporte de dados é armazenado no Storage do Azure. As informações de transação associada é entregue o Mediador de mensagens de entrada, embaladas cópias de segurança, assinadas e encaminhadas para o blockchain. Este processo aciona eventos, que são partilhados através do Mediador de mensagens de saída. A BD do SQL Server consome estas informações e envia-a para a base de dados para consultar mais tarde. Sistemas a jusante também foi consumir estes eventos para atuar conforme adequado.

## <a name="monitoring"></a>Monitorização

Workbench fornece o registo de aplicações utilizando o Application Insights e Monitor do Azure. Application Insights é utilizado para armazenar todas as informações de sessão do Blockchain Workbench e inclui as operações com êxito, avisos e erros. Application Insights podem ser utilizados pelos programadores a depurar problemas com Blockchain Workbench. 

Monitor do Azure fornece informações sobre o estado de funcionamento da rede blockchain. 

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Implementar o Azure Blockchain Workbench](blockchain-workbench-deploy.md)