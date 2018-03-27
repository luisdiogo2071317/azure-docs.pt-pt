---
title: Exemplos de modelos do Azure Resource Manager - Serviço de Aplicações | Microsoft Docs
description: Exemplos de modelo do Azure Resource Manager para a funcionalidade Aplicações Web do Serviço de Aplicações
services: app-service
documentationcenter: app-service
author: tfitzmac
manager: timlt
editor: ggailey777
tags: azure-service-management
ms.service: app-service
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: app-service
ms.date: 02/26/2018
ms.author: tomfitz
ms.custom: mvc
ms.openlocfilehash: 155b47fc4c664701ec0f21bdc5ae34f3d7f666ff
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/16/2018
---
# <a name="azure-resource-manager-templates-for-web-apps"></a>Modelos do Azure Resource Manager para Aplicações Web

A tabela seguinte inclui ligações para modelos do Azure Resource Manager para a funcionalidade Aplicações Web do Serviço de Aplicações do Azure. Para obter recomendações sobre como evitar erros comuns quando criar modelos de aplicações Web, veja [Orientações sobre a implementação de aplicações Web com modelos do Azure Resource Manager](web-sites-rm-template-guidance.md).

| | |
|-|-|
|**Dimensionar uma Aplicação Web**||
| [Aplicação Web ligada a um repositório do GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-github-deploy)| Implementa uma aplicação Web do Azure que solicita código a partir do GitHub. |
| [Aplicação Web com blocos de implementação personalizados](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-custom-deployment-slots)| Implementa uma aplicação Web do Azure com ambientes/blocos de implementação personalizada. |
|**Configurar uma aplicação Web**||
| [Certificado de aplicação Web do Key Vault](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-certificate-from-key-vault)| Implementa um certificado de aplicação Web do Azure a partir de um segredo do Azure Key Vault e utiliza-o para o enlace SSL. |
| [Aplicação Web com um domínio personalizado](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-custom-domain)| Implementa uma aplicação Web do Azure com um nome de anfitrião personalizado. |
| [Aplicação Web com um domínio personalizado e SSL](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-custom-domain-and-ssl)| Implementa uma aplicação Web do Azure com um nome de anfitrião personalizado e obtém um certificado de aplicação Web do Key Vault para o enlace SSL. |
| [Aplicação Web com uma extensão GoLang](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-with-golang)| Implementa uma aplicação Web do Azure com a extensão de site GoLang. Em seguida, pode executar as aplicações Web desenvolvidas no GoLang no Azure. |
| [Aplicação Web com Java 8 e Tomcat 8](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-java-tomcat)| Implementa uma aplicação Web do Azure com Java 8 e Tomcat 8 ativados. Em seguida, pode executar aplicações Java no Azure. |
|**Aplicação Web no Linux**||
| [Aplicação Web no Linux com MySQL](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-linux-managed-mysql) | Implementa uma aplicação Web do Azure no Linux com a Base de Dados do Azure para MySQL. |
| [Aplicação Web no Linux com PostgreSQL](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-linux-managed-postgresql) | Implementa uma aplicação Web do Azure no Linux com a Base de Dados do Azure para PostgreSQL. |
|**Aplicação Web com recursos ligados**||
| [Aplicação Web com MySQL](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-managed-mysql)| Implementa uma aplicação Web do Azure no Windows com a Base de Dados do Azure para MySQL. |
| [Aplicação Web com PostgreSQL](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-managed-postgresql)| Implementa uma aplicação Web do Azure no Windows com a Base de Dados do Azure para PostgreSQL. |
| [Aplicação Web com uma Base de Dados SQL](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-sql-database)| Implementa uma aplicação Web do Azure e uma base de dados SQL no nível de serviço Básico. |
| [Aplicação Web com uma ligação de armazenamento de blobs](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-blob-connection)| Implementa uma aplicação web do Azure com uma cadeia de ligação de armazenamento de Blobs do Azure. Em seguida, pode utilizar o armazenamento de Blobs aplicação Web. |
| [Aplicação Web com uma cache de Redis](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-with-redis-cache)| Implementa uma aplicação Web do Azure com uma cache de Redis. |
|**Ambiente do Serviço de Aplicações para PowerApps**||
| [Criar um Ambiente de Serviço de Aplicações v2](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-asev2-create) | Cria um ambiente de Serviço de Aplicações v2 na sua rede virtual. |
| [Criar um ambiente de Serviço de Aplicações v2 com um endereço ILB](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-asev2-ilb-create/) | Cria um ambiente de Serviço de Aplicações v2 na sua rede virtual com um endereço de balanceador de carga interno privado. |
| [Configura o certificado SSL predefinido para um ambiente de Serviço de Aplicações ILB ou um ambiente de Serviço de Aplicações v2 ILB](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-ase-ilb-configure-default-ssl) | Configura o certificado SSL predefinido para um ambiente de Serviço de Aplicações ILB ou um ambiente de Serviço de Aplicações v2 ILB. |
| | |
