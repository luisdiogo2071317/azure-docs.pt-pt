---
title: Introdução para a máquina de Virtual de ciência de dados com base em ambientes de equipa - Azure | Microsoft Docs
description: Padrões para a implementação de VM de ciência de dados como ambiente de equipas empresarial.
keywords: ligação avançada learning AI, ferramentas de ciência de dados, máquina de virtual de ciência de dados, geoespacial análise, o processo de ciência de dados de equipa
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2018
ms.author: gokuma
ms.openlocfilehash: 911d6484421cc9fddad0530bf8d9ab4f01d48bf8
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
---
# <a name="data-science-virtual-machine-based-team-analytics-and-ai-environment"></a>Máquina de Virtual de ciência de dados com base em equipa de análise e ambiente de AI 
O [máquinas de virtuais de ciência de dados](overview.md) (DSVM) fornece um ambiente avançado na nuvem do Azure com o software previamente concebido para análise AI e os dados. Tradicionalmente foi utilizado o DSVM como ambiente de trabalho de análise individual e cientistas de dados individuais obterem produtividade com este partilhado noção do respetivo ambiente de análise pré-criadas. Como as equipas de análise de grande planear respetivos ambientes de análise para os cientistas de dados e programadores de AI, um de temas periódicos é uma infraestrutura de desenvolvimento e experimentação de análise partilhados que é gerida de acordo com a TI empresariais políticas que também facilita a colaboração e consistência em toda a ciência de dados completo / equipas de análise. Uma infraestrutura partilhada também permite que as TI melhor utilizar o ambiente de análise. A ciência de dados baseada em equipa / infraestrutura análise também é referida por algumas organizações como "Sandbox de análise" que permite as cientistas de dados rapidamente compreender dados, executar experimentações, validar hipótese, criar modelos preditivos de forma segura sem afetar o ambiente de produção ao ter acesso a vários recursos de dados. 

Uma vez que o DSVM funciona ao nível da infraestrutura do Azure, os administradores de TI prontamente podem configurar o DSVM para funcionar em conformidade com as políticas de TI de enterprise e ofertas total flexibilidade na implementação de várias arquiteturas partilha com acesso ao recursos de dados empresariais de forma controlada. 

Esta secção descreve alguns padrões e orientações que podem ser utilizadas para implementar o DSVM como uma infraestrutura de ciência de dados baseada em equipa.  Os blocos modulares para estes padrões diretamente são aproveitados do IaaS do Azure (infraestrutura como serviço) e como tal aplicáveis a quaisquer VMs do Azure. O foco específico esta série de artigos é aplicar estas capacidades de infraestrutura do Azure standard para a VM de ciência de dados. 

Algumas dos blocos modulares chaves de um ambiente de análise da equipa de empresa são:

* [Agrupamento ampliada automática de máquinas de virtuais de ciência de dados](dsvm-pools.md)
* [Identidade e acesso à área de trabalho a partir de qualquer um dos DSVMs no agrupamento de comuns](dsvm-common-identity.md)
* [Proteger o acesso a origens de dados](dsvm-secure-access-keys.md)
* Governação e a deteção de conjuntos de dados empresariais e abrir

Esta série de artigos, indicadores e documentação de orientação são fornecidas em cada um destes aspetos. O [centro da arquitetura do Azure](https://docs.microsoft.com/en-us/azure/architecture/) fornece mais arquitetura detalhada de ponto a ponto para a sua infraestrutura de análise.  
