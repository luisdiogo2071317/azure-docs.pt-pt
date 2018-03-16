---
title: "Orientações sobre a implementação de aplicações web do Azure, utilizando modelos | Microsoft Docs"
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
ms.openlocfilehash: dc816bb6e95d2800d79124dfac60b55e88eaa500
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/16/2018
---
# <a name="guidance-on-deploying-web-apps-by-using-azure-resource-manager-templates"></a>Orientações sobre a implementação de aplicações web utilizando os modelos Azure Resource Manager

Este artigo fornece recomendações para criação de modelos Azure Resource Manager para implementar soluções de App Service do Azure. Estas recomendações podem ajudar a evitar problemas comuns.

## <a name="define-dependencies"></a>Definir dependências

Definir as dependências para aplicações web requer uma compreensão dos como interagem recursos dentro de uma aplicação web. Se especificar dependências por uma ordem incorreta, poderá causar erros de implementação ou criar uma condição provável de antecipação que parar a implementação.

> [!WARNING]
> Se incluir uma extensão de site MSDeploy no seu modelo, tem de definir quaisquer recursos de configuração como dependente do recurso de MSDeploy. As alterações de configuração fazer com que o site reiniciar no modo assíncrono. Ao tornar os recursos de configuração depende de MSDeploy, certifique-se de que MSDeploy é concluída antes do site for reiniciado. Sem estas dependências, o site poderá reiniciar durante o processo de implementação de MSDeploy. Para um modelo de exemplo, consulte [WordPress modelo com a dependência de implementar Web](https://github.com/davidebbo/AzureWebsitesSamples/blob/master/ARMTemplates/WordpressTemplateWebDeployDependency.json).

A imagem seguinte mostra a ordem da dependência de vários recursos do serviço de aplicações:

![Dependências de aplicações Web](media/web-sites-rm-template-guidance/web-dependencies.png)

Implementar recursos pela seguinte ordem:

**Camada 1**
* Plano de serviço aplicacional.
* Quaisquer outros recursos relacionados, como bases de dados ou contas de armazenamento.

**Camada 2**
* Web aplicação – depende do plano do App Service.
* Instância do Azure do Application Insights que tenha como destino o farm de servidores – depende do plano do App Service.

**Camada 3**
* Origem de controlo - depende da aplicação web.
* Extensão de site de MSDeploy – depende da aplicação web.
* Instância da aplicação Insights que tenha como destino o farm de servidores – depende da aplicação web.

**Camada 4**
* Certificado de serviço de aplicações – depende do controlo de origem ou MSDeploy se qualquer um dos está presente. Caso contrário, depende da aplicação web.
* Definições de configuração (cadeias de ligação, os valores de Web. config, as definições de aplicação) – depende de controlo de origem ou MSDeploy se a está presente. Caso contrário, depende da aplicação web.

**Camada 5**
* Anfitrião enlaces do nome – depende o certificado se estiver presente. Caso contrário, depende de um recurso de nível mais elevado.
* Site extensões – depende das definições de configuração se estiverem presentes. Caso contrário, depende de um recurso de nível mais elevado.

Normalmente, a solução inclui apenas algumas destas recursos e as camadas. Para as camadas em falta, mapear inferiores recursos para a camada seguinte superior.

O exemplo seguinte mostra faz parte de um modelo. O valor da cadeia de configuração de ligação depende a extensão de MSDeploy. A extensão de MSDeploy depende a aplicação web e a base de dados.

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
2. Procure a pasta no D:\home\LogFiles\SiteExtensions\MSDeploy.
3. Procure os ficheiros appManagerStatus.xml e appManagerLog.xml. O primeiro ficheiro regista o estado. O segundo ficheiro regista informações sobre o erro. Se o erro não é claro que, pode incluí-la quando está a pedir para obter ajuda no fórum do.

## <a name="choose-a-unique-web-app-name"></a>Escolha um nome de aplicação web exclusivo

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