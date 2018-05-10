---
title: Remediar problemas de certificado de pilha do Azure | Microsoft Docs
description: Utilize o Verificador de preparação de pilha do Azure para rever e resolver problemas de certificado.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/08/2018
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: 0d2c4d848f861e4e07dbd0de4609344955ca26f7
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
---
# <a name="remediate-common-issues-for-azure-stack-pki-certificates"></a>Resolver problemas comuns para certificados PKI de pilha do Azure
As informações deste artigo podem ajudar a compreender e resolver problemas comuns para certificados PKI de pilha do Azure. Pode detetar problemas ao utilizar a ferramenta do Verificador de preparação de pilha do Azure para [validar os certificados PKI de pilha do Azure](azure-stack-validate-pki-certs.md). A ferramenta verifica para se certificar de que os certificados cumprir os requisitos de PKI de uma implementação de pilha do Azure e a rotação de segredo de pilha do Azure e regista os resultados num [report.json ficheiro](azure-stack-validation-report.md).  

## <a name="read-pfx"></a>Ler PFX
**Aviso** -palavra-passe só protege as informações privadas do certificado.  
**Remediação** -Recomendamos exportar ficheiros PFX com a definição opcional para **ativar privacidade de certificado**.  

**Falha de** -inválido de ficheiro PFX.  
**Remediação** -novamente exportar o certificado utilizando os passos em [certificados PKI de pilha de Azure preparar para implementação](azure-stack-prepare-pki-certs.md).

## <a name="signature-algorithm"></a>Algoritmo de assinatura
**Falha de** -algoritmo de assinatura é SHA1.    
**Remediação** -utilize os passos na geração de pedido para voltar a gerar o certificado de assinatura de pedido (CSR) com o algoritmo de assinatura de SHA256 de assinatura de certificados de pilha do Azure. Em seguida, volte a submeter o CSR à autoridade de certificado a emitir o certificado.

## <a name="private-key"></a>Chave privada
**Falha de** -a chave privada está em falta ou não contém o atributo de máquina Local.  
**Remediação** - a partir do computador que gerou o CSR novamente exportar o certificado através dos passos certificados PKI de pilha de Azure preparar para a implementação. Estes passos incluem a exportação do arquivo de certificados de computador Local.

## <a name="certificate-chain"></a>Cadeia de certificados
**Falha de** -cadeia de certificados não está completa.  
**Remediação** -certificados devem conter uma cadeia de certificados concluída.  A exportar o certificado utilizando os passos no [certificados PKI de pilha de Azure preparar para implementação](azure-stack-prepare-pki-certs.md) e selecione a opção **incluir todos os certificados no caminho de certificação se possível.**

## <a name="dns-names"></a>Nomes DNS
**Falha de** -DNSNameList no certificado não contém o nome de ponto final do serviço de pilha do Azure ou uma correspondência de caráter universal válido.  Correspondências de caráter universal só são válidas para o espaço de nomes mais à esquerda do nome DNS. Por exemplo, _*. region.domain.com_ só é válido para *portal.region.domain.com*, não _*. table.region.domain.com_.  
**Remediação** -utilize os passos na geração de pedido para voltar a gerar CSR com os nomes DNS corretos de assinatura de certificados de pilha do Azure para suportar pontos finais de pilha do Azure. Volte a submeter o CSR para uma autoridade de certificação e, em seguida, siga os passos no [certificados PKI de pilha de Azure preparar para implementação](azure-stack-prepare-pki-certs.md) para exportar o certificado da máquina que gerou o CSR.  

## <a name="key-usage"></a>Utilização de chave
**Falha de** - utilização de chave está em falta a assinatura Digital ou falta cifragem, orEnhanced utilização de chave de autenticação de servidor ou a autenticação de cliente.  
**Remediação** -utilize os passos em [geração de pedido de assinatura de certificados de pilha do Azure](azure-stack-get-pki-certs.md) para voltar a gerar CSR com os atributos de utilização de chave corretos.  Volte a submeter o CSR à autoridade de certificação e confirme que um modelo de certificado não está a substituir a utilização de chave no pedido.

