---
title: Resolver problemas de configurações de delegação restringida de Kerberos para o Proxy de aplicações | Microsoft Docs
description: Resolver problemas de configurações de delegação restrita de Kerberos para o Proxy de aplicações
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2018
ms.author: barbkess
ms.reviewer: harshja
ms.openlocfilehash: 35b4343bac033708a0912f1e129cdfef146e0603
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34698786"
---
# <a name="troubleshoot-kerberos-constrained-delegation-configurations-for-application-proxy"></a>Resolver problemas de configurações de delegação restringida de Kerberos para o Proxy de aplicações

Os métodos disponíveis para alcançar o SSO para as aplicações publicadas podem variar entre uma aplicação para outra. Uma opção que o Proxy de aplicações do Azure Active Directory (Azure AD) oferece por predefinição é a delegação restringida de Kerberos (KCD). Pode configurar um conector, para que os utilizadores, execute a restrita de Kerberos de autenticação para aplicações de back-end.

O procedimento para ativar o KCD é simples. É não necessária nenhuma mais do que uma compreensão geral sobre os vários componentes e fluxo de autenticação que suportam o SSO. Mas, por vezes, KCD SSO não funcionar conforme esperado. Terá de boas fontes de informação para resolver estes cenários.

Este artigo fornece um único ponto de referência que ajuda a resolver problemas e automática retificar alguns dos problemas mais comuns. Também abrange o diagnóstico de problemas de implementação mais complexos.

Este artigo faz com que os seguintes pressupostos:

-   Implementação do Proxy de aplicações do Azure AD por [começar com o Proxy de aplicações](manage-apps/application-proxy-enable.md) e geral acesso a aplicações de KCD não funcionar conforme esperado.

-   A aplicação de destino publicados baseia-se nos serviços de informação Internet (IIS) e a implementação Microsoft do Kerberos.

