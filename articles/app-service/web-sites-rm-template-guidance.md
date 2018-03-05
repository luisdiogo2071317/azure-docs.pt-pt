---
title: "Orientações sobre a implementação de aplicações web do Azure com modelos | Microsoft Docs"
description: "Recomendações para criação de modelos Azure Resource Manager para implementar as aplicações web."
services: app-service
documentationcenter: app-service
author: tfitzmac
manager: timlt
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/26/2018
ms.author: tomfitz
ms.openlocfilehash: eff0b0e6258217fa8fbf7f15606f98d70fecd7c5
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/28/2018
---
# <a name="guidance-on-deploying-web-apps-with-azure-resource-manager-templates"></a>Orientações sobre a implementação de aplicações web com modelos Azure Resource Manager

Este artigo fornece recomendações para criação de modelos Azure Resource Manager para implementar soluções de serviço de aplicações. Estas recomendações podem ajudar a evitar problemas comuns.

## <a name="define-dependencies"></a>Definir dependências

Definir as dependências para aplicações Web requer uma compreensão dos como interagem recursos dentro de uma aplicação Web. Se especificar dependências por uma ordem incorreta, poderá causar erros de implementação ou criar uma condição provável de antecipação que parar a implementação.

> [!WARNING]
> Se incluir uma extensão de site MS Deploy no seu modelo, tem de definir quaisquer recursos de configuração como dependente do recurso Deploy MS. As alterações de configuração fazer com que o site reiniciar no modo assíncrono. Ao tornar os recursos de configuração depende Deploy MS, certifique-se que MS Deploy conclusão antes do site for reiniciado. Sem estas dependências, o site poderá reiniciar durante o processo de implementação do MS Deploy. Para um modelo de exemplo, consulte [Wordpress modelo com a dependência de implementar Web](https://github.com/davidebbo/AzureWebsitesSamples/blob/master/ARMTemplates/WordpressTemplateWebDeployDependency.json).

A imagem seguinte mostra a ordem da dependência de vários recursos do serviço de aplicações.

![Dependências de aplicações Web](media/web-sites-rm-template-guidance/web-dependencies.png)

Implementar recursos pela seguinte ordem:

**Camada 1**
* Plano do Serviço de Aplicações
* Quaisquer outros recursos relacionados, como bases de dados ou contas de armazenamento

**Camada 2**
* Aplicação Web – depende do plano do app service
* Application Insights direcionadas para o farm de servidores - depende do plano do app service

**Camada 3**
* Controlo de origem - depende da aplicação web
* Extensão de site de MSDeploy - depende da aplicação web
* Application Insights direcionadas para o farm de servidores - depende da aplicação web

**Camada 4**
* Certificado de serviço de aplicações - depende de controlo de origem ou MSDeploy quer esteja presente; caso contrário, depende da aplicação web
* Definições de configuração (cadeias de ligação, os valores de configuração da web, as definições de aplicação) - depende de controlo de origem ou MSDeploy quer esteja presente; caso contrário, depende da aplicação web

**Camada 5**
* Enlaces do nome de anfitrião – depende de certificado se estiver presente; caso contrário, um recurso de nível mais elevado
* As extensões do site – depende das definições de configuração se estiver presente; caso contrário, um recurso de nível mais elevado

Normalmente, a solução inclui apenas algumas destas recursos e as camadas. Para as camadas em falta, mapear inferiores recursos para a próxima camada superior.

O exemplo seguinte mostra faz parte de um modelo. O valor de configuração da cadeia de ligação depende a extensão de MSDeploy. A extensão de MSDeploy depende a aplicação web e a base de dados.

```json
{
    "name": "[parameters('name')]",
    "type": "Microsoft.Web/sites",
    "resources": [
      {
          "name": "MSDeploy",
          "type": "Extensions",
          "dependsOn": [
            "[concat('Microsoft.Web/Sites/', parameters('name'))]",
            "[concat('SuccessBricks.ClearDB/databases/', parameters('databaseName'))]"
          ],
          ...
      },
      {
          "name": "connectionstrings",
          "type": "config",
          "dependsOn": [
            "[concat('Microsoft.Web/Sites/', parameters('name'), '/Extensions/MSDeploy')]"
          ],
          ...
      }
    ]
}
```

## <a name="find-information-about-msdeploy-errors"></a>Localizar informações sobre os erros de MSDeploy

Se o seu modelo do Resource Manager utiliza MSDeploy, as mensagens de erro de implementação podem ser complicadas perceber. Para obter mais informações, após a implementação, tente os seguintes passos:

1. Vá para o site [consola Kudu](https://github.com/projectkudu/kudu/wiki/Kudu-console).
2. Navegue para a pasta em `D:\home\LogFiles\SiteExtensions\MSDeploy`.
3. Procure o *appManagerStatus.xml* e *appManagerLog.xml* ficheiros. O primeiro ficheiro regista o estado. O segundo ficheiro regista informações sobre o erro. Se o erro não é claro que, pode incluí-la quando pedir ajuda no fórum do.

## <a name="unique-web-app-name"></a>Nome da aplicação web exclusivo

O nome da sua aplicação web tem de ser globalmente exclusivo. Pode utilizar uma convenção de nomenclatura que é provável que seja exclusivo ou pode utilizar o [uniqueString função](../azure-resource-manager/resource-group-template-functions-string.md#uniquestring) para melhorar a gerar um nome exclusivo.

```json
{
  "apiVersion": "2016-08-01",
  "name": "[concat(parameters('siteNamePrefix'), uniqueString(resourceGroup().id))]",
  "type": "Microsoft.Web/sites",
  ...
}
```

## <a name="next-steps"></a>Passos Seguintes

* Para um tutorial sobre como implementar aplicações web com um modelo, consulte [aprovisionar e implementar micro-serviços previsibilidade no Azure](app-service-deploy-complex-application-predictably.md).