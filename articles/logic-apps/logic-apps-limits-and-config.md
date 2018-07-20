---
title: Limites e configuração - Azure Logic Apps | Documentos da Microsoft
description: Valores de configuração para o Azure Logic Apps e limites do serviço
services: logic-apps
author: ecfan
manager: jeconnoc
ms.author: estfan
ms.topic: article
ms.date: 05/30/2018
ms.service: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: 7726d7eae2d9b3a37ffc141e4a4cbdc9a1af459c
ms.sourcegitcommit: 727a0d5b3301fe20f20b7de698e5225633191b06
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/19/2018
ms.locfileid: "39145042"
---
# <a name="limits-and-configuration-information-for-azure-logic-apps"></a>Limites e informações de configuração para o Azure Logic Apps

Este artigo descreve os limites e os detalhes de configuração para criar e executar fluxos de trabalho automatizados com o Azure Logic Apps. Para o Microsoft Flow, veja [limites e configuração no Microsoft Flow](https://docs.microsoft.com/flow/limits-and-config).

<a name="definition-limits"></a>

## <a name="definition-limits"></a>Limites de definição

Aqui estão os limites para uma definição de aplicação lógica única:

| Nome | Limite | Notas | 
| ---- | ----- | ----- | 
| Ações por fluxo de trabalho | 500 | Para expandir este limite, pode adicionar fluxos de trabalho aninhados conforme necessário. |
| Profundidade de aninhamento para ações de permissão | 8 | Para expandir este limite, pode adicionar fluxos de trabalho aninhados conforme necessário. | 
| Fluxos de trabalho por região por subscrição | 1,000 | | 
| Acionadores por fluxo de trabalho | 10 | Ao trabalhar na vista de código, não o designer | 
| Limite de casos de âmbito do comutador | 25 | | 
| Variáveis por fluxo de trabalho | 250 | | 
| Carateres por expressão | 8,192 | | 
| Tamanho máximo de `trackedProperties` | 16.000 carateres | 
| Dê um nome para `action` ou `trigger` | 80 carateres | | 
| Comprimento de `description` | 256 carateres | | 
| Máximo `parameters` | 50 | | 
| Máximo `outputs` | 10 | | 
||||  

<a name="run-duration-retention-limits"></a>

## <a name="run-duration-and-retention-limits"></a>Limites de retenção e a duração da execução

Aqui estão os limites para uma execução da aplicação lógica única:

| Nome | Limite | Notas | 
|------|-------|-------| 
| Duração de execução | 90 dias | Para alterar este limite, consulte [a duração da execução de alteração](#change-duration). | 
| Retenção de armazenamento | hora de início de 90 dias a partir da execução | Para alterar este limite, consulte [alterar a retenção de armazenamento](#change-retention). | 
| Intervalo de periodicidade mínimo | 1 segundo | | 
| Intervalo de periodicidade máximo | dias de 500 | | 
|||| 

<a name="change-duration"></a>
<a name="change-retention"></a>

### <a name="change-run-duration-and-storage-retention"></a>Alterar a retenção de armazenamento e a duração da execução

Pode alterar este limite para um valor entre 7 dias e 90 dias. Fiquem acima do limite máximo, [contacte a equipa de Logic Apps](mailto://logicappsemail@microsoft.com) para obter ajuda com os seus requisitos.

1. No portal do Azure, no menu da sua aplicação lógica, escolha **definições de fluxo de trabalho**. 

2. Sob **opções de tempo de execução**, da **executar retenção do histórico em dias** lista, escolha **personalizado**. 

3. Introduza ou arraste o controlo de deslize para o número de dias em que quer.

<a name="looping-debatching-limits"></a>

## <a name="looping-and-debatching-limits"></a>Um loop e divisões de limites

Aqui estão os limites para uma execução da aplicação lógica única:

| Nome | Limite | Notas | 
| ---- | ----- | ----- | 
| Iterações until | 5.000 | | 
| Itens ForEach | 100.000 | Pode utilizar o [ação de consulta](../connectors/connectors-native-query.md) para filtrar matrizes maiores, conforme necessário. | 
| Paralelismo ForEach | 50 | A predefinição é 20. <p>Para alterar esse nível de predefinição num loop ForEach, defina o `runtimeConfiguration` propriedade o `foreach` ação. <p>Para executar sequencialmente um loop ForEach, defina o `operationOptions` propriedade como "Sequencial" no `foreach` ação. | 
| Itens SplitOn | 100.000 | | 
|||| 

<a name="throughput-limits"></a>

## <a name="throughput-limits"></a>Limites de débito

Aqui estão os limites para uma execução da aplicação lógica única:

| Nome | Limite | Notas | 
| ----- | ----- | ----- | 
| Execuções de ações por 5 minutos | 100.000 | Para aumentar o limite para 300.000, pode executar uma aplicação lógica no `High Throughput` modo. Para configurar em modo de alto débito, o `runtimeConfiguration` de recurso do fluxo de trabalho, defina o `operationOptions` propriedade para `OptimizedForHighThroughput`. <p>**Tenha em atenção**: modo de alto débito está em pré-visualização. Além disso, pode distribuir uma carga de trabalho em mais do que uma aplicação conforme necessário. | 
| Chamadas de saída simultâneas de ações | ~2,500 | Reduza o número de pedidos simultâneos ou reduza a duração, conforme necessário. | 
| Ponto final de tempo de execução: chamadas simultâneas de entrada | ~1,000 | Reduza o número de pedidos simultâneos ou reduza a duração, conforme necessário. | 
| Ponto final de tempo de execução: ler chamadas por 5 minutos  | 60,000 | Pode distribuir a carga de trabalho em mais do que uma aplicação conforme necessário. | 
| Ponto final de tempo de execução: invocar chamadas por 5 minutos| 45,000 | Pode distribuir a carga de trabalho em mais do que uma aplicação conforme necessário. | 
|||| 

Para passar destes limites no processamento normal ou executar o teste de carga que pode passar destes limites, [contacte a equipa de Logic Apps](mailto://logicappsemail@microsoft.com) para obter ajuda com os seus requisitos.

<a name="request-limits"></a>

## <a name="http-request-limits"></a>Limites de pedido de HTTP

Aqui estão os limites para uma única solicitação HTTP ou uma chamada síncrona de conector:

#### <a name="timeout"></a>Tempo Limite (excedido)

Algumas operações de conector fazem chamadas assíncronas ou escutam os pedidos de webhook, portanto, o tempo limite para essas operações pode ser maior do que estes limites. Para obter mais informações, consulte os detalhes técnicos para o conector específico e também [acionadores de fluxo de trabalho e as ações](../logic-apps/logic-apps-workflow-actions-triggers.md#http-action).

| Nome | Limite | Notas | 
| ---- | ----- | ----- | 
| Pedido de saída | 120 Segundos | Para mais operações em execução, utilize um [padrão de consulta assíncrona](../logic-apps/logic-apps-create-api-app.md#async-pattern) ou uma [até que o loop](../logic-apps/logic-apps-workflow-actions-triggers.md#until-action). | 
| Resposta síncrona | 120 Segundos | Para a solicitação original obter a resposta, todos os passos na resposta deverá ser concluída dentro do limite, a menos que chamar outra aplicação de lógica como um fluxo de trabalho aninhado. Para obter mais informações, consulte [chamar, acionar, ou aninhar aplicações lógicas](../logic-apps/logic-apps-http-endpoint.md). | 
|||| 

#### <a name="message-size"></a>Tamanho da mensagem

| Nome | Limite | Notas | 
| ---- | ----- | ----- | 
| Tamanho da mensagem | 100 MB | Para contornar este limite, consulte [processar mensagens grandes com a segmentação](../logic-apps/logic-apps-handle-large-messages.md). No entanto, algumas APIs e conectores podem não suportar a segmentação ou até mesmo o limite predefinido. | 
| Tamanho de mensagem com a segmentação | 1 GB | Este limite aplica-se a ações que nativamente suportam segmentação ou permitem-lhe ativar a divisão na respetiva configuração de tempo de execução. Para obter mais informações, consulte [processar mensagens grandes com a segmentação](../logic-apps/logic-apps-handle-large-messages.md). | 
| Limite de avaliação da expressão | 131,072 carateres | O `@concat()`, `@base64()`, `@string()` expressões não podem ser superior a este limite. | 
|||| 

#### <a name="retry-policy"></a>Política de repetição

| Nome | Limite | Notas | 
| ---- | ----- | ----- | 
| Tentativas de repetição | 90 | A predefinição é 4. Para alterar a predefinição, utilize o [parâmetro de política de repetição](../logic-apps/logic-apps-workflow-actions-triggers.md). | 
| Intervalo máx. de repetição | 1 dia | Para alterar a predefinição, utilize o [parâmetro de política de repetição](../logic-apps/logic-apps-workflow-actions-triggers.md). | 
| Intervalo mínimo de repetição | 5 segundos | Para alterar a predefinição, utilize o [parâmetro de política de repetição](../logic-apps/logic-apps-workflow-actions-triggers.md). |
|||| 

<a name="custom-connector-limits"></a>

## <a name="custom-connector-limits"></a>Limites de conector personalizado

Aqui estão os limites dos conectores personalizados que pode criar a partir de web APIs.

| Nome | Limite | 
| ---- | ----- | 
| Número de conectores personalizados | 1000 por subscrição do Azure | 
| Número de pedidos por minuto para cada ligação criada por um conector personalizado | 500 pedidos por ligação |
|||| 

<a name="integration-account-limits"></a>

## <a name="integration-account-limits"></a>Limites de conta de integração

<a name="artifact-number-limits"></a>

### <a name="artifact-limits-per-integration-account"></a>Limites de artefacto por conta de integração

Aqui estão os limites no número de artefactos para cada conta de integração. Para obter mais informações, consulte [preços de aplicações lógicas](https://azure.microsoft.com/pricing/details/logic-apps/). 

*Escalão gratuito*

Utilize o escalão gratuito apenas para cenários de exploratórios, não os cenários de produção. Este escalão restringe o débito e a utilização e não tem nenhum contrato de nível de serviço (SLA).

| Artefacto | Limite | Notas | 
|----------|-------|-------| 
| Parceiros comerciais EDI | 25 | | 
| Contratos comerciais de EDI | 10 | | 
| Maps | 25 | | 
| Esquemas | 25 | 
| Assemblagens | 10 | | 
| Configurações de batch | 5 | 
| Certificados | 25 | | 
|||| 

*Escalão básico*

| Artefacto | Limite | Notas | 
|----------|-------|-------| 
| Parceiros comerciais EDI | 2 | | 
| Contratos comerciais de EDI | 1 | | 
| Maps | 500 | | 
| Esquemas | 500 | 
| Assemblagens | 25 | | 
| Configurações de batch | 1 | | 
| Certificados | 2 | | 
|||| 

*Escalão Standard*

| Artefacto | Limite | Notas | 
|----------|-------|-------| 
| Parceiros comerciais EDI | 500 | | 
| Contratos comerciais de EDI | 500 | | 
| Maps | 500 | | 
| Esquemas | 500 | 
| Assemblagens | 50 | | 
| Configurações de batch | 5 |  
| Certificados | 50 | | 
|||| 

<a name="artifact-capacity-limits"></a>

### <a name="artifact-capacity-limits"></a>Limites de capacidade de artefacto

| Nome | Limite | Notas | 
| ---- | ----- | ----- | 
| Esquema | 8 MB | Para carregar ficheiros maiores do que 2 MB, utilize o [URI de blob](../logic-apps/logic-apps-enterprise-integration-schemas.md). | 
| Mapa (ficheiro XSLT) | 2 MB | | 
| Ponto final de tempo de execução: ler chamadas por 5 minutos | 60,000 | É possível distribuir a carga de trabalho em mais de uma conta conforme necessário. | 
| Ponto final de tempo de execução: invocar chamadas por 5 minutos | 45,000 | É possível distribuir a carga de trabalho em mais de uma conta conforme necessário. | 
| Ponto final de tempo de execução: controlo de chamadas por 5 minutos | 45,000 | É possível distribuir a carga de trabalho em mais de uma conta conforme necessário. | 
| Ponto final de tempo de execução: bloquear chamadas simultâneas | ~1,000 | Pode reduzir o número de pedidos simultâneos ou reduza a duração, se necessário. | 
||||  

<a name="b2b-protocol-limits"></a>

### <a name="b2b-protocol-as2-x12-edifact-message-size"></a>Tamanho da mensagem de protocolo de B2B (AS2, X12, EDIFACT)

Aqui estão os limites que se aplicam aos protocolos B2B:

| Nome | Limite | Notas | 
| ---- | ----- | ----- | 
| AS2 | 50 MB | Aplica-se a descodificar e codificar | 
| X12 | 50 MB | Aplica-se a descodificar e codificar | 
| EDIFACT | 50 MB | Aplica-se a descodificar e codificar | 
|||| 

<a name="configuration"></a>

## <a name="configuration-ip-addresses"></a>Configuração: Endereços IP

### <a name="azure-logic-apps-service"></a>Serviço de aplicações lógicas do Azure

Todas as aplicações de lógica numa região, utilize os mesmos intervalos de endereços IP. Para suportar as chamadas que as logic apps permitem diretamente com [HTTP](../connectors/connectors-native-http.md), [HTTP + Swagger](../connectors/connectors-native-http-swagger.md)e outros pedidos HTTP, configurados as configurações de firewall para que eles incluem estes endereços IP saídos e entrados, com base em no onde existem as logic apps:

| Região de aplicações lógicas | IP de saída |
|-------------------|-------------|
| Leste da Austrália | 13.75.149.4, 104.210.91.55, 104.210.90.241 |
| Sudeste da Austrália | 13.73.114.207, 13.77.3.139, 13.70.159.205 |
| Sul do Brasil | 191.235.82.221, 191.235.91.7, 191.234.182.26 |
| Canadá Central | 52.233.29.92, 52.228.39.241, 52.228.39.244 |
| Leste do Canadá | 52.232.128.155, 52.229.120.45, 52.229.126.25 |
| Índia Central | 52.172.154.168, 52.172.186.159, 52.172.185.79 |
| EUA Central | 13.67.236.125, 104.208.25.27, 40.122.170.198 |
| Ásia Oriental | 13.75.94.173, 40.83.127.19, 52.175.33.254 |
| EUA Leste | 13.92.98.111, 40.121.91.41, 40.114.82.191 |
| EUA Leste 2 | 40.84.30.147, 104.208.155.200, 104.208.158.174 |
| Leste do Japão | 13.71.158.3, 13.73.4.207, 13.71.158.120 |
| Oeste do Japão | 40.74.140.4, 104.214.137.243, 138.91.26.45 |
| EUA Centro-Norte | 168.62.248.37, 157.55.210.61, 157.55.212.238 |
| Europa do Norte | 40.113.12.95, 52.178.165.215, 52.178.166.21 |
| EUA Centro-Sul | 104.210.144.48, 13.65.82.17, 13.66.52.232 |
| Sul da Índia | 52.172.50.24, 52.172.55.231, 52.172.52.0 |
| Sudeste Asiático | 13.76.133.155, 52.163.228.93, 52.163.230.166 |
| EUA Centro-Oeste | 52.161.27.190, 52.161.18.218, 52.161.9.108 |
| Europa Ocidental | 40.68.222.65, 40.68.209.23, 13.95.147.65 |
| Índia Ocidental | 104.211.164.80, 104.211.162.205, 104.211.164.136 |
| EUA Oeste | 52.160.92.112, 40.118.244.241, 40.118.241.243 |
| EUA Oeste 2 | 13.66.210.167, 52.183.30.169, 52.183.29.132 |
| Reino Unido Sul | 51.140.74.14, 51.140.73.85, 51.140.78.44 |
| Reino Unido Oeste | 51.141.54.185, 51.141.45.238, 51.141.47.136 |
| | |

| Região de aplicações lógicas | IP de entrada |
|-------------------|------------|
| Leste da Austrália | 3.75.153.66, 104.210.89.222, 104.210.89.244 |
| Sudeste da Austrália | 13.73.115.153, 40.115.78.70, 40.115.78.237 |
| Sul do Brasil | 191.235.86.199, 191.235.95.229, 191.235.94.220 |
| Canadá Central | 13.88.249.209, 52.233.30.218, 52.233.29.79 |
| Leste do Canadá | 52.232.129.143, 52.229.125.57, 52.232.133.109 |
| Índia Central | 52.172.157.194, 52.172.184.192, 52.172.191.194 |
| EUA Central | 13.67.236.76, 40.77.111.254, 40.77.31.87 |
| Ásia Oriental | 168.63.200.173, 13.75.89.159, 23.97.68.172 |
| EUA Leste | 137.135.106.54, 40.117.99.79, 40.117.100.228 |
| EUA Leste 2 | 40.84.25.234, 40.79.44.7, 40.84.59.136 |
| Leste do Japão | 13.71.146.140, 13.78.84.187, 13.78.62.130 |
| Oeste do Japão | 40.74.140.173, 40.74.81.13, 40.74.85.215 |
| EUA Centro-Norte | 168.62.249.81, 157.56.12.202, 65.52.211.164 |
| Europa do Norte | 13.79.173.49, 52.169.218.253, 52.169.220.174 |
| EUA Centro-Sul | 52.172.9.47, 52.172.49.43, 52.172.51.140 |
| Sul da Índia | 52.172.9.47, 52.172.49.43, 52.172.51.140 |
| Sudeste Asiático | 52.163.93.214, 52.187.65.81, 52.187.65.155 |
| EUA Centro-Oeste | 52.161.26.172, 52.161.8.128, 52.161.19.82 |
| Europa Ocidental | 13.95.155.53, 52.174.54.218, 52.174.49.6 |
| Índia Ocidental | 104.211.164.112, 104.211.165.81, 104.211.164.25 |
| EUA Oeste | 52.160.90.237, 138.91.188.137, 13.91.252.184 |
| EUA Oeste 2 | 13.66.224.169, 52.183.30.10, 52.183.39.67 |
| Reino Unido Sul | 51.140.79.109, 51.140.78.71, 51.140.84.39 |
| Reino Unido Oeste | 51.141.48.98, 51.141.51.145, 51.141.53.164 |
| | |

### <a name="connectors"></a>Conectores

Para suportar as chamadas que [conectores](../connectors/apis-list.md) make, configurar as configurações de firewall para que eles incluem estes endereços IP de saída, com base nas regiões onde existem as logic apps.

> [!IMPORTANT]
>
> Se tiver configurações existentes, atualize-los **logo que possível antes de 1 de Setembro de 2018** para que incluem e correspondem aos endereços IP nesta lista para as regiões onde existem as logic apps. 

| Região de aplicações lógicas | IP de saída | 
|-------------------|-------------|  
| Leste da Austrália | 13.70.72.192 - 13.70.72.207, 13.72.243.10, 40.126.251.213 | 
| Sudeste da Austrália | 13.77.50.240 - 13.77.50.255, 13.70.136.174, 40.127.80.34 | 
| Sul do Brasil | 191.233.203.192 - 191.233.203.207, 104.41.59.51, 191.232.38.129 | 
| Canadá Central | 13.71.170.208 - 13.71.170.223, 13.71.170.224 - 13.71.170.239, 52.237.24.126, 52.233.31.197, 52.228.42.205, 52.228.33.76, 52.228.34.13, 52.233.26.83 | 
| Leste do Canadá | 40.69.106.240 - 40.69.106.255, 52.242.35.152, 52.229.123.98, 52.229.120.178, 52.229.126.202, 52.229.120.52, 52.229.120.131 | 
| Índia Central | 104.211.81.192 - 104.211.81.207, 52.172.211.12, 104.211.98.164 | 
| EUA Central | 13.89.171.80 - 13.89.171.95, 52.173.245.164, 40.122.49.51 | 
| Ásia Oriental | 13.75.36.64 - 13.75.36.79, 52.175.23.169, 23.99.116.181 | 
| EUA Leste | 40.71.11.80 - 40.71.11.95, 40.71.249.205, 191.237.41.52 | 
| EUA Leste 2 | 40.70.146.208 - 40.70.146.223, 52.232.188.154, 104.208.233.100 | 
| Leste do Japão | 13.78.108.0 - 13.78.108.15, 13.71.153.19, 40.115.186.96 | 
| Oeste do Japão | 40.74.100.224 - 40.74.100.239, 104.215.61.248, 40.74.130.77 | 
| EUA Centro-Norte | 52.162.107.160 - 52.162.107.175, 52.162.242.161, 65.52.218.230 | 
| Europa do Norte | 13.69.227.208 - 13.69.227.223, 52.178.150.68, 104.45.93.9 | 
| EUA Centro-Sul | 104.214.19.48 - 104.214.19.63, 13.65.86.57, 104.214.70.191 | 
| Sul da Índia | 40.78.194.240 - 40.78.194.255, 13.71.125.22, 104.211.227.225 | 
| Sudeste Asiático | 13.67.8.240 - 13.67.8.255, 52.187.68.19, 13.76.231.68 | 
| EUA Centro-Oeste | 13.71.195.32 - 13.71.195.47, 52.161.102.22, 52.161.27.108, 52.161.30.5, 52.161.29.35, 52.161.26.212, 52.161.128 | 
| Europa Ocidental | 13.69.64.208 - 13.69.64.223, 52.174.88.118, 40.115.50.13 | 
| Índia Ocidental | 104.211.146.224 - 104.211.146.239, 104.211.189.218, 104.211.161.203 | 
| EUA Oeste | 40.112.243.160 - 40.112.243.175, 104.42.122.49, 104.40.51.248 | 
| EUA Oeste 2 | 13.66.140.128 - 13.66.140.143, 52.183.78.157, 13.66.225.219, 13.66.218.78, 13.66.220.135, 13.66.219.14, 13.66.221.19 | 
| Reino Unido Sul | 51.140.148.0 - 51.140.148.15, 51.140.80.51 | 
| Reino Unido Oeste | 51.140.211.0 - 51.140.211.15, 51.141.47.105 | 
| | | 

## <a name="next-steps"></a>Passos Seguintes  

* Saiba como [criar a sua primeira aplicação lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md)  
* Saiba mais sobre [exemplos e cenários comuns](../logic-apps/logic-apps-examples-and-scenarios.md)
