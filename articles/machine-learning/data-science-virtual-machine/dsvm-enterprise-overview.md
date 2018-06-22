---
title: Introdução aos ambientes de equipa baseada na máquina de Virtual de ciência de dados - Azure | Microsoft Docs
description: Padrões para implementar a VM de ciência de dados num ambiente empresarial equipa.
keywords: ligação avançada learning AI, ferramentas de ciência de dados, máquina de virtual de ciência de dados, geoespacial análise, o processo de ciência de dados de equipa
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.component: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2018
ms.author: gokuma
ms.openlocfilehash: 8486b0be1fb5e1385da3c7ad55f6410a1059df93
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/21/2018
ms.locfileid: "36309253"
---
# <a name="data-science-virtual-machine-based-team-analytics-and-ai-environment"></a>Análise de equipa baseado em Máquina Virtual de ciência de dados e de ambiente de AI 
O [máquinas de virtuais de ciência de dados](overview.md) (DSVM) fornece um ambiente avançado na plataforma do Azure com o software prebuilt para artificial intelligence (AI) e a análise de dados. 

Tradicionalmente, o DSVM foi utilizado como um ambiente de trabalho de análise individual. Cientistas de dados individuais obtêm produtividade com este partilhado noção do respetivo ambiente prebuilt análise. Como as equipas de análise de grande planear respetivos ambientes de análise para os cientistas de dados e programadores de AI, uma dos temas periódicos é uma infraestrutura de análise partilhado para desenvolvimento e de experimentação. Esta infraestrutura é gerida em linha com TI empresariais políticas que também facilitam a colaboração e consistência entre as equipas de ciência/análise de dados. 

Uma infraestrutura partilhada também permite que as TI melhor utilizar o ambiente de análise. Algumas organizações chamar a infraestrutura de ciência/análise de dados baseada em equipa um "sandbox análise". Permite que os cientistas de dados aceder a vários recursos de dados rapidamente, compreender dados, executar experimentações, validar hypotheses e criar modelos preditivos sem afetar o ambiente de produção. 

Porque o DSVM funciona ao nível da infraestrutura do Azure, os administradores de TI podem prontamente configurar DSVM para funcionar em conformidade com as políticas de TI da empresa. O DSVM oferece total flexibilidade na implementação de várias arquiteturas partilha com acesso a recursos de dados empresariais de forma controlada. 

Esta secção descreve alguns padrões e diretrizes que pode utilizar para implementar o DSVM como uma infraestrutura de ciência de dados baseada em equipa. Os blocos modulares para estes padrões vêm da infraestrutura do Azure como um serviço (IaaS), pelo que estas são aplicadas a todas as VMs do Azure. O foco esta série de artigos é aplicar estas capacidades de infraestrutura do Azure standard para a VM de ciência de dados. 

Algumas dos blocos modulares chaves de um ambiente de análise da equipa de empresa são:

* [Dimensionado automaticamente conjunto de máquinas de virtuais de ciência de dados](dsvm-pools.md)
* [Identidade e acesso a uma área de trabalho a partir de qualquer um dos DSVMs no agrupamento de comuns](dsvm-common-identity.md)
* [Proteger o acesso a origens de dados](dsvm-secure-access-keys.md)


Esta série de artigos fornece orientações e ponteiros para cada um dos itens anteriores. -Não abrange as considerações e das necessidades da implementação DSVM nas configurações de grande empresa. Segue-se a outra documentação do Azure que pode utilizar ao implementar instâncias DSVM na sua empresa: 

* [Segurança da rede](https://docs.microsoft.com/azure/security/azure-network-security)
* [Monitorização](https://docs.microsoft.com/azure/virtual-machines/windows/monitor) e [gestão](https://docs.microsoft.com/azure/virtual-machines/windows/maintenance-and-updates)
* [Registo e auditoria](https://docs.microsoft.com/azure/security/azure-log-audit)
* [Controlo de acesso baseado em funções](https://docs.microsoft.com/azure/role-based-access-control/overview)
* [Definição de política e de imposição](https://docs.microsoft.com/azure/azure-policy/azure-policy-introduction)
* [Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware)
* [Encriptação](https://docs.microsoft.com/azure/virtual-machines/windows/encrypt-disks)
* [Deteção de dados e governação](https://docs.microsoft.com/azure/data-catalog/)

O [Centro de arquitetura do Azure](https://docs.microsoft.com/en-us/azure/architecture/) proporciona uma arquitetura de ponto a ponto detalhada e padrões para criar e gerir a sua infraestrutura de análise e baseados na nuvem. 
