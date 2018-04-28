---
title: Tutorial - Configurar o HTTPS num domínio personalizado da CDN do Azure | Microsoft Docs
description: Neste tutorial, vai aprender a ativar e desativar o HTTPS no seu domínio personalizado do ponto final da CDN do Azure.
services: cdn
documentationcenter: ''
author: dksimpson
manager: akucer
editor: ''
ms.assetid: 10337468-7015-4598-9586-0b66591d939b
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/12/2018
ms.author: rli
ms.custom: mvc
ms.openlocfilehash: a8f2da5a68552c35a55a7bbb764afc7b36af6962
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/18/2018
---
# <a name="tutorial-configure-https-on-an-azure-cdn-custom-domain"></a>Tutorial: Configurar o HTTPS num domínio personalizado da CDN do Azure

[!INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

Este tutorial mostra como ativar o protocolo HTTP num domínio personalizado que está associado a um ponto final da Rede de Entrega de Conteúdos (CDN) do Azure. A utilização do protocolo HTTPS no seu domínio personalizado (por exemplo, https:\//www.contoso.com) garante que os seus dados confidenciais são entregues em segurança através de encriptação SSL quando são enviados pela Internet. O HTTPS proporciona confiança e autenticação e protege as suas aplicações Web de ataques. O fluxo de trabalho para ativar o HTTPS é simplificado através da ativação de um clique e da gestão de certificados completa, ambas sem custos adicionais.

A CDN do Azure suporta o HTTPS num nome de anfitrião do ponto final da CDN, por predefinição. Por exemplo, se criar um ponto final da CDN (por exemplo, https:\//contoso.azureedge.net), o HTTPS é ativado automaticamente.  

Alguns dos principais atributos da funcionalidade HTTPS são:

- Sem custos adicionais: não há custos para a aquisição ou renovação de certificados nem para o tráfego HTTPS. Só paga pelos GB de saída da CDN.

- Ativação simples: está disponível no [portal do Azure](https://portal.azure.com) o aprovisionamento de um clique. Também pode utilizar a API REST ou outras ferramentas de programador para ativar a funcionalidade.

- Gestão de certificados completa: todas as atividades de aprovisionamento e gestão de certificados são feitas por si. Os certificados são aprovisionados e renovados automaticamente antes de expirarem, o que elimina os riscos de interrupção do serviço devido à expiração dos mesmos.

Neste tutorial, ficará a saber como:
> [!div class="checklist"]
> - Ativar o protocolo HTTPS no seu domínio personalizado
> - Validar o domínio
> - Desativar o protocolo HTTPS no seu domínio personalizado

## <a name="prerequisites"></a>Pré-requisitos

Antes de concluir os passos neste tutorial, tem primeiro de criar um perfil da CDN e, pelo menos, um ponto final da CDN. Para obter mais informações, veja [Início Rápido: Criar um perfil e um ponto final da CDN do Azure](cdn-create-new-endpoint.md).

Além disso, tem de associar um domínio personalizado da CDN do Azure no ponto final da CDN. Para obter mais informações, veja [Tutorial: Adicionar um domínio personalizado ao ponto final da CDN do Azure](cdn-map-content-to-custom-domain.md)

## <a name="enable-the-https-feature"></a>Ativar a funcionalidade HTTPS

Para ativar o HTTPS num domínio personalizado, siga estes passos:

1. No [portal do Azure](https://portal.azure.com), navegue para o seu perfil da **CDN do Azure Standard da Verizon** ou da **CDN do Azure Premium da Verizon**.

2. Na lista de pontos finais da CDN, selecione o ponto final que contém o seu domínio personalizado.

    ![Lista de pontos finais](./media/cdn-custom-ssl/cdn-select-custom-domain-endpoint.png)

    É apresentada a página **Ponto final**.

3. Na lista de domínios personalizados, selecione o domínio personalizado no qual pretende ativar o HTTPS.

    ![Lista de domínios personalizados](./media/cdn-custom-ssl/cdn-custom-domain.png)

    É apresentada a página **Domínio personalizado**.

4. Selecione **Ativar** para ativar o HTTPS e selecione **Aplicar**.

    ![Estado do HTTPS do domínio personalizado](./media/cdn-custom-ssl/cdn-enable-custom-ssl.png)


## <a name="validate-the-domain"></a>Validar o domínio

Se já tiver um domínio personalizado em utilização e que esteja mapeado para o ponto final personalizado com um registo CNAME, avance para  
[O domínio personalizado está mapeado para o ponto final da CDN](#custom-domain-is-mapped-to-your-cdn-endpoint-by-a-cname-record). Caso contrário, se a entrada do registo CNAME para o ponto final já não existir ou contiver o subdomínio cdnverify, avance para [O domínio personalizado não está mapeado para o ponto final da CDN](#custom-domain-is-not-mapped-to-your-cdn-endpoint).

### <a name="custom-domain-is-mapped-to-your-cdn-endpoint-by-a-cname-record"></a>O domínio personalizado está mapeado para o ponto final da CDN por um registo CNAME

Quando adicionou um domínio personalizado ao ponto final, criou um registo CNAME na tabela de DNS da sua entidade de registo de domínios para mapeá-lo para o nome de anfitrião do ponto final da CDN. Se esse registo CNAME ainda existir e não contiver o subdomínio cdnverify, a autoridade de certificação (AC) DigiCert utiliza-o para validar a propriedade do seu domínio personalizado. 

O registo CNAME deve estar no seguinte formato, em que *Nome* é o nome do seu domínio personalizado e *Valor* é o nome de anfitrião do ponto final da CDN:

| Nome            | Tipo  | Valor                 |
|-----------------|-------|-----------------------|
| www.contoso.com | CNAME | contoso.azureedge.net |

Para obter mais informações sobre os registos CNAME, veja [Criar o registo DNS CNAME](https://docs.microsoft.com/en-us/azure/cdn/cdn-map-content-to-custom-domain#create-the-cname-dns-records).

Se o registo CNAME estiver no formato correto, DigiCert verifica o seu nome de domínio personalizado automaticamente e adiciona-o ao certificado Nomes Alternativos do Requerente (SAN). DigitCert não lhe enviará um e-mail de verificação e não terá de aprovar o seu próprio pedido. O certificado é válido durante um ano e será renovado automaticamente antes de expirar. Avance para [Aguardar pela propagação](#wait-for-propagation). 

Normalmente, a validação automática demora alguns minutos. Se não vir o seu domínio validado ao fim de uma hora, abra um pedido de suporte.

>[!NOTE]
>Se tiver um registo Autorização de Autoridade de Certificação (CAA) com o seu fornecedor de DNS, tem de incluir DigiCert como AC válida. O registo CAA permite aos proprietários de domínios especificar junto dos respetivos fornecedores de DNS que ACs têm autorização para emitir certificados para os seus domínios. Se uma AC receber um pedido de certificado para um domínio que tenha um registo CAA e essa AC não estiver listada como emissora autorizada, estará proibida de emitir o certificado para esse domínio ou subdomínio. Para obter informações sobre como gerir registos CAA, veja [Manage CAA records](https://support.dnsimple.com/articles/manage-caa-record/) (Gerir registos CAA). Para obter uma ferramenta de registo CAA, veja [CAA Record Helper](https://sslmate.com/caa/) (Ajuda para Registos CAA).

### <a name="custom-domain-is-not-mapped-to-your-cdn-endpoint"></a>O domínio personalizado não está mapeado para o ponto final da CDN

Se a entrada do registo CNAME para o seu ponto final já não existir ou contiver o subdomínio cdnverify, siga o resto das instruções neste passo.

Depois de ativar o HTTPS no domínio personalizado, a autoridade de certificação (AC) DigiCert valida a propriedade do seu domínio ao contactar a entidade de registo do mesmo, de acordo com as respetivas informações de registo em [WHOIS](http://whois.domaintools.com/). O contacto é feito por endereço de e-mail (por predefinição) ou pelo número de telefone indicado no registo em WHOIS. Antes de o HTTPS estar ativo no domínio personalizado, tem de concluir a validação do domínio. Tem seis dias úteis para aprovar o domínio. Os pedidos que não forem aprovados ao fim de seis dias úteis são cancelados automaticamente. 

![Registo WHOIS](./media/cdn-custom-ssl/whois-record.png)

DigiCert também envia um e-mail de verificação para endereços de e-mail adicionais. Se as informações do registo em WHOIS forem privadas, confirme que pode aprovar diretamente a partir de um dos endereços seguintes:

admin@&lt;o-seu-nome-de-domínio.com&gt;  
administrador@&lt;o-seu-nome-de-domínio.com&gt;  
webmaster@&lt;o-seu-nome-de-domínio.com&gt;  
hostmaster@&lt;o-seu-nome-de-domínio.com&gt;  
postmaster@&lt;o-seu-nome-de-domínio.com&gt;  

Deverá receber um e-mail passados alguns minutos, semelhante ao seguinte exemplo, que lhe pede para aprovar o pedido. Se estiver a utilizar um filtro de spam, adicione admin@digicert.com à lista de permissões. Se não receber um e-mail passadas 24 horas, contacte o suporte da Microsoft.
    
![E-mail de validação do domínio](./media/cdn-custom-ssl/domain-validation-email.png)

Quando clica na ligação de aprovação, é direcionado para o seguinte formulário de aprovação online: 
    
![Formulário de validação do domínio](./media/cdn-custom-ssl/domain-validation-form.png)

Siga as instruções do formulário; tem duas opções de verificação:

- Pode aprovar todas as encomendas futuras feitas através da mesma conta para o mesmo domínio de raiz; por exemplo, contoso.com. Esta abordagem é recomendada se planear adicionar mais domínios personalizados para o mesmo domínio de raiz.

- Pode aprovar apenas o nome de anfitrião específico utilizado neste pedido. É necessária aprovação adicional para pedidos subsequentes.

Após a aprovação, a DigiCert adiciona o seu nome de domínio personalizado ao certificado SAN. O certificado é válido durante um ano e será renovado automaticamente antes de expirar.

## <a name="wait-for-propagation"></a>Aguardar pela propagação

Após a validação do nome de domínio, a ativação da funcionalidade HTTPS no domínio personalizado pode demorar entre 6 a 8 horas. Quando o processo estiver concluído, o estado do HTTPS personalizado no portal do Azure é definido como **Ativado** e os quatro passos da operação na caixa de diálogo de domínio personalizado são marcados como concluídos. O seu domínio personalizado está agora pronto para utilizar o HTTPS.

![Caixa de diálogo Ativar HTTPS](./media/cdn-custom-ssl/cdn-enable-custom-ssl-complete.png)

### <a name="operation-progress"></a>Progresso da operação

A tabela seguinte mostra o progresso da operação que ocorre quando ativa o HTTPS. Depois de ativar o HTTPS, aparecem quatro passos da operação na caixa de diálogo do domínio personalizado. À medida que cada passo é ativado, aparecem detalhes de subpassos adicionais abaixo dos passos. Nem todos estes subpassos vão ocorrer. Depois da conclusão bem-sucedida de um passo, aparece uma marca de verificação verde junto ao mesmo. 

| Passo da operação | Detalhes do subpasso da operação | 
| --- | --- |
| 1 Submeter o pedido | Submeter o pedido |
| | O seu pedido HTTPS está a ser submetido. |
| | O seu pedido HTTPS foi submetido com êxito. |
| 2 Validação do domínio | O domínio é validado automaticamente se estiver mapeado para o ponto final da CDN através de CNAME. Caso contrário, será enviado um pedido de verificação para o e-mail indicado nas informações de registo do seu domínio (registo WHOIS). Verifique o domínio assim que possível. |
| | A propriedade do domínio foi validada com êxito. |
| | O pedido de validação da propriedade do domínio expirou (é provável que o cliente não tenha respondido em seis dias). O HTTPS não será ativado no seu domínio. * |
| | O cliente rejeitou o pedido de validação da propriedade do domínio. O HTTPS não será ativado no seu domínio. * |
| 3 Aprovisionamento do certificado | A autoridade de certificação está a emitir o certificado necessário para ativar o HTTPS no seu domínio. |
| | O certificado foi emitido e está a ser implementado na rede CDN. Este processo poderá demorar até seis horas. |
| | O certificado foi implementado com êxito na rede CDN. |
| 4 Concluído | O HTTPS foi ativado com êxito no seu domínio. |

\* Esta mensagem não aparece, a menos que tenha ocorrido um erro. 

Se ocorrer um erro antes de o pedido ser submetido, será apresentada a seguinte mensagem de erro:

<code>
We encountered an unexpected error while processing your HTTPS request. Please try again and contact support if the issue persists.
</code>

## <a name="clean-up-resources---disable-https"></a>Limpar recursos - desativar HTTPS

Nos passos anteriores, ativou o protocolo HTTPS no seu domínio personalizado. Se já não pretender utilizar o seu domínio personalizado com o HTTPS, pode seguir os passos abaixo para desativá-lo:

### <a name="disable-the-https-feature"></a>Desativar a funcionalidade HTTPS 

1. No [portal do Azure](https://portal.azure.com), navegue para o seu perfil da **CDN do Azure Standard da Verizon** ou da **CDN do Azure Premium da Verizon**.

2. Na lista de pontos finais, clique no ponto final que contém o seu domínio personalizado.

3. Clique no domínio personalizado no qual pretende desativar o HTTPS.

    ![Lista de domínios personalizados](./media/cdn-custom-ssl/cdn-custom-domain-HTTPS-enabled.png)

4. Clique em **Desativar** para desativar o HTTPS e clique em **Aplicar**.

    ![Caixa de diálogo de HTTPS personalizado](./media/cdn-custom-ssl/cdn-disable-custom-ssl.png)

### <a name="wait-for-propagation"></a>Aguardar pela propagação

A desativação da funcionalidade HTTPS no domínio personalizado pode demorar entre 6 a 8 horas a ter efeito. Quando o processo estiver concluído, o estado do HTTPS personalizado no portal do Azure é definido como **Desativado** e os três passos da operação na caixa de diálogo de domínio personalizado são marcados como concluídos. O domínio personalizado já não pode utilizar o HTTPS.

![Caixa de diálogo Desativar HTTPS](./media/cdn-custom-ssl/cdn-disable-custom-ssl-complete.png)

#### <a name="operation-progress"></a>Progresso da operação

A tabela seguinte mostra o progresso da operação que ocorre quando desativa o HTTPS. Depois de desativar o HTTPS, aparecem três passos da operação na caixa de diálogo Domínio personalizado. À medida que cada passo é ativado, aparecem detalhes adicionais abaixo dos mesmos. Depois da conclusão bem-sucedida de um passo, aparece uma marca de verificação verde junto ao mesmo. 

| Progresso da operação | Detalhes da operação | 
| --- | --- |
| 1 Submeter o pedido | Submeter o pedido |
| 2 Desaprovisionamento do certificado | Eliminar o certificado |
| 3 Concluído | Certificado eliminado |

## <a name="frequently-asked-questions"></a>Perguntas mais frequentes

1. *Quem é o fornecedor do certificado e que tipo de certificado é utilizado?*

    A Microsoft utiliza um certificado Nomes Alternativo do Requerente (SAN), que é fornecido pela DigiCert. Os certificados SAN podem proteger vários nomes de domínio completamente qualificados com um único certificado.

2. *Posso utilizar o meu certificado dedicado?*
    
    Atualmente não, mas faz parte dos nossos planos.

3. *E se não receber o e-mail de verificação do domínio da DigiCert?*

    Se tiver uma entrada CNAME para o seu domínio personalizado que aponte diretamente para o nome de anfitrião do seu ponto final (e não estiver a utilizar o nome de subdomínio cdnverify), não receberá um e-mail de verificação do domínio. A validação ocorre automaticamente. Caso contrário, se não tiver uma entrada CNAME e não tiver recebido um e-mail passadas 24 horas, contacte o suporte da Microsoft.

4. *A utilização de um certificado SAN é mais insegura do que um certificado dedicado?*
    
    Os certificados SAN seguem as mesmas normas de encriptação e segurança dos certificados dedicados. Todos os certificados SSL emitidos utilizam SHA-256 para proporcionar mais segurança ao servidor.

5. *Posso utilizar um HTTPS no domínio personalizado com a CDN do Azure da Akamai?*

    Atualmente, esta funcionalidade só está disponível com a CDN do Azure da Verizon. A Microsoft está a trabalhar no sentido de oferecer suporte para esta funcionalidade na CDN do Azure da Akamai nos próximos meses.

6. *Preciso de um registo Autorização de Autoridade de Certificação junto do meu fornecedor de DNS?*

    Não, atualmente os registos Autorização de Autoridade de Certificação não são necessários. No entanto, se tiver um, o mesmo tem de incluir a DigiCert como AC válida.


## <a name="next-steps"></a>Passos seguintes

O que aprendeu:

> [!div class="checklist"]
> - A ativar o protocolo HTTPS no seu domínio personalizado
> - A validar o domínio
> - A desativar o protocolo HTTPS no seu domínio personalizado

Avance para o próximo tutorial para saber como configurar a colocação em cache no seu ponto final da CDN.

> [!div class="nextstepaction"]
> [Controlar o comportamento da colocação em cache da CDN do Azure com as regras de colocação em cache](cdn-caching-rules.md)

