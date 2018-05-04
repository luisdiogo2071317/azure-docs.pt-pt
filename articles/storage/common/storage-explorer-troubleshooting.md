---
title: Guia de resolução de problemas de Explorador de armazenamento do Azure | Microsoft Docs
description: Descrição geral dos dois depuração funcionalidade do Azure
services: virtual-machines
documentationcenter: ''
author: Deland-Han
manager: cshepard
editor: ''
ms.assetid: ''
ms.service: virtual-machines
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 09/08/2017
ms.author: delhan
ms.openlocfilehash: f58fb5090aba3c5052d1bbdec76225d0ae50e8f2
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="azure-storage-explorer-troubleshooting-guide"></a>Guia de resolução de problemas de Explorador de armazenamento do Azure

Explorador de armazenamento do Microsoft Azure é uma aplicação autónoma que lhe permite trabalhar facilmente com dados de armazenamento do Azure no Windows, macOS e Linux. A aplicação pode ligar a contas de armazenamento alojadas no Azure, nuvens nacionais e pilha do Azure.

Este guia resume soluções para problemas comuns vistos no Explorador de armazenamento.

## <a name="error-self-signed-certificate-in-certificate-chain-and-similar-errors"></a>Erro: O certificado Autoassinado na cadeia de certificados (e erros semelhantes)

Erros de certificado são causados por uma das duas seguintes situações:

1. A aplicação está ligada através de "proxy transparente", que significa um servidor (por exemplo, o servidor da empresa) é a intercetar tráfego HTTPS, desencriptação-lo e, em seguida, encriptá-la através de um certificado autoassinado.
2. Está a executar uma aplicação que é inserirem-se um certificado SSL autoassinado para as mensagens HTTPS recebidos. Exemplos de aplicações que injetar certificados inclui o software de inspeção de tráfego de rede e de antivírus.

Quando o Explorador de armazenamento vê um self assinado ou o certificado não fidedigno, pode já não sabe se a mensagem recebida do HTTPS foi alterada. Se tiver uma cópia do certificado autoassinado, pode instruir o Explorador de armazenamento confiar nele efetuando os seguintes passos:

1. Obter uma Base-64 codificado x. 509 (. cer) de cópia do certificado
2. Clique em **editar** > **certificados SSL** > **importar certificados**e, em seguida, utilize o Seletor de ficheiros para localizar, selecione e abra o ficheiro. cer

Este problema também pode ser resultado de vários certificados (de raiz e intermédio). Ambos os certificados têm de ser adicionados a ultrapassar o erro.

Se não souber de onde vem o certificado de, pode tentar estes passos para encontrá-lo:

