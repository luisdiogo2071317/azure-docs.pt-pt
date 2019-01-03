---
title: Configurar a conectividade SSL na base de dados do Azure para PostgreSQL
description: Instruções e informações para configurar a base de dados do Azure para PostgreSQL e aplicações associadas a corretamente utilizar ligações SSL.
author: JasonMAnderson
ms.author: janders
ms.service: postgresql
ms.topic: conceptual
ms.date: 02/28/2018
ms.openlocfilehash: bcb6f1da78cfccb4914f1f4008609f002a7b8c2d
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/17/2018
ms.locfileid: "53545438"
---
# <a name="configure-ssl-connectivity-in-azure-database-for-postgresql"></a>Configurar a conectividade SSL na base de dados do Azure para PostgreSQL
Base de dados do Azure para PostgreSQL prefere ligar as suas aplicações de cliente para o serviço de PostgreSQL utilizando Secure Sockets Layer (SSL). A imposição de ligações SSL entre o servidor de base de dados e as aplicações de cliente ajuda a proteger contra ataques "man-in-the-middle" ao encriptar o fluxo de dados entre o servidor e a sua aplicação.

Por predefinição, o serviço de base de dados PostgreSQL está configurado para exigir conexão SSL. Opcionalmente, pode desativar a exigência de SSL para ligar ao seu serviço de base de dados, se a aplicação de cliente não suporta a conectividade SSL. 

## <a name="enforcing-ssl-connections"></a>A imposição de ligações de SSL
Para todas as base de dados para servidores PostgreSQL aprovisionados através do portal do Azure e CLI, imposição de ligações SSL está ativada por predefinição. 

Da mesma forma, as cadeias de ligação previamente definidas nas definições de "Cadeias de ligação" no seu servidor no portal do Azure incluem os parâmetros necessários para idiomas comuns ao ligar ao seu servidor de base de dados através de SSL. O parâmetro SSL varia com base no conector, por exemplo "ssl = true" ou "sslmode = exigir" ou "sslmode = necessária" e outras variações.

## <a name="configure-enforcement-of-ssl"></a>Configurar a imposição de SSL
Opcionalmente, pode desativar a conectividade a imposição de SSL. Microsoft Azure recomenda a ativação da sempre **ligação impor SSL** definição para uma maior segurança.

### <a name="using-the-azure-portal"></a>Utilizar o portal do Azure
Visite a sua base de dados do Azure para o servidor PostgreSQL e clique em **segurança de ligação**. Utilize o botão de alternar para ativar ou desativar a **ligação impor SSL** definição. Em seguida, clique em **guardar**. 

![Segurança da ligação - desativar impor SSL](./media/concepts-ssl-connection-security/1-disable-ssl.png)

Pode confirmar a definição visualizando os **descrição geral** página para ver a **estado de imposição do SSL** indicador.

### <a name="using-azure-cli"></a>Utilizar a CLI do Azure
Pode ativar ou desativar a **imposição de ssl** usando o parâmetro `Enabled` ou `Disabled` valores respectivamente na CLI do Azure.

```azurecli
az postgres server update --resource-group myresourcegroup --name mydemoserver --ssl-enforcement Enabled
```

## <a name="ensure-your-application-or-framework-supports-ssl-connections"></a>Certifique-se seu aplicativo ou framework suporta ligações SSL
Muitas estruturas de aplicações comuns que utilizam o PostgreSQL para seus serviços de base de dados, como o Drupal e Django, não ative o SSL por padrão durante a instalação. Ativar a conectividade SSL deve ser feito após a instalação ou através de comandos da CLI específicos ao aplicativo. Se o servidor PostgreSQL é a imposição de ligações de SSL e o aplicativo associado não está configurado corretamente, a aplicação não poderá ligar ao seu servidor de base de dados. Consulte a documentação da sua aplicação para saber como ativar ligações SSL.


## <a name="applications-that-require-certificate-verification-for-ssl-connectivity"></a>Aplicações que necessitam de verificação de certificado para a conectividade SSL
Em alguns casos, os aplicativos exigem um arquivo de certificados local gerado a partir de um ficheiro de certificado autoridade de certificado (AC) de fidedigna (. cer) para ligar em segurança. Consulte os seguintes passos para obter o ficheiro. cer, decodificar o certificado e vinculá-lo à sua aplicação.

### <a name="download-the-certificate-file-from-the-certificate-authority-ca"></a>Transfira o ficheiro de certificado do certificado de autoridade (CA) 
O certificado necessário para se comunicar através de SSL com a base de dados do Azure para PostgreSQL server está localizado [aqui](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt). Transfira o ficheiro de certificado localmente.

### <a name="download-and-install-openssl-on-your-machine"></a>Transferir e instalar o OpenSSL no seu computador 
Para descodificar o ficheiro de certificado necessário para a sua aplicação ligar em segurança ao seu servidor de base de dados, terá de instalar o OpenSSL no seu computador local.

