---
title: Serviços do Azure que suportam a identidade de serviço geridas
description: Lista de serviços que suportam a identidade de serviço gerida e autenticação do Azure AD
services: active-directory
author: daveba
ms.author: daveba
ms.date: 03/28/2018
ms.topic: reference
ms.service: active-directory
manager: mtillman
ms.openlocfilehash: c78ba7b6c48ffddd44ef35340e9eb69a3ed32f40
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="services-that-support-managed-service-identity"></a>Serviços que suportam a identidade de serviço geridas 

Identidade de serviço geridas fornece serviços do Azure com uma identidade gerido automaticamente no Azure Active Directory. Utilizar uma identidade gerida, pode autenticar a qualquer serviço que suporta a autenticação do Azure AD sem ter de credenciais no seu código. Estamos no processo de integração autenticação MSI e o Azure AD através do Azure. Verifique back frequentemente para atualizações.

## <a name="azure-services-that-support-managed-service-identity"></a>Serviços do Azure que suportam a identidade de serviço geridas

Os seguintes serviços do Azure suportam uma identidade de serviço geridas.

| Serviço | Estado | Data | Configurar | Obter um token |
| ------- | ------ | ---- | --------- | ----------- |
| Máquinas Virtuais do Azure | Pré-visualização | Setembro de 2017 | [Portal do Azure](qs-configure-portal-windows-vm.md)<br>[PowerShell](qs-configure-powershell-windows-vm.md)<br>[CLI do Azure](qs-configure-cli-windows-vm.md)<br>[Modelos Azure Resource Manager](qs-configure-template-windows-vm.md) | [REST](how-to-use-vm-token.md#get-a-token-using-http)<br>[.NET](how-to-use-vm-token.md#get-a-token-using-c)<br>[Bash/Curl](how-to-use-vm-token.md#get-a-token-using-curl)<br>[Go](how-to-use-vm-token.md#get-a-token-using-go)<br>[PowerShell](how-to-use-vm-token.md#get-a-token-using-azure-powershell) |
| Serviço de Aplicações do Azure | Pré-visualização | Setembro de 2017 | [Portal do Azure](/azure/app-service/app-service-managed-service-identity#using-the-azure-portal)<br>[Modelo do Azure Resource Manager](/azure/app-service/app-service-managed-service-identity#using-an-azure-resource-manager-template) | [.NET](/azure/app-service/app-service-managed-service-identity#asal)<br>[REST](/azure/app-service/app-service-managed-service-identity#using-the-rest-protocol) |
| Funções do Azure | Pré-visualização | Setembro de 2017 | [Portal do Azure](/azure/app-service/app-service-managed-service-identity#using-the-azure-portal)<br>[Modelo do Azure Resource Manager](/azure/app-service/app-service-managed-service-identity#using-an-azure-resource-manager-template) | [.NET](/azure/app-service/app-service-managed-service-identity#asal)<br>[REST](/azure/app-service/app-service-managed-service-identity#using-the-rest-protocol) |
| Azure Data Factory V2 | Pré-visualização | Novembro de 2017 | [Portal do Azure](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity)<br>[PowerShell](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity-using-powershell)<br>[REST](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity-using-rest-api)<br>[SDK](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity-using-sdk) |

## <a name="azure-services-that-support-azure-ad-authentication"></a>Suporte do Azure AD que a autenticação de serviços do Azure

Os seguintes serviços de suportam a autenticação do Azure AD e foi testados com serviços de cliente que utilizam a identidade de serviço geridas.

| Serviço | ID do Recurso | Estado | Data | Atribuir acesso |
| ------- | ----------- | ------ | ---- | ------------- |
| Azure Resource Manager | https://management.azure.com | Disponível | Setembro de 2017 | [Portal do Azure](howto-assign-access-portal.md) <br>[PowerShell](howto-assign-access-powershell.md) <br>[CLI do Azure](howto-assign-access-CLI.md) |
| Azure Key Vault | https://vault.azure.net | Disponível | Setembro de 2017 | |
| Azure Data Lake | https://datalake.azure.net | Disponível | Setembro de 2017 | |
| SQL do Azure | https://database.windows.net | Disponível | Outubro de 2017 | |
| Azure Event Hubs | https://eventhubs.azure.net | Disponível | Dezembro de 2017 | |
| Service Bus do Azure | https://servicebus.azure.net | Disponível | Dezembro de 2017 | |