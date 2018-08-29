---
title: Esquemas para mensagens B2B - Azure Logic Apps de controlo de X12 | Documentos da Microsoft
description: Criar esquemas que monitorizar mensagens B2B no contas de integração para o Azure Logic Apps com o Enterprise Integration Pack de controlo de X12
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: a5413f80-eaad-4bcf-b371-2ad0ef629c3d
ms.date: 01/27/2017
ms.openlocfilehash: cfd195f2f812c8b2e09058d325d0dbb6f7a60d59
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/28/2018
ms.locfileid: "43125606"
---
# <a name="create-schemas-for-tracking-x12-messages-in-integration-accounts-for-azure-logic-apps"></a>Crie esquemas para controlo de X12 de mensagens em contas de integração para o Azure Logic Apps

Para ajudá-lo a êxito do monitor, erros e as propriedades da mensagem para as transações do empresa-empresa (B2B), pode usar esses esquemas na sua conta de integração de controlo de X12:

* X12 esquemas de controlo do conjunto de transações
* X12 confirmação esquemas de controlo do conjunto de transações
* Esquema de controlo do intercâmbio de X12
* Esquemas de controlo de reconhecimento do intercâmbio de X12
* Esquema de controlo de grupo funcional de X12
* Confirmação de esquemas de controlo de grupo funcional de X12

## <a name="x12-transaction-set-tracking-schema"></a>X12 esquemas de controlo do conjunto de transações

```json
{
   "agreementProperties": {
      "senderPartnerName": "",
      "receiverPartnerName": "",
      "senderQualifier": "",
      "senderIdentifier": "",
      "receiverQualifier": "",
      "receiverIdentifier": "",
      "agreementName": ""
   },
   "messageProperties": {
      "direction": "",
      "interchangeControlNumber": "",
      "functionalGroupControlNumber": "",
      "transactionSetControlNumber": "",
      "CorrelationMessageId": "",
      "messageType": "",
      "isMessageFailed": "",
      "isTechnicalAcknowledgmentExpected": "",
      "isFunctionalAcknowledgmentExpected": "",
      "needAk2LoopForValidMessages":  "",
      "segmentsCount": ""
   }
}
```

| Propriedade | Tipo | Descrição |
| --- | --- | --- |
| senderPartnerName | Cadeia | Nome do parceiro do remetente da mensagem X12. (Opcional) |
| receiverPartnerName | Cadeia | Nome do parceiro do destinatário da mensagem X12. (Opcional) |
| senderQualifier | Cadeia | Envie o qualificador de parceiro. (Obrigatório) |
| senderIdentifier | Cadeia | Envie o identificador de parceiro. (Obrigatório) |
| receiverQualifier | Cadeia | Receba o qualificador de parceiro. (Obrigatório) |
| receiverIdentifier | Cadeia | Recebe o identificador de parceiro. (Obrigatório) |
| agreementName | Cadeia | Nome de X12 contrato para o qual as mensagens são resolvidas. (Opcional) |
| direção | Enum | Direção do fluxo da mensagem, receber ou enviar. (Obrigatório) |
| interchangeControlNumber | Cadeia | Número de controlo de intercâmbio. (Opcional) |
| functionalGroupControlNumber | Cadeia | Número de controle funcional. (Opcional) |
| transactionSetControlNumber | Cadeia | Número de controlo do conjunto de transações. (Opcional) |
| CorrelationMessageId | Cadeia | ID de mensagem de correlação. Uma combinação de {AgreementName} {*GroupControlNumber*} {TransactionSetControlNumber}. (Opcional) |
| MessageType | Cadeia | Transação definir ou tipo de documento. (Opcional) |
| isMessageFailed | Booleano | Se o X12 mensagem de falha. (Obrigatório) |
| isTechnicalAcknowledgmentExpected | Booleano | Se a confirmação técnica está configurada no X12 contrato. (Obrigatório) |
| isFunctionalAcknowledgmentExpected | Booleano | Se a confirmação funcional é configurada no X12 contrato. (Obrigatório) |
| needAk2LoopForValidMessages | Booleano | Se o loop de ciclo AK2 é necessário para uma mensagem válida. (Obrigatório) |
| segmentsCount | Número inteiro | Número de segmentos numa X12 conjunto de transações. (Opcional) |
||||

