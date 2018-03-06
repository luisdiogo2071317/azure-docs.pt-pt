---
title: "Exemplos de modelos do Azure Resource Manager - Serviço de Aplicações | Microsoft Docs"
description: "Exemplos de modelos do Azure Resource Manager - Serviço de Aplicações"
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
ms.openlocfilehash: e28a27b9a00164fcbba6eb5d3e5435548486ffa4
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/27/2018
---
# <a name="azure-resource-manager-templates-for-azure-web-apps"></a>Modelos do Azure Resource Manager para Aplicações Web do Azure

A tabela seguinte inclui ligações para modelos do Azure Resource Manager. Para obter recomendações sobre como evitar erros comuns ao criar modelos de aplicações Web, veja [Orientações sobre a implementação de aplicações Web com modelos do Azure Resource Manager](web-sites-rm-template-guidance.md).

| | |
|-|-|
|**Implementar Aplicação Web**||
| [Aplicação Web ligada a um repositório do GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-github-deploy)| Implementa uma aplicação Web do Azure que solicita código a partir do GitHub. |
| [Aplicação Web com blocos de implementação personalizada](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-custom-deployment-slots)| Implementa uma aplicação Web do Azure com ambientes/blocos de implementação personalizada. |
|**Configurar Aplicação Web**||
| [Certificado de Aplicação Web do Key Vault](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-certificate-from-key-vault)| Implementa um certificado de aplicação Web do Azure a partir do segredo do Key Vault e utiliza-o para o enlace SSL. |
| [Aplicação Web com domínio personalizado](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-custom-domain)| Implementa uma aplicação Web do Azure com um nome de anfitrião personalizado. |
| [Aplicação Web com domínio personalizado e SSL](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-custom-domain-and-ssl)| Implementa uma aplicação Web do Azure com um nome de anfitrião personalizado e obtém o certificado de aplicação Web do Key Vault para o enlace SSL. |
| [Aplicação Web com extensão GoLang](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-with-golang)| Implementa uma aplicação Web do Azure com a extensão de site Golang, que lhe permite executar aplicações Web desenvolvidas no Golang no Azure. |
| [Aplicação Web com Java 8 e Tomcat 8](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-java-tomcat)| Implementa uma aplicação Web do Azure com Java 8 e Tomcat 8 ativados, o que lhe permite executar aplicações Java no Azure. |
|**Aplicação Web no Linux**||
| [Aplicação Web no Linux com MySQL](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-linux-managed-mysql) | Implementa uma aplicação Web do Azure no Linux com a base de dados do Azure para MySQL. |
| [Aplicação Web no Linux com PostgreSQL](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-linux-managed-postgresql) | Implementa uma aplicação Web do Azure no Linux com a base de dados do Azure para PostgreSQL. |
|**Aplicação Web com recursos ligados**||
| [Aplicação Web com MySQL](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-managed-mysql)| Implementa uma aplicação Web do Azure no Windows com a base de dados do Azure para MySQL. |
| [Aplicação Web com PostgreSQL](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-managed-postgresql)| Implementa uma aplicação Web do Azure no Windows com a base de dados do Azure para PostgreSQL. |
| [Aplicação Web com uma Base de Dados SQL](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-sql-database)| Implementa uma aplicação Web do Azure e a Base de Dados SQL no nível de serviço "Básico". |
| [Aplicação Web com ligação de Armazenamento de Blobs](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-blob-connection)| Implementa uma aplicação Web do Azure com uma cadeia de ligação de armazenamento de blobs, que lhe permite utilizar o Azure Blob Storage a partir da aplicação Web. |
| [Aplicação Web com Cache de Redis](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-with-redis-cache)| Implementa uma aplicação Web do Azure com cache de Redis. |
|**Ambiente do Serviço de Aplicações**||
| [Criar um Ambiente de Serviço de Aplicações v2](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-asev2-create) | Cria um Ambiente de Serviço de Aplicações v2 na sua rede virtual. |
| [Criar um Ambiente de Serviço de Aplicações v2 com um Endereço ILB](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-asev2-ilb-create/) | Cria um Ambiente de Serviço de Aplicações v2 na sua rede virtual com um endereço de balanceador de carga interno privado. |
| [Configurar o Certificado SSL Predefinido para um ASE ILB ou ASE v2 ILB](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-ase-ilb-configure-default-ssl) | Configura o certificado SSL predefinido para um Ambiente de Serviço de Aplicações ILB ou um Ambiente de Serviço de Aplicações v2 ILB |
| | |
