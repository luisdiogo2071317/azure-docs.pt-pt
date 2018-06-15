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
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/17/2018
ms.locfileid: "34307493"
---
Os seguintes limites aplicam-se aos recursos do Log Analytics por subscrição:

| Recurso | Limite Predefinido | Comentários
| --- | --- | --- |
| Número de áreas de trabalho gratuitas por subscrição | 10 | Não pode aumentar este limite. |
| Número de áreas de trabalho pagas por subscrição | N/A | Está limitado pelo número de recursos dentro de um grupo de recursos e pelo número de grupos de recursos por subscrição | 

>[!NOTE]
>A partir de 2 de Abril de 2018, áreas de trabalho nova numa nova subscrição irão utilizar automaticamente o *por GB* plano de preços.  Para as subscrições existentes criadas antes 2 de Abril, ou uma subscrição que foi ligada a uma inscrição EA existente, pode continuar a escolher entre os três escalões de preços para áreas de trabalho de novo. 
>

Os limites seguintes aplicam-se a cada área de trabalho do Log Analytics:

|  | Gratuito | Standard | Premium | Autónomo | OMS | Por GB |
| --- | --- | --- | --- | --- | --- |--- |
| Volume de dados recolhido por dia |500 MB<sup>1</sup> |Nenhuma |Nenhuma | Nenhuma | Nenhuma | Nenhuma
| Período de retenção de dados |7 dias |1 mês |12 meses | 1 mês<sup>2</sup> | 1 mês <sup>2</sup>| 1 mês <sup>2</sup>|

<sup>1</sup> Quando os clientes atingem o limite diário de transferência de dados de 500 MB, a análise de dados é parada e retomada no início do dia seguinte. Os dias são baseados no fuso horário UTC.

<sup>2</sup> o período de retenção de dados para o autónomo, OMS e por GB planos de preços pode ser aumentado para 730 dias.

| Categoria | Limites | Comentários
| --- | --- | --- |
| API do Recoletor de Dados | O tamanho máximo para um post individual é 30 MB<br>O tamanho máximo para os valores de campos é 32 KB | Dividir volumes maiores em vários posts<br>Os campos com mais de 32 KB são truncados. |
| API de Pesquisa | 5000 registos devolvidos para dados não agregados<br>500 000 registos para dados agregados | Os dados agregados são uma pesquisa que inclui o comando `summarize`
 
