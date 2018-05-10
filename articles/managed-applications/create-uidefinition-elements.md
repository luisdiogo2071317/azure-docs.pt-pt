---
title: Azure criar o elemento de definição de IU | Microsoft Docs
description: Descreve os elementos para utilizar quando criar definições de IU para o portal do Azure.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2018
ms.author: tomfitz
ms.openlocfilehash: cbfc9c02cbde8bcf9a253144ff41497676c98f13
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="createuidefinition-elements"></a>Elementos de CreateUiDefinition
Este artigo descreve as propriedades para todos os elementos suportados um CreateUiDefinition e o esquema. 

## <a name="schema"></a>Esquema

O esquema para a maioria dos elementos é o seguinte:

```json
{
  "name": "element1",
  "type": "Microsoft.Common.TextBox",
  "label": "Some text box",
  "defaultValue": "my value",
  "toolTip": "Provide a descriptive name.",
  "constraints": {},
  "options": {},
  "visible": true
}
```

| Propriedade | Necessário | Descrição |
| -------- | -------- | ----------- |
| nome | Sim | Um identificador interno para fazer referência a uma instância específica de um elemento. A utilização de mais comuns de nome de elemento está no `outputs`, onde os valores de saída dos elementos especificados estão mapeados para os parâmetros do modelo. Também pode utilizá-la para vincular o valor de saída de um elemento para a `defaultValue` de outro elemento. |
| tipo | Sim | O controlo de IU para compor para o elemento. Para obter uma lista dos tipos suportados, consulte [elementos](#elements). |
| Etiqueta | Sim | O texto a apresentar do elemento. Alguns tipos de elemento contém várias etiquetas, para que o valor pode ser um objeto que contém vários cadeias. |
| defaultValue | Não | O valor predefinido do elemento. Alguns tipos de elemento suportam valores predefinidos complexa, pelo que o valor pode ser um objeto. |
| Descrição | Não | O texto a apresentar a descrição do elemento. Semelhante ao `label`, alguns elementos suportam vários cadeias de sugestão de ferramenta. Ligações de inline podem ser incorporadas utilizando a sintaxe de Markdown.
| Restrições | Não | Uma ou mais propriedades que são utilizadas para personalizar o comportamento de validação do elemento. As propriedades suportadas restrições variam consoante o tipo de elemento. Alguns tipos de elemento não suporta a personalização do comportamento de validação e assim não ter nenhuma propriedade de restrições. |
| opções | Não | Propriedades adicionais personalizar o comportamento do elemento. Semelhante ao `constraints`, as propriedades suportadas variam consoante o tipo de elemento. |
| Visível | Não | Indica se o elemento é apresentado. Se `true`, o elemento e elementos subordinados aplicáveis são apresentados. O valor predefinido é `true`. Utilize [funções lógicas](create-uidefinition-functions.md#logical-functions) para controlar dinamicamente o valor desta propriedade.

## <a name="elements"></a>Elementos

A documentação de cada elemento contém um exemplo de IU, observações de esquema, o comportamento do elemento (normalmente concerning validação e personalização suportada) e saída de exemplo.

- [Microsoft.Common.DropDown](microsoft-common-dropdown.md)
- [Microsoft.Common.FileUpload](microsoft-common-fileupload.md)
- [Microsoft.Common.InfoBox](microsoft-common-infobox.md)
- [Microsoft.Common.OptionsGroup](microsoft-common-optionsgroup.md)
- [Microsoft.Common.PasswordBox](microsoft-common-passwordbox.md)
- [Microsoft.Common.Section](microsoft-common-section.md)
- [Microsoft.Common.TextBlock](microsoft-common-textblock.md)
- [Microsoft.Common.TextBox](microsoft-common-textbox.md)
- [Microsoft.Compute.CredentialsCombo](microsoft-compute-credentialscombo.md)
- [Microsoft.Compute.SizeSelector](microsoft-compute-sizeselector.md)
- [Microsoft.Compute.UserNameTextBox](microsoft-compute-usernametextbox.md)
- [Microsoft.Network.AvailabilityZoneDropDown](microsoft-network-availabilityzonedropdown.md)
- [Microsoft.Network.PublicIpAddressCombo](microsoft-network-publicipaddresscombo.md)
- [Microsoft.Network.VirtualNetworkCombo](microsoft-network-virtualnetworkcombo.md)
- [Microsoft.Storage.MultiStorageAccountCombo](microsoft-storage-multistorageaccountcombo.md)
- [Microsoft.Storage.StorageAccountSelector](microsoft-storage-storageaccountselector.md)

## <a name="next-steps"></a>Passos Seguintes
Para uma introdução ao criar as definições de IU, consulte [introdução CreateUiDefinition](create-uidefinition-overview.md).