## <a name="x12-transaction-set-acknowledgement-tracking-schema"></a>X12 confirmação esquemas de controlo do conjunto de transações

```json
{
   "agreementProperties": {
      "senderPartnerName": "",
      "receiverPartnerName": "",
      "senderQualifier": "",
      "senderIdentifier": "",
      "receiverQualifier": "",
      "receiverIdentifier": "",
      "agreementName": ""
   },
   "messageProperties": {
      "direction": "",
      "interchangeControlNumber": "",
      "functionalGroupControlNumber": "",
      "isaSegment": "",
      "gsSegment": "",
      "respondingfunctionalGroupControlNumber": "",
      "respondingFunctionalGroupId": "",
      "respondingtransactionSetControlNumber": "",
      "respondingTransactionSetId": "",
      "statusCode": "",
      "processingStatus": "",
      "CorrelationMessageId": "",
      "isMessageFailed": "",
      "ak2Segment": "",
      "ak3Segment": "",
      "ak5Segment": ""
   }
}
```

| Propriedade | Tipo | Descrição |
| --- | --- | --- |
| senderPartnerName | Cadeia | Nome do parceiro do remetente da mensagem X12. (Opcional) |
| receiverPartnerName | Cadeia | Nome do parceiro do destinatário da mensagem X12. (Opcional) |
| senderQualifier | Cadeia | Envie o qualificador de parceiro. (Obrigatório) |
| senderIdentifier | Cadeia | Envie o identificador de parceiro. (Obrigatório) |
| receiverQualifier | Cadeia | Receba o qualificador de parceiro. (Obrigatório) |
| receiverIdentifier | Cadeia | Recebe o identificador de parceiro. (Obrigatório) |
| agreementName | Cadeia | Nome de X12 contrato para o qual as mensagens são resolvidas. (Opcional) |
| direção | Enum | Direção do fluxo da mensagem, receber ou enviar. (Obrigatório) |
| interchangeControlNumber | Cadeia | Intercâmbio de número de controlo da confirmação funcional. Povoa o valor apenas para o lado de envio onde funcional confirmação é recebida para as mensagens enviadas para o parceiro. (Opcional) |
| functionalGroupControlNumber | Cadeia | Número de controlo do grupo funcional da confirmação funcional. Povoa o valor apenas para o lado de envio onde funcional confirmação é recebida para as mensagens enviadas para o parceiro. (Opcional) |
| isaSegment | Cadeia | Segmento ISA da mensagem. Povoa o valor apenas para o lado de envio onde funcional confirmação é recebida para as mensagens enviadas para o parceiro. (Opcional) |
| gsSegment | Cadeia | Segmento de GS da mensagem. Povoa o valor apenas para o lado de envio onde funcional confirmação é recebida para as mensagens enviadas para o parceiro. (Opcional) |
| respondingfunctionalGroupControlNumber | Cadeia | Número de controlo de intercâmbio de responder. (Opcional) |
| respondingFunctionalGroupId | Cadeia | ID do grupo funcional, que mapeia para AK101 na confirmação de responder. (Opcional) |
| respondingtransactionSetControlNumber | Cadeia | Número de controlo do conjunto de transações está a responder. (Opcional) |
| respondingTransactionSetId | Cadeia | ID, que mapeia para AK201 na confirmação do conjunto de transações está a responder. (Opcional) |
| statusCode | Booleano | Código de estado de confirmação do conjunto de transações. (Obrigatório) |
| segmentsCount | Enum | Código de estado de confirmação. Valores permitidos são **aceite**, **rejeitado**, e **AcceptedWithErrors**. (Obrigatório) |
| processingStatus | Enum | Estado de processamento da confirmação. Valores permitidos são **recebidos**, **Generated**, e **enviados**. (Obrigatório) |
| CorrelationMessageId | Cadeia | ID de mensagem de correlação. Uma combinação de {AgreementName} {*GroupControlNumber*} {TransactionSetControlNumber}. (Opcional) |
| isMessageFailed | Booleano | Se o X12 mensagem de falha. (Obrigatório) |
| ak2Segment | Cadeia | Confirmação de uma transação definido dentro do grupo funcional recebido. (Opcional) |
| ak3Segment | Cadeia | Relatórios de erros num segmento de dados. (Opcional) |
| ak5Segment | Cadeia | Se o conjunto identificadas no segmento do ciclo AK2 de transações é aceites ou rejeitadas e, por isso que os relatórios. (Opcional) |
||||