## <a name="key-size"></a>Tamanho da Chave
**Falha de** -tamanho de chave é inferior ao 2048    
**Remediação** -utilize os passos em [geração de pedido de assinatura de certificados de pilha do Azure](azure-stack-get-pki-certs.md) para voltar a gerar CSR com o comprimento de chave correta (2048) e, em seguida, volte a submeter o CSR à autoridade de certificação.

## <a name="chain-order"></a>Ordem de cadeia
**Falha de** -a ordem da cadeia de certificados está incorreta.  
**Remediação** -novamente exportar o certificado utilizando os passos em [certificados PKI de pilha de Azure preparar para implementação](azure-stack-prepare-pki-certs.md) e selecione a opção **incluir todos os certificados no caminho de certificação se possível.** Certifique-se de que apenas o certificado de folha está selecionado para exportação. 

## <a name="other-certificates"></a>Outros certificados
**Falha de** -pacote o PFX contém certificados que não sejam o certificado de folha ou parte da cadeia de certificados.  
**Remediação** -novamente exportar o certificado utilizando os passos em [certificados PKI de pilha de Azure preparar para implementação](azure-stack-prepare-pki-certs.md)e selecione a opção **incluir todos os certificados no caminho de certificação se possível.** Certifique-se de que apenas o certificado de folha está selecionado para exportação.

## <a name="fix-common-packaging-issues"></a>Corrigir problemas de empacotamento comuns
O AzsReadinessChecker pode importar e, em seguida, exportar um ficheiro PFX para corrigir problemas de empacotamento comuns, incluindo: 
 - *Chave privada* faltam atributos máquina Local.
 - *Cadeia de certificados* está incompleta ou errado. (O computador local tem de conter a cadeia de certificados se o pacote PFX não.) 
 - *Outros certificados*.
No entanto, o AzsReadinessChecker não pode ajudá-lo se precisar de gerar um novo CSR e volte a emitir um certificado. 

### <a name="prerequisites"></a>Pré-requisitos
Os seguintes pré-requisitos tem de ser implementado no computador onde a ferramenta é executada: 
 - Windows 10 ou Windows Server 2016, com a conectividade à internet.
 - PowerShell 5.1 ou posterior. Para verificar a sua versão, execute o seguinte cmd do PowerShell e, em seguida, reveja o *principais* versão e *secundárias* versões.

   > `$PSVersionTable.PSVersion`
 - Configurar [PowerShell para o Azure pilha](azure-stack-powershell-install.md). 
 - Transfira a versão mais recente do [Verificador de preparação de pilha do Microsoft Azure](https://aka.ms/AzsReadinessChecker) ferramenta.

### <a name="import-and-export-an-existing-pfx-file"></a>Importar e exportar um ficheiro PFX existente
1. Num computador que cumpra os pré-requisitos, abra uma linha de comandos do PowerShell administrativa e, em seguida, execute o seguinte comando para instalar o AzsReadinessChecker  
   > `Install-Module Microsoft.AzureStack.ReadinessChecker- Force`

2. A partir da linha de comandos PowerShell, execute o seguinte para definir a palavra-passe PFX. Substitua *PFXpassword* com a palavra-passe real. 
   > `$password = Read-Host -Prompt PFXpassword -AsSecureString`

3. A partir da linha de comandos PowerShell, execute o seguinte para exportar um novo ficheiro PFX.
   - Para *- PfxPath*, especifique o caminho para o ficheiro PFX estiver a trabalhar com.  No exemplo seguinte, o caminho é *.\certificates\ssl.pfx*.
   - Para *- ExportPFXPath*, especifique a localização e o nome do ficheiro PFX para exportação.  No exemplo seguinte, o caminho é *.\certificates\ssl_new.pfx*

   > `Start-AzsReadinessChecker -PfxPassword $password -PfxPath .\certificates\ssl.pfx -ExportPFXPath .\certificates\ssl_new.pfx`  

4. Depois de concluída a ferramenta, reveja o resultado para o sucesso: ![resultados](./media/azure-stack-remediate-certs/remediate-results.png)

## <a name="next-steps"></a>Passos Seguintes
[Saiba mais sobre a segurança de pilha do Azure](azure-stack-rotate-secrets.md)