#### <a name="for-linux-os-x-or-unix"></a>Para Linux, OS X ou Unix
As bibliotecas de OpenSSL são fornecidas no código-fonte diretamente a partir da [OpenSSL Software Foundation](https://www.openssl.org). As instruções seguintes guiá-lo pelos passos necessários para instalar o OpenSSL em seu computador Linux. Este artigo utiliza comandos conhecidos para trabalhar em Ubuntu 12.04 e superiores.

Abra uma sessão de terminal e transferir OpenSSL.
```bash
wget http://www.openssl.org/source/openssl-1.1.0e.tar.gz
``` 
Extraia os ficheiros do pacote transferido.
```bash
tar -xvzf openssl-1.1.0e.tar.gz
```
Introduza o diretório onde os arquivos foram extraídos. Por padrão, ele deve ser o seguinte.

```bash
cd openssl-1.1.0e
```
Configure OpenSSL, executando o seguinte comando. Se pretender que os ficheiros numa pasta diferente de /usr/local/openssl, certifique-se de alterar as seguintes opções conforme apropriado.

```bash
./config --prefix=/usr/local/openssl --openssldir=/usr/local/openssl
```
Agora que OpenSSL está configurada corretamente, terá de compilá-lo para converter o seu certificado. Para compilar, execute o seguinte comando:

```bash
make
```
Após a conclusão da compilação, está pronto para instalar o OpenSSL como um executável, executando o seguinte comando:
```bash
make install
```
Para confirmar que instalou com êxito OpenSSL no seu sistema, execute o seguinte comando e a verificação para se certificar de que obtém o mesmo resultado.

```bash
/usr/local/openssl/bin/openssl version
```
Se tiver êxito deverá ver a mensagem seguinte.
```bash
OpenSSL 1.1.0e 7 Apr 2014
```

#### <a name="for-windows"></a>Para Windows
Instalar o OpenSSL num PC Windows pode ser feito da seguinte maneira:
1. **(Recomendado)**  Usando a funcionalidade interna do Bash para o Windows no Windows 10 e superior, OpenSSL é instalado por predefinição. Pode encontrar instruções sobre como ativar a funcionalidade Bash para o Windows no Windows 10 [aqui](https://msdn.microsoft.com/commandline/wsl/install_guide).
2. Por meio de transferir uma aplicação de Win32/64 fornecida pela Comunidade. Enquanto o Software de OpenSSL Foundation não fornecem nem apoia qualquer determinados instaladores do Windows, eles fornecem uma lista de programas de instalação disponíveis [aqui](https://wiki.openssl.org/index.php/Binaries).

### <a name="decode-your-certificate-file"></a>Descodificar o ficheiro de certificado
O ficheiro transferido de AC de raiz está em formato encriptado. Utilize OpenSSL para descodificar o ficheiro de certificado. Para tal, execute este comando de OpenSSL:

```dos
openssl x509 -inform DER -in BaltimoreCyberTrustRoot.crt -text -out root.crt
```

### <a name="connecting-to-azure-database-for-postgresql-with-ssl-certificate-authentication"></a>Ligar à base de dados do Azure para PostgreSQL com a autenticação de certificado SSL
Agora que tem com êxito decodificada seu certificado, pode agora ligar ao seu servidor de base de dados em segurança através de SSL. Para permitir que a verificação de certificado do servidor, o certificado tem de ser colocado em ~/.postgresql/root.crt o ficheiro no diretório raiz do utilizador. (No Microsoft Windows o ficheiro com o nome % APPDATA%\postgresql\root.crt.). A seguir são fornecidas instruções para ligar à base de dados do Azure para PostgreSQL.

#### <a name="using-psql-command-line-utility"></a>Usando o utilitário de linha de comandos psql
O exemplo seguinte mostra como ligar com êxito para o servidor PostgreSQL com o utilitário de linha de comandos psql. Utilize o `root.crt` arquivo criado e o `sslmode=verify-ca` ou `sslmode=verify-full` opção.

Usando a interface de linha de comandos do PostgreSQL, execute o seguinte comando:
```bash
psql "sslmode=verify-ca sslrootcert=root.crt host=mydemoserver.postgres.database.azure.com dbname=postgres user=mylogin@mydemoserver"
```
Se tiver êxito, receberá a seguinte saída:
```bash
Password for user mylogin@mydemoserver:
psql (9.6.2)
WARNING: Console code page (437) differs from Windows code page (1252)
     8-bit characters might not work correctly. See psql reference
     page "Notes for Windows users" for details.
SSL connection (protocol: TLSv1.2, cipher: ECDHE-RSA-AES256-SHA384, bits: 256, compression: off)
Type "help" for help.

postgres=>
```

#### <a name="using-pgadmin-gui-tool"></a>Usando a ferramenta pgAdmin GUI
Configurar a 4 para ligar de forma segura através de SSL de pgAdmin requer que defina os `SSL mode = Verify-CA` ou `SSL mode = Verify-Full` da seguinte forma:

![Captura de ecrã do modo SSL de pgAdmin - connection - exigir](./media/concepts-ssl-connection-security/2-pgadmin-ssl.png)

## <a name="next-steps"></a>Passos Seguintes
Reveja as várias opções de conectividade de aplicativo seguindo [bibliotecas de ligação para base de dados do Azure para PostgreSQL](concepts-connection-libraries.md).
