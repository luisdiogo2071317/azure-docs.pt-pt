---
title: Configure o HTTPS um domínio personalizado de CDN do Azure | Microsoft Docs
description: Saiba como ativar ou desativar HTTPS no seu domínio personalizado de ponto final de CDN do Azure.
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
ms.topic: article
ms.date: 03/22/2018
ms.author: rli; v-deasim
ms.openlocfilehash: ca3dad18973197f63e69e6568b8ea5988b279e01
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="configure-https-on-an-azure-cdn-custom-domain"></a>Configure o HTTPS um domínio personalizado de CDN do Azure

[!INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

Azure entrega rede conteúdos (CDN) suporta o protocolo HTTPS para um domínio personalizado no ponto final da CDN. Utilizando o protocolo HTTPS no seu domínio personalizado, certifique-se de que os dados confidenciais é entregue em segurança através de encriptação SSL quando é enviado através da internet. Fornece HTTPS confiança de autenticação e protege as suas aplicações web contra ataques. Além disso, a fornecer a segura do conteúdo utilizando o seu próprio nome de domínio (por exemplo, https:\//www.contoso.com). O fluxo de trabalho para permitir HTTPS é simplificado através de ativação de um clique e gestão de certificados completo, todos os sem custos adicionais.

CDN do Azure também suporta HTTPS num nome de anfitrião de ponto final CDN, por predefinição. Por exemplo, se criar um ponto final de CDN (por exemplo, https:\//contoso.azureedge.net), HTTPS é ativado automaticamente.  

Alguns dos principais atributos da funcionalidade HTTPS são:

- Sem custos adicionais: existem sem custos de aquisição do certificado ou a renovação e sem custos adicionais para tráfego HTTPS. Só paga pelos GB de saída da CDN.

- Ativação Simple: aprovisionamento de um clique está disponível a partir de [portal do Azure](https://portal.azure.com). Também pode utilizar a REST API ou outras ferramentas de programador para ativar a funcionalidade.

- Conclua a gestão de certificados: todos os certificados de aprovisionamento e gestão é processada por si. Certificados são automaticamente aprovisionados e renovados antes da expiração, o que elimina os riscos de interrupção do serviço devido a um certificado prestes a expirar.

>[!NOTE] 
>Antes de ativar o suporte de HTTPS, tem de ter já estabeleceu um [domínio personalizado de CDN do Azure](./cdn-map-content-to-custom-domain.md).

## <a name="enabling-https"></a>Ativar HTTPS

Para ativar HTTPS, um domínio personalizado, siga estes passos:

### <a name="step-1-enable-the-feature"></a>Passo 1: Ativar a funcionalidade 

1. No [portal do Azure](https://portal.azure.com), navegue até à sua **CDN do Azure Standard da Verizon** ou **CDN do Azure Premium da Verizon** perfil da CDN.

2. Na lista de pontos finais, clique no ponto final que contém o domínio personalizado.

3. Clique no domínio personalizado para o qual pretende ativar HTTPS.

    ![Lista de domínios personalizados](./media/cdn-custom-ssl/cdn-custom-domain.png)

4. Clique em **no** para ativar HTTPS, em seguida, clique em **aplicar**.

    ![Estado HTTPS de domínio personalizado](./media/cdn-custom-ssl/cdn-enable-custom-ssl.png)


### <a name="step-2-validate-domain"></a>Passo 2: Validar o domínio

>[!NOTE]
>Se tiver um registo de autorização de autoridade de certificado (CAA) com o seu fornecedor DNS, tem de incluir DigiCert como uma AC válida. Um registo de CAA permite que os proprietários de domínio especificar os seus fornecedores de serviços de DNS que as ACs estão autorizados a emitir certificados para os domínios. Se uma AC recebe uma ordem de um certificado para um domínio que tenha um registo de CAA e essa AC não está indicada como um emissor autorizado, é proibido de emitir o certificado para esse domínio ou um subdomínio. Para obter informações sobre como gerir registos de CAA, consulte [registos gerir CAA](https://support.dnsimple.com/articles/manage-caa-record/). Para obter uma ferramenta de registo CAA, consulte [CAA registo auxiliar](https://sslmate.com/caa/).

#### <a name="custom-domain-is-mapped-to-cdn-endpoint"></a>Domínio personalizado é mapeado para o ponto final de CDN

Quando tiver adicionado a um domínio personalizado para o ponto final, criou um registo CNAME na tabela de DNS do seu registo de domínios para mapear para o nome de anfitrião de ponto final CDN. Se esse registo CNAME ainda existe e não contém o subdomínio cdnverify, a autoridade de certificação (AC) de DigiCert utiliza-o para validar a propriedade do seu domínio personalizado. 

O registo CNAME deve estar no seguinte formato, onde *nome* é o nome de domínio personalizado e *valor* é o nome de anfitrião de ponto final CDN:

| Nome            | Tipo  | Valor                 |
|-----------------|-------|-----------------------|
| www.contoso.com | CNAME | contoso.azureedge.net |

Para obter mais informações sobre registos CNAME, consulte [criar o registo CNAME DNS](https://docs.microsoft.com/en-us/azure/cdn/cdn-map-content-to-custom-domain#step-2-create-the-cname-dns-records).

Se o registo CNAME no formato correto, DigiCert verifica o seu nome de domínio personalizado automaticamente e adiciona-o para o certificado de nomes de alternativo do requerente (SAN). DigitCert não enviar uma mensagem de verificação e não terá de aprovar o pedido. O certificado é válido durante um ano e será automaticamente-renovada antes de expirar. Avance para [passo 3: aguardar propagação](#step-3-wait-for-propagation). 

A validação automática normalmente demora alguns minutos. Se não vir o seu domínio validado dentro de uma hora, abra um pedido de suporte.

#### <a name="cname-record-is-not-mapped-to-cdn-endpoint"></a>Registo CNAME não está mapeado para o ponto final de CDN

Se a entrada de registo CNAME para o ponto final já não existe ou contém o subdomínio cdnverify, siga o resto das instruções neste passo.

Depois de ativar HTTPS no seu domínio personalizado, a autoridade de certificação (AC) de DigiCert valida a propriedade do seu domínio ao contactar o respetivo registrant, de acordo com o domínio [WHOIS](http://whois.domaintools.com/) informações registrant. Contacte é efetuada através do endereço de e-mail (por predefinição) ou o número de telefone listados no registo WHOIS. Validação do domínio deve ser concluída antes de HTTPS estará ativo no seu domínio personalizado. Tem de seis dias úteis para aprovar o domínio. Pedidos que não estão aprovados dentro de seis dias úteis automaticamente foram cancelados. 

![Registo WHOIS](./media/cdn-custom-ssl/whois-record.png)

DigiCert também envia uma mensagem de verificação para endereços de e-mail adicionais. Se as informações de registrant WHOIS são privadas, certifique-se de que podem aprovar diretamente a partir de um dos seguintes endereços:

admin@&lt;your-domain-name.com&gt;  
administrator@&lt;your-domain-name.com&gt;  
webmaster@&lt;your-domain-name.com&gt;  
hostmaster@&lt;your-domain-name.com&gt;  
postmaster@&lt;your-domain-name.com&gt;  

Deverá receber uma mensagem de e-mail dentro de alguns minutos, semelhantes ao seguinte exemplo, pedir-lhe aprovar o pedido. Se estiver a utilizar um filtro de spam, adicione admin@digicert.com a lista branca. Se não receber uma mensagem de e-mail dentro de 24 horas, contacte o suporte da Microsoft.
    
![Correio eletrónico de validação do domínio](./media/cdn-custom-ssl/domain-validation-email.png)

Ao clicar na ligação de aprovação, o utilizador é direcionado para o seguinte formato de aprovação online: 
    
![Formulário de validação do domínio](./media/cdn-custom-ssl/domain-validation-form.png)

Siga as instruções no formulário; tem duas opções de verificação:

- Pode aprovar todas as ordens futuras colocadas através da mesma conta para o mesmo domínio de raiz; Por exemplo, contoso.com. Esta abordagem é recomendada se planear adicionar domínios personalizados adicionais para o mesmo domínio de raiz.

- Pode aprovar apenas o nome de anfitrião específico utilizado neste pedido. Aprovação adicional é necessária para os pedidos subsequentes.

Após a aprovação, DigiCert adiciona o seu nome de domínio personalizado para o certificado de SAN. O certificado é válido durante um ano e será automaticamente-renovada antes de ter expirado.

### <a name="step-3-wait-for-propagation"></a>Passo 3: Aguardar propagação

Depois do nome de domínio é validado, pode demorar até 6 8 horas para a funcionalidade HTTPS de domínio personalizado para ser ativado. Quando o processo estiver concluído, o estado HTTPS personalizado no portal do Azure está definido como **ativado** e os passos de operação de quatro na caixa de diálogo de domínio personalizado são marcados como concluídos. O domínio personalizado está agora pronto para utilizar HTTPS.

![Ativar a caixa de diálogo HTTPS](./media/cdn-custom-ssl/cdn-enable-custom-ssl-complete.png)

### <a name="operation-progress"></a>Progresso da operação

A tabela seguinte mostra o progresso da operação que ocorre quando ativar HTTPS. Depois de ativar HTTPS, quatro passos de operação aparecem na caixa de diálogo de domínio personalizado. À medida que cada passo for Active Directory, detalhes de substep adicionais são apresentados sob o passo como que avança. Nem todas estas substeps ocorrerá. Depois de um passo concluir com êxito, é apresentada uma marca de verificação verde junto ao mesmo. 

| Passo de operação | Detalhes de substep de operação | 
| --- | --- |
| 1 submeter pedido | Submeter pedido |
| | O seu pedido HTTPS está a ser submetido. |
| | O seu pedido HTTPS foi submetido com êxito. |
| Validação de domínio 2 | Domínio é validado automaticamente se for CNAME mapeado para o ponto final da CDN. Caso contrário, será enviado um pedido de verificação para o e-mail listado no registo do seu domínio (WHOIS registrant). Verifique o domínio logo que possível. |
| | A propriedade do domínio foi validada com êxito. |
| | Pedido de validação de propriedade de domínio expirou (cliente provavelmente não responder dentro de dias 6). HTTPS não será ativado no seu domínio. * |
| | Pedido de validação de propriedade de domínio foi rejeitado pelo cliente. HTTPS não será ativado no seu domínio. * |
| Aprovisionamento de certificados 3 | A autoridade de certificação está a emitir o certificado necessário para ativar o HTTPS no seu domínio. |
| | O certificado foi emitido e está atualmente a ser implementado à rede da CDN. Isto pode demorar até 6 horas. |
| | O certificado foi implementado com êxito na rede CDN. |
| 4 concluída | O HTTPS foi ativado com êxito no seu domínio. |

\* Esta mensagem não aparecer, a menos que ocorreu um erro. 


Se ocorrer um erro antes do pedido ser submetido, será apresentada a seguinte mensagem de erro:

<code>
We encountered an unexpected error while processing your HTTPS request. Please try again and contact support if the issue persists.
</code>

## <a name="disabling-https"></a>Desativar HTTPS

Depois de ter ativado HTTPS um domínio personalizado, pode desativá-lo mais tarde. Para desativar o HTTPS, siga estes passos:

### <a name="step-1-disable-the-feature"></a>Passo 1: Desativar a funcionalidade 

1. No [portal do Azure](https://portal.azure.com), navegue até à sua **CDN do Azure Standard da Verizon** ou **CDN do Azure Premium da Verizon** perfil da CDN.

2. Na lista de pontos finais, clique no ponto final que contém o domínio personalizado.

3. Clique no domínio personalizado para o qual pretende desativar o HTTPS.

    ![Lista de domínios personalizados](./media/cdn-custom-ssl/cdn-custom-domain-HTTPS-enabled.png)

4. Clique em **desativar** para desativar o HTTPS, em seguida, clique em **aplicar**.

    ![Caixa de diálogo HTTPS personalizada](./media/cdn-custom-ssl/cdn-disable-custom-ssl.png)

### <a name="step-2-wait-for-propagation"></a>Passo 2: Aguardar propagação

Depois da funcionalidade HTTPS de domínio personalizado está desativada, pode demorar até 6 8 horas para este efeito. Quando o processo estiver concluído, o estado HTTPS personalizado no portal do Azure está definido como **desativado** e os passos de operação de três na caixa de diálogo de domínio personalizado são marcados como concluídos. O domínio personalizado já não pode utilizar o HTTPS.

![Desative a caixa de diálogo HTTPS](./media/cdn-custom-ssl/cdn-disable-custom-ssl-complete.png)

### <a name="operation-progress"></a>Progresso da operação

A tabela seguinte mostra o progresso da operação que ocorre quando desativar o HTTPS. Depois de desativar HTTPS, três passos de operação aparecem na caixa de diálogo de domínio personalizado. À medida que cada passo for Active Directory, são apresentados detalhes adicionais no passo. Depois de um passo concluir com êxito, é apresentada uma marca de verificação verde junto ao mesmo. 

| Progresso da operação | Detalhes da operação | 
| --- | --- |
| 1 submeter pedido | A enviar o seu pedido |
| 2 desaprovisionamento de certificado | A eliminar certificado |
| 3 concluída | Certificado eliminado |

## <a name="frequently-asked-questions"></a>Perguntas mais frequentes

1. *Quem é o fornecedor de certificado e o tipo de certificado que é utilizado?*

    A Microsoft utiliza um certificado de nomes de alternativo do requerente (SAN) fornecido pelo DigiCert. Um certificado SAN pode proteger vários nomes de domínio completamente qualificado com um certificado.

2. *Pode utilizar a minha certificados dedicado?*
    
    Atualmente não, mas é no plano.

3. *E se não receber correio eletrónico de verificação de domínio do DigiCert?*

    Se tiver uma entrada CNAME para o seu domínio personalizado que aponta diretamente para o seu nome de anfitrião de ponto final (e não estiver a utilizar o nome de subdomínio cdnverify), não irão receber um e-mail de verificação de domínio. Validação ocorre automaticamente. Caso contrário, se não tiver uma entrada CNAME e que ainda não recebeu uma mensagem de e-mail dentro de 24 horas, contacte o suporte da Microsoft.

4. *Está a utilizar um certificado SAN menos seguro do que um certificado dedicado?*
    
    Um certificado SAN segue as normas de segurança e de encriptação mesmas como um certificado dedicado. Todos os certificados SSL emitidos utilizarem SHA-256 para segurança do servidor avançada.

5. *Pode utilizar um domínio personalizado HTTPS com o Azure CDN da Akamai?*

    Atualmente, esta funcionalidade só está disponível com a CDN do Azure da Verizon. Microsoft está a funcionar no suporte esta funcionalidade com o Azure CDN da Akamai nos próximos meses.

6. *É necessário um registo de autorização de autoridade do certificado com a minha fornecedor DNS?*

    Não, um registo de autorização de autoridade do certificado não é atualmente necessário. No entanto, se tiver um, tem de incluir DigiCert como uma AC válida.


## <a name="next-steps"></a>Passos Seguintes

- Saiba como configurar um [domínio personalizado no ponto final de CDN do Azure](./cdn-map-content-to-custom-domain.md)


