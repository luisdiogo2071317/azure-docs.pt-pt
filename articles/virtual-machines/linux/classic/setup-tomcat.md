---
title: Configurar o Apache Tomcat numa máquina virtual Linux | Documentos da Microsoft
description: Saiba como configurar o Apache Tomcat7 através da utilização de máquinas virtuais do Azure em execução no Linux.
services: virtual-machines-linux
documentationcenter: ''
author: NingKuang
manager: jeconnoc
editor: ''
tags: azure-service-management
ms.assetid: 45ecc89c-1cb0-4e80-8944-bd0d0bbedfdc
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 12/15/2015
ms.author: ningk
ms.openlocfilehash: 8c04c9fffbb85bb4db7a369b0dbbad6279f5d6f6
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/31/2018
ms.locfileid: "50420086"
---
# <a name="set-up-tomcat7-on-a-linux-virtual-machine-with-azure"></a>Configure o tomcat7 numa máquina virtual Linux com o Azure
Apache Tomcat (ou simplesmente Tomcat, também anteriormente denominado Tomcat Jacarta) é um servidor de web de código-fonte aberto e um contentor de servlet desenvolvidos pelo Apache Software Foundation (ASF). Tomcat implementa o Servlet Java e as especificações de páginas JavaServer (JSP), na Sun Microsystems. Tomcat fornece um ambiente de servidor de web de Java HTTP puro para executar código Java. A configuração mais simples, Tomcat é executado num processo único sistema operacional. Este processo é executado uma máquina virtual de Java (JVM). Cada solicitação de HTTP a partir de um browser para Tomcat é processada como um thread separado no processo de Tomcat.  