## <a name="x12-interchange-tracking-schema"></a>Esquema de controlo do intercâmbio de X12

```json
{
   "agreementProperties": {
      "senderPartnerName": "",
      "receiverPartnerName": "",
      "senderQualifier": "",
      "senderIdentifier": "",
      "receiverQualifier": "",
      "receiverIdentifier": "",
      "agreementName": ""
   },
   "messageProperties": {
      "direction": "",
      "interchangeControlNumber": "",
      "isaSegment": "",
      "isTechnicalAcknowledgmentExpected": "",
      "isMessageFailed": "",
      "isa09": "",
      "isa10": "",
      "isa11": "",
      "isa12": "",
      "isa14": "",
      "isa15": "",
      "isa16": ""
   }
}
```

| Propriedade | Tipo | Descrição |
| --- | --- | --- |
| senderPartnerName | Cadeia | Nome do parceiro do remetente da mensagem X12. (Opcional) |
| receiverPartnerName | Cadeia | Nome do parceiro do destinatário da mensagem X12. (Opcional) |
| senderQualifier | Cadeia | Envie o qualificador de parceiro. (Obrigatório) |
| senderIdentifier | Cadeia | Envie o identificador de parceiro. (Obrigatório) |
| receiverQualifier | Cadeia | Receba o qualificador de parceiro. (Obrigatório) |
| receiverIdentifier | Cadeia | Recebe o identificador de parceiro. (Obrigatório) |
| agreementName | Cadeia | Nome de X12 contrato para o qual as mensagens são resolvidas. (Opcional) |
| direção | Enum | Direção do fluxo da mensagem, receber ou enviar. (Obrigatório) |
| interchangeControlNumber | Cadeia | Número de controlo de intercâmbio. (Opcional) |
| isaSegment | Cadeia | Segmento ISA de mensagem. (Opcional) |
| isTechnicalAcknowledgmentExpected | Booleano | Se a confirmação técnica está configurada no X12 contrato. (Obrigatório) |
| isMessageFailed | Booleano | Se o X12 mensagem de falha. (Obrigatório) |
| isa09 | Cadeia | Intercâmbio de X12 documentos data. (Opcional) |
| isa10 | Cadeia | Tempo de intercâmbio de documentos X12. (Opcional) |
| isa11 | Cadeia | Controlo de intercâmbio de X12 identificador de padrões. (Opcional) |
| isa12 | Cadeia | Controlo de intercâmbio de X12 número da versão. (Opcional) |
| isa14 | Cadeia | X12 é solicitada a confirmação. (Opcional) |
| isa15 | Cadeia | Indicador de teste ou produção. (Opcional) |
| isa16 | Cadeia | Separador de elemento. (Opcional) |
||||

## <a name="x12-interchange-acknowledgement-tracking-schema"></a>Esquemas de controlo de reconhecimento do intercâmbio de X12

```json
{
   "agreementProperties": {
      "senderPartnerName": "",
      "receiverPartnerName": "",
      "senderQualifier": "",
      "senderIdentifier": "",
      "receiverQualifier": "",
      "receiverIdentifier": "",
      "agreementName": ""
   },
   "messageProperties": {
      "direction": "",
      "interchangeControlNumber": "",
      "isaSegment": "",
      "respondingInterchangeControlNumber": "",
      "isMessageFailed": "",
      "statusCode": "",
      "processingStatus": "",
      "ta102": "",
      "ta103": "",
      "ta105": ""
   }
}
```

