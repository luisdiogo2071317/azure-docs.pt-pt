---
title: Introdução ao Serviço de Aplicações no Linux | Microsoft Docs
description: Saiba mais sobre o Serviço de Aplicações do Azure no Linux.
keywords: serviço de aplicações do azure, linux, oss
services: app-service
documentationcenter: ''
author: naziml
manager: cfowler
editor: ''
ms.assetid: bc85eff6-bbdf-410a-93dc-0f1222796676
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 02/16/2017
ms.author: wesmc
ms.custom: mvc
ms.openlocfilehash: c9c4cb42e6bf9f60c883242fad6963f72b772063
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="introduction-to-azure-app-service-on-linux"></a>Introdução ao Serviço de Aplicações do Azure no Linux

A [Aplicação Web](../app-service-web-overview.md) é uma plataforma de computação completamente gerida, está otimizada para o alojamento de sites e de Aplicações Web. Os clientes podem utilizar o Serviço de Aplicações no Linux para alojar aplicações Web nativamente no Linux para pilhas de aplicações suportadas. As secções seguintes listam as pilhas de aplicação atualmente suportadas.

## <a name="languages"></a>Linguagens

O Serviço de Aplicações no Linux suporta um número de imagens Incorporadas para aumentar a produtividade do programador. Se o runtime pedido pela sua aplicação não for suportado nas imagens incorporadas, existem instruções sobre como [criar a sua própria imagem do Docker](tutorial-custom-docker-image.md) para implementar a Aplicação Web para Contentores.

| Idioma | Versões Suportadas |
|---|---|
| Node.js | 4.4, 4.5, 4.8, 6.2, 6.6, 6.9, 6.10, 6.11, 8.0, 8.1, 8.2, 8.8, 8.9, 9.4 |
| Java * | 8.0 |
| PHP | 5.6, 7.0, 7.2 |
| .NET Core | 1.0, 1.1, 2.0 |
| Ruby | 2.3 |
| Ir | 1.0 |
| Apache Tomcat | 8.5, 9.0 |

Consulte [Criar uma aplicação Web Java no Serviço de Aplicações no Linux](https://docs.microsoft.com/azure/app-service/containers/quickstart-java) para mais detalhes.

## <a name="deployments"></a>Implementações

* FTP
* Git Local
* GitHub
* Bitbucket

## <a name="devops"></a>DevOps

* Ambientes de teste
* [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/container-registry-intro) e DockerHub CI/CD

## <a name="console-publishing-and-debugging"></a>Consola, Publicação e Depuração

* Ambientes
* Implementações
* Consola básica
* SSH

## <a name="scaling"></a>Dimensionamento

* Os clientes podem aumentar ou reduzir aplicações Web ao alterar o escalão do respetivo [plano do Serviço de Aplicações](https://docs.microsoft.com/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview?toc=%2fazure%2fapp-service-web%2ftoc.json)

## <a name="locations"></a>Localizações

Verifique o [Dashboard de Estado do Azure](https://azure.microsoft.com/status).

## <a name="limitations"></a>Limitações

O portal do Azure mostra apenas as funcionalidades que funcionam atualmente para a Aplicação Web para Contentores. À medida que ativamos mais funcionalidades, estas tornam-se visíveis no portal.

Algumas funcionalidades, como a integração da rede virtual, a autenticação do Azure Active Directory/terceiros ou as extensões de site do Kudu, ainda não estão disponíveis. Assim que estas funcionalidades estiverem disponíveis, atualizaremos a nossa documentação e o blogue com as alterações.

O Serviço de Aplicações no Linux é suportado apenas com os planos do serviço de aplicações [Básico e Standard](https://azure.microsoft.com/pricing/details/app-service/plans/) e não tem um escalão [Gratuito ou Partilhado](https://azure.microsoft.com/pricing/details/app-service/plans/). Não pode criar a Aplicação Web para Contentores num plano do Serviço de Aplicações que já aloja Aplicações Web que não são do Linux.

## <a name="troubleshooting"></a>Resolução de problemas

Quando a aplicação não consegue iniciar ou pretende verificar o registo da sua aplicação, verifique os registos do Docker no diretório LogFiles. Pode aceder a este diretório através do seu site SCM ou do FTP.
Para registar o `stdout` e `stderr` a partir do seu contentor, tem de ativar o **Registo do Contentor do Docker** em **Registos de Diagnóstico**.

![Ativar o Registo][2]

![Utilizar o Kudu para ver registos do Docker][1]

Pode aceder ao site do SCM através das **Ferramentas Avançadas** no menu **Ferramentas de Desenvolvimento**.

## <a name="next-steps"></a>Passos seguintes

Veja as ligações seguintes para começar a utilizar o Serviço de Aplicações no Linux. Pode publicar perguntas e problemas no [nosso fórum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).

* [Como utilizar uma imagem personalizada do Docker para as Aplicações Web para Contentores](quickstart-docker-go.md)
* [Utilizar o .NET Core no Serviço de Aplicações do Azure no Linux](quickstart-dotnetcore.md)
* [Utilizar o Ruby no Serviço de Aplicações do Azure no Linux](quickstart-ruby.md)
* [FAQ sobre a Aplicação Web do Serviço de Aplicações do Azure para Contentores](app-service-linux-faq.md)
* [Suporte SSH para o Serviço de Aplicações do Azure no Linux](app-service-linux-ssh-support.md)
* [Configurar ambientes de teste no Serviço de Aplicações do Azure](../../app-service/web-sites-staged-publishing.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
* [Implementação Contínua do Hub do Docker com a Aplicação Web para Contentores](./app-service-linux-ci-cd.md)

<!--Image references-->
[1]: ./media/app-service-linux-intro/kudu-docker-logs.png
[2]: ./media/app-service-linux-intro/logging.png
