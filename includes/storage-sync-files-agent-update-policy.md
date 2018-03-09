O agente de sincronização de ficheiros do Azure é atualizado regularmente para adicionar novas funcionalidades e a resolver problemas. Recomendamos que configure o Microsoft Update para obter as atualizações para o agente de sincronização de ficheiros de Azure conforme estiverem disponíveis. Iremos compreender algumas organizações, como controlar estritamente e testar as atualizações.

Para implementações que utilizam versões anteriores do agente de sincronização de ficheiros do Azure:

- Após o lançamento inicial de uma nova versão principais, o serviço de sincronização de armazenamento honra a versão principal anterior durante três meses. Por exemplo, o serviço de sincronização de armazenamento suporta a versão 1. \* durante três meses após o lançamento da versão 2. \*.
- Depois de ter decorrido durante três meses, o serviço de sincronização de armazenamento bloqueia servidores registado com a versão expirada de sincronização com os respetivos grupos de sincronização.
- Durante três meses é honrada a versão principal anterior, todas as correções de erros, visite apenas a versão de principal atual (nova).

> [!Note]  
> Se a versão de sincronização de ficheiros do Azure expirar no prazo de três meses, está a notificado através de notificação de alerta no portal do Azure.
