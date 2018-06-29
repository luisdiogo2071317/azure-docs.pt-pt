---
title: Serviços do Azure que suportam a identidade de serviço geridas
description: Lista de serviços que suportam a identidade de serviço gerida e autenticação do Azure AD
services: active-directory
author: daveba
ms.author: daveba
ms.date: 06/27/2018
ms.topic: reference
ms.service: active-directory
ms.component: msi
manager: mtillman
ms.openlocfilehash: b39d5144eacfe6d621dae54da66b1306db516476
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37060159"
---
# <a name="services-that-support-managed-service-identity"></a>Serviços que suportam a identidade de serviço geridas 

Identidade de serviço geridas fornece serviços do Azure com uma identidade gerido automaticamente no Azure Active Directory. Utilizar uma identidade gerida, pode autenticar a qualquer serviço que suporta a autenticação do Azure AD sem ter de credenciais no seu código. Estamos no processo de integração autenticação MSI e o Azure AD através do Azure. Verifique back frequentemente para atualizações.

## <a name="azure-services-that-support-managed-service-identity"></a>Serviços do Azure que suportam a identidade de serviço geridas

Os seguintes serviços do Azure suportam uma identidade de serviço geridas.

| Serviço | Estado | Date | Configurar | Obter um token |
| ------- | ------ | ---- | --------- | ----------- |
| Máquinas Virtuais do Azure | Pré-visualização | Setembro de 2017 | [Portal do Azure](qs-configure-portal-windows-vm.md)<br>[PowerShell](qs-configure-powershell-windows-vm.md)<br>[CLI do Azure](qs-configure-cli-windows-vm.md)<br>[Modelos Azure Resource Manager](qs-configure-template-windows-vm.md) | [REST](how-to-use-vm-token.md#get-a-token-using-http)<br>[.NET](how-to-use-vm-token.md#get-a-token-using-c)<br>[Bash/Curl](how-to-use-vm-token.md#get-a-token-using-curl)<br>[Go](how-to-use-vm-token.md#get-a-token-using-go)<br>[PowerShell](how-to-use-vm-token.md#get-a-token-using-azure-powershell) |
| Serviço de Aplicações do Azure | Disponível | De 2018 Junho | [Portal do Azure](/azure/app-service/app-service-managed-service-identity#using-the-azure-portal)<br>[CLI do Azure](/azure/app-service/app-service-managed-service-identity#using-the-azure-cli)<br>[Azure PowerShell](/azure/app-service/app-service-managed-service-identity#using-azure-powershell)<br>[Modelo do Azure Resource Manager](/azure/app-service/app-service-managed-service-identity#using-an-azure-resource-manager-template) | [REST](/azure/app-service/app-service-managed-service-identity#using-the-rest-protocol)<br>[.NET](/azure/app-service/app-service-managed-service-identity#asal)<br>[JavaScript](/azure/app-service/app-service-managed-service-identity#token-js)<br>[PowerShell](/azure/app-service/app-service-managed-service-identity#token-powershell)  |
| Funções do Azure | Disponível | De 2018 Junho | [Portal do Azure](/azure/app-service/app-service-managed-service-identity#using-the-azure-portal)<br>[CLI do Azure](/azure/app-service/app-service-managed-service-identity#using-the-azure-cli)<br>[Azure PowerShell](/azure/app-service/app-service-managed-service-identity#using-azure-powershell)<br>[Modelo do Azure Resource Manager](/azure/app-service/app-service-managed-service-identity#using-an-azure-resource-manager-template) | [REST](/azure/app-service/app-service-managed-service-identity#using-the-rest-protocol)<br>[.NET](/azure/app-service/app-service-managed-service-identity#asal)<br>[JavaScript](/azure/app-service/app-service-managed-service-identity#token-js)<br>[PowerShell](/azure/app-service/app-service-managed-service-identity#token-powershell) |
| Azure Data Factory V2 | Pré-visualização | Novembro de 2017 | [Portal do Azure](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity)<br>[PowerShell](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity-using-powershell)<br>[REST](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity-using-rest-api)<br>[SDK](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity-using-sdk) |


## <a name="azure-services-that-support-azure-ad-authentication"></a>Suporte do Azure AD que a autenticação de serviços do Azure

Os seguintes serviços de suportam a autenticação do Azure AD e foi testados com serviços de cliente que utilizam a identidade de serviço geridas.

| Serviço | ID do Recurso | Estado | Date | Atribuir acesso |
| ------- | ----------- | ------ | ---- | ------------- |
| Azure Resource Manager | https://management.azure.com/ | Disponível | Setembro de 2017 | [Portal do Azure](howto-assign-access-portal.md) <br>[PowerShell](howto-assign-access-powershell.md) <br>[CLI do Azure](howto-assign-access-CLI.md) |
| Azure Key Vault | https://vault.azure.net | Disponível | Setembro de 2017 | |
| Azure Data Lake | https://datalake.azure.net/ | Disponível | Setembro de 2017 | |
| SQL do Azure | https://database.windows.net/ | Disponível | Outubro de 2017 | |
| Azure Event Hubs | https://eventhubs.azure.net | Disponível | Dezembro de 2017 | |
| Service Bus do Azure | https://servicebus.azure.net | Disponível | Dezembro de 2017 | |
| Storage do Azure | https://storage.azure.com/ | Pré-visualização | Maio de 2018 | |
