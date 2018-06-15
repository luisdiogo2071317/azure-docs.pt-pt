---
title: Utilizar MSI na Shell de nuvem do Azure | Microsoft Docs
description: Autenticar código com MSI na Shell de nuvem do Azure
services: azure
documentationcenter: ''
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 04/14/2018
ms.author: juluk
ms.openlocfilehash: 99577faf7328dc773a9da5f7c1227aa63600aa0a
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/18/2018
ms.locfileid: "31517444"
---
# <a name="use-msi-in-azure-cloud-shell"></a>Utilizar MSI na Shell de nuvem do Azure

Shell de nuvem do Azure suporta autorização com identidades de serviço geridas (MSI). Utilize esta opção para obter os tokens de acesso para comunicar de forma segura com os serviços do Azure.

## <a name="about-managed-service-identity-msi"></a>Sobre a identidade de serviço geridas (MSI)
Um desafio comum quando a criação de aplicações em nuvem como gerir em segurança as credenciais que têm de ser no seu código para autenticar a serviços em nuvem. Na Shell de nuvem poderá ter de obtenção do Cofre de chaves para uma credencial de um script poderá ter de autenticar.

Identidade de serviço geridas (MSI) permite a resolver este problema mais simples, conferindo aos serviços do Azure uma identidade gerida automaticamente no Azure Active Directory (Azure AD). Pode utilizar esta identidade para autenticar a qualquer serviço que suporta a autenticação do Azure AD, incluindo o Cofre de chaves, sem ter as credenciais no seu código.

## <a name="acquire-access-token-in-cloud-shell"></a>Adquirir token de acesso na Shell de nuvem

Execute os seguintes comandos para definir o seu token de acesso do MSI como uma variável de ambiente, `access_token`.
```
response=$(curl http://localhost:50342/oauth2/token --data "resource=https://management.azure.com/" -H Metadata:true -s)
access_token=$(echo $response | python -c 'import sys, json; print (json.load(sys.stdin)["access_token"])')
echo The MSI access token is $access_token
```

## <a name="handling-token-expiration"></a>Processamento de expiração do token

O subsistema MSI local coloca em cache tokens. Por conseguinte, pode chamar-as vezes que é que gosta e resulta de uma chamada no-durante a transmissão para o Azure AD apenas se:
- ocorre uma falha de acerto na cache devido a nenhum token na cache
- o token expirou

Se a cache o token no seu código, estará preparado para processar cenários em que o recurso indica que o token expirou.

Para processar erros token, visite o [página MSI sobre curling tokens de acesso do MSI](https://docs.microsoft.com/azure/active-directory/managed-service-identity/how-to-use-vm-token#error-handling).

## <a name="next-steps"></a>Passos Seguintes
[Saiba mais sobre MSI](https://docs.microsoft.com/azure/active-directory/managed-service-identity/overview)  
[Adquirir tokens de acesso de VMs de MSI](https://docs.microsoft.com/azure/active-directory/managed-service-identity/how-to-use-vm-token)
