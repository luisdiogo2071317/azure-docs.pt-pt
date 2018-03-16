O agente de sincronização de ficheiros do Azure é atualizado regularmente para adicionar novas funcionalidades e a resolver problemas. Recomendamos que configure o Microsoft Update para obter as atualizações para o agente de sincronização de ficheiros de Azure conforme estiverem disponíveis.

#### <a name="major-vs-minor-agent-versions"></a>Principais vs. versões de secundárias de agente
* Muitas vezes, as versões do agente principais contém novas funcionalidades e tem um número crescente de como a primeira parte do número de versão. Por exemplo: *2.\*.\**
* Versões de secundárias agente são também denominadas "patches" e são lançadas com maior frequência com que as versões principais. Muitas vezes, contêm correções de erros e melhorias mais pequenas, mas não existem novas funcionalidades. Por exemplo:  *\*.3.\**

#### <a name="upgrade-paths"></a>Caminhos de atualização
Existem três aprovado e testado formas para instalar as atualizações de agente de sincronização de ficheiros do Azure. Atualizar estes caminhos de trabalho para as versões principais e secundárias.
1. **(Preferencial) Configure o Microsoft Update para transferir e instalar as atualizações de agente automaticamente.**  
    Recomendamos sempre colocar todas as atualizações de sincronização de ficheiros do Azure para se certificar de que tem acesso as correções mais recentes para o agente de servidor. O Microsoft Update simplifica este processo totalmente integrada, por automaticamente transferir e instalar atualizações para si.
2. **Aplicar o patch de um agente de sincronização de ficheiros do Azure existente ao utilizar um ficheiro de patch do Microsoft Update ou de um. msp executável. O pacote de atualização mais recente sincronização de ficheiros do Azure pode ser transferido a partir de [catálogo Microsoft Update](https://www.catalog.update.microsoft.com/Search.aspx?q=Azure%20File%20Sync).**  
    Executar um. msp executável irá atualizar a instalação de sincronização de ficheiros do Azure com o mesmo método utilizado automaticamente pelo Microsoft Update no caminho de atualização anterior. Aplicar uma correção do Microsoft Update irá efetuar uma atualização direta de uma instalação de sincronização de ficheiros do Azure.
3. **Transferir o instalador do agente de sincronização de ficheiros do Azure mais recente do [Microsoft Download Center](https://go.microsoft.com/fwlink/?linkid=858257). A transferência do programa de instalação é um pacote do Microsoft Installer ou. um msi executável.**  
    Para atualizar uma instalação de agente de sincronização de ficheiros do Azure existente, desinstalar a versão mais antiga e, em seguida, instale a versão mais recente a partir de instalador que transferiu. O registo do servidor, grupos de sincronização e quaisquer outras definições são mantidas pelo instalador da sincronização de ficheiros do Azure.

#### <a name="agent-lifecycle-and-change-management-guarantees"></a>Garante que a gestão de ciclo de vida e de alterações de agente
Sincronização de ficheiros do Azure é um serviço em nuvem, permitindo continuamente introdução de novas funcionalidades e funções. Isto significa que uma versão de agente de sincronização de ficheiros de Azure específica só pode ser suportada por um período de tempo limitado. Para facilitar a implementação, as seguintes regras para garantir tem suficiente hora e a notificação para acomodar agente atualizações/melhoramentos no seu processo de gestão de alteração:

- Versões do agente principais são suportadas para, pelo menos, de seis meses a contar da data da versão inicial.
- Podemos garantir que não há uma sobreposição de, pelo menos, três meses entre o suporte de versões do agente principais. 
- Avisos são emitidos para os servidores registados através de um agente expirado em breve-para-ser, pelo menos, três meses antes da expiração. Pode verificar se um servidor registado está a utilizar uma versão mais antiga do agente na secção servidores registados de um serviço de sincronização de armazenamento.
- A duração de uma versão secundária do agente está vinculada à versão principal associada. Por exemplo, quando for lançada a versão de agente 3.0, as versões do agente 2. \* serão todas definida para expirar em conjunto.

> [!Note]
> Instalar uma versão do agente com um aviso de expiração irá apresentar um aviso, mas com êxito. A tentar instalar ou ligar a uma versão de agente expirado, não é suportada e será bloqueada.