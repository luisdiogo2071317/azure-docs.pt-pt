---
title: Utilizar o Cofre de chaves do Azure com aplicações geridas | Documentos da Microsoft
description: Mostra como utilizar o acesso aos segredos no Azure Key Vault ao implementar aplicações geridas
services: managed-applications
author: tfitzmac
manager: timlt
ms.service: managed-applications
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.date: 07/09/2018
ms.author: tomfitz
ms.openlocfilehash: 232bea437b38335bdaa189e504d4e5fd9b080a05
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38724058"
---
# <a name="access-key-vault-secret-when-deploying-azure-managed-applications"></a>Segredo do Key Vault acesso ao implementar aplicações geridas do Azure

Quando precisar passar um valor seguro (como uma palavra-passe) como um parâmetro durante a implementação, pode recuperar o valor de uma [do Azure Key Vault](../key-vault/key-vault-whatis.md). Para aceder ao Cofre de chave ao implementar aplicações geridas, tem de conceder acesso para o **fornecedor de recursos da aplicação** principal de serviço. Este artigo descreve como configurar o Cofre de chaves para trabalhar com aplicações geridas.

## <a name="enable-template-deployment"></a>Ativar a implementação do modelo

1. No portal, selecione o seu Cofre de chaves.

1. Selecione **Políticas de acesso**.   

   ![Selecione políticas de acesso](./media/key-vault-access/select-access-policies.png)

1. Selecione **clique aqui para mostrar as políticas de acesso avançadas**.

   ![Mostrar as políticas de acesso avançadas](./media/key-vault-access/advanced.png)

1. Selecione **ativar o acesso ao Azure Resource Manager para a implementação de modelo**. Em seguida, selecione **Guardar**.

   ![Ativar a implementação do modelo](./media/key-vault-access/enable-template.png)

## <a name="add-service-as-contributor"></a>Adicionar o serviço como contribuinte

1. Selecione **controlo de acesso (IAM)**.

   ![Selecione o controlo de acesso](./media/key-vault-access/access-control.png)

1. Selecione **Adicionar**.

   ![Selecione adicionar](./media/key-vault-access/add-access-control.png)

1. Selecione **contribuinte** para a função. Procure **fornecedor de recursos da aplicação** e selecione-o nas opções disponíveis.

   ![Pesquisar por fornecedor](./media/key-vault-access/search-provider.png)

1. Selecione **Guardar**.

## <a name="next-steps"></a>Passos Seguintes

Configurou o seu Cofre de chaves para ser acessível durante a implementação de uma aplicação gerida.

* Para obter informações sobre a transmissão de um valor de um cofre de chave como um parâmetro de modelo, consulte [do Azure com o Key Vault para transmitir o valor do parâmetro segura durante a implementação](../azure-resource-manager/resource-manager-keyvault-parameter.md).
* Para obter exemplos de aplicação gerida, veja [aplicativos gerenciados de projetos de exemplo para o Azure](sample-projects.md).
* Para saber como criar um ficheiro de definição de IU para uma aplicação gerida, veja [Introdução a CreateUiDefinition](create-uidefinition-overview.md).