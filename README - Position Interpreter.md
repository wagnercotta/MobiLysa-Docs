Serviço - Position Interpreter
==================

Este programa corresponde a um módulo componente do Sistema MobiLysa – Serviço de Localização e Navegação do Robô Lysa para Ambientes Internos. Este sistema corresponde a um conjunto de módulos que juntos permitem que o cão-guia robô Lysa seja localizado em um ambiente interno e que, a partir do destino desejado, um planejamento de navegação seja realizado. Tanto a localização, quanto navegação são efetuadas em tempo real até que o robô atinja o seu destino final. O ambiente deve estar equipado com uma rede de câmeras IP e possuir cobertura de WiFi por toda a área onde o robô deve se movimentar.

Especificamente, este módulo pode ser descrito da seguinte forma:
Position Interpreter (PI) é um serviço que realiza a conversão de uma palavra, referente a uma posição, para coordenadas entendíveis pelo serviço PathPlanner. O PI aguarda o recebimento de uma mensagem serializada do tipo is_msg.Phrase. Ao receber, ele realiza a decodificação da mensagem, extrai a identificação do PathPlanner que receberá as coordenadas, realiza a conversão para coordenadas e as envia para o serviço PathPlanner. A conversão para coordenadas é feita através da correspondência da palavra recebida com um dicionário, contendo palavras em português, inglês e suas respectivas coordenadas. Caso haja correspondência, uma mensagem do tipo is_msg.PathRequest é criada contendo as informações do gateway e, posteriormente, enviada para o serviço PathPlanner. Após o envio, o serviço PI retorna ao estado de espera de mensagem, para repetir o ciclo ao receber uma nova mensagem.


Dependências:
-----
Este serviço não possui dependências em relação a outros serviços.

Eventos:
--------
<img width=850/> ⇒ Mensagem recebida | <img width=850/> Encaminha ⇒ | <img width=500/> Descrição  
:------------ | :-------- | :----------
:incoming_envelope: **tópico:** `SpeechRecognition.*.Phrase` <br> :gem: **tipo de mensagem:** [Phrase] | :incoming_envelope: **tópico:** `PathPlanner.AreaID.{area_id}.GetPath` <br> :gem: **tipo de mensagem::** [PathRequest] | `Realização da conversão de uma palavra, referente a uma posição, para coordenadas entendíveis pelo serviço PathPlanner`

[Phrase]: https://github.com/labviros/is-msgs/tree/master/docs#is.common.Phrase
[PathRequest]: https://github.com/labviros/is-msgs/tree/master/docs#is.robot.PathRequest
```
{area_id} = número identificador do ambiente, passado no arquivo de configuração.
```

Configuração:
----------------
A configuração do serviço é realizada através de um arquivo JSON, passado como argumento. Um exemplo de configuração pode ser encontrado em: [`etc/conf/options.json`]

Exemplos:
------------
Um exemplo, que realiza um conversão de uma palavra, referente a uma posição, para coordenadas entendíveis pelo serviço PathPlanner, pode ser encontrado em [`examples/client.py`](examples/client.py).


