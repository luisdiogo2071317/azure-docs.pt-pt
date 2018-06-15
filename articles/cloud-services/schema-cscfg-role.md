---
title: Esquema de função de serviços em nuvem do Azure | Microsoft Docs
ms.custom: ''
ms.date: 12/07/2016
services: cloud-services
ms.reviewer: ''
ms.service: cloud-services
ms.suite: ''
ms.tgt_pltfrm: ''
ms.topic: reference
ms.assetid: e4fbffc1-98eb-449c-971c-de415e45ab34
caps.latest.revision: 12
author: thraka
ms.author: adegeo
manager: timlt
ms.openlocfilehash: 2f5c657bb80ad0788bcc3dd19d962b3f21afa4a8
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
ms.locfileid: "34359004"
---
# <a name="azure-cloud-services-config-role-schema"></a>Esquema de configuração de função de serviços em nuvem do Azure

O `Role` elemento do ficheiro de configuração especifica o número de instâncias de função para implementar para cada função no serviço, os valores das definições de configuração, e os thumbprints para todos os certificados associados a uma função.

Para obter mais informações sobre o esquema de configuração de serviço do Azure, consulte [esquema de configuração do serviço em nuvem (clássica)](schema-cscfg-file.md). Para obter mais informações sobre o esquema de definição de serviço do Azure, consulte [esquema de definição de serviço em nuvem (clássica)](schema-csdef-file.md).

##  <a name="Role"></a> Elemento de função
O seguinte exemplo mostra o `Role` elemento e os respetivos elementos subordinados.

```xml 
<ServiceConfiguration>
  <Role name="<role-name>" vmName="<vm-name>">
    <Instances count="<number-of-instances>"/>
    <ConfigurationSettings>
      <Setting name="<setting-name>" value="<setting-value>" />
    </ConfigurationSettings>
    <Certificates>
      <Certificate name="<certificate-name>" thumbprint="<certificate-thumbprint>" thumbprintAlgorithm="<algorithm>"/>
    </Certificates>
  </Role>
</ServiceConfiguration>
```

A tabela seguinte descreve os atributos para o `Role` elemento.

| Atributo | Descrição |
| --------- | ----------- |
| nome   | Necessário. Especifica o nome da função. O nome tem de corresponder ao nome fornecido para a função no ficheiro de definição de serviço.|
| vmName | Opcional. Especifica o nome DNS para uma Máquina Virtual. O nome tem de ser 10 carateres ou menos.|

A tabela seguinte descreve os elementos subordinados do `Role` elemento.

| Elemento | Descrição |
| ------- | ----------- |
| Instâncias | Necessário. Especifica o número de instâncias a implementar para a função. O número de instâncias é definido por um número inteiro para o `count` atributo.|
| Definição   | Opcional. Especifica um nome de definição e valor de uma coleção de definições para uma função. O nome da definição é definido por uma cadeia para o `name` atributo e o valor da definição é definido por uma cadeia para o `value` atributo.|
| Certificado | Opcional. Especifica o nome, a thumbprint e o algoritmo de um certificado de serviço que está a ser associados à função. O nome do certificado é definido por uma cadeia para o `name` atributo. O thumbprint do certificado é definido por uma cadeia de números hexadecimal sem espaços para o `thumbprint` atributo. Os números hexadecimal devem ser representados com dígitos e carateres alfanuméricos em maiúsculas. O algoritmo de certificado é definido por uma cadeia para o `thumbprintAlgorithm` atributo.|

## <a name="see-also"></a>Consultar Também
[Esquema de configuração (clássica) serviço em nuvem](schema-cscfg-file.md)