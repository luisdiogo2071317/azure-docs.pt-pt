---
title: "Descrição geral das Web Apps | Microsoft Docs"
description: Saiba como o App Service do Azure o ajuda a desenvolver e alojar Web Apps
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: 94af2caf-a2ec-4415-a097-f60694b860b3
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 01/04/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: f8510bb6b412e9af8aad30ba32bc74206c22042f
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/30/2017
---
# <a name="web-apps-overview"></a>Descrição geral das Web Apps

*Aplicações Web do Serviço de Aplicações* (ou apenas Aplicações Web) é um serviço para o alojamento de aplicações Web, APIs REST e back-ends móveis. Pode desenvolver no seu idioma favorito, seja .NET, .NET Core, Java, Ruby, Node.js, PHP ou Python. Pode executar e dimensionar aplicações com facilidade em VMs Windows ou Linux (veja [App Service on Linux (Serviço de Aplicações no Linux)](containers/app-service-linux-intro.md)). 

As Aplicações Web não só adicionam a capacidade do Microsoft Azure à sua aplicação, como também acrescentam segurança, balanceamento de carga, dimensionamento automático e gestão automatizada. Também pode tirar partido das respetivas capacidades do DevOps, como a implementação contínua do VSTS, do GitHub, do Docker Hub e de outras origens, gestão de pacotes, ambientes de teste, domínio personalizado e certificados SSL. 

Com o Serviço de Aplicações, paga pelos recursos de computação do Azure que utilizar. Os recursos de computação que utiliza são determinados pelo _plano do Serviço de Aplicações_ em que executa as suas Aplicações Web. Para obter mais informações, veja [App Service plans in Azure Web Apps (Planos do Serviço de Aplicações nas Aplicações Web do Azure)](azure-web-sites-web-hosting-plans-in-depth-overview.md).

O seguinte vídeo de 5 minutos apresenta as Web Apps do App Service do Azure.

>[!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Web-Apps-with-Yochay-Kiriaty/player]
>
>

## <a name="why-use-web-apps"></a>Porquê utilizar Web Apps?
Apresentamos a seguir algumas das principais funcionalidades das Aplicações Web do Serviço de Aplicações:

* **Várias linguagens e arquiteturas** - As Aplicações Web dispõem de suporte de primeira classe para ASP.NET, ASP.NET Core, Java, Ruby, Node.js, PHP ou Python. Também pode executar o [PowerShell e outros scripts ou executáveis](web-sites-create-web-jobs.md) como serviços em segundo plano.
* **Otimização de DevOps** – configure [a integração e a implementação contínuas](app-service-continuous-deployment.md) com os Visual Studio Team Services, GitHub, BitBucket, Docker Hub ou Azure Container Service. Promova atualizações através de [ambientes de teste](web-sites-staged-publishing.md). Faça a gestão das suas aplicações nas Aplicações Web com o [Azure PowerShell](/powershell/azureps-cmdlets-docs) ou a [interface de linha de comandos (CLI) de várias plataformas](/cli/azure/install-azure-cli).
* **Dimensionamento global com elevada disponibilidade** – aumente [verticalmente](web-sites-scale.md) ou [horizontalmente](../monitoring-and-diagnostics/insights-how-to-scale.md) de forma manual ou automática. Aloje as aplicações em qualquer lugar da infraestrutura do datacenter global da Microsoft e o App Service [SLA](https://azure.microsoft.com/support/legal/sla/app-service/) promete elevada disponibilidade.
* **Ligações a plataformas SaaS e dados no local** – escolha entre mais de 50 [conectores](../connectors/apis-list.md) para sistemas empresariais (como SAP), serviços SaaS (como o Salesforce) e serviços Internet (como o Facebook). Aceda a dados no local ao utilizar [Ligações Híbridas](../biztalk-services/integration-hybrid-connection-overview.md) e [Azure Virtual Networks](web-sites-integrate-with-vnet.md).
* **Segurança e conformidade** – o App Service está [em conformidade com ISO, SOC e PCI](https://www.microsoft.com/TrustCenter/). Autentique os utilizadores com o [Azure Active Directory](app-service-mobile-how-to-configure-active-directory-authentication.md) ou com início de sessão social ([Google](app-service-mobile-how-to-configure-google-authentication.md), [Facebook](app-service-mobile-how-to-configure-facebook-authentication.md), [Twitter](app-service-mobile-how-to-configure-twitter-authentication.md) e [ Microsoft](app-service-mobile-how-to-configure-microsoft-authentication.md)). Crie [restrições de endereço IP](app-service-ip-restrictions.md) e [faça a gestão de identidades de serviço](app-service-managed-service-identity.md).
* **Modelos de aplicação** – escolha entre uma lista extensa de modelos de aplicação no [Azure Marketplace](https://azure.microsoft.com/marketplace/), como o WordPress, o Joomla e o Drupal.
* **Integração do Visual Studio** – as ferramentas dedicadas do Visual Studio simplificam o trabalho de criar, implementar e depurar.
* **Funcionalidades móveis e de API** - As Aplicações Web dispõem de suporte de CORS pronto a utilizar para cenários de RESTful API e simplificam cenários de aplicações móveis ao ativar a autenticação, sincronizar dados offline, notificações push e muito mais.
* **Código sem servidor** - Execute um fragmento de código ou script a pedido, sem ter de aprovisionar ou gerir a infraestrutura explicitamente, e pague apenas o tempo de computação que o seu código utiliza (veja [Azure Functions (Funções do Azure)](/azure/azure-functions/)).

Para além das Web Apps no App Service, o Azure oferece outros serviços que podem ser utilizados para alojar sites e Web Apps. Para a maioria dos cenários, as Web Apps são a melhor escolha.  Para uma arquitetura de microsserviço, considere o [Service Fabric](https://azure.microsoft.com/documentation/services/service-fabric). Se precisar de mais controlo sobre as VMs em que o seu código é executado, considere as [Máquinas Virtuais do Azure](https://azure.microsoft.com/documentation/services/virtual-machines/). Para obter mais informações sobre como escolher entre estes serviços do Azure, consulte a [Comparação entre o App Service do Azure, as Virtual Machines, o Service Fabric e os Cloud Services](choose-web-site-cloud-service-vm.md).

## <a name="next-steps"></a>Passos seguintes

Crie a sua primeira aplicação Web.

> [!div class="nextstepaction"]
> [ASP.NET](app-service-web-get-started-dotnet.md)

> [!div class="nextstepaction"]
> [PHP](app-service-web-get-started-php.md)

> [!div class="nextstepaction"]
> [Node.js](app-service-web-get-started-nodejs.md)

> [!div class="nextstepaction"]
> [Java](app-service-web-get-started-java.md)

> [!div class="nextstepaction"]
> [python](app-service-web-get-started-python.md)

> [!div class="nextstepaction"]
> [HTML](app-service-web-get-started-html.md)

