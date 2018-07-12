```json
{
    "queues": {
      "maxPollingInterval": 2000,
      "visibilityTimeout" : "00:00:30",
      "batchSize": 16,
      "maxDequeueCount": 5,
      "newBatchThreshold": 8
    }
}
```

|Propriedade  |Predefinição | Descrição |
|---------|---------|---------| 
|maxPollingInterval|60000|O intervalo máximo em milissegundos entre inquéritos de fila de mensagens em fila.| 
|visibilityTimeout|0|O intervalo de tempo entre tentativas durante o processamento de uma mensagem de falha.| 
|batchSize|16|O número de mensagens em fila que o runtime das funções em simultâneo recupera e processa em paralelo. Quando o número a ser processado obtém para o `newBatchThreshold`, o tempo de execução obtém outro lote e começa a processar essas mensagens. Portanto, é o número máximo de mensagens em simultâneo a ser processado por função `batchSize` plus `newBatchThreshold`. Este limite aplica-se em separado para cada função acionada por fila. <br><br>Se quiser evitar a execução paralela para mensagens recebidas numa fila, pode definir `batchSize` como 1. No entanto, esta definição elimina a simultaneidade apenas, desde que a aplicação de função for executada numa única máquina virtual (VM). Se a aplicação de funções aumenta horizontalmente para várias VMs, cada VM pode executar uma instância de cada função acionada por fila.<br><br>O máximo `batchSize` é 32. | 
|maxDequeueCount|5|O número de vezes para tentar processar uma mensagem antes de passar para a fila não processáveis.| 
|newBatchThreshold|batchSize/2|Sempre que obtém o número de mensagens a ser processadas em simultâneo para baixo para este número, o tempo de execução obtém outro lote.| 



