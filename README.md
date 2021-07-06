# 🐰 Introdução ao RabbitMQ 🐇

- [🐰 Introdução ao RabbitMQ 🐇](#markdown-header--introdução-ao-rabbitmq-)
  - [Etapas](#markdown-header-etapas)
    - [Publisher](#markdown-header-publisher)
    - [Exchange](#markdown-header-exchange)
    - [Queue](#markdown-header-queue)
    - [Consumer](#markdown-header-consumer)
  - [Resumo](#markdown-header-resumo)
  - [Instalação com Docker e docker-compose](#markdown-header-instalação-com-docker-e-docker-compose)
    - [Acessando o painel RabbitMQ](#markdown-header-acessando-o-painel-rabbitmq)

![Ilustracao](https://media.giphy.com/media/6pa2yJv88FhcRTwW2t/source.gif)

[**RabbitMQ**](https://www.rabbitmq.com/) é um [message broker](https://en.wikipedia.org/wiki/Message_broker) projetado para comunicação entre aplicações - geralmente, microsserviços - servindo como um `middleware` entre multiplas aplicações, capaz de gerenciar e garantir o processamento de mensagens.

## Etapas

O sistema de mensageria funciona através de 4 etapas:

- [Publisher](#publisher)
- [Exchange](#exchange)
- [Queue](#queue)
- [Consumer](#consumer)

### Publisher

![Publisher Illustration](https://image.prntscr.com/image/cjykbCTsR06vEEdjz1Zdaw.png)

**Publisher** é a etapa de recebimento da mensagem pelo **RabbitMQ**.

É o momento em que a mensagem é criada por alguma aplicação conectada ao serviço **como um publisher** e envia uma mensagem.

### Exchange

![Exchange Illustration](https://image.prntscr.com/image/JVPHWmTiTfGtF2MrKbaeew.png)

**Exchange** é o processo de pré-manipulação da mensagem onde, [dependendo da regra da exchange](https://lostechies.com/derekgreer/2012/03/28/rabbitmq-for-windows-exchange-types/) a mensagem pode passar por uma ou mais [filas associadas à exchange](https://www.rabbitmq.com/tutorials/tutorial-four-python.html).

### Queue

![Queue Illustration](https://image.prntscr.com/image/ZO47-olATMuG0vv1Ty_hSg.png)

Após a determinação da exchange, a mensagem prossegue para uma das filas associadas e fica a espera do próximo [consumer](#consumer) disponível para receber a mensagem.

### Consumer

O **Consumer** representa a aplicação no extremo do fluxo, que irá receber a mensagem. Ela é a responsável pro cumprir com sua funcinalidade de acordo com a mensagem enviada e finalizar enviando um `feedback` de volta para o RabbitMQ informando se a mensagem foi processada com sucesso ou rejeitada.

## Resumo

RabbitMQ possibilita que a arquitetura de um projeto não necessite de algum acoplamento entre serviços distintos. Permitindo escalabilidade, confiabilidade na transição de mensagens e flexibilidade no fluxo do projeto, uma vez que um serviço é independente do outro.

## Instalação com [Docker](https://www.docker.com/products/docker-desktop) e docker-compose

É possivel iniciar um container RabbitMQ usando um simples comando docker:

```sh
docker run -d --hostname my-rabbit --name rabbit13 -p 8080:15672 -p 5672:5672 -p 25676:25676 rabbitmq:3-management
```

ou é possivel iniciar uma `stack` com docker composer:

```yaml
version: "3"
services:
  rabbitmq:
    image: rabbitmq:3-management-alpine
    container_name: 'rabbitmq'
    ports:
        - 5672:5672
        - 15672:15672
    volumes:
        - ./.docker-conf/rabbitmq/data/:/var/lib/rabbitmq/
        - ./.docker-conf/rabbitmq/log/:/var/log/rabbitmq
```

> Este arquivo está no repostório. é possível usar o comando `git clone` e usar o comando `docker-compose up`

### Acessando o painel RabbitMQ

![Login RabbitMQ](https://image.prntscr.com/image/rFG9dqNwR1_N4PwHpzftzQ.png)

Após o temrmino da inicialização do container RabbitMQ, estará disponível na porta **15672** o acesso web ao painel do RabbitMQ.

> O usuário e senha padrões são `guest`

Após o login So dashboard será mostrado

![Dashboard RabbitMQ](https://image.prntscr.com/image/xVISsDA-QR2iCAdt9A8f7Q.png)
