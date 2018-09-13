---
title: Problemas e guia de resolução de problemas conhecidos | Documentos da Microsoft
description: Lista dos problemas conhecidos e um guia para ajudar a resolver problemas
services: machine-learning
author: svankam
ms.author: svankam
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 01/12/2018
ms.openlocfilehash: dc57509475634b6a8038179dbb205533c3ea9d99
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/12/2018
ms.locfileid: "35947677"
---
# <a name="azure-machine-learning-workbench---known-issues-and-troubleshooting-guide"></a>Azure Machine Learning Workbench - problemas conhecidos e solução de problemas 
Este artigo ajuda-o, encontre e corrija erros ou falhas encontrados como parte da utilização da aplicação do Azure Machine Learning Workbench. 

## <a name="find-the-workbench-build-number"></a>Determinar o número de compilação da bancada de trabalho
Ao se comunicar com a equipa de suporte, é importante incluir o número de compilação da aplicação Workbench. No Windows, pode descobrir o número de compilação ao clicar no **ajudar** menu e escolha **sobre o Azure ML Workbench**. No macOS, pode clicar no **do Azure ML Workbench** menu e escolha **sobre o Azure ML Workbench**.

## <a name="machine-learning-msdn-forum"></a>Fórum MSDN do Machine Learning
Temos um fórum de MSDN que pode postar perguntas. A equipe do produto o fórum é monitorizado ativamente. O Fórum do URL é [ https://aka.ms/azureml-forum ](https://aka.ms/azureml-forum). 

## <a name="gather-diagnostics-information"></a>Recolher informações de diagnóstico
Por vezes, pode ser útil se pode fornecer informações de diagnóstico quando pedir ajuda. É aqui onde residem os ficheiros de registo:

### <a name="installer-log"></a>Registo de instalador
Caso se depare com problema durante a instalação, os ficheiros de registo do instalador estão aqui:

```
# Windows:
%TEMP%\amlinstaller\logs\*

# macOS:
/tmp/amlinstaller/logs/*
```
Pode zip backup do conteúdo desses diretórios e envie-para obter um diagnóstico.

### <a name="workbench-desktop-app-log"></a>Log de aplicativo de desktop da bancada de trabalho
Se tiver problemas ao iniciar sessão ou no caso de falha de ambiente de trabalho a Bancada de trabalho, pode encontrar aqui os ficheiros de registo:
```
# Windows
%APPDATA%\AmlWorkbench

# macOS
~/Library/Application Support/AmlWorkbench
``` 
Pode zip backup do conteúdo desses diretórios e envie-para obter um diagnóstico.

### <a name="experiment-execution-log"></a>Registo de execução da experimentação
Se um determinado script falhar durante a submissão a partir da aplicação de ambiente de trabalho, tente voltar a submetê-lo através da CLI com `az ml experiment submit` comando. Isso deve lhe dar a mensagem de erro completas no formato JSON e, mais importante é que ele contém um **ID da operação** valor. Envie-no ficheiro JSON, incluindo o **ID da operação** e podemos ajudar a diagnosticar. 

Se um script em particular tem êxito no envio, mas falha na execução, ele deve imprimir o **ID de execução** para identificar dessa execução específica. Capaz de empacotar os ficheiros de registo relevantes usando o seguinte comando:

```azurecli
# Create a ZIP file that contains all the diagnostics information
$ az ml experiment diagnostics -r <run_id> -t <target_name>
```

O `az ml experiment diagnostics` comando gera um `diagnostics.zip` ficheiro na pasta de raiz do projeto. O pacote ZIP contém a pasta de projeto inteiro no Estado no momento que foi executada, além de informações de registo. Não se esqueça de remover quaisquer informações confidenciais que não pretende incluir antes de nos enviar o ficheiro de diagnóstico.

## <a name="send-us-a-frown-or-a-smile"></a>Envie-nos comentários negativos (ou um sorriso)

Quando estiver a trabalhar no Azure ML Workbench, também pode enviar-nos comentários negativos (ou um sorriso) ao clicar no ícone de face de smiley no canto inferior esquerdo do shell do aplicativo. Opcionalmente, pode optar por incluir o seu endereço de e-mail (então, podemos recuperar para), e/ou uma captura de ecrã do estado atual. 

## <a name="known-service-limits"></a>Limites de serviço conhecidas
- Tamanho da pasta de projeto máximo permitido: 25 MB.
    >[!NOTE]
    >Este limite não se aplica a `.git`, `docs` e `outputs` pastas. Estes nomes de pastas diferenciam maiúsculas de minúsculas. Se estiver a trabalhar com arquivos grandes, consulte [persistir alterações e lidar com arquivos grandes](../desktop-workbench/how-to-read-write-files.md).

- Máximo permitido de tempo de execução de experimentação: sete dias

- Tamanho máx. de ficheiros registados no `outputs` pasta após uma execução: 512 MB
  - Isso significa que se o seu script produz um ficheiro mais de 512 MB na pasta saídas, ele não é coletado lá. Se estiver a trabalhar com arquivos grandes, consulte [persistir alterações e lidar com arquivos grandes](../desktop-workbench/how-to-read-write-files.md).

- Não são suportadas chaves SSH ao ligar a uma máquina remota ou o cluster do Spark através de SSH. Modo de nome de utilizador/palavra-passe só é suportado atualmente.

- Ao utilizar o cluster de HDInsight como destino de computação, tem de utilizar Azure blob como armazenamento primário. Não é suportada a utilizar o armazenamento do Azure Data Lake.

- Transformações de clusters de texto não são suportadas no Mac.

- Biblioteca de RevoScalePy só é suportada no Windows e Linux (em contentores do Docker). Não é suportada em macOS.

- Blocos de notas do Jupyter têm um limite de tamanho máximo de 5 MB quando abri-los a partir da aplicação Workbench. É possível abrir grandes blocos de notas de CLI com o comando de "Iniciar bloco de notas do az ml", célula limpa e dá como para reduzir o tamanho do ficheiro.

## <a name="cant-update-workbench"></a>Não é possível atualizar a Bancada de trabalho
Quando uma nova atualização está disponível, a página inicial da aplicação Workbench exibe uma mensagem a informar sobre a atualização de novo. Deverá ver um destaque de atualização que aparece no canto inferior esquerdo da aplicação no ícone de sino. Clique em destaque e siga o Assistente do instalador para instalar a atualização. 

![Atualizar imagem](./media/known-issues-and-troubleshooting-guide/update.png)

Se não vir a notificação, tente reiniciar a aplicação. Se ainda não vir a notificação de atualização após o reinício, poderá haver alguns motivos para isso.

### <a name="you-are-launching-workbench-from-a-pinned-shortcut-on-the-task-bar"></a>Estiver a iniciar Bancada de trabalho de um atalho afixado na barra de tarefas
Pode de já ter instalada a atualização. Mas o atalho afixado ainda está a apontar para o bits antigo no disco. Pode verificar isto ao navegar para o `%localappdata%/AmlWorkbench` ver se tiver a versão mais recente está instalado ali e examinar a propriedade do atalho afixado para ver onde está a apontar para e de pasta. Se verificar, basta remover o atalho antigo, inicie o Workbench a partir do menu Iniciar e, opcionalmente, criar um novo atalho afixado na barra de tarefas.

### <a name="you-installed-workbench-using-the-install-azure-ml-workbench-link-on-a-windows-dsvm"></a>Instalou a Bancada de trabalho através da ligação "instalar o Azure ML Workbench" numa DSVM do Windows
Infelizmente, não há nenhuma correção fácil neste. Tem de efetuar os seguintes passos para remover os bits instalados e transferir o instalador mais recente para instalar de novo a Bancada de trabalho: 
   - remover a pasta `C:\Users\<Username>\AppData\Local\amlworkbench`
   - remover o script `C:\dsvm\tools\setup\InstallAMLFromLocal.ps1`
   - remover o atalho de ambiente de trabalho que inicia o script acima
   - Transferir o instalador https://aka.ms/azureml-wb-msi e volte a instalar.

## <a name="stuck-at-checking-experimentation-account-screen-after-logging-in"></a>Preso no ecrã de "Verificação de conta de experimentação" depois de iniciar sessão
Após iniciar sessão, a aplicação Workbench poderá ficar preso numa tela em branco com uma mensagem que mostra "Conta de experimentação bancária" com a roda de rotação. Para resolver este problema, siga os passos seguintes:
1. Encerramento da aplicação
2. Elimine o ficheiro seguinte:
  ```
  # on Windows
  %appdata%\AmlWorkbench\AmlWb.settings

  # on macOS
  ~/Library/Application Support/AmlWorkbench/AmlWb.settings
  ```
3. Reinicie a aplicação.

## <a name="cant-delete-experimentation-account"></a>Não é possível eliminar a conta de experimentação
Pode utilizar a CLI ao eliminar uma conta de experimentação, mas tem de eliminar as áreas de trabalho subordinado e os projetos de filho dentro dessas áreas de trabalho subordinados primeiro. Caso contrário, verá o erro "não é possível eliminar recursos antes de recursos aninhados serem eliminados."

```azure-cli
# delete a project
$ az ml project delete -g <resource group name> -a <experimentation account name> -w <workspace name> -n <project name>

# delete a workspace 
$ az ml workspace delete -g <resource group name> -a <experimentation account name> -n <workspace name>

# delete an experimentation account
$ az ml account experimentation delete -g <resource group name> -n <experimentation account name>
```

Também pode eliminar a projetos e áreas de trabalho do dentro da aplicação Workbench.

## <a name="cant-open-file-if-project-is-in-onedrive"></a>Não é possível abrir o ficheiro se projeto estiver no OneDrive
Se tiver o Windows 10 Fall Creators Update e o projeto ser criado numa pasta local mapeada para o OneDrive, pode achar que não é possível abrir qualquer ficheiro na bancada de trabalho. Isso é devido a um bug introduzido por Fall Creators Update que faz com que o código node. js efetuar a ativação numa pasta do OneDrive. O bug será corrigido em breve pela atualização do Windows, mas até lá, não crie projetos numa pasta do OneDrive.

## <a name="file-name-too-long-on-windows"></a>Nome de arquivo muito tempo no Windows
Se usar a Bancada de trabalho no Windows, pode acabar encontrando o limite de comprimento do nome do padrão ficheiro máximo de 260 carateres, a que pode surgir como um erro "o sistema não é possível localizar o caminho especificado". É possível modificar uma definição de chave de registo para permitir que muito mais tempo do nome de caminho de ficheiro. Revisão [este artigo](https://msdn.microsoft.com/library/windows/desktop/aa365247%28v=vs.85%29.aspx?#maxpath) para obter mais detalhes sobre como definir o _MAX_PATH_ chave de registo.

## <a name="interrupt-cli-execution-output"></a>Resultado da execução da CLI de interrupção
Se iniciar uma experimentação executar com o `az ml experiment submit` ou `az ml notebook start` e gostaria de interromper a saída: 
- No Windows utilize Ctrl-Break combinação de teclas no teclado
- No macOS, utilize Ctrl-C.

Tenha em atenção que apenas isso interrompe o fluxo de saída na janela da CLI. Não, na verdade, impede que uma tarefa que está a ser executada. Se quiser cancelar uma tarefa em curso, utilize `az ml experiment cancel -r <run_id> -t <target name>` comando.

Em computadores Windows com teclados que não têm a chave de garantia de reparação possíveis alternativas incluem Fn-B, Ctrl-Fn-B ou Fn + Esc. Para uma combinação de teclas específica, consulte a documentação do fornecedor de hardware.

## <a name="docker-error-read-connection-refused"></a>Erro de docker "ler: ligação recusada"
Quando em execução em relação a um contentor do Docker local, por vezes, poderá ver o erro seguinte: 
```
Get https://registry-1.docker.io/v2/: 
dial tcp: 
lookup registry-1.docker.io on [::1]:53: read udp [::1]:49385->[::1]:53: 
read: connection refused
```

Pode corrigi-lo ao alterar o servidor DNS Docker a partir `automatic` para um valor fixo de `8.8.8.8`.

## <a name="remove-vm-execution-error-no-tty-present"></a>Remover o erro de execução da VM "presente nenhum tty"
Quando em execução em relação a um contentor do Docker num computador Linux remoto, pode encontrar a seguinte mensagem de erro:
```
sudo: no tty present and no askpass program specified.
``` 
Isto pode acontecer se utilizar o portal do Azure para alterar a palavra-passe de raiz de uma VM do Linux Ubuntu. 

O Azure Machine Learning Workbench requer acesso de sudoers sem palavra-passe para executar em hosts remotos. A forma mais simples de fazer isso é usar _visudo_ para editar o ficheiro seguinte (pode criar o ficheiro se não existir):

```
$ sudo visudo -f /etc/sudoers
```

>[!IMPORTANT]
>É importante editar o ficheiro com _visudo_ e não outro comando. _visudo_ sintaxe procura automaticamente a todos os ficheiros de configuração de sudo e falha para produzir um arquivo de sudoers sintaticamente correta pode bloquear a sua utilização sudo.

Insira a seguinte linha no final do ficheiro:

```
username ALL=(ALL) NOPASSWD:ALL
```

Em que _nome de utilizador_ é o nome do Azure Machine Learning Workbench irá utilizar para iniciar sessão no seu anfitrião remoto.

A linha tem de ser colocada depois #includedir "/ etc/sudoers.d", caso contrário, deve ser substituída por outra regra.

Se tiver uma configuração de sudo mais complicada, convém consultar a documentação de sudo para Ubuntu disponível aqui: https://help.ubuntu.com/community/Sudoers

O erro acima também pode acontecer se não estiver a utilizar uma VM do Linux baseada em Ubuntu no Azure como um destino de execução. Suportamos apenas baseada em Ubuntu VM do Linux para execução remota. 

## <a name="vm-disk-is-full"></a>VM de disco está cheio
Por predefinição quando cria uma nova VM do Linux no Azure, obtém um disco de 30 GB para o sistema operativo. Motor do docker por padrão usa o mesmo disco para puxando imagens e contentores em execução. Isso pode preencher o disco do SO e vir um erro de "VM disco é completo" quando isso acontece.

É uma correção rápida remover todas as imagens do Docker que deixará de utilizar. O seguinte comando do Docker faz exatamente isso. (Claro que precisa SSH na VM para executar o comando Docker a partir de uma shell de bash.)

```
$ docker system prune -a
```

Também pode adicionar um disco de dados e configurar o motor do Docker para utilizar o disco de dados para armazenar imagens. Eis [como adicionar um disco de dados](https://docs.microsoft.com/azure/virtual-machines/linux/add-disk). Pode então [alteração em que o Docker armazena imagens](https://forums.docker.com/t/how-do-i-change-the-docker-image-installation-directory/1169).

Em alternativa, pode expandir o disco do SO e, não precisa tocar em configuração do motor de Docker. Eis [como pode expandir o disco do SO](https://docs.microsoft.com/azure/virtual-machines/linux/expand-disks).

```azure-cli
# Deallocate VM (stopping will not work)
$ az vm deallocate --resource-group myResourceGroup  --name myVM

# Get VM's Disc Name
az disk list --resource-group myResourceGroup --query '[*].{Name:name,Gb:diskSizeGb,Tier:accountType}' --output table

# Update Disc Size using above name
$ az disk update --resource-group myResourceGroup --name myVMdisc --size-gb 250
    
# Start VM    
$ az vm start --resource-group myResourceGroup  --name myVM
```

## <a name="sharing-c-drive-on-windows"></a>Partilha de unidade C em Windows
Se está a executar num contentor do Docker local no Windows, a definição `sharedVolumes` para `true` no `docker.compute` do ficheiro em `aml_config` pode melhorar o desempenho de execução. No entanto, isto requer a unidade C e na partilha a _ferramenta de Docker para Windows_. Se não é possível partilhar a unidade C, experimente as sugestões seguintes:

* Verifique o compartilhamento na unidade C com o Explorador de ficheiros
* Abra as definições do adaptador de rede e desinstalar/reinstalar "Compartilhamento de arquivos e impressoras em redes Microsoft" para vEthernet
* Abrir as definições do docker e partilhar a unidade C de configurações de docker
* As alterações para a palavra-passe do Windows afetam o compartilhamento. Abra o Explorador de ficheiros, voltar a partilhar a unidade C e introduza a palavra-passe nova.
* Também pode encontrar o problema de firewall quando tentar compartilhar sua unidade C com o Docker. Isso [Stack Overflow post](http://stackoverflow.com/questions/42203488/settings-to-windows-firewall-to-allow-docker-for-windows-to-share-drive/43904051) pode ser útil.
* Quando partilha a unidade C, com as credenciais de domínio, o compartilhamento poderá parar de funcionar em redes onde o controlador de domínio não está acessível (por exemplo, rede doméstica, pública Wi-Fi etc.). Para obter mais informações, consulte [esta publicação](https://blogs.msdn.microsoft.com/stevelasker/2016/06/14/configuring-docker-for-windows-volumes/).

Também pode evitar o problema de partilha, num custo, através da definição de desempenho pequeno `sharedVolumne` para `false` no `docker.compute` ficheiro.

## <a name="wipe-clean-workbench-installation"></a>Instalação limpa da bancada de trabalho de eliminação
Em geral, não precisa de fazer isso. Mas caso terá que apagar limpa uma instalação, aqui estão os passos:

- No Windows:
  - Primeiro, certifique-se de que utiliza _adicionar ou remover programas_ miniaplicativo no _painel de controlo_ para remover o _Azure Machine Learning Workbench_ entrada de aplicativo.  
  - Em seguida, pode transferir e executar um dos seguintes scripts:
    - [Script de linha de comandos do Windows](https://github.com/Azure/MachineLearning-Scripts/blob/master/cleanup/cleanup_win.cmd).
    - [Script do Windows PowerShell](https://github.com/Azure/MachineLearning-Scripts/blob/master/cleanup/cleanup_win.ps1). (Poderá ter de executar `Set-ExecutionPolicy Unrestricted` numa janela do PowerShell privilégios elevados antes de poder executar o script.)
- Em macOS:
  - Acabou de transferir e executar o [script de shell de bash de macOS](https://github.com/Azure/MachineLearning-Scripts/blob/master/cleanup/cleanup_mac.sh).

## <a name="azure-ml-using-a-different-python-location-than-the-azure-ml-installed-python-environment"></a>O Azure ML a utilização de uma localização de python diferentes que o ambiente de python instalada do Azure ML
Devido a uma alteração recente no Azure Machine Learning Workbench, os utilizadores poderão reparar que execuções locais não podem apontar para o ambiente de python instalado pelo Azure ML Workbench mais. Isto pode acontecer se o utilizador tiver outro ambiente de python instalado em seus computadores e o caminho de "Python" está definido para apontar para esse ambiente. Para poder utilizar o Azure ML Workbench instalado no ambiente do Python, siga estes passos:
- Aceda ao ficheiro de local.compute em sua pasta de aml_config na raiz do projeto.
- Alterar a variável "pythonLocation" para apontar para o caminho físico do Azure ML workbench instalado o ambiente do python. Pode obter este caminho de duas formas:
    - Localização de python do Azure ML pode ser encontrada em %localappdata%\AmlWorkbench\python\python.exe
    - Pode abrir cmd da bancada de trabalho do Azure ML, escreva o python no prompt de comando, importar sys.exe, executar sys.executable e obter o caminho a partir daí. 



## <a name="some-useful-docker-commands"></a>Alguns comandos úteis do Docker

Eis alguns comandos de Docker úteis:

```sh
# display all running containers
$ docker ps

# dislplay all containers (running or stopped)
$ docke ps -a

# display all images
$ docker images

# show Docker logs of a container
$ docker logs <container_id>

# create a new container and launch into a bash shell
$ docker run <image_id> /bin/bash

# launch into a bash shell on a running container
$ docker exec -it <container_id> /bin/bash

# stop an running container
$ docker stop <container_id>

# delete a container
$ docker rm <container_id>

# delete an image
$ docker rmi <image_id>

# delete all unussed Docker images 
$ docker system prune -a

```
