---
title: Perguntas mais frequentes (FAQ) sobre os serviços FHIR no Azure - FHIR na API do Azure
description: Este artigo de perguntas Freqüentes responde a perguntas mais frequentes sobre a API do Azure para FHIR
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.topic: reference
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: 7403a23e236c14d77672d5b80d953b1e11088f8a
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/07/2019
ms.locfileid: "55824137"
---
# <a name="frequently-asked-questions-about-azure-api-for-fhir"></a>Perguntas mais frequentes sobre a API do Azure para FHIR

## <a name="storage-location"></a>Localização do armazenamento

**São os dados por trás do FHIR&reg; APIs armazenados no Azure?** Sim, os dados são armazenados em bases de dados geridas no Azure. A API do Azure para FHIR não fornece acesso direto ao armazenamento de dados subjacente.

## <a name="identity-providers"></a>Fornecedores de identidade

Suportamos atualmente o Microsoft Azure Active Directory como o fornecedor de identidade.

## <a name="supported-fhir-version"></a>Versão suportada do FHIR

Atualmente suportamos versão 3.0.1. Ver [funcionalidades suportadas](fhir-features-supported.md) para obter detalhes.

## <a name="oss-and-azure-api-for-fhir"></a>API de sistemas operacionais e do Azure para FHIR

O que é a diferença entre o servidor de Open FHIR de Microsoft de origem para o Azure e a API do Azure para FHIR? A API do Azure para FHIR é uma versão de alojadas e gerida de sistemas operacionais Microsoft FHIR Server para o Azure. No serviço gerido, a Microsoft fornece a manutenção, atualizações, etc. Ao executar o servidor de FHIR de sistemas operacionais do Azure, tem acesso direto para os serviços subjacentes, mas também é responsáveis por manter, atualizar o servidor e todo o trabalho de conformidade necessário se armazenar dados PHI.

## <a name="next-steps"></a>Passos Seguintes

Neste artigo, já tenha lido algumas das perguntas mais frequentes sobre a API do Azure para FHIR. Leia sobre as funcionalidades de API suportadas no servidor de Microsoft FHIR para o Azure.
 
>[!div class="nextstepaction"]
>[Funcionalidades FHIR suportadas](fhir-features-supported.md)