1. Instalar o Open SSL

    * [Windows](https://slproweb.com/products/Win32OpenSSL.html) (qualquer uma das versões leves deve ser suficiente)
    * Mac e Linux: devem ser incluídos com o sistema operativo
2. Executar o Open SSL

    * Windows: Abra o diretório de instalação, clique em **/bin/** e, em seguida, faça duplo clique **openssl.exe**.
    * Mac e Linux: executar **openssl** de um terminal.
3. Execute `s_client -showcerts -connect microsoft.com:443`
4. Procure certificados autoassinados. Se não souber qual são autoassinados, procure em qualquer lugar assunto `("s:")` e o emissor `("i:")` são os mesmos.
5. Quando encontrar quaisquer certificados autoassinados, para cada um, copie e cole tudo a partir e incluindo **---BEGIN CERTIFICATE---** para **---fim certificado---** para um novo ficheiro. cer.
6. Abra o Explorador de armazenamento, clique em **editar** > **certificados SSL** > **importar certificados**e, em seguida, utilize o Seletor de ficheiros para localizar, selecione e abra os ficheiros. cer que criou.

Se não encontrar quaisquer certificados autoassinados através dos passos anteriores, contacte-nos através da ferramenta de comentários para obter mais ajuda. Em alternativa, pode escolher iniciar o Explorador de armazenamento na linha de comandos com o `--ignore-certificate-errors` sinalizador. Quando é iniciada com este sinalizador, Explorador de armazenamento irão ignorar erros de certificado.

## <a name="sign-in-issues"></a>Problemas de início de sessão

Se não for possível iniciar sessão, tente os seguintes métodos de resolução de problemas:

* Reinicie o Explorador de armazenamento
* Se a janela de autenticação está em branco, aguarde, pelo menos, um minuto antes de fechar a caixa de diálogo de autenticação.
* Certifique-se de que as definições estão configuradas corretamente para o seu computador e o Explorador de armazenamento de certificados de proxy e
* Se estiver no Windows e ter acesso ao Visual Studio 2017 na mesma máquina e início de sessão, experimente iniciar sessão no Visual Studio 2017

Se nenhum dos seguintes métodos de trabalho [abrir um problema no GitHub](https://github.com/Microsoft/AzureStorageExplorer/issues).

## <a name="unable-to-retrieve-subscriptions"></a>Não é possível obter subscrições

Se não for possível obter as suas subscrições após a sessão com êxito, tente os seguintes métodos de resolução de problemas:

* Certifique-se de que a conta tem acesso às subscrições que esperava. Pode verificar se tem acesso ao iniciar sessão portal para o ambiente do Azure que está a tentar utilizar.
* Certifique-se de que tiver iniciado sessão com o Azure correto ambiente (do Azure, Azure China, Datacenters do Azure, Azure US Government ou ambiente de personalizado).
* Se estiver atrás de um proxy, certifique-se de que configurou o proxy do Explorador de armazenamento corretamente.
* Tente remover e readding a conta.
* Veja a consola de ferramentas de programador (ajudar > Ferramentas de Programador de alternar) enquanto o Explorador de armazenamento está a carregar as subscrições. Procure mensagens de erro (texto vermelho) ou qualquer mensagem que contém o texto "não foi possível carregar as subscrições do inquilino." Se vir quaisquer mensagens de concerning [abrir um problema no GitHub](https://github.com/Microsoft/AzureStorageExplorer/issues).

## <a name="cannot-remove-attached-account-or-storage-resource"></a>Não é possível remover o recurso de conta ou o armazenamento ligado

Se não for possível remover uma conta de ligado ou recursos de armazenamento através da IU, pode eliminar manualmente todos os recursos ligados ao eliminar as seguintes pastas:

* Windows: `%AppData%/StorageExplorer`
* macOS: `/Users/<your_name>/Library/Applicaiton Support/StorageExplorer`
* Linux: `~/.config/StorageExplorer`

> [!NOTE]
>  Feche o Explorador de armazenamento antes de eliminar as pastas acima.

> [!NOTE]
>  Se alguma vez tenha importado de todos os certificados SSL, o conteúdo de cópia de segurança a `certs` diretório. Mais tarde, pode utilizar a cópia de segurança para importe novamente os certificados SSL.

## <a name="proxy-issues"></a>Problemas de proxy

Em primeiro lugar, certifique-se de que as seguintes informações que introduziu estão corretos:

* O URL de proxy e o número de porta * nome de utilizador e palavra-passe se for necessário pelo proxy

### <a name="common-solutions"></a>Soluções comuns

Se ainda ocorrerem problemas, tente os seguintes métodos de resolução de problemas:

* Se pode ligar à Internet sem utilizar o proxy, certifique-se de que o Explorador de armazenamento funciona sem as definições de proxy ativadas. Se for este o caso, poderá existir um problema com as definições de proxy. Trabalhar com o seu administrador de proxy para identificar os problemas.
* Certifique-se de que as outras aplicações utilizando o servidor proxy funcionam conforme esperado.
* Verifique se pode ligar para o portal para o ambiente do Azure que está a tentar utilizar
* Certifique-se de que pode receber respostas do seu pontos finais de serviço. Introduza um URL de ponto final no seu browser. Se pode ligar, deverá receber um InvalidQueryParameterValue ou semelhante resposta XML.
* Se também que mais alguém está a utilizar o Explorador de armazenamento com o seu servidor proxy, certifique-se de que possam estabelecer a ligação. Se pode ligar-se, poderá ter de contactar o administrador do servidor proxy.

### <a name="tools-for-diagnosing-issues"></a>Ferramentas para diagnosticar problemas

Se tiver de ferramentas de rede, como o Fiddler para Windows, poderá diagnosticar problemas da seguinte forma:

* Se tiver de funcionar através do proxy, poderá ter de configurar a ferramenta de rede para ligar através do proxy.
* Verifique o número da porta utilizado pela ferramenta de sua rede.
* Introduza o URL de anfitrião local e o número de porta a ferramenta de rede, como definições de proxy no Explorador de armazenamento. Quando efetuadas corretamente, a ferramenta de rede inicia o registo de pedidos de rede efetuados pelo Explorador de armazenamento, gestão e pontos finais de serviço. Por exemplo, introduza https://cawablobgrs.blob.core.windows.net/ para o ponto final do blob no browser e, irá receber uma resposta é semelhante ao seguinte, o que sugere o recurso existe, apesar de não é possível aceder-lhe.

![exemplo de código](./media/storage-explorer-troubleshooting/4022502_en_2.png)

### <a name="contact-proxy-server-admin"></a>Contacte o administrador do servidor proxy

Se as definições de proxy estão corretas, poderá ter de contactar o seu administrador de servidor proxy, e

* Certifique-se de que o proxy não bloqueia o tráfego para pontos finais do Azure de gestão ou recurso.
* Certifique-se o protocolo de autenticação utilizado pelo seu servidor proxy. Explorador de armazenamento não suporta atualmente os proxies NTLM.

## <a name="unable-to-retrieve-children-error-message"></a>Mensagem de erro "Não é possível obter subordinados"

Se estiver ligado ao Azure através de um proxy, certifique-se de que as definições de proxy estão corretas. Se foi concedido acesso a um recurso do proprietário da subscrição ou conta, certifique-se de que leu ou lista de permissões para esse recurso.

### <a name="issues-with-sas-url"></a>Problemas com o SAS URL
Se estiver a ligar a um serviço utilizando um URL SAS e estão a ocorrer este erro:

* Certifique-se de que o URL fornece as permissões necessárias para ler ou lista de recursos.
* Certifique-se de que o URL não expirou.
* Se o URL de SAS baseiam-se uma política de acesso, certifique-se de que a política de acesso não foi revogada.

Se acidentalmente anexados utilizando um URL SAS inválido e não é possível anular a exposição, siga estes passos:
1.  Ao executar o Explorador de armazenamento, premir a tecla F12 para abrir a janela de ferramentas de programador.
2.  Clique no separador de aplicação, em seguida, clique em armazenamento Local > file:// na árvore à esquerda.
3.  Localize a chave associada com o tipo de serviço do URI de SAS problemáticas. Por exemplo, se o incorretos URI de SAS para um contentor de blob, procure a chave com o nome `StorageExplorer_AddStorageServiceSAS_v1_blob`.
4.  O valor da chave deve ser uma matriz JSON. Localizar o objeto associado ao URI incorreto e removê-lo.
5.  Prima Ctrl + R recarregar o Explorador de armazenamento.

## <a name="linux-dependencies"></a>Dependências do Linux

Para Linux distros diferente Ubuntu 16.04, poderá ter de instalar manualmente algumas dependências. Em geral, os pacotes seguintes são necessários:
* [.NET core 2. x](https://docs.microsoft.com/dotnet/core/linux-prerequisites?tabs=netcore2x)
* `libsecret`
* `libgconf-2-4`
* GCC atualizado

Dependendo do seu distro, poderão existir outros pacotes que tem de instalar. O Explorador de armazenamento [notas de versão](https://go.microsoft.com/fwlink/?LinkId=838275&clcid=0x409) contém passos específicos para algumas distros.

## <a name="next-steps"></a>Passos Seguintes

Se nenhuma das soluções de funcionar, em seguida, [abrir um problema no GitHub](https://github.com/Microsoft/AzureStorageExplorer/issues). Pode também rapidamente depara-se ao GitHub utilizando o botão de "Problema de relatório para GitHub" no canto inferior esquerdo.

![Comentários](./media/storage-explorer-troubleshooting/feedback-button.PNG)
