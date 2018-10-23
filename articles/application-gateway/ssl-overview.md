---
title: Ativar SSL de ponta a ponta no Gateway de aplicação do Azure
description: Este artigo é uma visão geral do Gateway de aplicação que suporte SSL ponto a ponto.
services: application-gateway
author: amsriva
ms.service: application-gateway
ms.topic: article
ms.date: 10/23/2018
ms.author: amsriva
ms.openlocfilehash: 626db07a81c6482a689329b8cddc9f40b464bb7e
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/23/2018
ms.locfileid: "49649079"
---
# <a name="overview-of-end-to-end-ssl-with-application-gateway"></a>Descrição geral do SSL ponto a ponto com o Gateway de Aplicação

Gateway de aplicação suporta a terminação de SSL no gateway, após o qual o tráfego normalmente flui desencriptado para os servidores de back-end. Esta funcionalidade permite que os servidores Web estejam livres de sobrecarga de encriptação e desencriptação dispendiosa. No entanto, para alguns clientes, a comunicação sem encriptação para os servidores de back-end não é uma opção aceitável. Esta comunicação desencriptada pode dever-se aos requisitos de segurança, requisitos de conformidade ou a aplicação pode aceitar apenas uma ligação segura. Para essas aplicações, o gateway de aplicação suporta a encriptação SSL de ponta a ponta.

SSL de ponto a ponto permite-lhe transmitir dados confidenciais para o back-end encriptado e tirar partido dos benefícios das funcionalidades de balanceamento de carga de camada 7 que o gateway de aplicação fornece de forma segura. Algumas destas funcionalidades são a afinidade do cookie baseado na sessão, encaminhamento baseado em URL, suporte para encaminhamento baseado em sites ou capacidade para injetar cabeçalhos Encaminhados para X-*.

Quando configurado com o modo de comunicação SSL de ponta a ponta, o gateway de aplicação termina as sessões de SSL no gateway e desencripta o tráfego de utilizador. Em seguida, aplica as regras configuradas para selecionar uma instância de conjunto de back-end adequada para encaminhar o tráfego. O gateway de aplicação, em seguida, inicia uma nova ligação SSL ao servidor de back-end e encripta novamente os dados com o certificado de chave pública do servidor de back-end antes de transmitir o pedido para o back-end. SSL de ponto a ponto é ativada ao configurar a definição do protocolo no **BackendHTTPSetting** como HTTPS que, em seguida, é aplicado a um conjunto de back-end. Cada servidor de back-end no conjunto de back-end com SSL de ponta a ponta ativado deve ser configurado com um certificado, para permitir a comunicação segura.

![cenário de SSL ponto a ponto][1]

Neste exemplo, os pedidos que utilizam o TLS1.2 são encaminhados para servidores de back-end no Pool1 através do SSL ponto a ponto.

## <a name="end-to-end-ssl-and-whitelisting-of-certificates"></a>SSL ponto a ponto e lista de certificados permitidos

O gateway de aplicação comunica apenas com instâncias de back-end conhecidas que colocaram o respetivo certificado na lista de permissões com o gateway de aplicação. Para ativar a lista de certificados permitidos, tem de carregar a chave pública dos certificados de servidor de back-end para o gateway de aplicação (e não o certificado de raiz). Apenas são permitidas ligações a back-ends conhecidos e na lista de permissões. Os restantes back-ends resultam num erro de gateway. Os certificados autoassinados são apenas para fins de teste e não são recomendados para cargas de trabalho de produção. Esses certificados têm de estar na lista de permissões com o gateway de aplicação, conforme descrito nos passos anteriores, antes de poderem ser utilizados.

> [!NOTE]
> Configuração de certificado de autenticação não é necessária para serviços do Azure confiáveis, como aplicações Web do Azure.

## <a name="end-to-end-ssl-with-the-v2-sku"></a>Ponto a ponto SSL com o SKU de v2

Certificados de autenticação foram preteridos e substituídos por certificados de raiz fidedigna no SKU do Gateway de aplicação v2. Eles funcionam da mesma forma para certificados de autenticação com algumas diferenças importantes:

- Certificados assinados por bem conhecidas autoridades de AC cuja CN coincide com o nome de anfitrião nas definições de back-end de HTTP não requerem qualquer passo adicional para o SSL de ponta a ponta trabalhar. 

   Por exemplo, se os certificados de back-end são emitidos por uma AC conhecida e tem um CN de contoso.com, e o campo de anfitrião a definição de http de back-end também está definido como contoso.com, em seguida, não são necessários passos adicionais. Pode definir o http de back-end na definição de protocolo HTTPS, e tanto o estado de funcionamento sonda e dos dados caminho seria SSL ativado. Se estiver a utilizar aplicações Web do Azure ou outros serviços da web do Azure como o back-end, em seguida, estes são implicitamente confiáveis também e nenhuma outra etapa é necessária para o SSL ponto a ponto.
- Se o certificado é autoassinado ou assinado pelo intermediários desconhecidos, em seguida ativar o SSL de ponta a ponta no SKU de v2 um certificado de raiz fidedigna tem de ser definido. Gateway de aplicação irá apenas comunicar com o certificado de raiz de cujo certificado de servidor corresponde a uma lista de certificados de raiz fidedigna na definição de http de back-end associados ao agrupamento de back-ends.
- Além de correspondência do certificado de raiz, o Gateway de aplicação também valida se corresponder a configuração especificada na definição de http de back-end de Host que o nome comum (CN) apresentado pelo certificado SSL do servidor back-end. Ao tentar estabelecer uma conexão SSL para o back-end, o Gateway de aplicação define a extensão de indicação de nome de servidor (SNI) para o anfitrião especificado na definição de http de back-end.
- Se **escolha o nome de anfitrião do endereço de back-end** for escolhida em vez do campo de anfitrião na definição de http de back-end, em seguida, o cabeçalho SNI está sempre definido como o conjunto de back-end FQDN e o CN no servidor de back-end SSL certificado tem de corresponder ao respetivo FQDN. Membros do agrupamento de back-end com IPs não são suportados neste cenário.
- O certificado de raiz é um certificado de raiz com codificação de base64 dos certificados de servidor de back-end.

## <a name="next-steps"></a>Passos Seguintes

Após a aprendizagem sobre SSL de ponto a ponto, aceda a [Configurar um gateway de aplicação através da terminação de SSL com o portal do Azure](create-ssl-portal.md), para criar um gateway de aplicação com SSL de ponto a ponto.

<!--Image references-->

[1]: ./media/ssl-overview/scenario.png
