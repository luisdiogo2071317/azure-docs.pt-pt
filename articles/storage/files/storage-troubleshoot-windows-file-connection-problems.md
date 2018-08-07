---
title: Resolução de problemas de ficheiros do Azure no Windows | Documentos da Microsoft
description: Resolução de problemas de ficheiros do Azure no Windows
services: storage
author: jeffpatt24
tags: storage
ms.service: storage
ms.topic: article
ms.date: 05/11/2018
ms.author: jeffpatt
ms.component: files
ms.openlocfilehash: 935d4a3ba3fc3199177be5bd4e70f82239c3c971
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/06/2018
ms.locfileid: "39531534"
---
# <a name="troubleshoot-azure-files-problems-in-windows"></a>Resolução de problemas de ficheiros do Azure no Windows

Este artigo lista problemas comuns relacionados com ficheiros do Microsoft Azure quando se liga a partir de clientes do Windows. Ele também fornece possíveis causas e resoluções para esses problemas. Além dos passos de resolução de problemas neste artigo, pode também usar [AzFileDiagnostics](https://gallery.technet.microsoft.com/Troubleshooting-tool-for-a9fa1fe5) para garantir que o ambiente de cliente do Windows tem pré-requisitos corretos. AzFileDiagnostics automatiza a deteção da maioria dos sintomas mencionados neste artigo e ajuda a configurar o ambiente para obter um desempenho ideal. Também pode encontrar estas informações no [solucionador de problemas de partilhas de ficheiros do Azure](https://support.microsoft.com/help/4022301/troubleshooter-for-azure-files-shares) que fornece os passos para ajudá-lo com problemas de partilhas de ficheiros do Azure de ligar/mapeamento/montagem.


<a id="error53-67-87"></a>
## <a name="error-53-error-67-or-error-87-when-you-mount-or-unmount-an-azure-file-share"></a>Erro 53, erro 67 ou erro 87 ao montar ou desmontar uma partilha de ficheiros do Azure

Ao tentar montar uma partilha de ficheiros no local ou a partir de um centro de dados diferente, pode receber os seguintes erros:

- Ocorreu um erro de sistema 53. O caminho de rede não foi encontrado.
- Ocorreu um erro de sistema 67. Não é possível localizar o nome da rede.
- Ocorreu um erro de sistema 87. O parâmetro está incorreto.

### <a name="cause-1-unencrypted-communication-channel"></a>Causa 1: Canal de comunicação sem encriptação

Por motivos de segurança, as ligações a partilhas de ficheiros do Azure são bloqueadas se o canal de comunicação não é encriptado e se a tentativa de ligação não é feita a partir do mesmo datacenter onde residem as partilhas de ficheiros do Azure. Encriptação de canal de comunicação é fornecida apenas se o SO de cliente do usuário oferece suporte a encriptação SMB.

Windows 8, Windows Server 2012 e versões posteriores de cada sistema negociam pedidos que incluem o SMB 3.0, que suporta a encriptação.

### <a name="solution-for-cause-1"></a>Solução para causa 1

Ligar a partir de um cliente que faz o seguinte:

- Cumpre os requisitos do Windows 8 e Windows Server 2012 ou versões posteriores
- Liga-se de uma máquina virtual no mesmo datacenter como a conta de armazenamento do Azure que é utilizada para a partilha de ficheiros do Azure

### <a name="cause-2-port-445-is-blocked"></a>Causa 2: A porta 445 está bloqueada

Erro de sistema 53 ou erro 67 do sistema pode ocorrer se a porta 445 comunicação saída para um centro de dados de ficheiros do Azure está bloqueada. Para ver o resumo de ISPs que permitem ou não o acesso de porta 445, aceda à [TechNet](http://social.technet.microsoft.com/wiki/contents/articles/32346.azure-summary-of-isps-that-allow-disallow-access-from-port-445.aspx).

Para compreender se este é o motivo por trás da mensagem de "Erro de sistema 53", pode utilizar o Portqry para consultar o ponto de extremidade TCP:445. Se o ponto de extremidade TCP:445 é apresentado como filtrada, a porta TCP está bloqueada. Segue-se uma consulta de exemplo:

  `g:\DataDump\Tools\Portqry>PortQry.exe -n [storage account name].file.core.windows.net -p TCP -e 445`

Se a porta TCP 445 estiver bloqueada por uma regra no caminho de rede, verá a seguinte saída:

  `TCP port 445 (microsoft-ds service): FILTERED`

Para obter mais informações sobre como utilizar o Portqry, veja [Descrição do utilitário de linha de comandos Portqry.exe](https://support.microsoft.com/help/310099).

### <a name="solution-for-cause-2"></a>Solução para causa 2

Trabalhar com o seu departamento de TI para abrir a porta 445 de saída para [intervalos de IP do Azure](https://www.microsoft.com/download/details.aspx?id=41653).

### <a name="cause-3-ntlmv1-is-enabled"></a>Causa 3: NTLMv1 está ativada

Erro de sistema 53 ou erro de sistema 87 pode ocorrer se a comunicação de NTLMv1 estiver ativada no cliente. Ficheiros do Azure suportam apenas a autenticação NTLMv2. Ter NTLMv1 ativado cria um cliente menos seguras. Por conseguinte, a comunicação é bloqueada para ficheiros do Azure. 

Para determinar se se trata a causa do erro, certifique-se de que a seguinte subchave do registo está definida como um valor de 3:

**HKLM\SYSTEM\CurrentControlSet\Control\Lsa > LmCompatibilityLevel**

Para obter mais informações, consulte a [LmCompatibilityLevel](https://technet.microsoft.com/library/cc960646.aspx) tópico no TechNet.

### <a name="solution-for-cause-3"></a>Solução para causa 3

Reverter o **LmCompatibilityLevel** valor para o valor predefinido de 3 na seguinte subchave do registo:

  **HKLM\SYSTEM\CurrentControlSet\Control\Lsa**

<a id="error1816"></a>
## <a name="error-1816-not-enough-quota-is-available-to-process-this-command-when-you-copy-to-an-azure-file-share"></a>Erro 1816 "quota não suficiente está disponível para processar este comando" quando copiar para uma partilha de ficheiros do Azure

### <a name="cause"></a>Causa

Ocorre um erro 1816 quando atingir o limite superior de identificadores abertos em simultâneo permitidos para um ficheiro no computador onde a partilha de ficheiros está sendo montada.

### <a name="solution"></a>Solução

Reduzir o número de identificadores abertos em simultâneo por fechar alguns identificadores e tente novamente. Para obter mais informações, consulte [lista de verificação de armazenamento do Microsoft Azure, desempenho e escalabilidade](../common/storage-performance-checklist.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

<a id="slowfilecopying"></a>
## <a name="slow-file-copying-to-and-from-azure-files-in-windows"></a>Tornar mais lenta de copiar o arquivo de e para ficheiros do Azure no Windows

Poderá ver um desempenho lento ao tentar transferir ficheiros para o serviço de ficheiros do Azure.

- Se não tiver um requisito de tamanho de e/s mínimo específico, recomendamos que utilize 1 MiB como o tamanho de e/s para otimizar o desempenho.
-   Se conhece o tamanho final de um ficheiro que estiver a expandir com gravações e seu software não tem problemas de compatibilidade quando a cauda implícitas no arquivo contém zeros, em seguida, defina o tamanho do ficheiro com antecedência, em vez de fazer uma gravação de expansão de cada gravação.
-   Utilize o método de cópia certo:
    -   Uso [AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) para qualquer transferência entre duas partilhas de ficheiros.
    -   Uso [Robocopy](https://blogs.msdn.microsoft.com/granth/2009/12/07/multi-threaded-robocopy-for-faster-copies/) entre partilhas de ficheiros num computador no local.

### <a name="considerations-for-windows-81-or-windows-server-2012-r2"></a>Considerações para o Windows 8.1 ou Windows Server 2012 R2

Para os clientes que estejam a executar o Windows 8.1 ou Windows Server 2012 R2, certifique-se de que o [KB3114025](https://support.microsoft.com/help/3114025) correção está instalada. Esta correção melhora o desempenho de criar e fechar identificadores.

Pode executar o script seguinte para verificar se a correção foi instalada:

`reg query HKLM\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies`

Se a correção está instalada, é apresentada a seguinte saída:

`HKEY_Local_MACHINE\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies {96c345ef-3cac-477b-8fcd-bea1a564241c} REG_DWORD 0x1`

> [!Note]
> Imagens do Windows Server 2012 R2 no Azure Marketplace tem correção KB3114025 instalado por predefinição, a partir de Dezembro de 2015.

<a id="shareismissing"></a>
## <a name="no-folder-with-a-drive-letter-in-my-computer"></a>Nenhuma pasta com uma letra de unidade no **meu computador**

Se mapear uma partilha de ficheiros do Azure como administrador com a utilização de rede, o compartilhamento parece estar em falta.

### <a name="cause"></a>Causa

Por predefinição, o Explorador de ficheiros do Windows não é executado como administrador. Se executar o net use a partir de uma linha de comandos administrativa, mapear a unidade de rede como administrador. Como as unidades mapeadas são centrada no utilizador, a conta de utilizador que tem sessão iniciada não apresenta as unidades se eles estão montados numa conta de utilizador diferente.

### <a name="solution"></a>Solução
Monte a partilha a partir de uma linha de comando não-administrador. Em alternativa, pode seguir [este tópico do TechNet](https://technet.microsoft.com/library/ee844140.aspx) para configurar o **EnableLinkedConnections** valor de registo.

<a id="netuse"></a>
## <a name="net-use-command-fails-if-the-storage-account-contains-a-forward-slash"></a>Comando net use falha se a conta de armazenamento contém uma barra

### <a name="cause"></a>Causa

O comando net use interpreta uma barra (/) como uma opção de linha de comandos. Se o nome da sua conta de utilizador for iniciado com uma barra, o mapeamento de unidade falha.

### <a name="solution"></a>Solução

Pode utilizar qualquer um dos seguintes passos para resolver o problema:

- Execute o seguinte comando do PowerShell:

  `New-SmbMapping -LocalPath y: -RemotePath \\server\share -UserName accountName -Password "password can contain / and \ etc" `

  De um arquivo de lote, pode executar o comando desta forma:

  `Echo new-smbMapping ... | powershell -command –`

- Coloque as aspas à volta a chave para solucionar este problema, a menos que a barra é o primeiro caráter. Se for, utilize o modo interativo e introduza a palavra-passe em separado ou regenerar as chaves para obter uma chave que não começa com uma barra.

<a id="cannotaccess"></a>
## <a name="application-or-service-cannot-access-a-mounted-azure-files-drive"></a>Aplicação ou serviço não é possível aceder uma unidade de ficheiros do Azure montada

### <a name="cause"></a>Causa

Unidades estão montadas por utilizador. Se a sua aplicação ou serviço está em execução sob uma conta de utilizador diferente daquela que montar a unidade, o aplicativo não verão a unidade.

### <a name="solution"></a>Solução

Utilize uma das seguintes soluções:

-   Monte a unidade a partir da mesma conta de utilizador, que contém a aplicação. Pode usar uma ferramenta como o PsExec.
- Transmita o nome da conta de armazenamento e a chave o nome de utilizador e os parâmetros de palavra-passe do net use o comando.
- Utilize o comando cmdkey para adicionar as credenciais no Gestor de credenciais. Fazer a partir de uma linha de comandos no contexto de conta de serviço, por meio de um início de sessão interativo ou através de runas.
  
  `cmdkey /add:<storage-account-name>.file.core.windows.net /user:AZURE\<storage-account-name> /pass:<storage-account-key>`
- Mapear a partilha diretamente sem utilizar uma letra de unidade mapeada. Alguns aplicativos podem não restabelece ligação para a letra de unidade corretamente, para que utilizar o caminho UNC completo pode ser mais fiável. 

  `net use * \\storage-account-name.file.core.windows.net\share`

Depois de seguir estas instruções, poderá receber a seguinte mensagem de erro ao executar o net use para a conta de serviço do sistema/rede: "erro de sistema 1312 ocorreu. Uma sessão de início de sessão especificado não existe. Ele já foi foi terminado." Se isto ocorrer, certifique-se de que o nome de utilizador que é passada para net utilização inclui informações de domínio (por exemplo: "[nome da conta de armazenamento]. file.core.windows .net").

<a id="doesnotsupportencryption"></a>
## <a name="error-you-are-copying-a-file-to-a-destination-that-does-not-support-encryption"></a>Erro "Está a copiar um ficheiro para um destino que não suporta encriptação"

Quando um ficheiro é copiado através da rede, o ficheiro é desencriptado no computador de origem, transmitido em texto não criptografado e encriptado novamente no destino. No entanto, poderá ver o seguinte erro quando está a tentar copiar um ficheiro encriptado: "Está a copiar o ficheiro para um destino que não suporta a encriptação."

### <a name="cause"></a>Causa
Este problema pode ocorrer se estiver a utilizar o Encrypting File System (EFS). Ficheiros encriptados por BitLocker podem ser copiados para ficheiros do Azure. No entanto, os ficheiros do Azure não suporta EFS de NTFS.

### <a name="workaround"></a>Solução
Para copiar um ficheiro através da rede, tem de desencriptá-lo primeiro. Utilize um dos seguintes métodos:

- Utilize o **copiar /d** comando. Ele permite que os ficheiros encriptados para serem guardados como ficheiros desencriptados no destino.
- Defina a seguinte chave de registo:
  - Caminho = HKLM\Software\Policies\Microsoft\Windows\System
  - Tipo de valor DWORD de =
  - Name = CopyFileAllowDecryptedRemoteDestination
  - Valor = 1

Lembre-se de que definir a chave do registo afeta todas as operações de cópia que são feitas a compartilhamentos de rede.

## <a name="need-help-contact-support"></a>Precisa de ajuda? Contacte o suporte.
Se precisar de ajuda, ainda [contacte o suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para seu problema resolvido rapidamente.