| Propriedade | Tipo | Descrição |
| --- | --- | --- |
| senderPartnerName | Cadeia | Nome do parceiro do remetente da mensagem X12. (Opcional) |
| receiverPartnerName | Cadeia | Nome do parceiro do destinatário da mensagem X12. (Opcional) |
| senderQualifier | Cadeia | Envie o qualificador de parceiro. (Obrigatório) |
| senderIdentifier | Cadeia | Envie o identificador de parceiro. (Obrigatório) |
| receiverQualifier | Cadeia | Receba o qualificador de parceiro. (Obrigatório) |
| receiverIdentifier | Cadeia | Recebe o identificador de parceiro. (Obrigatório) |
| agreementName | Cadeia | Nome de X12 contrato para o qual as mensagens são resolvidas. (Opcional) |
| direção | Enum | Direção do fluxo da mensagem, receber ou enviar. (Obrigatório) |
| interchangeControlNumber | Cadeia | Número de controlo da confirmação técnica recebidos de parceiros de intercâmbio. (Opcional) |
| isaSegment | Cadeia | Segmento ISA para a confirmação de técnica de parceiros é recebido. (Opcional) |
| respondingInterchangeControlNumber |Cadeia | Número de controlo para a confirmação técnica recebidos de parceiros de intercâmbio. (Opcional) |
| isMessageFailed | Booleano | Se o X12 mensagem de falha. (Obrigatório) |
| statusCode | Enum | Intercâmbio do código de estado de confirmação. Valores permitidos são **aceite**, **rejeitado**, e **AcceptedWithErrors**. (Obrigatório) |
| processingStatus | Enum | Estado de confirmação. Valores permitidos são **recebidos**, **Generated**, e **enviados**. (Obrigatório) |
| TA102 | Cadeia | Intercâmbio data. (Opcional) |
| ta103 | Cadeia | Intercâmbio do tempo. (Opcional) |
| ta105 | Cadeia | Intercâmbio do código de nota. (Opcional) |
||||

## <a name="x12-functional-group-tracking-schema"></a>Esquema de controlo de grupo funcional de X12

```json
{
   "agreementProperties": {
      "senderPartnerName": "",
      "receiverPartnerName": "",
      "senderQualifier": "",
      "senderIdentifier": "",
      "receiverQualifier": "",
      "receiverIdentifier": "",
      "agreementName": ""
   },
   "messageProperties": {
      "direction": "",
      "interchangeControlNumber": "",
      "functionalGroupControlNumber": "",
      "gsSegment": "",
      "isTechnicalAcknowledgmentExpected": "",
      "isFunctionalAcknowledgmentExpected": "",
      "isMessageFailed": "",
      "gs01": "",
      "gs02": "",
      "gs03": "",
      "gs04": "",
      "gs05": "",
      "gs07": "",
      "gs08": ""
   }
}
```

| Propriedade | Tipo | Descrição |
| --- | --- | --- |
| senderPartnerName | Cadeia | Nome do parceiro do remetente da mensagem X12. (Opcional) |
| receiverPartnerName | Cadeia | Nome do parceiro do destinatário da mensagem X12. (Opcional) |
| senderQualifier | Cadeia | Envie o qualificador de parceiro. (Obrigatório) |
| senderIdentifier | Cadeia | Envie o identificador de parceiro. (Obrigatório) |
| receiverQualifier | Cadeia | Receba o qualificador de parceiro. (Obrigatório) |
| receiverIdentifier | Cadeia | Recebe o identificador de parceiro. (Obrigatório) |
| agreementName | Cadeia | Nome de X12 contrato para o qual as mensagens são resolvidas. (Opcional) |
| direção | Enum | Direção do fluxo da mensagem, receber ou enviar. (Obrigatório) |
| interchangeControlNumber | Cadeia | Número de controlo de intercâmbio. (Opcional) |
| functionalGroupControlNumber | Cadeia | Número de controle funcional. (Opcional) |
| gsSegment | Cadeia | Segmento de mensagem GS. (Opcional) |
| isTechnicalAcknowledgmentExpected | Booleano | Se a confirmação técnica está configurada no X12 contrato. (Obrigatório) |
| isFunctionalAcknowledgmentExpected | Booleano | Se a confirmação funcional é configurada no X12 contrato. (Obrigatório) |
| isMessageFailed | Booleano | Se o X12 mensagem de falha. (Obrigatório)|
| gs01 | Cadeia | Código de identificação funcional. (Opcional) |
| gs02 | Cadeia | Código do remetente do aplicativo. (Opcional) |
| gs03 | Cadeia | Código do destinatário do aplicativo. (Opcional) |
| gs04 | Cadeia | Data do grupo funcional. (Opcional) |
| gs05 | Cadeia | Hora do grupo funcional. (Opcional) |
| gs07 | Cadeia | Código de agência responsável. (Opcional) |
| gs08 | Cadeia | Código de identificação de versão/versão/da indústria. (Opcional) |
||||

