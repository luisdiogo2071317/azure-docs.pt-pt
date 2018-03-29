---
title: Proxy de aplicações do Azure AD e Qlik sentido | Microsoft Docs
description: Ativar o Proxy da aplicação no portal do Azure e instalar os conectores para o proxy reverso.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 03/26/2018
ms.author: markvi
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 331f8ed2e77a076dd8969dc37add1cdeafc852dc
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/28/2018
---
# <a name="application-proxy-and-qlik-sense"></a>Proxy de aplicações e Qlik sentido 
Azure Active Directory para o Proxy de aplicações e Qlik sentido tem parcerias em conjunto para se certificar de que é facilmente capazes de utilizar o Proxy de aplicações para fornecer acesso remoto para a sua implementação Qlik sentido.  

## <a name="prerequisites"></a>Pré-requisitos 
O resto deste cenário pressupõe que efetuar o seguinte:
 
- Configurado [Qlik sentido](https://community.qlik.com/docs/DOC-19822). 
- Instalar um conector do Proxy de aplicações 

## <a name="install-an-application-proxy-connector"></a>Instalar um conector do Proxy de aplicações 
Se já tiver ativado o Proxy de aplicações e tem um conector instalado, pode ignorar esta secção e mover para [adicionar a sua aplicação para o Azure AD com o Proxy da aplicação](application-proxy-ping-access.md). 

O conector do Proxy de aplicações é um serviço do Windows Server que direciona o tráfego do que os empregados remotos para as aplicações publicadas. Para obter mais instruções de instalação, consulte [ativar o Proxy da aplicação no portal do Azure](active-directory-application-proxy-enable.md). 


1. Inicie sessão no [Portal do Azure](https://portal.azure.com/) como administrador global. 
2. Selecione o Azure Active Directory > proxy de aplicações. 
3. Selecione a transferir o conector para iniciar a transferência de conector do Proxy de aplicações. Siga as instruções de instalação. 
 
>[!NOTE]
>Transferir o conector automaticamente deve ativar o Proxy de aplicações para o seu diretório, mas se não pode selecionar **ativar Proxy de aplicações**. 
 
## <a name="publish-your-applications-in-azure"></a>Publicar as suas aplicações no Azure 
Para publicar QlikSense, terá de publicar duas aplicações no Azure.  

### <a name="application-1"></a>Aplicação #1: 
Siga estes passos para publicar a aplicação. Para um mais detalhadas instruções dos passos 1-8, consulte [publicar aplicações através do Proxy de aplicações do Azure AD](application-proxy-publish-azure-portal.md). 


1. Se não na última secção, inicie sessão no portal do Azure como um administrador global. 
2. Selecione **do Azure Active Directory** > **aplicações empresariais**. 
3. Selecione **adicionar** na parte superior do painel. 
4. Selecione **no local aplicação**. 
5.       Preencha os campos obrigatórios, com informações sobre a nova aplicação. Utilize as seguintes orientações para as definições: 
    - **URL interno**: esta aplicação deve ter um URL interno que é o URL de QlikSense próprio. Por exemplo, **https&#58;//demo.qlikemm.com** 
    - **Método de pré-autenticação**: Azure Active Directory (recomendado mas não é necessária) 
1.       Selecione **adicionar** na parte inferior do painel. A aplicação é adicionada e abre o menu de início rápido. 
2.       No menu de início rápido, selecione **atribuir um utilizador de teste**, e adicione, pelo menos, um utilizador para a aplicação. Certifique-se que esta conta de teste tem acesso à aplicação no local. 
3.       Selecione **atribuir** para guardar a atribuição de utilizador de teste. 
4.       (Opcional) No painel de gestão de aplicações, selecione o início de sessão único. Escolha **delegação restrita de Kerberos** do menu pendente e preenchimento enviados os campos obrigatórios, com base na sua configuração Qlik. Selecione **Guardar**. 

### <a name="application-2"></a>Aplicação #2: 
Siga os mesmos passos para aplicação n. º 1, com as seguintes exceções: 

**Passo #5**: O URL interno deve ser o URL de QlikSense com a porta de autenticação utilizada pela aplicação. A predefinição é **4244** para HTTPS e 4248 para HTTP. Ex: **https&#58;//demo.qlik.com:4244** 
**passo #10:** não configurar o SSO e deixe o **único início de sessão desativado**
 
 
## <a name="testing"></a>Testes 
A aplicação está agora pronta para testar. O URL externo que utilizou para publicar QlikSense na aplicação n. º 1 e o início de sessão como um utilizador atribuído a ambas as aplicações de acesso.  

## <a name="next-steps"></a>Próximos Passos

- [Publicar aplicações com o Proxy de aplicações](application-proxy-publish-azure-portal.md)
- [Trabalhar com conetores da Proxy da aplicação](active-directory-application-proxy-connectors-azure-portal.md).
