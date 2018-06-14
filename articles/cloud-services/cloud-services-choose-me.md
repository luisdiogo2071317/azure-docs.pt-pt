---
title: Opções de computação do Azure - Cloud Services do Azure | Microsoft Docs
description: 'Saiba mais sobre as opções e como funcionam de alojamento de computação do Azure: serviço de aplicações, Cloud Services do Azure e máquinas virtuais'
services: cloud-services
documentationcenter: ''
author: Thraka
manager: timlt
ms.assetid: ed7ad348-6018-41bb-a27d-523accd90305
ms.service: multiple
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: adegeo
ms.openlocfilehash: 2871a8c02db0ffc6d9033724e7c9f4a454afef8e
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/09/2018
ms.locfileid: "29120289"
---
# <a name="should-i-choose-azure-cloud-services-or-something-else"></a>Devo escolher Cloud Services do Azure ou outra coisa?
É a escolha de Cloud Services do Azure para si? O Azure oferece diferentes modelos de alojamento para a execução de aplicações. Cada um deles fornece um conjunto diferente de serviços. Qual deles escolher depende exatamente o que está a tentar fazer.

[!INCLUDE [compute-table](../../includes/compute-options-table.md)]

<a name="tellmecs"></a>

## <a name="tell-me-about-azure-cloud-services"></a>Conte-me Cloud Services do Azure
Serviços Cloud do Azure é um exemplo de um [plataforma como serviço](https://azure.microsoft.com/overview/what-is-paas/) (PaaS). Como [App Service do Azure](../app-service/app-service-web-overview.md), esta tecnologia foi concebida para suportar aplicações que são dimensionáveis, fiáveis e económico operar. Da mesma forma que serviço de aplicações está alojado em máquinas virtuais (VMs), por isso, é demasiado Cloud Services do Azure. No entanto, pode ter mais controlo sobre as VMs. Pode instalar o seu próprio software em VMs que utilizam os serviços de nuvem do Azure e pode aceder remotamente.

![Diagrama de serviços em nuvem do Azure](./media/cloud-services-choose-me/diagram.png)

Mais controlo também significa que menos facilidade de utilização. A menos que terá das opções de controlo adicional, normalmente, é mais rápida e mais fácil de obter uma aplicação web de cópia de segurança e em execução nas Web Apps funcionalidade do serviço de aplicações em comparação com a Cloud Services do Azure.

Existem dois tipos de funções de Cloud Services do Azure. A única diferença entre os dois é como a sua função está alojada nas VMs:

* **Função da Web**: automaticamente implementa e aloja a aplicação através do IIS.

* **Função de trabalho**: não utiliza o IIS e executa a aplicação autónoma.

Por exemplo, uma aplicação simples pode utilizar apenas uma única função da web, que servem um Web site. Uma aplicação mais complexa pode utilizar uma função da web para processar pedidos de entrada de utilizadores e, então, passe esses pedidos para uma função de trabalho para processamento. (Esta comunicação poderá utilizar [Service Bus do Azure](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md) ou [armazenamento de filas do Azure](../storage/common/storage-introduction.md).)

Como sugere a ilustração anterior, todas as VMs numa única aplicação é executado no mesmo serviço em nuvem. Os utilizadores acedam a aplicação através de um único endereço IP público, com pedidos de carga equilibrada nos VMs da aplicação. A plataforma [dimensiona e implementa](cloud-services-how-to-scale-portal.md) as VMs de uma aplicação do Cloud Services do Azure de uma forma que evita a um ponto único de falha de hardware.

Apesar das aplicações são executadas em VMs, é importante compreender que Cloud Services do Azure fornece PaaS, não uma infraestrutura como serviço (IaaS). Eis uma forma de pensar em-lo. Com a IaaS, tais como máquinas de virtuais do Azure, primeiro criar e configurar o ambiente da que sua aplicação é executada. Em seguida, implementar a aplicação para este ambiente. Está responsável por gerir muito este mundo, ao fazer coisas como a implementação de novas versões do sistema operativo em cada VM aplicadas. PaaS, por outro lado, é como se o ambiente já existe. Tudo o que tem de fazer é implementar a sua aplicação. Gestão da plataforma que é executada, incluindo a implementação de novas versões do sistema operativo, é processada por si.

## <a name="scaling-and-management"></a>Dimensionamento e gestão
Com os serviços de nuvem do Azure, não crie máquinas virtuais. Em vez disso, é fornecer um ficheiro de configuração que diz ao Azure quantos de cada quiser, como "três instâncias de função web" e "dois trabalho instâncias de função." A plataforma, em seguida, cria-los por si. Escolher ainda [que tamanho](cloud-services-sizes-specs.md) as VMs de segurança devem ser, mas não explicitamente criá-los por si. Se a aplicação tem de processar uma carga maior, pode pedir para as VMs mais e Azure cria nessas instâncias. Se a carga diminui, pode encerrar essas instâncias e parar a pagar para os mesmos.

Uma aplicação de Cloud Services do Azure é normalmente disponibilizada aos utilizadores através de um processo de dois passos. Um programador primeiro [carrega a aplicação](cloud-services-how-to-create-deploy-portal.md) para área de transição da plataforma. Quando o programador está pronto para disponibilizar a aplicação em direto, que utilizam o portal do Azure para trocar com a produção de teste. Isto [alternar entre transição e produção](cloud-services-how-to-manage-portal.md#swap-deployments-to-promote-a-staged-deployment-to-production) pode ser efetuada sem período de indisponibilidade, que permite uma aplicação em execução ser atualizado para uma nova versão sem preventiva os seus utilizadores.

## <a name="monitoring"></a>Monitorização
Serviços Cloud do Azure também fornece a monitorização. Como máquinas virtuais, Deteta um servidor físico com falhas e reinicia as VMs que estavam em execução nesse servidor num computador novo. Mas Cloud Services do Azure também Deteta VMs com falhas e de aplicações, não apenas as falhas de hardware. Ao contrário das máquinas virtuais, tem um agente dentro de cada função da web e de trabalho e, por isso, é possível iniciar novas VMs e instâncias da aplicação quando ocorrem falhas.

A natureza PaaS Cloud Services do Azure tem outras implicações demasiado. Uma das mais importante é que as aplicações incorporadas nesta tecnologia devem ser escritas para ser executada corretamente quando ocorre uma falha de qualquer instância de função web ou de trabalho. Para atingir esse objetivo, uma aplicação de Cloud Services do Azure não deve manter o estado no sistema de ficheiros do seus próprio VMs. Ao contrário das VMs criadas com máquinas virtuais, escritas efetuadas para VMs de serviços de nuvem do Azure não persistentes. Não há nothing como um disco de dados de máquinas virtuais. Em vez disso, uma aplicação de Cloud Services do Azure, deve escrever todos os Estados explicitamente a SQL Database do Azure, blobs, tabelas ou alguns outro armazenamento externo. Compilar aplicações desta forma, torna-los mais fáceis de escala e mais resistente a falhas, que são ambos os objetivos importantes do Cloud Services do Azure.

## <a name="next-steps"></a>Passos Seguintes
* [Criar uma aplicação de serviço em nuvem no .NET](cloud-services-dotnet-get-started.md) 
* [Criar uma aplicação de serviço em nuvem no Node.js](cloud-services-nodejs-develop-deploy-app.md) 
* [Criar uma aplicação de serviço em nuvem no PHP](../cloud-services-php-create-web-role.md) 
* [Criar uma aplicação de serviço em nuvem no Python](cloud-services-python-ptvs.md)