## <a name="x12-functional-group-acknowledgement-tracking-schema"></a>Confirmação de esquemas de controlo de grupo funcional de X12

```json
{
   "agreementProperties": {
      "senderPartnerName": "",
      "receiverPartnerName": "",
      "senderQualifier": "",
      "senderIdentifier": "",
      "receiverQualifier": "",
      "receiverIdentifier": "",
      "agreementName": ""
   },
   "messageProperties": {
      "direction": "",
      "interchangeControlNumber": "",
      "functionalGroupControlNumber": "",
      "isaSegment": "",
      "gsSegment": "",
      "respondingfunctionalGroupControlNumber": "",
      "respondingFunctionalGroupId": "",
      "isMessageFailed": "",
      "statusCode": "",
      "processingStatus": "",
      "ak903": "",
      "ak904": "",
      "ak9Segment": ""
   }
}
```

| Propriedade | Tipo | Descrição |
| --- | --- | --- |
| senderPartnerName | Cadeia | Nome do parceiro do remetente da mensagem X12. (Opcional) |
| receiverPartnerName | Cadeia | Nome do parceiro do destinatário da mensagem X12. (Opcional) |
| senderQualifier | Cadeia | Envie o qualificador de parceiro. (Obrigatório) |
| senderIdentifier | Cadeia | Envie o identificador de parceiro. (Obrigatório) |
| receiverQualifier | Cadeia | Receba o qualificador de parceiro. (Obrigatório) |
| receiverIdentifier | Cadeia | Recebe o identificador de parceiro. (Obrigatório) |
| agreementName | Cadeia | Nome de X12 contrato para o qual as mensagens são resolvidas. (Opcional) |
| direção | Enum | Direção do fluxo da mensagem, receber ou enviar. (Obrigatório) |
| interchangeControlNumber | Cadeia | Número de controlo do intercâmbio, que preenche para o lado de envio, quando uma técnica confirmação é recebida de parceiros. (Opcional) |
| functionalGroupControlNumber | Cadeia | Número de controlo do grupo funcional da confirmação técnico, que preenche para o lado de envio, quando uma técnica confirmação é recebida de parceiros. (Opcional) |
| isaSegment | Cadeia | Mesmo que o intercâmbio de controlar o número, mas preenchida apenas em casos específicos. (Opcional) |
| gsSegment | Cadeia | Mesmo que o grupo funcional controlar o número, mas preenchida apenas em casos específicos. (Opcional) |
| respondingfunctionalGroupControlNumber | Cadeia | Número de controlo do grupo funcional original. (Opcional) |
| respondingFunctionalGroupId | Cadeia | ID do Maps para AK101 no grupo funcional de confirmação. (Opcional) |
| isMessageFailed | Booleano | Se o X12 mensagem de falha. (Obrigatório) |
| statusCode | Enum | Código de estado de confirmação. Valores permitidos são **aceite**, **rejeitado**, e **AcceptedWithErrors**. (Obrigatório) |
| processingStatus | Enum | Estado de processamento da confirmação. Valores permitidos são **recebidos**, **Generated**, e **enviados**. (Obrigatório) |
| ak903 | Cadeia | Número de conjuntos de transação recebido. (Opcional) |
| ak904 | Cadeia | Número de conjuntos de transações aceites no grupo funcional identificado. (Opcional) |
| ak9Segment | Cadeia | Se o grupo funcional identificado no segmento AK1 é aceites ou rejeitado e por quê. (Opcional) |
|||| 

## <a name="b2b-protocol-tracking-schemas"></a>Esquemas de controlo de protocolo de B2B

Para obter informações sobre esquemas de controlo de protocolo de B2B, consulte:

* [Esquemas de controlo de AS2](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)   
* [B2B personalizadas de esquemas de controlo](logic-apps-track-integration-account-custom-tracking-schema.md)

## <a name="next-steps"></a>Passos Seguintes

* Saiba mais sobre [monitorizar mensagens B2B](logic-apps-monitor-b2b-message.md).
* Saiba mais sobre [mensagens B2B no Log Analytics de controlo](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).
