---
title: Proteger mensagens B2B com certificados no Azure Logic Apps | Microsoft Docs
description: "Adicione certificados para proteger mensagens B2B com o pacote de integração do Enterprise"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: 4cbffd85-fe8d-4dde-aa5b-24108a7caa7d
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/03/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 708bdcddede71186c48ae7d4034cc9df0bd61391
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/27/2018
---
# <a name="secure-b2b-messages-with-certificates"></a>Mensagens B2B seguras com certificados

Por vezes, terá de manter a comunicação de B2B confidencial. Para o ajudar a proteger a comunicação de B2B para a sua empresa aplicações de integração, especificamente as logic apps, pode adicionar certificados à sua conta de integração. Os certificados são documentos digitais que verifiquem a identidade dos participantes eletrónicas comunicações.
Certificados de ajudam a proteger a comunicação das seguintes maneiras:

* Encriptar o conteúdo da mensagem
* Assinar digitalmente mensagens  

Pode utilizar estes certificados nas suas aplicações de integração do enterprise:

* Certificados públicos, tem de ser adquiridos de uma autoridade de certificação (AC).
* Certificados privados, pode emitir por si. Estes certificados são, por vezes, referidos como certificados autoassinados.

## <a name="upload-a-public-certificate"></a>Carregar um certificado público

Para utilizar um *certificado público* nas logic apps que têm capacidades de B2B, primeiro tem de carregar o certificado à sua conta de integração. Depois de definir as propriedades no [contratos](logic-apps-enterprise-integration-agreements.md) que criar, o certificado está disponível para ajudar a proteger as mensagens B2B.

1. Inicie sessão no [portal do Azure](https://portal.azure.com).

2. No menu principal do Azure, selecione **todos os serviços**. Na caixa de pesquisa, introduza "integração" e, em seguida, selecione **contas de automatização**.

   ![Localizar a conta de integração](media/logic-apps-enterprise-integration-certificates/overview-1.png)  

3. Em **contas de automatização**, selecione a conta de integração em que pretende adicionar o certificado.

   ![Selecione a conta de integração para o qual pretende adicionar o certificado](media/logic-apps-enterprise-integration-certificates/overview-3.png)  

4. Escolha o **certificados** mosaico.  

   ![Escolha "Certificados"](media/logic-apps-enterprise-integration-certificates/certificate-1.png)

5. Em **certificados**, escolha **adicionar**.

   ![Escolha "Adicionar"](media/logic-apps-enterprise-integration-certificates/certificate-2.png)

6. Em **adicionar certificado**, forneça os detalhes para o seu certificado.
   
   1. Introduza o seu certificado **nome**. Para o tipo de certificado, selecione **pública**.

   2. No lado direito do **certificado** caixa, escolha o ícone da pasta. 
   Localize e selecione o ficheiro de certificado que pretende carregar. 
   Quando tiver terminado, escolha **OK**.

      ![Carregar o certificado público](media/logic-apps-enterprise-integration-certificates/certificate-3.png)

   Azure carrega o certificado depois de confirmar a sua seleção.

   ![Ver o novo certificado](media/logic-apps-enterprise-integration-certificates/certificate-4.png) 

## <a name="upload-a-private-certificate"></a>Carregar um certificado privado

Para utilizar um *certificado privado* nas logic apps que têm capacidades de B2B, primeiro tem de carregar o certificado à sua conta de integração. Terá também de ter uma chave privada que adicionar primeiro a [Cofre de chaves do Azure](../key-vault/key-vault-get-started.md). 

Depois de definir as propriedades no [contratos](logic-apps-enterprise-integration-agreements.md) que criar, o certificado está disponível para ajudar a proteger as mensagens B2B.

> [!NOTE]
> Para certificados privados, certifique-se de que adiciona um certificado público correspondente a aparecer no [contratos AS2](logic-apps-enterprise-integration-as2.md) enviar e receber as definições de assinatura e encriptação de mensagens.

1. [Adicionar a chave privada para o Cofre de chaves do Azure](../key-vault/key-vault-get-started.md#add) e fornecer um **nome da chave**.
   
2. Autorize Azure Logic Apps para executar operações no Cofre de chaves do Azure. Para conceder acesso para o principal de serviço Logic Apps, utilize o comando do PowerShell, [Set-AzureRmKeyVaultAccessPolicy](https://docs.microsoft.com/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy), por exemplo:

   `Set-AzureRmKeyVaultAccessPolicy -VaultName 'TestcertKeyVault' -ServicePrincipalName 
   '7cd684f4-8a78-49b0-91ec-6a35d38739ba' -PermissionsToKeys decrypt, sign, get, list`
 
3. Inicie sessão no [portal do Azure](https://portal.azure.com).

4. No menu principal do Azure, selecione **todos os serviços**. Na caixa de pesquisa, introduza "integração" e, em seguida, selecione **contas de automatização**.

   ![Localizar a conta de integração](media/logic-apps-enterprise-integration-certificates/overview-1.png) 

5. Em **contas de automatização**, selecione a conta de integração em que pretende adicionar o certificado.

6. Escolha o **certificados** mosaico.  

   ![Escolha o mosaico de certificados](media/logic-apps-enterprise-integration-certificates/certificate-1.png)

7. Em **certificados**, escolha **adicionar**.   

   ![Escolha o botão de adição](media/logic-apps-enterprise-integration-certificates/certificate-2.png)

8. Em **adicionar certificado**, forneça os detalhes para o seu certificado.
   
   1. Introduza o seu certificado **nome**. Para o tipo de certificado, selecione **privada**.

   2. No lado direito do **certificado** caixa, escolha o ícone da pasta. 
   Localize e selecione o ficheiro de certificado que pretende carregar. 
   Além disso, selecione o **grupo de recursos**, **Cofre de chaves**, e **nome da chave**. 
   Quando tiver terminado, escolha **OK**.

      ![Adicionar certificado](media/logic-apps-enterprise-integration-certificates/privatecertificate-1.png)

   Azure carrega o certificado depois de confirmar a sua seleção.

   ![Ver o novo certificado](media/logic-apps-enterprise-integration-certificates/privatecertificate-2.png)

## <a name="next-steps"></a>Passos Seguintes

* [Criar um contrato de B2B](logic-apps-enterprise-integration-agreements.md)
