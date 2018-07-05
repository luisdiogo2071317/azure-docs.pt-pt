A tarefa produz um ficheiro de saída do JSON que contém metadados sobre rostos detetados e monitorizados. Os metadados incluem coordenadas que indica a localização de rostos, bem como um número de identificação de face indicando o rastreamento dessa pessoa. Números de ID do rosto são suscetíveis a repor em circunstâncias, o mostrador frontal caso de perda ou overlapped no frame, resultando em alguns indivíduos introdução atribuídos a vários IDs.

A saída JSON inclui os seguintes elementos:

### <a name="root-json-elements"></a>Elementos de JSON de raiz

| Elemento | Descrição |
| --- | --- |
| versão |Refere-se para a versão da API de vídeo. |
| escala temporal |"Ticks" por segundo do vídeo. |
| deslocamento |Este é o desvio de tempo para carimbos. Na versão 1.0 de APIs de vídeo, ela sempre será 0. No futuro cenários que suportamos, pode alterar este valor. |
| largura, elevado |A largura e o elevado número do quadro de vídeo de saída, em pixéis.|
| framerate |Quadros por segundo do vídeo. |
| [fragments](#fragments-json-elements) |Os metadados é segmentado cópias em diferentes segmentos chamados fragmentos. Cada fragmento contém um início, a duração, o número do intervalo e o evento (s). |

### <a name="fragments-json-elements"></a>Elementos JSON de fragmentos

|Elemento|Descrição|
|---|---|
| start |A hora de início do primeiro evento na "ticks". |
| duração |O comprimento do fragmento, em "ticks". |
| índice | (Se aplica apenas para o Azure Media Redactor) define o índice do quadro do evento atual. |
| intervalo |O intervalo de cada entrada de evento dentro do fragmento, na "ticks". |
| eventos |Cada evento contém os rostos detetados e monitorizados dentro desse período de tempo. É uma matriz de eventos. A matriz externa representa um intervalo de tempo. A matriz interna é constituído por 0 ou mais eventos que ocorreram neste ponto no tempo. Um [Reto de fecho vazio] significa que nenhum rostos foram detetados. |
| ID |O ID do mostrador da que está a ser controlado. Este número pode alterar inadvertidamente se um rosto ficar detectado. Uma determinada pessoa deve ter o mesmo ID em todo o vídeo geral, mas isso não pode ser garantido devido a limitações no algoritmo de deteção (oclusão, etc.). |
| x, y |A parte superior esquerda X e Y coordenadas do mostrador da caixa num normalizado de dimensionamento de 0,0 a 1,0 delimitadora. <br/>-X e Y coordenadas são relativamente à paisagem sempre, portanto, se tiver um retrato vídeo (ou vantagem pendente, no caso do iOS), terá transpor as coordenadas em conformidade. |
| largura, altura |A largura e altura do mostrador da caixa num normalizado de dimensionamento de 0,0 a 1,0 delimitadora. |
| facesDetected |Isso é encontrado no final dos resultados JSON e resume o número de faces que o algoritmo detectado durante o vídeo. Uma vez que os IDs podem ser repostos inadvertidamente, se um rosto torna-se não detetado (por exemplo, o mostrador apague ecrã, se parece distância), este número não poderá igual sempre o número VERDADEIRO de rostos em vídeo. |
