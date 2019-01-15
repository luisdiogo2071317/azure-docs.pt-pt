---
title: Guia de resolução de problemas do Explorador de armazenamento do Azure | Documentos da Microsoft
description: Descrição geral dos dois depuração funcionalidade do Azure
services: virtual-machines
author: Deland-Han
ms.service: virtual-machines
ms.topic: troubleshooting
ms.date: 06/15/2018
ms.author: delhan
ms.component: common
ms.openlocfilehash: 2434c04260c223fdcc03bc7f91a6fd147e1d8503
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/14/2019
ms.locfileid: "54264110"
---
# <a name="azure-storage-explorer-troubleshooting-guide"></a>Guia de resolução de problemas do Explorador de armazenamento do Azure

Explorador de armazenamento do Microsoft Azure é uma aplicação autónoma que lhe permite trabalhar facilmente com dados de armazenamento do Azure no Windows, macOS e Linux. A aplicação pode ligar a contas de armazenamento alojadas no Azure, Clouds nacionais e o Azure Stack.

Este guia resume as soluções para problemas comuns detetados no Explorador de armazenamento.

## <a name="error-self-signed-certificate-in-certificate-chain-and-similar-errors"></a>Erro: Certificado Autoassinado na cadeia de certificados (e erros semelhantes)

Erros de certificado são causados por uma das duas seguintes situações:

1. A aplicação está conectada por meio de um "proxy transparente", que significa que um servidor (por exemplo, o servidor da empresa) é intercetar o tráfego HTTPS, desencriptá-lo e, em seguida, criptografando-a utilizar um certificado autoassinado.
2. Está a executar uma aplicação que está a injetar um certificado SSL autoassinado nas mensagens HTTPS recebidos. Exemplos de aplicativos que inserir certificados inclui software de inspeção de tráfego de rede e software antivírus.

Quando o Explorador de armazenamento vê um self assinado ou o certificado não fidedigno, pode já não sabe se a mensagem recebida do HTTPS foi alterada. Se tiver uma cópia do certificado autoassinado, pode instruir o Explorador de armazenamento confiar nele efetuando os seguintes passos:

1. Obter uma Base-64 com codificação X.509 (. cer) de cópia do certificado
2. Clique em **edite** > **certificados SSL** > **importar certificados**e, em seguida, utilize o Seletor de ficheiros para localizar, selecione e abra o ficheiro. cer

Este problema também pode ser o resultado de vários certificados (raiz e intermediário). Ambos os certificados devem ser adicionados para superar o erro.

Se tiver a certeza de onde o certificado é proveniente, pode experimentar estas etapas para encontrá-lo:

