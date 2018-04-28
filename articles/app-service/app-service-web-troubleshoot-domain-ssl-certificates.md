---
title: Resolver problemas de domínio e problemas de certificado SSL no aplicações web do Azure | Microsoft Docs
description: Resolver problemas de domínio e problemas de certificado SSL no aplicações web do Azure
services: app-service\web
documentationcenter: ''
author: genlin
manager: cshepard
editor: ''
tags: top-support-issue
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/18/2018
ms.author: genli
ms.openlocfilehash: ba21475b771f1688c0a3f2f34c8d961fba5cd182
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="troubleshoot-domain-and-ssl-certificate-problems-in-azure-web-apps"></a>Resolver problemas de domínio e problemas de certificado SSL no aplicações web do Azure

Este artigo apresenta uma lista de problemas comuns que poderá encontrar ao configurar o domínio ou o certificado SSL para as suas aplicações web do Azure. Também descreve possíveis causas e soluções para esses problemas.

Se precisar de mais ajuda, a qualquer altura neste artigo, pode contactar as especialistas do Azure no [do MSDN Azure e os fóruns Stack Overflow](https://azure.microsoft.com/support/forums/). Em alternativa, pode também ficheiro um incidente de suporte do Azure. Vá para o [site de suporte do Azure](https://azure.microsoft.com/support/options/) e clique em **obter suporte**.

## <a name="certificate-problems"></a>Problemas de certificado

### <a name="unable-to-add-bind-ssl-certificate-to-a-web-app"></a>Não é possível adicionar o certificado SSL do enlace a uma aplicação Web 

### <a name="symptom"></a>Sintoma

Quando adiciona um enlace SSL, recebe a seguinte mensagem de erro:

**Falha ao adicionar o enlace SSL. Não é possível definir o certificado para o VIP existente porque outro VIP já utiliza essa certificados.**

### <a name="cause"></a>Causa

Este problema pode ocorrer se tiver vários enlaces SSL baseado em IP para o mesmo endereço IP por várias aplicações web. Por exemplo, a aplicação web A tem SSL baseado em IP com certificado antigo. Aplicação Web B com SSL baseado em IP com o novo certificado para o mesmo endereço IP. Quando atualizar o enlace de SSL de aplicação web com o novo certificado, irá falhar com este erro, uma vez que está a ser utilizado o mesmo endereço IP para outra aplicação. 

### <a name="solution"></a>Solução 

Para corrigir este problema, utilize um dos seguintes métodos:

- Elimine o enlace SSL baseado em IP na aplicação web que utiliza o certificado antigo. 
- Crie um novo enlace SSL baseado em IP que utiliza o novo certificado.

### <a name="unable-to-delete-a-certificate"></a>Não é possível eliminar um certificado 

### <a name="symptom"></a>Sintoma

Ao tentar eliminar um certificado, recebe a seguinte mensagem de erro:

**Não é possível eliminar o certificado, porque está a ser atualmente utilizado um enlace SSL. O enlace SSL tem de ser removido antes de poder eliminar o certificado.**

### <a name="cause"></a>Causa

Este problema pode ocorrer se o certificado é utilizado por outra aplicação web.

### <a name="solution"></a>Solução

Remova o enlace SSL para esse certificado das aplicações web. Em seguida, tente eliminar o certificado. Se ainda não é possível eliminar o certificado, limpar a cache do browser de Internet, volte a abrir o portal do Azure numa nova janela do browser. E, em seguida, tente eliminar o certificado.

### <a name="unable-to-purchase-an-app-service-certificate"></a>Não é possível adquirir um certificado de serviço de aplicações 

### <a name="symptom"></a>Sintoma
Não é possível adquirir um [certificado de serviço de aplicações](./web-sites-purchase-ssl-web-site.md) do portal do Azure.

### <a name="cause-and-solution"></a>Causa e soluções
Este problema pode ocorrer por qualquer um dos seguintes motivos:

- O plano de serviço de aplicações é "Livre" ou "Partilhado". Não suportamos SSL para estes escalões de preço. 

    **Solução**: atualizar o plano de serviço aplicacional da aplicação web a "Standard".

- A subscrição não tem um cartão de crédito válido.

    **Solução**: adicionar um cartão de crédito válido à sua subscrição. 

- A oferta da subscrição não suporta a compra de um certificado de serviço de aplicações, tais como Microsoft Student.  

    **Solução**: atualizar a subscrição. 

- A subscrição atingiu o limite máximo de compras que são permitidos em subscrições.

    **Solução**: certificados de serviço de aplicações têm um limite de 10 compras de certificado para Pay-como-Go e EA tipos de subscrições. Para outros tipos de subscrição, o limite é 3. Para aumentar o limite, contacte [suporte do Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
- O certificado de serviço de aplicações foi marcado como fraude. Recebe a seguinte mensagem de erro: "o certificado foi sinalizado de possível fraude. O pedido está atualmente a ser revisão. Se o certificado não ficar utilizável dentro de 24 horas".

    **Solução**: se o certificado está marcado como fraude e não foi resolvido após 24 horas, em seguida, siga estes passos:

    1. Inicie sessão no [Portal do Azure](https://portal.azure.com).
    2. Aceda a **certificados de serviço de aplicações**, selecione o certificado.
    3. Selecione **configuração do certificado** > **passo 2: verificar** > **verificação domínio**. Esta ação envia um aviso de correio eletrónico para o fornecedor de certificados do Azure para resolver o problema.

## <a name="domain-problems"></a>Problemas de domínio

### <a name="purchased-ssl-certificate-for-wrong-domain"></a>Comprou o certificado SSL para o domínio incorreto

### <a name="symptom"></a>Sintoma

Adquirido um certificado de serviço de aplicações para o domínio de problema e não é possível atualizar o certificado a utilizar o domínio correto.

### <a name="solution"></a>Solução

- Eliminar esse certificado e, em seguida, comprar um novo certificado.
- Se o certificado atual que utiliza o domínio de problema está no estado "Emitido", em seguida, será também faturado para esse certificado. Os certificados de serviço de aplicações não são reembolsável, mas pode contactar [suporte do Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para ver se existem outras opções. 

### <a name="app-service-certificate-was-renewed-but-still-shows-the-old-certificate"></a>Certificado de serviço de aplicações foi renovado, mas o certificado antigo continua a mostrar 

### <a name="symptom"></a>Sintoma

Foi renovar o certificado de serviço de aplicações, mas a aplicação web que utiliza o certificado de serviço de aplicações ainda está a utilizar o certificado antigo. Além disso, foi recebido um aviso se o protocolo HTTPS é necessário.

### <a name="cause"></a>Causa 
O serviço de aplicações Web é executada uma tarefa em segundo plano a cada oito horas e sincroniza-se o recurso de certificado, se existirem quaisquer alterações. Por conseguinte, quando rodar ou atualizar um certificado, por vezes, a aplicação está ainda a obter o certificado antigo e não o certificado recentemente atualizado. Isto acontece porque a tarefa para sincronizar o recurso de certificado ainda não foi executado. 
 
### <a name="solution"></a>Solução

Pode forçar uma sincronização do certificado:

1. Inicie sessão no [Portal do Azure](https://portal.azure.com). Selecione o **certificados de serviço de aplicações**e, em seguida, selecione o certificado.
2. Clique em **recodificar e sincronizar**e, em seguida, clique em **sincronização**. Esta ação demora algum tempo a concluir. 
3. Quando a sincronização estiver concluída, verá a seguinte notificação: "Atualizada com êxito todos os recursos com o certificado mais recente".

### <a name="domain-verification-is-not-working"></a>Verificação de domínio não está a funcionar 

### <a name="symptom"></a>Sintoma 
O certificado de serviço de aplicações requer verificação de domínio antes do certificado está pronto a utilizar. Ao clicar em **verifique**, o processo de falha.

### <a name="solution"></a>Solução
Verifique manualmente o seu domínio ao adicionar um registo TXT:
 
1.  Vá para o fornecedor de serviço de nomes de domínio (DNS) que aloja o seu nome de domínio.
2.  Adicione um registo TXT no seu domínio que utiliza o valor do token de domínio que é apresentado no portal do Azure. 

Aguarde alguns minutos para que a propagação de DNS executar e, em seguida, clique **atualizar** botão para acionar a verificação. 

Método alternativo para verificar manualmente é o "method página Web HTML" que pode ser utilizada para permitir a autoridade de certificação confirmar a propriedade de domínio do domínio que o certificado é emitido para.

1.  Crie um ficheiro HTML com o nome *.HTML {Token de verificação do domínio}. 
2.  O conteúdo deste ficheiro deve ser o valor do Token de verificação do domínio.
3.  Carregar este ficheiro na raiz do servidor web que está a alojar o seu domínio
4.  Clique em **atualizar** para verificar o estado do certificado. Poderá demorar alguns minutos para fins de verificação concluir.

Por exemplo, se são comprar um certificado padrão para azure.com com o Token de verificação do domínio '1234abcd', em seguida, web efetuado um pedido para http://azure.com/1234abcd.html deverá devolver 1234abcd. 

> [!IMPORTANT]
> Uma ordem de certificado tem apenas 15 dias para concluir a operação de verificação de domínio. Após 15 dias, o certificado é negado por uma autoridade de certificação e não lhe serem cobrados para o certificado. Nesta situação, elimine este certificado e tente novamente.
>
> 

### <a name="unable-to-purchase-a-domain"></a>Não é possível comprar um domínio

### <a name="symptom"></a>Sintoma
Não é possível comprar um domínio de aplicação Web ou de domínio da aplicação de serviço no portal do Azure.

### <a name="cause-and-solution"></a>Causa e soluções

Este problema ocorre para uma das seguintes razões:

- Sem cartões de crédito na subscrição do Azure ou cartão de crédito inválido.

    **Solução**: adicionar um cartão de crédito válido à sua subscrição, se não tiver uma.

- Se não for o proprietário da subscrição, poderá não ter permissão para comprar o domínio.

    **Solução**: [adicionar a função de proprietário](../billing/billing-add-change-azure-subscription-administrator.md) à sua conta ou contacte o administrador de subscrição para obter permissões para comprar um domínio.
- Atingiu o limite para a compra de domínios na sua subscrição. O limite atual é 20.

    **Solução**: A pedido aumentar o limite de, contacto [suporte do Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
- O tipo de subscrição do Azure não suporta a compra de domínio do serviço de aplicações.

    **Solução**: atualizar a sua subscrição do Azure para outros tipos de subscrição, tais como uma subscrição pay as you go.

### <a name="unable-to-add-a-hostname-to-web-app"></a>Não é possível adicionar um nome de anfitrião para a aplicação Web 

### <a name="symptom"></a>Sintoma

Quando adiciona um nome de anfitrião, o processo não consegue validar e verificar o domínio.

### <a name="cause"></a>Causa 

Este problema ocorre para uma das seguintes razões:

- Não tem permissão para adicionar um nome de anfitrião.

    **Solução**: verificação com o administrador de subscrição para se certificar de que tem uma permissão para adicionar um nome de anfitrião.
- Não foi possível verificar a propriedade de domínio.

    **Solução**: verificar se o CNAME ou um registo estão configurados corretamente. Para mapear o domínio personalizado para a aplicação web, crie um CNAME ou o registo. Se pretender utilizar o domínio de raiz, tem de utilizar registos A e TXT:

    |Tipo de registo|Anfitrião|Aponte para|
    |------|------|-----|
    |A|@|Endereço IP para a aplicação web|
    |TXT|@|< nome da aplicação >. azurewebsites.net|
    |CNAME|www|< nome da aplicação >. azurewebsites.net|

### <a name="dns-cannot-be-resolved"></a>Não é possível resolver o DNS

### <a name="symptom"></a>Sintoma

Receberá uma mensagem de erro "o registo DNS não foi possível localizar".

### <a name="cause"></a>Causa
Este problema ocorre para uma das seguintes razões:

- Não expirou o tempo para o período em direto (TTL). Verifique a configuração de DNS para o seu domínio determinar o valor de TTL e, em seguida, aguarde que o período de expirar.
- A configuração de DNS está incorreta.

### <a name="solution"></a>Solução
- Aguarde 48 horas para que este problema seja resolvido automaticamente.
- Se o pode alterar a definição de TTL na sua configuração de DNS, altere o valor para 5 minutos para ver se este procedimento resolve o problema.
- Utilize [WhatsmyDNS.net](https://www.whatsmydns.net/) para verificar que o seu domínio aponta para o endereço IP da aplicação web. Se não existir, configure o registo para o endereço IP correto da aplicação web.

### <a name="restore-a-deleted-domain"></a>Restaurar um domínio eliminado 

### <a name="symptom"></a>Sintoma
O domínio já não é visível no portal do Azure.

### <a name="cause"></a>Causa 
O domínio pode acidentalmente ter sido eliminado pelo proprietário da subscrição.

### <a name="solution"></a>Solução
Se o domínio foi eliminado há menos de sete dias, o domínio ainda não foi iniciado o processo de eliminação. Neste caso, pode comprar o mesmo domínio novamente no portal do Azure na mesma subscrição (Certifique-se de que escreva o nome de domínio exata na caixa de pesquisa). Não será cobrado novamente para este domínio. Se o domínio tiver sido eliminado há mais de sete dias, contacte o [suporte do Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para obter ajuda restaurar o domínio.

### <a name="custom-domain-returns-404-or-site-inaccessible"></a>Domínio personalizado devolve 404 ou site inacessível 

### <a name="symptom"></a>Sintoma

Ao navegar para o site utilizando o nome de domínio personalizado, recebe a seguinte mensagem de erro:

**Erro 404-Web app não encontrado.**


### <a name="cause-and-solution"></a>Causa e soluções

**Causa 1** 

O domínio personalizado que configurou em falta um registo CNAME ou um. 

**Solução para causa 1**

- Se tiver adicionado um registo, certifique-se de que um registo TXT é também adicionado. Para obter mais informações, consulte [criar-a-a-registo](./app-service-web-tutorial-custom-domain.md#create-the-a-record).
- Se tiver de utilizar o domínio de raiz para a sua aplicação web, recomenda-se para utilizar um registo CNAME, em vez de um registo.
- Não utilize um CNAME e um registo para o mesmo domínio. Isto pode causar conflito e impedir que o domínio resolver. 

**Causa 2** 

Browser da Internet poderão ainda estar a colocação em cache o endereço IP antigo para o seu domínio. 

**Solução para causa 2**

Limpe o browser. Para dispositivos Windows, pode executar o comando `ipconfig /flushdns`. Utilize [WhatsmyDNS.net](https://www.whatsmydns.net/) para verificar que o seu domínio aponta para o endereço IP da aplicação web. 

### <a name="unable-to-add-subdomain"></a>Não é possível adicionar o subdomínio 

### <a name="symptom"></a>Sintoma

Não é possível adicionar um novo nome de anfitrião para uma aplicação web para atribuir um subdomínio.

### <a name="solution"></a>Solução

- Contacte o administrador de subscrição para se certificar de que tem permissões para adicionar um nome de anfitrião para a aplicação web.
- Se precisar de mais subdomínios, recomendamos que altere o alojamento de domínio ao DNS do Azure. Ao utilizar o DNS do Azure, pode adicionar nomes de 500 anfitrião à sua aplicação web. Para obter mais informações, consulte [adicionar um subdomínio](https://blogs.msdn.microsoft.com/waws/2014/10/01/mapping-a-custom-subdomain-to-an-azure-website/).











 


















