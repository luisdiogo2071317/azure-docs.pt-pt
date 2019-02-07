---
title: Implementar a API do Azure para FHIR através do Portal do Azure
description: Implemente a API do Azure para FHIR através do Portal do Azure.
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.topic: quickstart
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: 8699abfa8cc9b6675c15c8cd9b7cbb5bb5e148cd
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/07/2019
ms.locfileid: "55823547"
---
# <a name="quickstart-deploy-azure-api-for-fhir-using-azure-portal"></a>Início rápido: Implementar a API do Azure para FHIR através do portal do Azure

Neste início rápido, irá aprender a implementar a API do Azure para FHIR através do Portal do Azure.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="create-new-resource"></a>Criar novo recurso

Abra o [portal de pré-visualização do Azure](https://preview.portal.azure.com) e clique em **criar um recurso**

![Criar um recurso](media/quickstart-paas-portal/portal-create-resource.png)

## <a name="search-for-azure-api-for-fhir"></a>Procure a API do Azure para FHIR

Pode encontrar a API do Azure para FHIR digitando "FHIR" na caixa de pesquisa:

![Procurar APIs de cuidados de saúde](media/quickstart-paas-portal/portal-search-healthcare-apis.png)

## <a name="create-azure-api-for-fhir-account"></a>Criar API do Azure para a conta FHIR

Selecione **criar** para criar uma nova API do Azure para a conta FHIR:

![Criar API do Azure para a conta FHIR](media/quickstart-paas-portal/portal-create-healthcare-apis.png)

## <a name="enter-account-details"></a>Introduza os detalhes da conta

Selecione um grupo de recursos existente ou crie um novo, escolha um nome para a conta e, finalmente, clique em **rever + criar**:

![Detalhes da nova api cuidados de saúde](media/quickstart-paas-portal/portal-new-healthcareapi-details.png)

Confirmar a criação e implementação de API de FHIR de await.

## <a name="additional-settings"></a>Definições adicionais

Clique em **seguinte: Definições adicionais** para configurar o objeto de identidade IDs que deve ter permissão para aceder a esta API do Azure para FHIR:

![Configurar os IDs de objeto permitidos](media/quickstart-paas-portal/configure-allowed-oids.png)

Ver [como encontrar IDs de objeto de identidade](find-identity-object-ids.md) para obter detalhes sobre como localizar a identidade IDs de objeto para os utilizadores e principais de serviço.

## <a name="fetch-fhir-api-capability-statement"></a>Obter a declaração de capacidade de API de FHIR

Para validar que a nova conta de API de FHIR é aprovisionada, buscar uma instrução de capacidade ao apontar um navegador para `https://<ACCOUNT-NAME>.azurehealthcareapis.com/metadata`.

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não for necessário, pode eliminar o grupo de recursos, a API do Azure para FHIR e todos os recursos relacionados. Para tal, selecione o grupo de recursos que contém a API do Azure para a conta FHIR, selecione **eliminar grupo de recursos**, em seguida, confirme o nome do grupo de recursos para eliminar.

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, implementou a API do Azure para FHIR na sua subscrição. Para saber como aceder à API de FHIR com o Postman, avance para o tutorial do Postman.

>[!div class="nextstepaction"]
>[Aceda à API de FHIR com o Postman](access-fhir-postman-tutorial.md)