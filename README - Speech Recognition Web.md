Serviço - Speech Recognition Web
==================

Este programa corresponde a um módulo componente do Sistema MobiLysa – Serviço de Localização e Navegação do Robô Lysa para Ambientes Internos. Este sistema corresponde a um conjunto de módulos que juntos permitem que o cão-guia robô Lysa seja localizado em um ambiente interno e que, a partir do destino desejado, um planejamento de navegação seja realizado. Tanto a localização, quanto navegação são efetuadas em tempo real até que o robô atinja o seu destino final. O ambiente deve estar equipado com uma rede de câmeras IP e possuir cobertura de WiFi por toda a área onde o robô deve se movimentar.
Especificamente, é responsável por realizar o reconhecimento de frases a partir da fala, utilizando um microfone conectado ao computador. Este módulo consistem em um servidor web responsável por renderizar uma página exibida que é exibida em um navegador, contendo um botão para dar início à captura, além de uma região que é fornecido um retorno visual das frases reconhecidas. Para cada frase reconhecida, uma mensagem é enviada através de um barramento de mensagens contendo a lista de palavras na ordem em que foram pronunciadas, além do idioma desta. O idioma por sua vez, pode ser previamente configurado, e considera como o padrão sendo o português do Brasil.

Dependências:
-----
Este serviço não possui dependências em relação a outros serviços. Entretanto, deve ser executado em um navegador baseado no Chromium.

Eventos:
--------
<img width=850/> ⇒ Mensagem recebida | <img width=850/> Encaminha ⇒ | <img width=500/> Descrição  
:------------ | :-------- | :----------
:microphone: Uma frase dita em um microfone | :incoming_envelope: **tópico:** `SpeechRecognition.{microphone_id}.Phrase` <br> :gem: **tipo de mensagem:** [Phrase] | [Stream] Reconhece frases ditas em um microfone e publica o conteúdo, a confiaça do reconhecimento e a linguagem.

[Phrase]: https://github.com/labviros/is-msgs/tree/master/docs#is.common.Phrase

Configuração:
----------------
<img width=1000/> Environment variable| <img width=1000/> Description | <img width=200/> Default  
:------------ | :-------- | :----------
BROKER_URI | Endereço do broker utilizado para publicação das mensagens | `amqp://localhost:5672`
HOST_IP_ADDRESS | Quando implementado em um cluster Kubernetes usando um serviço `NodePort` para obter um serviço acessível, um endereço IP de um dos nós do cluster deve ser especificado. Isso é necessário para que o navegador possa enviar o conteúdo da frase para o back-end. | `localhost`
EXTERNAL_SERVICE_PORT | Assim como `HOST_IP_ADDRESS`, a porta do serviço externo deve ser especificada ao executar em um cluster Kubernetes. | `5000`
RECOGNITION_LANGUAGE | Identificador de idioma. Uma lista dos disponíveis pode ser encontrada [aqui] (https://docs.microsoft.com/en-us/cpp/c-runtime-library/language-strings?view=vs-2019). Para assumir o idioma da navegação, deixe a variável de ambiente vazia. | `pt-BR`
MICROPHONE_ID | Número identificador usado para publicar mensagens. | `0`

Executando:
--------

Em um ambiente de desenvolvimento, instale os requisitos com o seguinte comando:

```shell
pip install -r requirements.txt
```
And then execute:
```shell
python -m server
```

O ambiente de produção requer um servidor WSGI como o `gunicorn`. Ele é instalado como uma dependência e é usado no arquivo `Dockerfile` na instrução `CMD`.

### Habilitando o acesso ao microfone no Chrome para origens não seguras

A execução em um ambiente no qual os serviços da web são entregues sem um certificado SSL, precisará de uma configuração extra no seu navegador para permitir o acesso ao microfone.

No seu navegador, acesse o URL `chrome: // flags / # inseguro-trate-inseguro-origem-como-segura` e encontre a seção` Origens inseguras tratadas como seguras`. Adicione à lista os hosts que você deseja ignorar essa restrição de segurança e ative-a. Lembre-se de colocar o URL completo, por exemplo `http: //10.10.2.1: 30500`.
