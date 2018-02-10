---
title: "Azure Active Directory dos serviços de domínio: Resolução de problemas configuração de LDAP seguro | Microsoft Docs"
description: "LDAP seguro de resolução de problemas para os serviços de domínio do Azure AD"
services: active-directory-ds
documentationcenter: 
author: eringreenlee
manager: 
editor: 
ms.assetid: 81208c0b-8d41-4f65-be15-42119b1b5957
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/02/2018
ms.author: ergreenl
ms.openlocfilehash: c6c47d1f9776f9e45711d8139661255dcd44c2a1
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/09/2018
---
# <a name="azure-ad-domain-services---troubleshooting-secure-ldap-configuration"></a>Serviços de domínio do Azure AD - configuração de resolução de problemas LDAP seguro

Este artigo fornece resoluções para comuns problemas quando [configuração de LDAP seguro](active-directory-ds-admin-guide-configure-secure-ldap.md) para serviços de domínio do Azure AD.

## <a name="aadds101-secure-ldap-network-security-group-configuration"></a>AADDS101: Configuração do grupo de segurança de rede de LDAP seguro

**Mensagem de alerta:**

*LDAP seguro através da internet está ativada para o domínio gerido. No entanto, o acesso à porta 636 não está bloqueado para baixo a utilização de um grupo de segurança de rede. Isto poderá expor contas de utilizador no domínio gerido para ataques de força bruta de palavra-passe.*

### <a name="secure-ldap-port"></a>Porta segura do LDAP

Quando o LDAP seguro está ativado, recomendamos que crie regras adicionais para permitir o acesso de entrada de LDAPS apenas a partir de determinados endereços IP. Estas regras protejam o seu domínio contra ataques de força bruta que podem implicar uma ameaça de segurança. Porta 636 permite o acesso ao seu domínio gerido. Eis como atualizar o NSG para permitir o acesso para proteger o LDAP:

1. Navegue para o [separador de grupos de segurança de rede](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.Network%2FNetworkSecurityGroups) no portal do Azure
2. Escolha o NSG associado o domínio da tabela.
3. Clique em **regras de segurança de entrada**
4. Criar a regra de portas 636
   1. Clique em **adicionar** na barra de navegação superior.
   2. Escolha **endereços IP** para a origem.
   3. Especifique os intervalos de portas de origem para esta regra.
   4. Introduza "636" para intervalos de portas de destino.
   5. Protocolo é **TCP**.
   6. Atribua a regra, um nome adequado, a descrição e a prioridade. Prioridade desta regra deve ser superior a prioridade a regra "Deny todos os", se tiver uma.
   7. Clique em **OK**.
5. Certifique-se de que a regra foi criada.
6. Verifique o estado de funcionamento do seu domínio nas duas horas para se certificar de que concluiu os passos corretamente.

> [!TIP]
> Porta 636 não é a regra apenas necessária para serviços de domínio do Azure AD para serem executados facilmente. Para mais informações, visite o [diretrizes de redes](active-directory-ds-networking.md) ou [configuração de resolução de problemas de NSG](active-directory-ds-troubleshoot-nsg.md) artigos.
>

## <a name="contact-us"></a>Contacte-nos
Contacte a equipa de produto do Azure Active Directory Domain Services para [partilhar comentários ou para suporte](active-directory-ds-contact-us.md).
