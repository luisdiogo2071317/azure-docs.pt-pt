---
title: Resolução de problemas de ficheiros do Azure no Linux | Documentos da Microsoft
description: Resolução de problemas de ficheiros do Azure no Linux
services: storage
documentationcenter: ''
author: jeffpatt24
manager: aungoo
editor: tamram
tags: storage
ms.service: storage
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/11/2018
ms.author: jeffpatt
ms.openlocfilehash: 5781a3c2e121b81275683d73eb3047ba949857c7
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39414946"
---
# <a name="troubleshoot-azure-files-problems-in-linux"></a>Resolução de problemas de ficheiros do Azure no Linux

Este artigo lista problemas comuns relacionados com ficheiros do Microsoft Azure quando se liga a partir de clientes de Linux. Ele também fornece possíveis causas e resoluções para esses problemas.

<a id="permissiondenied"></a>
## <a name="permission-denied-disk-quota-exceeded-when-you-try-to-open-a-file"></a>"quota de disco [permissão negada] foi excedido" quando tenta abrir um ficheiro

No Linux, receberá uma mensagem de erro que é semelhante ao seguinte:

**<filename> [permissão negada] Quota de disco foi excedida**

### <a name="cause"></a>Causa

Atingiu o limite superior de identificadores abertos em simultâneo permitidos para um ficheiro.

### <a name="solution"></a>Solução

