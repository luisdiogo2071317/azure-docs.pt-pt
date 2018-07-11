---
title: Como utilizar o PowerShell para criar certificados X.509 | Documentos da Microsoft
description: Como utilizar o PowerShell para criar certificados X.509 localmente e ativar o X.509 com a base de segurança do seu hub IoT do Azure num ambiente simulado.
author: dsk-2015
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: dkshir
ms.openlocfilehash: d0063ff79a0bda88fffb486f03286f6784ece7fa
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/10/2018
ms.locfileid: "34637604"
---
# <a name="powershell-scripts-to-manage-ca-signed-x509-certificates"></a>Scripts do PowerShell para gerir os certificados X.509 assinado pela AC

A segurança baseada em certificados X.509 no IoT Hub, tem de começar com uma [cadeia de certificados X.509](https://en.wikipedia.org/wiki/X.509#Certificate_chains_and_cross-certification), que inclui o certificado de raiz, bem como quaisquer certificados intermédios até o certificado de folha. Isso *como* guia o orienta através de scripts do PowerShell de exemplo que utilizam [OpenSSL](https://www.openssl.org/) para criar e assinar os certificados X.509. Recomendamos que utilize este guia para a experimentação apenas, uma vez que muitos destes passos irão acontecer durante o processo no mundo real de fabrico. Pode utilizar estes certificados para simular a segurança no seu hub IoT do Azure com o *autenticação de certificados X.509*. Os passos neste guia criam certificados localmente no seu computador Windows. 

## <a name="prerequisites"></a>Pré-requisitos
Este tutorial parte do princípio de que tiver adquirido os binários de OpenSSL. Pode optar por
    - Baixe o código-fonte OpenSSL e criar os binários no seu computador, ou 
    - Transfira e instale qualquer [binários de OpenSSL de terceiros](https://wiki.openssl.org/index.php/Binaries), por exemplo, do [neste projeto no SourceForge](https://sourceforge.net/projects/openssl/).

<a id="createcerts"></a>

## <a name="create-x509-certificates"></a>Criar certificados X.509
Os passos seguintes mostram um exemplo de como criar os certificados de raiz X.509 localmente. 

1. Abra uma janela do PowerShell como um *administrador*.  
   **Nota:** tem de abrir isto no PowerShell em si, não ISE do PowerShell, Visual Studio Code ou outras ferramentas que encapsulam a consola do PowerShell subjacente.  PowerShell com base através de uma consola não irá resultar em `openssl` comandos abaixo.

2. Navegue para o seu diretório de trabalho. Execute o seguinte script para definir as variáveis globais. 
    ```PowerShell
    $openSSLBinSource = "<full_path_to_the_binaries>\OpenSSL\bin"
    $errorActionPreference    = "stop"

    # Note that these values are for test purpose only
    $_rootCertCommonName      = "Azure IoT Root CA"
    $_rootCertSubject         = "CN=$_rootCertCommonName"
    $_intermediateCertSubject = "Azure IoT Intermediate {0} CA"
    $_privateKeyPassword      = "123"

    $rootCACerFileName          = "./RootCA.cer"
    $rootCAPemFileName          = "./RootCA.pem"
    $intermediate1CAPemFileName = "./Intermediate1.pem"
    $intermediate2CAPemFileName = "./Intermediate2.pem"
    $intermediate3CAPemFileName = "./Intermediate3.pem"

    $openSSLBinDir              = Join-Path $ENV:TEMP "openssl-bin"

    # Whether to use ECC or RSA.
    $useEcc                     = $true
    ```
3. Execute o seguinte script, que copia os binários de OpenSSL para seu diretório de trabalho e define as variáveis de ambiente:

    ```PowerShell
    function Initialize-CAOpenSSL()
    {
        Write-Host ("Beginning copy of openssl binaries to {0} (and setting up env variables...)" -f $openSSLBinDir)
        if (-not (Test-Path $openSSLBinDir))
        {
            mkdir $openSSLBinDir | Out-Null
        }

        robocopy $openSSLBinSource $openSSLBinDir * /s 
        robocopy $openSSLBinSource . * /s 

        Write-Host "Setting up PATH and other environment variables."
        $ENV:PATH += "; $openSSLBinDir"
        $ENV:OPENSSL_CONF = Join-Path $openSSLBinDir "openssl.cnf"

        Write-Host "Success"
    }
    Initialize-CAOpenSSL
    ```
4. Em seguida, execute o seguinte script que procura se um certificado especificado pelo *nome do requerente* já estiver instalado, e se OpenSSL está configurado corretamente no seu computador:
    ```PowerShell
    function Get-CACertBySubjectName([string]$subjectName)
    {
        $certificates = gci -Recurse Cert:\LocalMachine\ |? { $_.gettype().name -eq "X509Certificate2" }
        $cert = $certificates |? { $_.subject -eq $subjectName -and $_.PSParentPath -eq "Microsoft.PowerShell.Security\Certificate::LocalMachine\My" }
        if ($NULL -eq $cert)
        {
            throw ("Unable to find certificate with subjectName {0}" -f $subjectName)
        }
    
        write $cert
    }
    function Test-CAPrerequisites()
    {
        $certInstalled = $null
        try
        {
            $certInstalled = Get-CACertBySubjectName $_rootCertSubject
        }
        catch {}

        if ($NULL -ne $certInstalled)
        {
            throw ("Certificate {0} already installed.  Cleanup CA certs 1st" -f $_rootCertSubject)
        }

        if ($NULL -eq $ENV:OPENSSL_CONF)
        {
            throw ("OpenSSL not configured on this system.  Run 'Initialize-CAOpenSSL' (even if you've already done so) to set everything up.")
        }
        Write-Host "Success"
    }
    Test-CAPrerequisites
    ```
    Se tudo está configurada corretamente, deverá ver "Êxito" mensagem. 

<a id="createcertchain"></a>

## <a name="create-x509-certificate-chain"></a>Criar cadeia de certificados X.509
Criar uma cadeia de certificados com uma raiz de AC, por exemplo, "CN = AC de raiz de IoT do Azure" que este exemplo utiliza, executando o seguinte script do PowerShell. Este script também atualiza o arquivo de certificados de SO Windows, também cria ficheiros de certificado no seu diretório de trabalho. 
    1. O seguinte script cria uma função do PowerShell para criar um certificado autoassinado, para um determinado *nome do requerente* e a autoridade de assinatura. 
    ```PowerShell
    function New-CASelfsignedCertificate([string]$commonName, [object]$signingCert, [bool]$isASigner=$true)
    {
        # Build up argument list
        $selfSignedArgs =@{"-DnsName"=$commonName; 
                           "-CertStoreLocation"="cert:\LocalMachine\My";
                           "-NotAfter"=(get-date).AddDays(30); 
                          }

        if ($isASigner -eq $true)
        {
            $selfSignedArgs += @{"-KeyUsage"="CertSign"; }
            $selfSignedArgs += @{"-TextExtension"= @(("2.5.29.19={text}ca=TRUE&pathlength=12")); }
        }
        else
        {
            $selfSignedArgs += @{"-TextExtension"= @("2.5.29.37={text}1.3.6.1.5.5.7.3.2,1.3.6.1.5.5.7.3.1", "2.5.29.19={text}ca=FALSE&pathlength=0")  }
        }

        if ($signingCert -ne $null)
        {
            $selfSignedArgs += @{"-Signer"=$signingCert }
        }

        if ($useEcc -eq $true)
        {
            $selfSignedArgs += @{"-KeyAlgorithm"="ECDSA_nistP256";
                             "-CurveExport"="CurveName" }
        }

        # Now use splatting to process this
        Write-Host ("Generating certificate {0} which is for prototyping, NOT PRODUCTION.  It will expire in 30 days." -f $subjectName)
        write (New-SelfSignedCertificate @selfSignedArgs)
    }
    ``` 
    2. A seguinte função do PowerShell cria certificados X.509 intermediários, usando a função anterior, bem como os binários de OpenSSL. 
    ```PowerShell
    function New-CAIntermediateCert([string]$commonName, [Microsoft.CertificateServices.Commands.Certificate]$signingCert, [string]$pemFileName)
    {
        $certFileName = ($commonName + ".cer")
        $newCert = New-CASelfsignedCertificate $commonName $signingCert
        Export-Certificate -Cert $newCert -FilePath $certFileName -Type CERT | Out-Null
        Import-Certificate -CertStoreLocation "cert:\LocalMachine\CA" -FilePath $certFileName | Out-Null

        # Store public PEM for later chaining
        openssl x509 -inform deer -in $certFileName -out $pemFileName

        del $certFileName
   
        write $newCert
    }  
    ```
    3. A seguinte função do PowerShell cria a cadeia de certificados X.509. Leia [cadeias de certificados](https://en.wikipedia.org/wiki/X.509#Certificate_chains_and_cross-certification) para obter mais informações.
    ```PowerShell
    function New-CACertChain()
    {
        Write-Host "Beginning to install certificate chain to your LocalMachine\My store"
        $rootCACert =  New-CASelfsignedCertificate $_rootCertSubject $null
    
        Export-Certificate -Cert $rootCACert -FilePath $rootCACerFileName  -Type CERT
        Import-Certificate -CertStoreLocation "cert:\LocalMachine\Root" -FilePath $rootCACerFileName

        openssl x509 -inform der -in $rootCACerFileName -out $rootCAPemFileName

        $intermediateCert1 = New-CAIntermediateCert ($_intermediateCertSubject -f "1") $rootCACert $intermediate1CAPemFileName
        $intermediateCert2 = New-CAIntermediateCert ($_intermediateCertSubject -f "2") $intermediateCert1 $intermediate2CAPemFileName
        $intermediateCert3 = New-CAIntermediateCert ($_intermediateCertSubject -f "3") $intermediateCert2 $intermediate3CAPemFileName
        Write-Host "Success"
    }    
    ```
    Este script cria um ficheiro denominado *RootCA.cer* no diretório de trabalho. 
    4. Por último, utilize as funções de PowerShell acima para criar a cadeia de certificados X.509, ao executar o comando `New-CACertChain` na janela do PowerShell. 


<a id="signverificationcode"></a>

## <a name="proof-of-possession-of-your-x509-ca-certificate"></a>Prova de posse do seu certificado X.509 de AC

Este script executa o *uma prova de posse* fluxo para o seu certificado X.509. 

Na janela do PowerShell no seu ambiente de trabalho, execute o seguinte código:
   
   ```PowerShell
   function New-CAVerificationCert([string]$requestedSubjectName)
   {
       $verifyRequestedFileName = ".\verifyCert4.cer"
       $rootCACert = Get-CACertBySubjectName $_rootCertSubject
       Write-Host "Using Signing Cert:::" 
       Write-Host $rootCACert
   
       $verifyCert = New-CASelfsignedCertificate $requestedSubjectName $rootCACert $false

       Export-Certificate -cert $verifyCert -filePath $verifyRequestedFileName -Type Cert
       if (-not (Test-Path $verifyRequestedFileName))
       {
           throw ("Error: CERT file {0} doesn't exist" -f $verifyRequestedFileName)
       }
   
       Write-Host ("Certificate with subject {0} has been output to {1}" -f $requestedSubjectName, (Join-Path (get-location).path $verifyRequestedFileName)) 
   }
   New-CAVerificationCert "<your verification code>"
   ```

Este código cria um certificado com o nome de requerente determinado, assinado pela AC, como um ficheiro denominado *VerifyCert4.cer* no diretório de trabalho. Este ficheiro de certificado irá ajudar a validar com o seu hub IoT que tem a permissão de assinatura (ou seja, a chave privada) desta AC.


<a id="createx509device"></a>

## <a name="create-leaf-x509-certificate-for-your-device"></a>Criar certificado X.509 de folha para o seu dispositivo

Esta secção mostra que pode utilizar um script do PowerShell que cria um certificado de folha de dispositivo e a cadeia de certificados correspondentes. 

Na janela do PowerShell no seu computador local, execute o seguinte script para criar um certificado X.509 assinado pela AC para este dispositivo:

   ```PowerShell
   function New-CADevice([string]$deviceName, [string]$signingCertSubject=$_rootCertSubject)
   {
       $newDevicePfxFileName = ("./{0}.pfx" -f $deviceName)
       $newDevicePemAllFileName      = ("./{0}-all.pem" -f $deviceName)
       $newDevicePemPrivateFileName  = ("./{0}-private.pem" -f $deviceName)
       $newDevicePemPublicFileName   = ("./{0}-public.pem" -f $deviceName)
   
       $signingCert = Get-CACertBySubjectName $signingCertSubject ## "CN=Azure IoT CA Intermediate 1 CA"

       $newDeviceCertPfx = New-CASelfSignedCertificate $deviceName $signingCert $false
   
       $certSecureStringPwd = ConvertTo-SecureString -String $_privateKeyPassword -Force -AsPlainText

       # Export the PFX of the cert we've just created.  The PFX is a format that contains both public and private keys.
       Export-PFXCertificate -cert $newDeviceCertPfx -filePath $newDevicePfxFileName -password $certSecureStringPwd
       if (-not (Test-Path $newDevicePfxFileName))
       {
           throw ("Error: CERT file {0} doesn't exist" -f $newDevicePfxFileName)
       }

       # Begin the massaging.  First, turn the PFX into a PEM file which contains public key, private key, and other attributes.
       Write-Host ("When prompted for password by openssl, enter the password as {0}" -f $_privateKeyPassword)
       openssl pkcs12 -in $newDevicePfxFileName -out $newDevicePemAllFileName -nodes

       # Convert the PEM to get formats we can process
       if ($useEcc -eq $true)
       {
           openssl ec -in $newDevicePemAllFileName -out $newDevicePemPrivateFileName
       }
       else
       {
           openssl rsa -in $newDevicePemAllFileName -out $newDevicePemPrivateFileName
       }
       openssl x509 -in $newDevicePemAllFileName -out $newDevicePemPublicFileName
 
       Write-Host ("Certificate with subject {0} has been output to {1}" -f $cnNewDeviceSubjectName, (Join-Path (get-location).path $newDevicePemPublicFileName)) 
   }
   ```

Em seguida, execute `New-CADevice "<yourTestDevice>"` na janela do PowerShell, utilizando o nome amigável que utilizou para criar o seu dispositivo. Quando lhe for pedido para a palavra-passe para a chave privada da AC, introduza "123". Esta ação cria um  _<yourTestDevice>. pfx_ ficheiro no diretório de trabalho.

## <a name="clean-up-certificates"></a>Limpar os certificados

Na sua barra de início ou **configurações** aplicação, procure e selecione **gerir os certificados de computador**. Remover quaisquer certificados emitidos pela * * do Azure IoT AC TestOnly ***. Estes certificados devem existir nos seguintes três locais: 

* Certificados - Computador Local > pessoal > certificados
* Certificados - Computador Local > autoridades de certificação de raiz fidedigna > certificados
* Certificados - Computador Local > autoridades de certificação intermediárias > certificados

   ![Remover certificados de AC TestOnly do Azure IoT](./media/iot-hub-security-x509-create-certificates/cleanup.png)
