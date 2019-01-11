---
title: Exemplos de modelo de política | Microsoft Docs
description: Exemplos de modelo de política do Azure para a Rede Virtual.
services: virtual-network
documentationcenter: ''
author: jimdial
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: ''
ms.workload: ''
ms.date: 05/02/2018
ms.author: jdial
ms.custom: mvc
ms.openlocfilehash: a0153cf11c3fe817ce397b4d0c47a786a4334f39
ms.sourcegitcommit: e7312c5653693041f3cbfda5d784f034a7a1a8f1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/11/2019
ms.locfileid: "54214815"
---
# <a name="azure-policy-sample-templates-for-virtual-network"></a>Exemplos de modelo de política do Azure para a rede virtual

A seguinte tabela inclui ligações para modelos de exemplo do [Azure Policy](../azure-policy/azure-policy-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Os exemplos encontram-se no [repositório de exemplos do Azure Policy](https://github.com/Azure/azure-policy).

| | |
|---|---|
|**Rede**||
| [NSG X em cada NIC](../azure-policy/scripts/nsg-on-nic.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Exige que um grupo de segurança de rede específico seja utilizado em cada interface de rede virtual. Especifica o ID do grupo de segurança de rede a utilizar. |
| [NSG X em cada sub-rede](../azure-policy/scripts/nsg-on-subnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Exige que um grupo de segurança de rede específico seja utilizado em cada sub-rede virtual. Especifica o ID do grupo de segurança de rede a utilizar. |
| [Sem tabela de rotas](../azure-policy/scripts/no-user-def-route-table.md?toc=%2fazure%2fvirtual-network%2ftoc.json)  |Proíbe que as redes virtuais sejam implementadas com uma tabela de rotas. |
| [Utilizar sub-rede aprovada para interfaces de rede de VM](../azure-policy/scripts/use-approved-subnet-vm-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Exige que as interfaces de rede utilizem uma sub-rede aprovada. Especifica o ID da sub-rede aprovada. |
| [Utilizar vNet aprovada para interfaces de rede de VM](../azure-policy/scripts/use-approved-vnet-vm-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Exige que as interfaces de rede utilizem uma rede virtual aprovada. Especifica o ID da rede virtual aprovada. |
|**Monitorização**||
| [Definição de diagnóstico de auditoria](../azure-policy/scripts/audit-diag-setting.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Audita se as definições de diagnóstico não estão ativadas para tipos de recursos especificados. Especifica uma matriz de tipos de recursos para verificar se as definições de diagnóstico estão ativadas. |
|**Convenções de nome e texto**||
| [Permitir múltiplos padrões de nome](../azure-policy/scripts/allow-multiple-name-patterns.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Permita que um dos muitos padrões de nomes seja utilizado para recursos. |
| [Exigir padrão like](../azure-policy/scripts/enforce-like-pattern.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Certifique-se de que os nomes de recursos cumprem a condição *like* de um padrão. |
| [Exigir padrão de correspondência](../azure-policy/scripts/enforce-match-pattern.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Certifique-se de que os nomes de recursos correspondem a um padrão de nomenclatura especificado. |
| [Exigir padrão de correspondência de etiqueta](../azure-policy/scripts/enforce-tag-match-pattern.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Certifique-se de que um valor de etiqueta corresponde a um padrão de texto. |
|**Etiquetas**||
| [Iniciativa de política de etiquetas de faturação](../governance/policy/samples/billing-tags-policy-initiative.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Exige valores de etiqueta especificados para o nome do produto e o centro de custos. Utiliza políticas incorporadas para aplicar e impor etiquetas necessárias. O utilizador especifica os valores necessários para as etiquetas.  |
| [Impor a etiqueta e o respetivo valor em grupos de recursos](../azure-policy/scripts/enforce-tag-rg.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Exige uma etiqueta e um valor num grupo de recursos. Especifica o nome de etiqueta e o valor necessário.  |
| [Impor etiqueta e o respetivo valor](../azure-policy/scripts/enforce-tag-val.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Exige um nome de etiqueta e um valor especificado. Especifica o nome de etiqueta e o valor a impor.  |
| [Aplicar etiqueta e o respetivo valor predefinido](../azure-policy/scripts/apply-tag-def-val.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Acrescenta um nome de etiqueta e um valor especificado, se essa etiqueta não for fornecida. Especifica o nome de etiqueta e o valor a aplicar.  |
|**Geral**||
| [Localizações permitidas](../azure-policy/scripts/allowed-locs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Exige que todos os recursos sejam implementados nas localizações aprovadas. Especifica uma matriz de localizações aprovadas.  |
| [Tipos de recursos permitidos](../azure-policy/scripts/allowed-res-types.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Garante que apenas os tipos de recursos aprovados são implementados. Especifica uma matriz dos tipos de recursos que são permitidos.  |
| [Tipos de recursos não permitidos](../azure-policy/scripts/not-allowed-res-type.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Proíbe a implementação de tipos de recursos especificados. Especifica uma matriz dos tipos de recursos a bloquear.  |