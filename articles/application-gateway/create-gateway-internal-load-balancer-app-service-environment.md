---
title: Resolver problemas relacionados com um Gateway de aplicação no Azure – ASE de ILB | Documentos da Microsoft
description: Saiba como resolver problemas de um gateway de aplicação utilizando um balanceador de carga interno com um ambiente de serviço de aplicações no Azure
services: vpn-gateway
documentationCenter: na
author: genlin
manager: cshepard
editor: ''
tags: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/06/2018
ms.author: genli
ms.openlocfilehash: 16cfe4c1db8fe9ba4c80f6451611237e3ee12c55
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/13/2018
ms.locfileid: "51617882"
---
# <a name="back-end-server-certificate-is-not-whitelisted-for-an-application-gateway-using-an-internal-load-balancer-with-an-app-service-environment"></a>Certificado de servidor back-end não está na lista de permissões para um gateway de aplicação com um balanceador de carga interno com um ambiente de serviço de aplicações

Este artigo soluciona problemas no seguinte problema: um certificado não está na lista de permissões quando cria um gateway de aplicação com uma carga balanceador interno (ILB), juntamente com um ambiente de serviço de aplicações (ASE) no back-end ao utilizar o SSL de ponta a ponta no Azure.

## <a name="symptoms"></a>Sintomas

Quando cria um gateway de aplicação utilizando um ILB com um ASE no back-end, o servidor de back-end poderá deixar de mau estado de funcionamento. Este problema ocorre se o certificado de autenticação de gateway de aplicação não coincide com o certificado configurado no servidor de back-end. Consulte o seguinte cenário como um exemplo:

**Configuração do Gateway de aplicação:**

- **Serviço de escuta:** múltiplos sites
- **Porta:** 443
- **Nome do anfitrião:** test.appgwtestase.com
- **Certificado SSL:** CN=test.appgwtestase.com
- **Conjunto de back-end:** IP ou FQDN do endereço
- **Endereço IP:**: 10.1.5.11
- **Definições de HTTP:** HTTPS
- **Porta:**: 443
- **Sonda personalizada:** nome de anfitrião – test.appgwtestase.com
- **Certificado de autenticação:** . cer de test.appgwtestase.com
- **Estado de funcionamento do back-end:** mau estado de funcionamento – certificado de servidor de back-end não está na lista de permissões com o Gateway de aplicação.

**Configuração do ASE:**

- **IP DO ILB:** 10.1.5.11
- **Nome de domínio:** appgwtestase.com
- **Serviço de aplicações:** test.appgwtestase.com
- **Enlace de SSL:** SNI SSL – CN=test.appgwtestase.com

Quando acessa o gateway de aplicação, receberá a seguinte mensagem de erro porque o servidor de back-end está em mau estado de funcionamento:

**502 – servidor web recebeu uma resposta inválida enquanto funcionava como um servidor de gateway ou proxy.**

## <a name="solution"></a>Solução

Quando não utiliza um nome de anfitrião para aceder a um Web site HTTPS, o servidor de back-end irá devolver o certificado configurado no Web site predefinido. Para um ASE de ILB, o certificado predefinido é proveniente do certificado ILB. Se não existirem não existem certificados configurados para o ILB, o certificado é proveniente do certificado de aplicação do ASE.

Quando utiliza um nome de domínio completamente qualificado (FQDN) para acessar o ILB, o servidor de back-end irá devolver o certificado correto, que é carregado nas definições de HTTP. Neste caso, considere as seguintes opções:

- Utilize o FQDN do conjunto de back-end do gateway de aplicação para apontar para o endereço IP do ILB. Esta opção só funciona se tiver uma zona DNS privada ou um DNS personalizado configurado. Caso contrário, terá de criar um registo "A" para um DNS público.

- Utilize o certificado carregado no ILB ou o certificado predefinido nas definições de HTTP. O gateway de aplicação obtém o certificado quando acede a um IP do ILB para a sonda.

- Utilize um certificado de caráter universal no ILB e o servidor de back-end.

- Limpar o **utilização para o serviço de aplicações** opção para o gateway de aplicação.

Para reduzir a sobrecarga, pode carregar o certificado ILB nas definições de HTTP para fazer o caminho de sonda de trabalho. (Este passo é apenas para a lista de permissões. Não será usado para comunicação SSL.) Pode recuperar o certificado ILB acessando o ILB com o respetivo endereço IP em HTTPS, em seguida, o SSL a exportar certificado na Base 64 codificada formato CER e carregar o certificado nas respetivas definições de HTTP.

## <a name="need-help-contact-support"></a>Precisa de ajuda? Contactar o suporte

Se precisar de ajuda, ainda [contacte o suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para a sua questão resolvidos rapidamente.
