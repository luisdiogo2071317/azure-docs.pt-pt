---
title: incluir ficheiro
description: incluir ficheiro
services: log-analytics
author: MGoedtel
ms.service: log-analytics
ms.topic: include
ms.date: 05/16/2018
ms.author: magoedte
ms.custom: include file
ms.openlocfilehash: 66cd09df128d454973d008adf4ffc5dd1017a18f
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38756043"
---
Os seguintes limites aplicam-se aos recursos do Log Analytics por subscrição:

| Recurso | Limite Predefinido | Comentários
| --- | --- | --- |
| Número de áreas de trabalho gratuitas por subscrição | 10 | Não pode aumentar este limite. |
| Número de áreas de trabalho pagas por subscrição | N/A | Está limitado pelo número de recursos dentro de um grupo de recursos e pelo número de grupos de recursos por subscrição | 

>[!NOTE]
>A partir de 2 de Abril de 2018, novas áreas de trabalho numa nova subscrição utilizará automaticamente o *por GB* plano de preços.  Para as subscrições existentes criadas antes 2 de Abril, ou uma subscrição que estava associada a uma inscrição EA já existente, pode continuar a escolher entre três escalões de preço para novas áreas de trabalho. 
>

Os limites seguintes aplicam-se a cada área de trabalho do Log Analytics:

|  | Gratuito | Standard | Premium | Autónomo | OMS | Por GB |
| --- | --- | --- | --- | --- | --- |--- |
| Volume de dados recolhido por dia |500 MB<sup>1</sup> |Nenhuma |Nenhuma | Nenhuma | Nenhuma | Nenhuma
| Período de retenção de dados |7 dias |1 mês |12 meses | 1 mês<sup>2</sup> | 1 mês <sup>2</sup>| 1 mês <sup>2</sup>|

<sup>1</sup> Quando os clientes atingem o limite diário de transferência de dados de 500 MB, a análise de dados é parada e retomada no início do dia seguinte. Os dias são baseados no fuso horário UTC.

<sup>2</sup> o período de retenção de dados para o autónomo, OMS e planos de preços por GB, pode ser aumentado para 730 dias.

| Categoria | Limites | Comentários
| --- | --- | --- |
| API do Recoletor de Dados | O tamanho máximo para um post individual é 30 MB<br>O tamanho máximo para os valores de campos é 32 KB | Dividir volumes maiores em vários posts<br>Os campos com mais de 32 KB são truncados. |
| API de Pesquisa | 5000 registos devolvidos para dados não agregados<br>500 000 registos para dados agregados | Os dados agregados são uma pesquisa que inclui o comando `summarize`
 