Reduzir o número de identificadores abertos em simultâneo por fechar alguns identificadores e, em seguida, repita a operação. Para obter mais informações, consulte [lista de verificação de armazenamento do Microsoft Azure, desempenho e escalabilidade](../common/storage-performance-checklist.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

<a id="slowfilecopying"></a>
## <a name="slow-file-copying-to-and-from-azure-files-in-linux"></a>Tornar mais lenta de copiar o arquivo de e para ficheiros do Azure no Linux

- Se não tiver um requisito de tamanho de e/s mínimo específico, recomendamos que utilize 1 MiB como o tamanho de e/s para otimizar o desempenho.
- Se souber o tamanho final de um ficheiro que estiver a expandir através da utilização de escritas, e seu software não têm problemas de compatibilidade, quando uma cauda implícitas no arquivo contém zeros, em seguida, defina o tamanho do ficheiro com antecedência, em vez de fazer uma gravação de expansão de cada gravação.
- Utilize o método de cópia certo:
    - Uso [AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) para qualquer transferência entre duas partilhas de ficheiros.
    - Uso [Robocopy](https://blogs.msdn.microsoft.com/granth/2009/12/07/multi-threaded-robocopy-for-faster-copies/) entre partilhas de ficheiros num computador no local.

<a id="error112"></a>
## <a name="mount-error112-host-is-down-because-of-a-reconnection-time-out"></a>"Montar error(112): anfitrião não está funcional" devido a um limite de tempo de restabelecimento de ligação

Quando o cliente tem estado inativo durante muito tempo, ocorre um erro de montagem "112" no cliente Linux. Depois de tempo de inatividade expandido, o cliente se desliga e a ligação exceder o tempo limite.  

### <a name="cause"></a>Causa

A ligação pode estar inativa pelos seguintes motivos:

-   Falhas de comunicação de rede que impedem a voltar a estabelecer uma ligação de TCP para o servidor quando é utilizada a opção de montagem "soft" padrão
-   Correções de restabelecimento de ligação recente que não estão presentes nos kernels anteriores

### <a name="solution"></a>Solução

Esse problema de restabelecimento de ligação no kernel do Linux agora é fixado como parte das seguintes alterações:

- [Correção de voltar a ligar a difere da sessão de smb3 voltar a ligar depois restabelecimento de ligação de socket](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/fs/cifs?id=4fcd1813e6404dd4420c7d12fb483f9320f0bf93)
- [Chamar o serviço de eco imediatamente depois de restabelecimento de ligação de socket](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=b8c600120fc87d53642476f48c8055b38d6e14c7)
- [CIFS: Corrigir uma corrupção de memória durante o restabelecimento de ligação](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/?id=53e0e11efe9289535b060a51d4cf37c25e0d0f2b)
- [CIFS: Corrigir uma possível duplo bloqueio do mutex durante a restabelecer ligação (para o kernel v4.9 e posterior)](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/?id=96a988ffeb90dba33a71c3826086fe67c897a183)

No entanto, essas alterações poderão não ser transportadas ainda para todas as distribuições de Linux. Esta correção e outras correções de restabelecimento de ligação são efetuadas nos kernels de Linux populares seguintes: 4.4.40 4.8.16 e 4.9.1. Pode obter esta correção com a atualização para uma destas versões de kernel recomendada.

### <a name="workaround"></a>Solução

Pode contornar este problema, especificando uma montagem do disco rígida. Isso força o cliente Aguarde até que é estabelecida uma ligação ou até que ele é interrompido explicitamente e pode ser utilizado para evitar erros devido a tempo limite de rede. No entanto, esta solução pode causar a espera indefinida. Esteja preparado para parar as ligações conforme necessário.

Se é possível atualizar para as versões mais recentes do kernel, pode contornar este problema, mantendo um ficheiro na partilha de ficheiros do Azure que escreve a cada 30 segundos ou menos. Tem de ser uma operação de escrita, como a reescrever a data de criação ou modificação no arquivo. Caso contrário, poderá obter resultados em cache e a operação não pode acionar o restabelecimento.

<a id="error115"></a>
## <a name="mount-error115-operation-now-in-progress-when-you-mount-azure-files-by-using-smb-30"></a>"Montar error(115): operação agora em curso" quando montar ficheiros do Azure utilizando o SMB 3.0

### <a name="cause"></a>Causa

Algumas distribuições de Linux ainda não suporta recursos de criptografia no SMB 3.0 e os usuários podem receber uma mensagem de erro "115" se tentarem ficheiros do Azure de montagem, utilizando o SMB 3.0 devido a um recurso em falta. SMB 3.0 com a encriptação completa só é suportada neste momento ao utilizar o Ubuntu 16.04 ou posterior.

### <a name="solution"></a>Solução

Funcionalidade de encriptação no SMB 3.0 para Linux foi introduzida no 4.11 kernel. Esta funcionalidade permite a montagem de partilha de ficheiros do Azure a partir de uma região diferente do Azure ou no local. No momento da publicação, essa funcionalidade foi backported Ubuntu 17.04 e Ubuntu 16.10. Se o cliente Linux SMB não suporta a encriptação, ficheiros do Azure de montagem utilizando o SMB 2.1 a partir de uma VM do Linux do Azure que está no mesmo datacenter como a conta de armazenamento de ficheiros.

<a id="slowperformance"></a>
## <a name="slow-performance-on-an-azure-file-share-mounted-on-a-linux-vm"></a>Desempenho lento numa partilha de ficheiros do Azure montada numa VM do Linux

### <a name="cause"></a>Causa

Uma causa possível de um desempenho lento está desativada a colocação em cache.

### <a name="solution"></a>Solução

Para verificar se a colocação em cache está desativada, procure o **cache =** entrada. 

**Cache = none** indica que a colocação em cache está desativada.  Voltar a montar a partilha com o comando de montagem de predefinição ou adicionando explicitamente a **cache = strict** opção para o comando de montagem para garantir que esse padrão de colocação em cache ou o modo de colocação em cache "estrito" estiver ativada.

Em alguns cenários, o **serverino** pode fazer com que a opção de montagem a **ls** comando para executar stat em relação a cada entrada de diretório. Este comportamento resulta na degradação do desempenho quando está a lista um diretório grande. Pode verificar as opções de montagem em sua **etc/fstab** entrada:

`//azureuser.file.core.windows.net/cifs /cifs cifs vers=2.1,serverino,username=xxx,password=xxx,dir_mode=0777,file_mode=0777`

Também pode verificar se as opções corretas estão a ser utilizadas ao executar o **sudo mount | grep cifs** comando e verificar a sua saída, por exemplo, o resultado de exemplo seguinte:

`//azureuser.file.core.windows.net/cifs on /cifs type cifs (rw,relatime,vers=2.1,sec=ntlmssp,cache=strict,username=xxx,domain=X,uid=0,noforceuid,gid=0,noforcegid,addr=192.168.10.1,file_mode=0777, dir_mode=0777,persistenthandles,nounix,serverino,mapposix,rsize=1048576,wsize=1048576,actimeo=1)`

Se o **cache = strict** ou **serverino** opção é não apresentar, desmonte e montar ficheiros do Azure novamente ao executar o comando de montagem do [documentação](../storage-how-to-use-files-linux.md). Em seguida, verificar que o **etc/fstab** entrada tem as opções corretas.

<a id="timestampslost"></a>
## <a name="time-stamps-were-lost-in-copying-files-from-windows-to-linux"></a>Carimbos de data / hora tenham sido perdidos devido copiar os ficheiros do Windows para Linux

Em plataformas Linux/Unix, o **cp -p** comando falhar se o ficheiro de 1 e 2 do ficheiro são pertencentes a utilizadores diferentes.

### <a name="cause"></a>Causa

O sinalizador force **f** no COPYFILE resulta na execução **cp -p -f** no Unix. Este comando também falha preservar o carimbo de data / hora do ficheiro que não possui.

### <a name="workaround"></a>Solução

Utilize o utilizador de conta de armazenamento para copiar os ficheiros:

- `Useadd : [storage account name]`
- `Passwd [storage account name]`
- `Su [storage account name]`
- `Cp -p filename.txt /share`

## <a name="cannot-connect-or-mount-an-azure-file-share"></a>Não é possível ligar ou montar uma partilha de ficheiros do Azure

### <a name="cause"></a>Causa

Causas comuns para este problema são:


- Está a utilizar um cliente de distribuição de Linux incompatível. Recomendamos que utilize o seguinte distribuições do Linux para ligar à partilha de ficheiros do Azure:

* **Mínimo recomendado para as versões com recursos de montagem correspondentes (versão do SMB versão 2.1 vs do SMB 3.0)**    
    
    |   | SMB 2.1 <br>(Monta em VMs na mesma região do Azure) | SMB 3.0 <br>(Monta no local e em várias regiões) |
    | --- | :---: | :---: |
    | Ubuntu Server | 14.04 + | 16.04 + |
    | RHEL | 7 e posteriores | 7.5 + |
    | CentOS | 7 e posteriores |  7.5 + |
    | Debian | 8 e posteriores |   |
    | openSUSE | 13.2 + | 42.3 + |
    | SUSE Linux Enterprise Server | 12 | 12 SP3 + |

- Utils do CIFS não estão instalados no cliente.
- O SMB/CIFS mínimo a versão 2.1 não está instalado no cliente.
- Encriptação do SMB 3.0 não é suportada no cliente. Encriptação do SMB 3.0 está disponível no Ubuntu 16.4 e versão posterior, SUSE 12.3 e versão posterior. Outras distribuições requerem kernel 4.11 e versão posterior.
- Está a tentar ligar a uma conta de armazenamento através da porta TCP 445 que não é suportada.
- Está a tentar a tentar ligar à partilha de ficheiros do Azure a partir de uma VM do Azure e a VM não está localizada na mesma região que a conta de armazenamento.

### <a name="solution"></a>Solução

Para resolver o problema, utilize o [ferramenta de resolução de problemas para erros de montagem no Linux dos ficheiros do Azure](https://gallery.technet.microsoft.com/Troubleshooting-tool-for-02184089). Essa ferramenta ajuda-o para validar o cliente com o ambiente, detetar a configuração de cliente incompatível que podem causar a falha de acesso para ficheiros do Azure, oferece orientação prescritiva sobre Self-corrigir e recolhe os rastreios de diagnóstico.

## <a name="ls-cannot-access-ltpathgt-inputoutput-error"></a>ls: não é possível aceder '&lt;caminho&gt;': erro de entrada/saída

Quando tenta à lista de arquivos num arquivo do Azure partilham ao utilizar o comando de ls, ls comando pára quando lista os ficheiros recebem o erro seguinte:

**ls: não é possível aceder '&lt;caminho&gt;': erro de entrada/saída**


### <a name="solution"></a>Solução
Atualize o kernel do Linux para as seguintes versões que têm a solução para este problema:

- 4.4.87+
- 4.9.48+
- 4.12.11+
- Todas as versões que é maior ou igual a 4.13

## <a name="cannot-create-symbolic-links---ln-failed-to-create-symbolic-link-t-operation-not-supported"></a>Não é possível criar links simbólicos - ln: Falha ao criar a ligação simbólica T' ": a operação não é suportada

### <a name="cause"></a>Causa
Por predefinição, a montagem de partilhas de ficheiros do Azure no Linux utilizar CIFS não permite suporte para Links simbólicos. Verá um erro ao ligar esta:
```
ln -s linked -n t
ln: failed to create symbolic link 't': Operation not supported
```
### <a name="solution"></a>Solução
O cliente de Linux CIFS não suporta a criação de links simbólicos do estilo Windows através do protocolo de SMB2/3. Atualmente, o cliente Linux suporta outro estilo de links simbólicos, chamados [Mishall + francês links simbólicos] (https://wiki.samba.org/index.php/UNIX_Extensions#Minshall.2BFrench_symlinks) para ambos criar e siga as operações. Os clientes que precisam de links simbólicos podem utilizar a opção de montagem de "mfsymlinks". "mfsymlinks", normalmente, são recomendadas uma vez que também é o formato utilizado pela Macs.

Para conseguir utilizar links simbólicos, adicione o seguinte ao final do comando de montagem de CIFS:

```
,mfsymlinks
```

Portanto, o comando será algo parecido com:

```
sudo mount -t cifs //<storage-account-name>.file.core.windows.net/<share-name> <mount-point> -o vers=<smb-version>,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino,mfsynlinks
```

Uma vez adicionado, poderá criar links simbólicos como sugerido no [Wiki](https://wiki.samba.org/index.php/UNIX_Extensions#Storing_symlinks_on_Windows_servers).

## <a name="need-help-contact-support"></a>Precisa de ajuda? Contacte o suporte.

Se precisar de ajuda, ainda [contacte o suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para seu problema resolvido rapidamente.