1. Instalar o Open SSL

    * [Windows](https://slproweb.com/products/Win32OpenSSL.html) (qualquer uma das versões simples devem ser suficientes)
    * Mac e Linux: devem ser incluídos com o sistema operativo
2. Executar o Open SSL

    * Windows: abrir o diretório de instalação, clique em **/bin/** e, em seguida, faça duplo clique em **openssl.exe**.
    * Mac e Linux: execute **openssl** partir de um terminal.
3. Execute `s_client -showcerts -connect microsoft.com:443`
4. Procure certificados autoassinados. Se não souber quais são autoassinados, procure em qualquer lugar o assunto `("s:")` e o emissor `("i:")` são os mesmos.
5. Quando encontrar certificados autoassinados, para cada um, copie e cole tudo entre incluindo **---BEGIN CERTIFICATE---** ao **---END CERTIFICATE---** para um novo ficheiro. cer.
6. Abra o Explorador de armazenamento, clique em **edite** > **certificados SSL** > **importar certificados**e, em seguida, utilize o Seletor de ficheiros para localizar, selecionar, e Abra os ficheiros. cer que criou.

Se não conseguir encontrar nenhum certificado autoassinado utilizando os passos anteriores, contacte-nos por meio da ferramenta de comentários para obter mais ajuda. Em alternativa, pode optar por iniciar o Explorador de armazenamento a partir da linha de comandos com o `--ignore-certificate-errors` sinalizador. Quando iniciado com esse sinalizador, o Explorador de armazenamento irão ignorar erros de certificado.

## <a name="sign-in-issues"></a>Problemas de início de sessão

### <a name="reauthentication-loop-or-upn-change"></a>Ciclo de reautenticação ou alteração UPN
Se estiver num loop de reautenticação, ou tiver alterado o UPN de uma das suas contas, experimente o seguinte:
1. Remover todas as contas e, em seguida, feche o Explorador de armazenamento
2. Eliminar o. Pasta de IdentityService partir do seu computador. No Windows, a pasta está localizada em `C:\users\<username>\AppData\Local`. Para Mac e Linux, pode encontrar a pasta na raiz do seu diretório do utilizador.
3. Se estiver no Mac ou Linux, também terá de eliminar a entrada de Microsoft.Developer.IdentityService de keystore de seu sistema operacional. No Mac, o keystore é o aplicativo de "Gnome Keychain". Para o Linux, o aplicativo é normalmente chamado de "Porta-chaves", mas o nome pode ser diferente dependendo de sua distribuição.

### <a name="conditional-access"></a>Acesso Condicional
Acesso condicional não é suportado quando o Explorador de armazenamento está a ser utilizado no Windows 10, Linux ou macOS. Isso é devido a uma limitação na biblioteca do AAD utilizado pelo Explorador de armazenamento.

## <a name="mac-keychain-errors"></a>Erros de Keychain do Mac
O macOS Keychain, às vezes, pode chegar a um Estado que provoca problemas para a biblioteca de autenticação do Explorador de armazenamento. Para obter a keychain fora do try este estado os seguintes passos:
1. Feche o Explorador de armazenamento.
2. Keychain aberto (**cmd + espaço**, escreva numa keychain, pressionar introduza).
3. Selecione a keychain "início de sessão".
4. Clique no ícone de cadeado para bloquear a porta-chaves (o cadeado será animar para uma posição bloqueada quando terminar, pode demorar alguns segundos, consoante o que tem de aplicações abertas).

    ![image](./media/storage-explorer-troubleshooting/unlockingkeychain.png)

5. Inicie o Explorador de armazenamento.
6. Um pop-up deverá aparecer dizendo algo como "hub de serviço pretende aceder a keychain". Quando, introduza a palavra-passe da conta do administrador de Mac e clique em **sempre permitir** (ou **permitir** se **permitir sempre** não está disponível).
7. Tente iniciar sessão.

### <a name="general-sign-in-troubleshooting-steps"></a>Início de sessão resolução de problemas passos gerais
* Se estiver no macOS e a janela de início de sessão nunca será exibido sobre a caixa de diálogo "A aguardar para autenticação...", em seguida, tente [estes passos](#mac-keychain-errors)
* Reinicie o Explorador de armazenamento
* Se a janela de autenticação está em branco, aguarde, pelo menos, um minuto antes de fechar a caixa de diálogo de autenticação.
* Certifique-se de que o proxy e as definições estão configuradas corretamente para o seu computador e o Explorador de armazenamento de certificado.
* Se estiver no Windows e ter acesso ao Visual Studio 2017 na mesma máquina e início de sessão, tente iniciar sessão no Visual Studio 2017. Após um êxito início de sessão para o Visual Studio 2017, deve ser capaz de abrir o Explorador de armazenamento e ver a sua conta no painel de conta.

Se nenhum desses métodos funcionam [abra um problema no GitHub](https://github.com/Microsoft/AzureStorageExplorer/issues).

### <a name="missing-subscriptions-and-broken-tenants"></a>Inscrições em falta e inquilinos quebrados

Se não é possível obter as suas subscrições depois de entrar com êxito, tente os seguintes métodos de resolução de problemas:

* Certifique-se de que a conta tem acesso às subscrições que espera. Pode verificar se tem acesso ao iniciar sessão no portal para o ambiente do Azure que está a tentar utilizar.
* Certifique-se de que iniciou sessão com o Azure correto ambiente (do Azure, Azure China, Azure Alemanha, Azure US Government ou ambiente personalizado).
* Se estiver atrás de um proxy, certifique-se de que configurou o proxy do Explorador de armazenamento corretamente.
* Tente remover e adicionar novamente a conta.
* Se existir uma ligação de "Obter mais informações", procure e consulte as mensagens de erro estão a ser reportadas para os inquilinos que estão a falhar. Se não souber ao certo o que fazer com o mensagens de erro, consulte, em seguida, não hesite em [abra um problema no GitHub](https://github.com/Microsoft/AzureStorageExplorer/issues).

## <a name="cannot-remove-attached-account-or-storage-resource"></a>Não é possível remover o recurso de conta ou armazenamento anexado

Se não é possível remover um recurso de armazenamento através da IU ou conta anexada, pode eliminar manualmente todos os recursos anexados ao eliminar as seguintes pastas:

* Windows: `%AppData%/StorageExplorer`
* macOS: `/Users/<your_name>/Library/Application Support/StorageExplorer`
* Linux: `~/.config/StorageExplorer`

> [!NOTE]
>  Feche o Explorador de armazenamento antes de eliminar as pastas acima.

> [!NOTE]
>  Se alguma vez tiver importado os certificados SSL, em seguida, o conteúdo de cópia de segurança a `certs` diretório. Mais tarde, pode utilizar a cópia de segurança para reimportar os certificados SSL.

## <a name="proxy-issues"></a>Problemas de proxy

Em primeiro lugar, certifique-se de que as seguintes informações que introduziu estão corretos:

* O URL de proxy e o número de porta
* Nome de utilizador e palavra-passe se for necessário pelo proxy

Tenha em atenção que o Explorador de armazenamento não suporta .pac ficheiro para configurar definições de proxy.

### <a name="common-solutions"></a>Soluções comuns

Se ainda ocorrerem problemas, tente os seguintes métodos de resolução de problemas:

* Se consegue ligar à Internet sem utilizar o proxy, certifique-se de que o Explorador de armazenamento funciona sem as definições de proxy ativadas. Se for este o caso, pode haver um problema com as suas definições de proxy. Contactar o administrador de proxy para identificar os problemas.
* Certifique-se de que outros aplicativos usando o servidor proxy funcionam conforme esperado.
* Verifique se pode ligar-se para o portal para o ambiente do Azure que está a tentar utilizar
* Certifique-se de que pode receber respostas de seus pontos finais de serviço. Introduza um dos URLs de ponto final no seu browser. Se pode ligar, deverá receber uma InvalidQueryParameterValue ou resposta semelhante de XML.
* Se outra pessoa também é utilizar o Explorador de armazenamento com o servidor proxy, certifique-se de que eles podem se conectar. Se pode ligar-se, poderá ter de contactar o seu administrador do servidor proxy.

### <a name="tools-for-diagnosing-issues"></a>Ferramentas para diagnosticar problemas

Se tiver ferramentas de rede, como o Fiddler para Windows, podem ser capazes de diagnosticar os problemas da seguinte forma:

* Se tem que trabalhar através do seu proxy, poderá ter de configurar a sua ferramenta de rede para ligar através do proxy.
* Verificar o número de porta utilizado pela sua ferramenta de rede.
* Introduza o URL de local host e o número de porta de rede da ferramenta, como definições de proxy no Explorador de armazenamento. Se for feito corretamente, sua ferramenta de rede inicia o registo de pedidos de rede feitos pelo Explorador de armazenamento para a gestão e pontos finais de serviço. Por exemplo, introduza https://cawablobgrs.blob.core.windows.net/ o ponto final do blob num navegador e receberá uma resposta é semelhante ao seguinte, o que sugere o recurso existe, embora não é possível acessá-lo.

![Exemplo de código](./media/storage-explorer-troubleshooting/4022502_en_2.png)

### <a name="contact-proxy-server-admin"></a>Contacte o administrador do servidor proxy

Se as definições de proxy estão corretas, poderá ter de contactar o seu administrador de servidor proxy, e

* Certifique-se de que o proxy não bloqueia o tráfego para pontos finais do Azure de gestão ou recurso.
* Verifique se o protocolo de autenticação utilizado pelo seu servidor proxy. Explorador de armazenamento não suporta atualmente os proxies NTLM.

## <a name="unable-to-retrieve-children-error-message"></a>Mensagem de erro "Não é possível obter subordinados"

Se estiver ligado ao Azure através de um proxy, certifique-se de que as definições de proxy estão corretas. Se foi concedido acesso a um recurso do proprietário da subscrição ou conta, certifique-se de que tem de leitura ou lista de permissões para esse recurso.

## <a name="connection-string-does-not-have-complete-configuration-settings"></a>Cadeia de ligação não tem definições de configuração concluída

Se receber esta mensagem de erro, é possível que não tem as permissões necessárias para obter as chaves para a sua conta de armazenamento. Para confirmar se for este o caso, aceda ao portal e localize a sua conta de armazenamento. Pode rapidamente fazer isso clicando com o botão direito do rato no nó para a sua conta de armazenamento e clicar em "Abrir no Portal". Depois de o fazer, aceda ao painel "Chaves de acesso". Se não tiver permissões para ver as chaves, em seguida, verá uma página com a mensagem "Não tem acesso". Para resolver este problema, pode obter a chave de conta de outra pessoa e anexar com nome e a chave, ou pode pedir a alguém para uma SAS para a conta de armazenamento e utilizá-lo para anexar a conta de armazenamento.

Se vir as chaves de conta, em seguida, submeta um problema no GitHub, de modo que pode ajudá-lo a resolver o problema.

## <a name="issues-with-sas-url"></a>Problemas com o URL de SAS
Se estiver a ligar a um serviço através de um URL de SAS e com este erro:

* Certifique-se de que o URL fornece as permissões necessárias para ler ou lista de recursos.
* Certifique-se de que o URL não expirou.
* Se o URL de SAS baseia-se uma política de acesso, certifique-se de que a política de acesso não foi revogada.

Se acidentalmente anexados utilizando um URL de SAS inválido e não é possível anular a exposição, siga estes passos:
1.  Ao executar o Explorador de armazenamento, premir a tecla F12 para abrir a janela de ferramentas de desenvolvedor.
2.  Clique no separador de aplicativo, em seguida, clique em armazenamento Local > file:// na árvore à esquerda.
3.  Localize a chave associada com o tipo de serviço do URI de SAS problemático. Por exemplo, se o mau URI de SAS para um contentor de BLOBs, procure a chave com o nome `StorageExplorer_AddStorageServiceSAS_v1_blob`.
4.  O valor da chave deve ser uma matriz JSON. Encontrar o objeto associado ao URI ruim e removê-lo.
5.  Prima Ctrl + R para recarregar o Explorador de armazenamento.

## <a name="linux-dependencies"></a>Dependências do Linux

Para distribuições de Linux que não seja o Ubuntu 16.04, terá de instalar manualmente algumas dependências. Em geral, os seguintes pacotes são necessários:
* [.NET Core 2.x](https://docs.microsoft.com/dotnet/core/linux-prerequisites?tabs=netcore2x)
* `libsecret`
* `libgconf-2-4`
* GCC atualizado

Dependendo de sua distribuição, pode haver outros pacotes, que tem de instalar. O Explorador de armazenamento [notas de versão](https://go.microsoft.com/fwlink/?LinkId=838275&clcid=0x409) conter passos específicos para algumas distribuições.

## <a name="next-steps"></a>Passos Seguintes

Se nenhuma das soluções funcionam para si, em seguida, [abra um problema no GitHub](https://github.com/Microsoft/AzureStorageExplorer/issues). Pode também rapidamente obter ao GitHub com o botão "Comunicar o problema para o GitHub" no canto inferior esquerdo.

![Comentários](./media/storage-explorer-troubleshooting/feedback-button.PNG)
