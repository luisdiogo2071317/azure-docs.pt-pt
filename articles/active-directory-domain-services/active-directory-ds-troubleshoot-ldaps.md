---
title: 'Azure Active Directory Domain Services: Resolução de problemas configuração de Secure LDAP | Documentos da Microsoft'
description: Resolução de problemas de Secure LDAP para serviços de domínio do Azure AD
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: ''
editor: ''
ms.assetid: 81208c0b-8d41-4f65-be15-42119b1b5957
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/21/2018
ms.author: ergreenl
ms.openlocfilehash: e3a31749407f9ec0494e8452b602ed9966c5ab83
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/03/2018
ms.locfileid: "39504214"
---
# <a name="azure-ad-domain-services---troubleshooting-secure-ldap-configuration"></a>Serviços de domínio do Azure AD - configuração de resolução de problemas do LDAP seguro

Este artigo fornece resoluções para comuns com problemas ao [configurar o secure LDAP](active-directory-ds-admin-guide-configure-secure-ldap.md) para serviços de domínio do Azure AD.

## <a name="aadds101-secure-ldap-network-security-group-configuration"></a>AADDS101: Configuração do grupo de segurança de rede LDAP seguro

**Mensagem de alerta:**

*Secure LDAP via internet está ativada para o domínio gerido. No entanto, o acesso à porta 636 não está bloqueado através de um grupo de segurança de rede. Isso pode expor as contas de utilizador no domínio gerido a ataques de força bruta de palavra-passe.*

### <a name="secure-ldap-port"></a>Porta LDAP segura

Quando o LDAP seguro está ativado, recomendamos que crie regras adicionais para permitir o acesso de entrada de LDAPS apenas a partir de certos endereços IP. Estas regras protejam contra ataques de força bruta que poderiam representar uma ameaça de segurança para o seu domínio. Porta 636 permite o acesso ao seu domínio gerido. Eis como atualizar o seu NSG para permitir o acesso de Secure LDAP:

1. Navegue para o [guia de grupos de segurança de rede](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.Network%2FNetworkSecurityGroups) no portal do Azure
2. Escolha o NSG associado com o domínio da tabela.
3. Clique em **regras de segurança de entrada**
4. Criar a regra de porta 636
   1. Clique em **adicionar** na barra de navegação superior.
   2. Escolher **endereços IP** para a origem.
   3. Especifique os intervalos de porta de origem para esta regra.
   4. Entrada "636" para os intervalos de portas de destino.
   5. Protocolo é **TCP**.
   6. Dê a regra de um nome apropriado, a descrição e a prioridade. Prioridade desta regra deve ser superior a prioridade de sua regra de "Negar todos os", se tiver uma.
   7. Clique em **OK**.
5. Certifique-se de que a regra foi criada.
6. Verifique o estado de funcionamento do seu domínio nas duas horas para se certificar de que concluiu os passos corretamente.

> [!TIP]
> Porta 636 não é a única regra necessária para os serviços de domínio do Azure AD sem problemas. Para saber mais, visite o [orientações de redes](active-directory-ds-networking.md) ou [configuração de NSG de resolução de problemas](active-directory-ds-troubleshoot-nsg.md) artigos.
>

## <a name="aadds502-secure-ldap-certificate-expiring"></a>AADDS502: Secure LDAP certificado prestes a expirar

**Mensagem de alerta:**

*O certificado de secure LDAP para o domínio gerido irá expirar em [date]].*

**Resolução:**

Criar um novo certificado de secure LDAP ao seguir os passos descritos no [configurar o secure LDAP](active-directory-ds-admin-guide-configure-secure-ldap.md) artigo.

## <a name="contact-us"></a>Contacte-nos
Contacte a equipa de produto do Azure Active Directory Domain Services para [partilhar comentários ou para o suporte](active-directory-ds-contact-us.md).