-   Os anfitriões de servidor e aplicação de residir num único domínio do Azure Active Directory. Para obter informações detalhadas sobre os cenários entre domínios e florestas, consulte o [KCD documento técnico](https://aka.ms/KCDPaper).

-   A aplicação de assunto está publicada um Azure inquilino com a pré-autenticação ativada. Espera-se que os utilizadores autenticados no Azure através da autenticação baseada em formulários. Cenários de autenticação de cliente avançada não são abrangidos por este artigo. Podem ser adicionados, a determinada altura no futuro.

## <a name="prerequisites"></a>Pré-requisitos

Proxy de aplicações do Azure AD pode ser implementado em vários tipos de infraestruturas ou ambientes. As arquiteturas variam de organização para organização. As causas mais comuns dos problemas relacionados com o KCD não são os ambientes. Configurações incorretas simples ou prende geral fazer com que a maioria dos problemas.

Por este motivo, é melhor certificar-se de que cumpriu todos os pré-requisitos em [utilizando SSO KCD com o Proxy de aplicações](manage-apps/application-proxy-configure-single-sign-on-with-kcd.md) antes de iniciar a resolução de problemas. Tenha em atenção a secção sobre como configurar a delegação restringida de Kerberos no 2012R2. Este processo utiliza uma abordagem diferente para configurar KCD em versões anteriores do Windows. Além disso, ser mindful destas considerações:

-   Não é invulgar para um servidor membro do domínio abrir uma caixa de diálogo de canal seguro com um controlador de domínio específico (DC). Em seguida, o servidor pode mover para outra caixa de diálogo em qualquer momento. Por isso, anfitriões de conector não são limitadas a comunicação com o site local específico apenas os controladores de domínio.

-   Cenários entre domínios dependem referências direcionam um anfitrião do conector para controladores de domínio que podem estar fora do perímetro de rede local. Nestes casos, é igualmente importante também enviar tráfego onward para DCs que representam outros respetivos domínios. Caso contrário, falha de delegação.

-   Sempre que possível, evite colocar quaisquer dispositivos ativos de IPS ou os IDS entre anfitriões de conector e os controladores de domínio. Estes dispositivos, por vezes, são overintrusive e interferir com núcleo tráfego RPC.

Delegação de teste em cenários simples. As variáveis mais introduz, mais tenham disputam com. Para poupar tempo, limite os testes para um único conector. Adicione conectores adicionais depois do problema foi resolvido.

Alguns fatores ambientais também podem contribuir para um problema. Para evitar estes fatores, estará a minimizar arquitetura quanto possível durante os testes. Por exemplo, a firewall interno mal configurada ACLs são comuns. Se for possível, envie todo o tráfego de um conector direitas através de DCs e aplicação de back-end.

O melhor local para a posição conectores é mais parecido possível com os respetivos destinos. Uma firewall que se encontre inline quando testar o adiciona a complexidade desnecessária e pode prolongar as investigações.

O que apresenta um problema KCD? Existem várias indicações comuns que KCD SSO está a falhar. Os primeiro sinais de um problema de aparecem no browser.

   ![Erro de configuração de KCD incorreto](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic1.png)

   ![Autorização falhou devido a falta de permissões](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic2.png)

Ambas estas imagens mostram a mesma sintoma: falha SSO. Acesso de utilizador para a aplicação é negado.

## <a name="troubleshooting"></a>Resolução de problemas

Como resolver depende do problema e os sintomas que observar. Antes de pode aceder qualquer farther, explore os seguintes artigos. Fornecem informações úteis de resolução de problemas:

-   [Resolver problemas de Proxy de aplicações e as mensagens de erro](active-directory-application-proxy-troubleshoot.md)

-   [Erros de Kerberos e sintomas](active-directory-application-proxy-troubleshoot.md#kerberos-errors)

-   [Trabalhar com o SSO quando no local e na nuvem identidades não são idênticas](manage-apps/application-proxy-configure-single-sign-on-with-kcd.md#working-with-different-on-premises-and-cloud-identities)

Se recebeu a este ponto, o problema principal existe. Para começar, separe o fluxo para as três fases seguintes que pode resolver.

### <a name="client-pre-authentication"></a>Pré-autenticação de cliente 
O utilizador externo a autenticação no Azure através de um browser. A capacidade de pré-autenticação do Azure é necessária para KCD SSO para a função. Testar e resolver esta capacidade se existem quaisquer problemas. A fase de pré-autenticação não está relacionado com KCD ou a aplicação publicada. É fácil corrigir quaisquer discrepâncias sanity verificando se a conta de assunto existe no Azure. Também-se de que não está desativado ou bloqueado. A resposta de erro no browser é descritiva para explicar a causa. Se estiver incerto, consulte outros artigos de resolução de problemas da Microsoft para verificar.

### <a name="delegation-service"></a>Serviço de delegação 
O conector do Proxy do Azure que obtém uma permissão de serviço Kerberos para os utilizadores a partir de um centro de distribuição de chave de Kerberos (KCD).

As comunicações entre o cliente e o front-end do Azure externas não tem nenhum efeito no KCD. Estas comunicações apenas Certifique-se de que o KCD funciona. O serviço de Proxy do Azure é fornecido um ID de utilizador válido que é utilizado para obter uma permissão Kerberos. Sem este ID, KCD não é possível e falha.

Como mencionado anteriormente, as mensagens de erro do browser fornece algumas processáveis boas sobre por que motivo coisas falharem. Certifique-se anotar o ID de atividade e timestamp na resposta. Esta informação ajuda-o a correlacionar o comportamento para reais eventos no registo de eventos de Proxy do Azure.

   ![Erro de configuração de KCD incorreto](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic3.png)

As entradas correspondentes vistas no registo de eventos mostram como eventos 13019 ou 12027. Localizar os registos de eventos do conector no **registos de serviços e aplicações** &gt; **Microsoft** &gt; **AadApplicationProxy** &gt;  **Conector** &gt; **Admin**.

   ![Evento 13019 de registo de eventos de Proxy de aplicações](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic4.png)

   ![12027 de eventos de registo de eventos de Proxy de aplicações](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic5.png)

1.   Utilize um **A** gravar no seu DNS interno para o endereço da aplicação, não um **CName**.

2.   Reconfirm que o anfitrião do conector foi concedido o direito para delegar SPN a conta de destino designado. Reconfirm que **utilizar qualquer protocolo de autenticação** está selecionada. Para obter mais informações, consulte o [artigo de configuração de SSO](manage-apps/application-proxy-configure-single-sign-on-with-kcd.md).

3.   Verifique se existe apenas uma instância do SPN sessão no Azure AD. Problema `setspn -x` numa linha de comandos em qualquer anfitrião membro do domínio.

4.   Verificar que é aplicada a política de domínio que limita a [tamanho máximo de tokens emitidos do Kerberos](https://blogs.technet.microsoft.com/askds/2012/09/12/maxtokensize-and-windows-8-and-windows-server-2012/). Esta política interrompe o conector de obter um token se é possível encontrá-lo para ser excessiva.

Um rastreio de rede que capture as trocas entre o anfitrião do conector e um domínio KDC é o próximo passo melhor para obter mais detalhes de baixo nível sobre os problemas. Para obter mais informações, consulte o [detalhada da resolução de problemas documento](https://aka.ms/proxytshootpaper).

Se procura boa a emissão de permissões, verá um evento nos registos a indicar que a autenticação falhou porque a aplicação devolveu um 401. Este evento indica que a aplicação de destino rejeitou o pedido de suporte. Vá para a próxima fase.

### <a name="target-application"></a>Aplicação de destino 
O consumidor do bilhete Kerberos fornecido pelo conector. Nesta fase, esperar que o conector ter enviado um pedido de serviço Kerberos para o back-end. Esta permissão é um cabeçalho no primeiro pedido de aplicação.

1.   Ao utilizar de URL interno a aplicação definido no portal, confirme que a aplicação está acessível diretamente a partir do browser no anfitrião do conector. Em seguida, pode iniciar sessão com êxito. Podem ser encontrados detalhes no conector **Troubleshoot** página.

2.   Ainda no anfitrião do conector, confirme que a autenticação entre o browser e a aplicação utiliza o Kerberos. Efetue uma das seguintes ações:

3.  Executar DevTools (**F12**) no Internet Explorer ou utilizar [Fiddler](https://blogs.msdn.microsoft.com/crminthefield/2012/10/10/using-fiddler-to-check-for-kerberos-auth/) do anfitrião de conector. Aceda à aplicação utilizando o URL interno. Inspecione os cabeçalhos de autorização de WWW oferecidos devolvidos em resposta da aplicação para se certificar de que quer negociar ou Kerberos está presente. 

    a. O blob Kerberos seguinte, que é devolvido na resposta do browser para a aplicação começa com **YII**. Estas letras dizer que Kerberos está em execução. Microsoft NT LAN Manager (NTLM), por outro lado, sempre começa com **TlRMTVNTUAAB**, que lê o fornecedor de suporte de segurança da NTLM (NTLMSSP) quando descodificar a partir de Base64. Se vir **TlRMTVNTUAAB** no início do blob, Kerberos não está disponível. Se não vir **TlRMTVNTUAAB**, Kerberos é provável que disponíveis.

       > [!NOTE]
       > Se utilizar o Fiddler, este método requer que desativar temporariamente a proteção expandida na configuração da aplicação no IIS.

       ![Janela de inspeção de rede do browser](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic6.png)

    b. O blob desta figura não comece com **TIRMTVNTUAAB**. Por conseguinte, neste exemplo, Kerberos está disponível e o blob de Kerberos não comece com **YII**.

4.  Remova temporariamente NTLM da lista de fornecedores no site do IIS. Aceder à aplicação diretamente a partir do Internet Explorer no anfitrião de conector. NTLM já não consta da lista de fornecedores. Pode aceder à aplicação ao utilizar apenas Kerberos. Se falhar de acesso, podem existir um problema com a configuração da aplicação. A autenticação Kerberos não está a funcionar.

    a. Se Kerberos não estiver disponível, verifique as definições de autenticação da aplicação no IIS. Certifique-se **negociar** está listada na parte superior, com o NTLM imediatamente por baixo. Se vir **não negociar**, **Kerberos ou negociar**, ou **ao PKU2U**, continue apenas se o Kerberos é funcional.

       ![Fornecedores de autenticação do Windows](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic7.png)
   
    b. Com o Kerberos e NTLM no local, desative temporariamente pré-autenticação para a aplicação no portal. Tente aceder a partir da internet utilizando o URL externo. Lhe for pedida a autenticação. Está a conseguir fazê-lo com a mesma conta utilizada no passo anterior. Caso contrário, não há um problema com a aplicação de back-end, não KCD.

    c. Volte a ativar a pré-autenticação no portal. Autenticar através do Azure ao tentar ligar-se para a aplicação através do respetivo URL externo. Se falhar de SSO, verá uma mensagem de erro proibido no browser e o evento 13022 no registo de:

       *Conector do Proxy de aplicações do Microsoft AAD não é possível autenticar o utilizador porque o servidor de back-end responde a tentativas de autenticação Kerberos com um erro de HTTP 401.*

       ![401 HTTTP proibido erro](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic8.png)

    d. Verifique a aplicação do IIS. Certifique-se de que o conjunto aplicacional configurado e o SPN estão configurados para utilizar a mesma conta no Azure AD. Navegue no IIS, conforme mostrado na ilustração seguinte:

       ![Janela de configuração de aplicação do IIS](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic9.png)

       Depois de conheçam a identidade, certifique-se de que esta conta está configurada com o SPN em questão. Um exemplo é `setspn –q http/spn.wacketywack.com`. Introduza o texto seguinte numa linha de comandos:

       ![Janela de comandos SetSPN](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic10.png)

    e. Verifique o SPN definido contra definições da aplicação no portal. Certifique-se de que o mesmo SPN configurado contra o destino da conta do Azure AD é utilizado por conjunto aplicacional da aplicação.

       ![Configuração de SPN no portal do Azure](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic11.png)
   
    f. Vá para o IIS e selecione o **Editor de configuração** opção para a aplicação. Navegue para **system.webServer/security/authentication/windowsAuthentication**. Certifique-se de que o valor **UseAppPoolCredentials** é **verdadeiro**.

       ![Opção de credencial de agrupamentos de aplicações de configuração do IIS](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic12.png)

       Altere este valor para **verdadeiro**. Remova todas as permissões de Kerberos em cache do servidor back-end, executando o seguinte comando:

       ```powershell
       Get-WmiObject Win32_LogonSession | Where-Object {$_.AuthenticationPackage -ne 'NTLM'} | ForEach-Object {klist.exe purge -li ([Convert]::ToString($_.LogonId, 16))}
       ``` 

Para obter mais informações, consulte [remover a cache de pedido de suporte de cliente Kerberos para todas as sessões](https://gallery.technet.microsoft.com/scriptcenter/Purge-the-Kerberos-client-b56987bf).



Se deixar o modo de Kernel ativado, melhora o desempenho de operações do Kerberos. Mas também faz com que o pedido de suporte para o serviço pedido ser desencriptados utilizando a conta da máquina. Esta conta também é denominada o sistema Local. Definir este valor como **verdadeiro** para interromper KCD quando a aplicação fique alojada em mais de um servidor num farm.

-   Como uma verificação adicional, desative **expandida** proteção demasiado. Em alguns cenários, **expandida** proteção quebrou KCD quando tiver sido ativado nas configurações específicas. Nesses casos, uma aplicação foi publicada como uma subpasta do Web site predefinido. Esta aplicação está configurada para apenas a autenticação anónima. Todas as caixas de diálogo estão desativadas, que sugere objetos subordinados não herdam as definições do Active Directory. Recomendamos que teste, mas não se esqueça de restaurar este valor para **ativada**, sempre que possível.

    Esta verificação adicional coloca é controlado para utilizar a aplicação publicada. Pode a rotação dos conectores adicionais que também estejam configurados para delegar. Para obter mais informações, leia a passagem de técnica mais aprofundada, [resolução de problemas de Proxy de aplicações do Azure AD](https://aka.ms/proxytshootpaper).

Se ainda não é possível efetuar progresso, o suporte da Microsoft pode ajudá-lo. Crie um pedido de suporte diretamente no portal. Um engenheiro irá contactá-lo.

## <a name="other-scenarios"></a>Outros cenários

- Proxy de aplicações do Azure pede uma permissão Kerberos antes de enviar o pedido para uma aplicação. Algumas aplicações de terceiros como o Tableau Server não gostar deste método de autenticação. Estas aplicações esperam as negociações mais convencionais seja realizada. O primeiro pedido é anónimo, que permite que a aplicação responder com os tipos de autenticação que suporta através de um 401.

- Autenticação multi-HOP é normalmente utilizada em cenários em que uma aplicação está em camadas, com um back-end e front-end, onde requerem ambos autenticação, tais como SQL Server Reporting Services. Para configurar o cenário multihop, consulte o artigo de suporte [restrita delegação pode necessitar de transição do protocolo Kerberos em cenários de multi-HOP](https://support.microsoft.com/help/2005838/kerberos-constrained-delegation-may-require-protocol-transition-in-mul).

## <a name="next-steps"></a>Passos Seguintes
[Configurar o KCD num domínio gerido](../active-directory-domain-services/active-directory-ds-enable-kcd.md).
