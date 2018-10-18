---
title: Filtros de ligação de IP de Hub IoT do Azure | Documentos da Microsoft
description: Como utilizar o IP de filtragem para bloquear as conexões de endereços IP específicos ao seu hub IoT do Azure. Pode bloquear ligações a partir de individuais ou intervalos de endereços IP.
author: rezasherafat
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 05/23/2017
ms.author: rezas
ms.openlocfilehash: 903f8284327d3d5b9ef386305a436ce44a8a11b2
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/17/2018
ms.locfileid: "49378107"
---
# <a name="use-ip-filters"></a>Utilizar filtros de IP

A segurança é um aspecto importante de qualquer solução de IoT com base no IoT Hub do Azure. Às vezes precisa especificar explicitamente os endereços IP a partir do qual podem ligar dispositivos como parte da sua configuração de segurança. O *filtro IP* funcionalidade permite-lhe configurar regras para rejeitar ou aceitar o tráfego de endereços de IPv4 específicos.

## <a name="when-to-use"></a>Quando utilizar

Existem dois casos de utilização específicos quando ela é útil bloquear os pontos de extremidade do IoT Hub para determinados endereços IP:

* Seu IoT hub deve receber o tráfego apenas a partir de um especificado intervalo de endereços IP e rejeitar tudo o resto. Por exemplo, estiver a utilizar o hub IoT com [Azure Express Route](https://azure.microsoft.com/documentation/articles/expressroute-faqs/#supported-services) para criar ligações privadas entre um hub IoT e a sua infraestrutura no local.

* Precisa recusar tráfego a partir de endereços IP que tenham sido identificados como sendo suspeitas pelo administrador de hub IoT.

## <a name="how-filter-rules-are-applied"></a>Como são aplicadas as regras de filtro

As regras de filtro IP são aplicadas ao nível de serviço do IoT Hub. Por isso as regras de filtro IP aplicam-se a todas as ligações de dispositivos e aplicações de back-end usando todos os protocolos suportados.

Qualquer tentativa de ligação de um endereço IP que corresponde a uma regra IP rejeitando no seu hub IoT recebe um código de 401 status não autorizado e a descrição. A mensagem de resposta não menciona a regra IP.

## <a name="default-setting"></a>Definição predefinida

Por predefinição, o **filtro IP** grelha no portal para um hub IoT está vazia. Essa configuração padrão significa que o seu hub aceita ligações a partir de qualquer endereço IP. Essa configuração padrão é equivalente a uma regra que aceita o intervalo de endereços IP 0.0.0.0/0.

![Configurações de filtro IP de padrão de IoT Hub](./media/iot-hub-ip-filtering/ip-filter-default.png)

## <a name="add-or-edit-an-ip-filter-rule"></a>Adicionar ou editar uma regra de filtro IP

Ao adicionar uma regra de filtro IP, lhe for pedido para os seguintes valores:

* Uma **nome da regra de filtro IP** que tem de ser uma cadeia de caracteres exclusiva, maiúsculas de minúsculas, de alfanumérica até 128 carateres de comprimento. Apenas os ASCII de 7 bits carateres alfanuméricos adição `{'-', ':', '/', '\', '.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '''}` são aceites.

* Selecione um **rejeitar** ou **aceitar** como o **ação** para a regra de filtro IP.

* Fornece um único endereço IPv4 ou um bloco de endereços IP na notação CIDR. Por exemplo, no CIDR notação 192.168.100.0/22 representa endereços 1024 IPv4 de 192.168.100.0 para 192.168.103.255.

![Adicionar uma regra de filtro IP para um hub IoT](./media/iot-hub-ip-filtering/ip-filter-add-rule.png)

Depois de guardar a regra, verá um alerta notificando-o de que a atualização está em curso.

![Notificação sobre como guardar uma regra de filtro IP](./media/iot-hub-ip-filtering/ip-filter-save-new-rule.png)

O **adicionar** opção é desativada quando atingir o máximo de 10 regras de filtro IP.

Pode editar uma regra existente clicando duas vezes na linha que contém a regra.

> [!NOTE]
> Rejeitar IP endereços podem impedir que outros serviços do Azure (por exemplo, o Azure Stream Analytics, máquinas virtuais do Azure ou o Device Explorer no portal) de interagir com o hub IoT.

> [!WARNING]
> Se utilizar o Azure Stream Analytics (ASA) para ler mensagens a partir de um hub IoT com a filtragem de IP ativado, utilize o nome compatível com o Event Hub e o ponto final do IoT Hub na cadeia de ligação de ASA.

## <a name="delete-an-ip-filter-rule"></a>Eliminar uma regra de filtro IP

Para eliminar uma regra de filtro IP, selecione uma ou mais regras na grade e clique em **eliminar**.

![Eliminar uma regra de filtro de IP de Hub IoT](./media/iot-hub-ip-filtering/ip-filter-delete-rule.png)

## <a name="ip-filter-rule-evaluation"></a>Avaliação da regra de filtro IP

Regras do filtro de IP serão aplicadas por ordem e a primeira regra que corresponde ao endereço IP determina a ação de aceitar ou rejeitar.

Por exemplo, se pretender aceitar endereços no 192.168.100.0/22 intervalo e rejeitar tudo o resto, a primeira regra na grade deve aceitar o endereço intervalo 192.168.100.0/22. A seguinte regra deve rejeitar a todos os endereços mediante a utilização do intervalo 0.0.0.0/0.

Pode alterar a ordem das suas regras de filtro IP na grade ao clicar nos três pontos verticais no início de uma linha e através de arrastar e soltar.

Para guardar o seu pedido de nova regra de filtro IP, clique em **guardar**.

![Alterar a ordem das suas regras de filtro de IP de Hub IoT](./media/iot-hub-ip-filtering/ip-filter-rule-order.png)

## <a name="next-steps"></a>Passos Seguintes

Para explorar ainda mais os recursos do IoT Hub, veja:

* [Monitorização de operações](iot-hub-operations-monitoring.md)
* [Métricas do IoT Hub](iot-hub-metrics.md)