> [!IMPORTANT]
> O Azure tem dois modelos de implementação diferentes para criar e trabalhar com recursos: [do Azure Resource Manager e clássica](../../../resource-manager-deployment-model.md). Este artigo aborda como usar o modelo de implementação clássica. Recomendamos que as implementações mais novas utilizem o modelo de Gestor de recursos. Para utilizar um modelo do Resource Manager para implementar uma VM do Ubuntu com Open JDK e Tomcat, veja [este artigo](https://azure.microsoft.com/documentation/templates/openjdk-tomcat-ubuntu-vm/).
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

Neste artigo, instalará o Tomcat7 numa imagem do Linux e implementá-la no Azure.  

Aprenderá:  

* Como criar uma máquina virtual no Azure.
* Como preparar a máquina virtual para Tomcat7.
* Como instalar Tomcat7.

Presume que já tem uma subscrição do Azure.  Se não, pode inscrever-se numa avaliação gratuita em [Web site do Azure](https://azure.microsoft.com/). Se tiver uma assinatura do MSDN, consulte [preços especiais do Microsoft Azure: MSDN, MPN e benefícios do BizSpark](https://azure.microsoft.com/pricing/member-offers/msdn-benefits/?c=14-39). Para saber mais sobre o Azure, veja [o que é o Azure?](https://azure.microsoft.com/overview/what-is-azure/).

Este artigo pressupõe que tem um conhecimento funcional básico do Tomcat e Linux.  

## <a name="phase-1-create-an-image"></a>Fase 1: Criar uma imagem
Nesta fase, irá criar uma máquina virtual utilizando uma imagem do Linux no Azure.  

### <a name="step-1-generate-an-ssh-authentication-key"></a>Passo 1: Gerar uma chave de autenticação SSH
O SSH é uma ferramenta importante para os administradores de sistema. No entanto, a configurar a segurança de acesso com base numa palavra-passe determinado por humanos não é recomendado. Utilizadores mal intencionados podem dividir em seu sistema com base num nome de utilizador e uma palavra-passe fraca.

A boa notícia é que existe uma forma de deixar o acesso remoto abertas e não se preocupar sobre palavras-passe. Este método é composta por autenticação com a criptografia assimétrica. Chave particular do usuário é o que concede a autenticação. Ainda pode bloquear a conta de utilizador para não permitir a autenticação de palavra-passe.

Outra vantagem deste método é que não precisa palavras-passe diferentes para iniciar sessão em servidores diferentes. Pode autenticar ao utilizar a chave privada pessoa em todos os servidores, que evita a necessidade de se lembrar de várias senhas.



Siga estes passos para gerar a chave de autenticação SSH.

1. Transfira e instale o PuTTYgen da seguinte localização: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)
2. Execute Puttygen.exe.
3. Clique em **gerar** para gerar as chaves. No processo, pode aumentar a aleatoriedade, movendo o mouse sobre a área em branco na janela.  
   ![Captura de ecrã gerador de chave puTTY que mostra a nova chave botão gerar][1]
4. Depois do processo de gerar, Puttygen.exe mostrará a nova chave pública.  
   ![Captura de ecrã gerador de chave puTTY que mostra a nova chave pública e salvar botão de chave privada][2]
5. Selecione e copie a chave pública e guarde-o num ficheiro denominado publicKey.pem. Não clique **Guardar chave pública**, porque o formato de ficheiro da chave pública guardado é diferente da chave pública que queremos.
6. Clique em **Guardar chave privada**e guarde-o num ficheiro denominado privateKey.ppk.

### <a name="step-2-create-the-image-in-the-azure-portal"></a>Passo 2: Criar a imagem no portal do Azure
1. Na [portal](https://portal.azure.com/), clique em **criar um recurso** na barra de tarefas para criar uma imagem. Em seguida, selecione a imagem do Linux com base nas suas necessidades. O exemplo seguinte utiliza a imagem de Ubuntu 14.04.
![Captura de ecrã do portal que mostra o botão novo][3]

2. Para **nome de anfitrião**, especifique o nome para o URL que e os clientes de Internet, irão utilizar para aceder a esta máquina virtual. Defina a última parte do nome DNS, por exemplo, tomcatdemo. Azure, em seguida, irá gerar o URL como tomcatdemo.cloudapp.net.  

3. Para **chave de autenticação SSH**, copie o valor da chave do ficheiro publicKey.pem, que contém a chave pública gerada pelo PuTTYgen.  
![Caixa de chave de autenticação SSH no portal][4]

4. Configure outras definições conforme necessário e, em seguida, clique em **criar**.  

## <a name="phase-2-prepare-your-virtual-machine-for-tomcat7"></a>Fase 2: Preparar a sua máquina virtual para Tomcat7
Nesta fase, irá configurar um ponto final para tráfego de Tomcat e, em seguida, ligar a sua nova máquina virtual.

### <a name="step-1-open-the-http-port-to-allow-web-access"></a>Passo 1: Abra a porta HTTP para permitir o acesso web
Pontos finais no Azure são compostos por um protocolo TCP ou UDP, juntamente com uma porta pública e privada. A porta privada é a porta que o serviço está escutando para a máquina virtual. A porta pública é a porta que o serviço cloud do Azure escuta externamente para o tráfego de entrada, baseados na Internet.  

Porta TCP 8080 é o número de porta predefinido que Tomcat utiliza para escutar. Se esta porta é aberta com um ponto final do Azure, e a outros clientes de Internet podem aceder a páginas do Tomcat.  

1. No portal, clique em **navegue** > **máquinas virtuais**e, em seguida, clique na máquina de virtual que criou.  
   ![Captura de ecrã do diretório de máquinas virtuais][5]
2. Para adicionar um ponto final à máquina virtual, clique nas **pontos de extremidade** caixa.
   ![Captura de ecrã que mostra a caixa de pontos finais][6]
3. Clique em **Adicionar**.  

   1. Para o ponto final, introduza um nome para o ponto final no **ponto final**e, em seguida, introduza 80 **Porta pública**.  

      Se definido como 80, não precisa de incluir o número de porta no URL que é utilizada para aceder a Tomcat. Por exemplo, http://tomcatdemo.cloudapp.net.    

      Se defini-lo a outro valor, como a 81, terá de adicionar o número de porta para o URL para aceder a Tomcat. Por exemplo, http://tomcatdemo.cloudapp.net:81/.
   2. Introduza 8080 na **porta privada**. Por predefinição, o Tomcat escuta TCP na porta 8080. Se tiver alterado o padrão escutar a porta do Tomcat, atualize **porta privada** para ser o mesmo que o Tomcat escutar a porta.  
      ![Captura de ecrã da interface do Usuário que mostra o comando Add, Porta pública e porta privada][7]
4. Clique em **OK** para adicionar o ponto final à máquina virtual.

### <a name="step-2-connect-to-the-image-you-created"></a>Passo 2: Ligar-se para a imagem criada
Pode escolher uma ferramenta de SSH para ligar à máquina virtual. Neste exemplo, vamos utilizar o PuTTY.  

1. Obtenha o nome DNS da sua máquina virtual a partir do portal.
    1. Clique em **navegue** > **máquinas virtuais**.
    2. Selecione o nome da sua máquina virtual e, em seguida, clique em **propriedades**.
    3. Na **propriedades** mosaico, procure **nome de domínio** caixa para obter o nome DNS.  

2. Obter o número de porta para ligações SSH a partir da **SSH** caixa.  
![Captura de ecrã que mostra o número de porta de ligação de SSH][8]

3. Baixe [PuTTY](http://www.putty.org/).  

4. Depois de baixar, clique no ficheiro executável Putty.exe. Configuração do PuTTY, configure as opções básicas com o nome de anfitrião e o número que é obtido a partir das propriedades da sua máquina virtual porta.   
![Opções de nome e a porta de anfitrião de captura de ecrã que mostra a configuração do PuTTY][9]

5. No painel esquerdo, clique em **conexão** > **SSH** > **Auth**e, em seguida, clique em **procurar** para especificar o localização do ficheiro privateKey.ppk. O ficheiro de privateKey.ppk contém a chave privada que é gerada pelo PuTTYgen anteriormente no "fase 1: criar uma imagem" secção deste artigo.  
![Captura de ecrã que mostra a hierarquia de diretório de conexão e o botão Procurar][10]

6. Clique em **Open** (Abrir). O utilizador poderá ser alertado por uma caixa de mensagem. Se tiver configurado o nome DNS e corretamente o número de porta, clique em **Sim**.
![Captura de ecrã que mostra a notificação][11]

7. For solicitado a digitar seu nome de utilizador.  
![Captura de ecrã que mostra onde se deve introduzir o nome de utilizador][12]

8. Introduza o nome de utilizador que utilizou para criar a máquina virtual no "fase 1: criar uma imagem" descrita anteriormente neste artigo. Verá algo semelhante ao seguinte:  
![Captura de ecrã que mostra a confirmação de autenticação][13]

## <a name="phase-3-install-software"></a>Fase 3: Instalar o software
Nesta fase, instale o ambiente de tempo de execução Java, Tomcat7 e outros componentes de Tomcat7.  

### <a name="java-runtime-environment"></a>Ambiente de tempo de execução de Java
Tomcat é escrito em Java. Ver [JDKs suportados pelo Azure](https://aka.ms/azure-jdks) para obter informações sobre como obter totalmente suportada Java runtimes. Também pode levar seu próprio, mas o resto deste artigo irá utilizar as versões suportados pelo Azure.


#### <a name="install-azure-supported-jdk"></a>Instalar o JDK suportado do Azure

Siga o `apt-get` documentadas de instruções de instalação do [Azul Zulu Enteprise para o Azure](https://www.azul.com/downloads/azure-only/zulu/#apt-repo) Web site.

#### <a name="confirm-that-java-installation-is-successful"></a>Confirme que a instalação de Java é efetuada com êxito
Pode utilizar um comando semelhante ao seguinte para testar se o ambiente de tempo de execução Java está instalado corretamente:  
    Java-versão  

Deverá ver uma mensagem semelhante ao seguinte: ![mensagem de instalação OpenJDK com êxito][14]


### <a name="install-tomcat7"></a>Instalar Tomcat7
Utilize o seguinte comando para instalar Tomcat7.  

    sudo apt-get install tomcat7  

Se não estiver a utilizar o Tomcat7, utilize a variação adequada deste comando.  

#### <a name="confirm-that-tomcat7-installation-is-successful"></a>Confirme que a instalação de Tomcat7 é efetuada com êxito
Para verificar se o Tomcat7 estiver instalado com êxito, navegue para o nome DNS do seu servidor Tomcat. Neste artigo, o URL de exemplo é http://tomcatexample.cloudapp.net/. Se vir uma mensagem semelhante à seguinte, o Tomcat7 está instalado corretamente.
![Mensagem de instalação de Tomcat7 com êxito][16]

### <a name="install-other-tomcat7-components"></a>Instalar outros componentes de Tomcat7
Existem outros componentes de Tomcat opcionais que pode instalar.  

Utilize o **tomcat7 de pesquisa de cache de apt sudo** comando para ver todos os componentes disponíveis. Utilize os seguintes comandos para instalar alguns componentes útil.  

    sudo apt-get install tomcat7-admin      #admin web applications

    sudo apt-get install tomcat7-user         #tools to create user instances  

## <a name="phase-4-configure-tomcat7"></a>Fase 4: Configurar o Tomcat7
Nesta fase, administrar Tomcat.

### <a name="start-and-stop-tomcat7"></a>Iniciar e parar o Tomcat7
O servidor de Tomcat7 inicia automaticamente quando o instalar. Também pode iniciar com o seguinte comando:   

    sudo /etc/init.d/tomcat7 start

Para parar o Tomcat7:

    sudo /etc/init.d/tomcat7 stop

Para ver o estado de Tomcat7:

    sudo /etc/init.d/tomcat7 status

Para reiniciar os serviços de Tomcat: 

    sudo /etc/init.d/tomcat7 restart

### <a name="tomcat7-administration"></a>Administração de Tomcat7
Pode editar o ficheiro de configuração de utilizador do Tomcat para configurar as credenciais de administrador. Utilize o seguinte comando:  

    sudo vi  /etc/tomcat7/tomcat-users.xml   

Segue-se um exemplo:  
![Captura de ecrã que mostra a saída do comando vi sudo][17]  

> [!NOTE]
> Crie uma palavra-passe segura para o nome de utilizador de administrador.  

Depois de editar este ficheiro, deve reiniciar o Tomcat7 serviços com o seguinte comando para se certificar de que as alterações entrem em vigor:  

    sudo /etc/init.d/tomcat7 restart  

Abra o browser e introduza **http://<your tomcat server DNS name>/manager/html** como o URL. Para o exemplo neste artigo, o URL é http://tomcatexample.cloudapp.net/manager/html.  

Depois de ligar, deverá ver algo semelhante ao seguinte:  
![Captura de ecrã do Gestor de aplicação de Web de Tomcat][18]

## <a name="common-issues"></a>Problemas comuns
### <a name="cant-access-the-virtual-machine-with-tomcat-and-moodle-from-the-internet"></a>Não é possível aceder à máquina virtual com Tomcat e Moodle a partir da Internet
#### <a name="symptom"></a>Sintoma  
  Está a executar o Tomcat, mas não é possível ver a página predefinida do Tomcat com o seu browser.
#### <a name="possible-root-cause"></a>Causa possível   

  * A porta de escuta do Tomcat não é o mesmo que a porta privada do ponto final de sua máquina virtual para o tráfego de Tomcat.  

     Verifique a sua porta pública e as definições de ponto final de porta privada e certifique-se de que a porta privada é que o mesmo que o Tomcat escutar a porta. Consulte "fase 1: criar uma imagem" secção deste artigo para obter instruções sobre como configurar pontos finais para a máquina virtual.  

     Para determinar a porta de escuta do Tomcat, abra /etc/httpd/conf/httpd.conf (versão do Red Hat) ou /etc/tomcat7/server.xml (versão Debian). Por predefinição, a porta de escuta do Tomcat é 8080. Segue-se um exemplo:  

        <Connector port="8080" protocol="HTTP/1.1"  connectionTimeout="20000"   URIEncoding="UTF-8"            redirectPort="8443" />  

     Se estiver a utilizar uma máquina virtual como Debian ou Ubuntu e quiser alterar a predefinição porta do Tomcat escutar (por exemplo 8081), deverá também abrir a porta para o sistema operativo. Em primeiro lugar, abra o perfil:  

        sudo vi /etc/default/tomcat7  

     Em seguida, anule os comentários da última linha e altere o "não" para "Sim".  

        AUTHBIND=yes
  2. O firewall desativou a porta de escuta do Tomcat.

     Só pode ver a página predefinida do Tomcat do anfitrião local. O problema é mais provável que a porta, o que está atendendo a solicitações por Tomcat, é bloqueada pela firewall. Pode utilizar a ferramenta de w3m para procurar a página Web. Os seguintes comandos instalar w3m e navegue para a página padrão do Tomcat:  


        sudo yum install w3m w3m img


        w3m http://localhost:8080  
#### <a name="solution"></a>Solução

  * Se o Tomcat escutar porta não é o mesmo que a porta privada do ponto final para o tráfego para a máquina virtual, tem de alterar a porta privada para ser que o mesmo que o Tomcat escutar a porta.   
  2. Se o problema é causado por firewall/iptables, adicione as seguintes linhas para /etc/sysconfig/iptables. A segunda linha só é necessário para tráfego https:  

      -A -p tcp -m tcp – dport 80 -j ACCEPT de entrada

      -A -p tcp -m tcp – dport 443 -j ACCEPT de entrada  

     > [!IMPORTANT]
     > Certifique-se de que as linhas anteriores são posicionadas acima quaisquer linhas que globalmente seriam restringem o acesso, como o seguinte: - A -j REJEITAR – reject-com proibida de anfitrião de icmp de entrada



Recarregar o iptables, execute o seguinte comando:

    service iptables restart

Isto foi testado em CentOS 6.3.

### <a name="permission-denied-when-you-upload-project-files-to-varlibtomcat7webapps"></a>Permissão negada ao carregar arquivos de projeto para /var/lib/tomcat7/webapps /
#### <a name="symptom"></a>Sintoma
  Ao utilizar um cliente SFTP (por exemplo, o FileZilla) para se ligar à máquina virtual e navegue para /var/lib/tomcat7/webapps/para publicar o seu site, receberá uma mensagem de erro semelhante ao seguinte:  

     status:    Listing directory /var/lib/tomcat7/webapps
     Command:    put "C:\Users\liang\Desktop\info.jsp" "info.jsp"
     Error:    /var/lib/tomcat7/webapps/info.jsp: open for write: permission denied
     Error:    File transfer failed
#### <a name="possible-root-cause"></a>Causa possível
  Não ter permissões para aceder à pasta /var/lib/tomcat7/webapps.  
#### <a name="solution"></a>Solução  
  Tem de obter a permissão da conta de raiz. Pode alterar a propriedade de pasta de raiz para o nome de utilizador que utilizou quando aprovisionou a máquina. Eis um exemplo com o nome da conta azureuser:  

     sudo chown azureuser -R /var/lib/tomcat7/webapps

  Utilize a opção -R para aplicar as permissões para todos os ficheiros dentro de um diretório também.  

  Este comando também funciona para diretórios. A opção -R altera as permissões para todos os ficheiros e diretórios dentro do diretório. Segue-se um exemplo:  

     sudo chown -R username:group directory  

  Este comando altera a propriedade (utilizadores e de grupo) para todos os ficheiros e diretórios que estão dentro do diretório.  

  O seguinte comando altera apenas a permissão do diretório de pasta. Os ficheiros e pastas dentro do diretório não são alteradas.  

     sudo chown username:group directory

[1]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-01.png
[2]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-02.png
[3]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-03.png
[4]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-04.png
[5]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-05.png
[6]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-06.png
[7]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-07.png
[8]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-08.png
[9]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-09.png
[10]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-10.png
[11]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-11.png
[12]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-12.png
[13]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-13.png
[14]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-14.png
[15]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-15.png
[16]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-16.png
[17]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-17.png
[18]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-18.png
