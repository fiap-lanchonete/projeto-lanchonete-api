# Tech Challenge - Pós-Tech SOAT - FIAP

Alunos:

* Pedro Henrique de Marins da Silva - RM348617
* Gustavo Jorge Franco Teles dos Santos - RM349553

## O PROBLEMA

Há uma lanchonete de bairro que está expandindo devido seu grande sucesso. Porém, com a expansão e sem um sistema de controle de pedidos, o atendimento aos clientes pode ser caótico e confuso. Por exemplo, imagine que um cliente faça um pedido complexo, como um hambúrguer personalizado com ingredientes específicos, acompanhado de batatas fritas e uma bebida. O atendente pode anotar o pedido em um papel e entregá-lo à cozinha, mas não há garantia de que o pedido será preparado corretamente.

Sem um sistema de controle de pedidos, pode haver confusão entre os atendentes e a cozinha, resultando em atrasos na preparação e entrega dos pedidos. Os pedidos podem ser perdidos, mal interpretados ou esquecidos, levando à insatisfação dos clientes e a perda de negócios.

Em resumo, um sistema de controle de pedidos é essencial para garantir que a lanchonete possa atender os clientes de maneira eficiente, gerenciando seus pedidos e estoques de forma adequada. Sem ele, expandir a lanchonete pode acabar não dando certo, resultando em clientes insatisfeitos e impactando os negócios de forma negativa.

Para solucionar o problema, a lanchonete irá investir em um sistema de autoatendimento de fast food, que é composto por uma série de dispositivos e interfaces que permitem aos clientes selecionar e fazer pedidos sem precisar interagir com um atendente, com as seguintes

## REPOSITÓRIOS:
  - [1 repositório para Lambda](https://github.com/fiap-lanchonete/auth-lambda)
  - [1 repositório para Infra Kubernetes](https://github.com/fiap-lanchonete/projeto-lanchonete-infra)
  - [1 repositório para infra do banco de dados](https://github.com/fiap-lanchonete/infra-db)
  - [1 repositório para aplicação que é executada no Kubernetes](https://github.com/fiap-lanchonete/projeto-lanchonete-api)

## BANCO DE DADOS
![Schema](https://i.imgur.com/SH3RgVa.png)

Para esta aplicação, optamos por utilizar o Amazon RDS juntamente com o PostgreSQL. O Amazon RDS é um serviço de banco de dados relacional que facilita a configuração, a operação e a escalabilidade de bancos de dados na AWS. Ele fornece um gerenciamento de banco de dados relacional econômico e redimensionável. Já o PostgreSQL é um sistema de gerenciamento de banco de dados relacional (RDBMS) poderoso, de código aberto e orientado a objetos. Ele oferece recursos avançados, como suporte completo a chaves estrangeiras, junções, visualizações, triggers e procedimentos armazenados. A combinação dessas duas tecnologias nos permite gerenciar eficientemente nossos dados e garantir um desempenho robusto para nossa aplicação.

## AUTENTICAÇÃO
![Authentication Diagram](https://i.imgur.com/jhxn4db.jpg)

O diagrama acima ilustra o fluxo de autenticação do nosso sistema. O usuário inicia o processo fornecendo o CPF, ou de forma anônima. Em seguida, o sistema valida essas credenciais e, se estiverem corretas, gera um token de acesso. Este token é então usado para autenticar todas as solicitações subsequentes do usuário. Isso garante que apenas usuários autenticados possam acessar recursos protegidos (criação de pedido).

### FLUXO DE PAGAMENTO COM MERCADO PAGO
<img  src="https://i.imgur.com/Df8C9c6.png" />

### ROTAS DA API

- **POST /v1/order** Está rota permite criar um pedido.

- **POST /v1/order/payment/:order_id:** Esta rota permite criar um pagamento para um pedido específico identificado pelo parâmetro `order_id` usando o `Mercado Pago`.

- **GET /v1/order/payment/:order_id:** Esta rota verifica status de pagamento e seu descritivo para o pedido específico identificado pelo parâmetro `order_id` usando o `Mercado Pago`.

- **PUT /v1/order/preparation/:order_id:** Esta rota permite atualizar o estado de preparação de um pedido específico identificado pelo parâmetro `order_id`.

- **PUT /v1/order/ready/:order_id:** Esta rota permite atualizar o estado de "pronto para entrega" de um pedido específico identificado pelo parâmetro `order_id`.

- **PUT /v1/order/delivered/:order_id:** Esta rota permite atualizar o estado de entrega de um pedido específico identificado pelo parâmetro `order_id`.

- **PUT /v1/order/product/:order_id:** Esta rota permite atualizar informações sobre os produtos em um pedido específico identificado pelo parâmetro `order_id`.

- **GET /v1/order/queue:** Esta rota permite obter informações sobre a fila de pedidos, geralmente usada para listar os pedidos pendentes.

- **POST /v1/product:** Esta rota permite criar um novo produto.

- **GET /v1/product:** Esta rota permite obter a lista de produtos disponíveis.

- **GET /v1/product/:id:** Esta rota permite obter informações sobre um produto específico identificado pelo parâmetro `id`.

- **PUT /v1/product/:id:** Esta rota permite atualizar as informações de um produto específico identificado pelo parâmetro `id`.

- **DELETE /v1/product/:id:** Esta rota permite excluir um produto específico identificado pelo parâmetro `id`.

- **POST /v1/user:** Esta rota permite criar um novo usuário.

- **GET /v1/user:** Esta rota permite obter a lista de usuários registrados.

- **GET /v1/user/:id:** Esta rota permite obter informações sobre um usuário específico identificado pelo parâmetro `id`.

- **PUT /v1/user/:id:** Esta rota permite atualizar informações de um usuário específico identificado pelo parâmetro `id`.

- **DELETE /v1/user/:id:** Esta rota permite excluir um usuário específico identificado pelo parâmetro `id`.

- **POST /v1/category:** Esta rota permite criar uma nova categoria.

- **GET /v1/category:** Esta rota permite obter a lista de categorias disponíveis.

- **GET /v1/category/:id:** Esta rota permite obter informações sobre uma categoria específica identificada pelo parâmetro `id`.

- **PUT /v1/category/:id:** Esta rota permite atualizar informações de uma categoria específica identificada pelo parâmetro `id`.

- **DELETE /v1/category/:id:** Esta rota permite excluir uma categoria específica identificada pelo parâmetro `id`.

## ARQUITETURA

A arquitetura dessa API segue os princípios de Arquitetura Hexagonal (Ports and Adapters). Essa arquitetura pode ser dividida em duas partes:

  - Condutores: (ou atores primários): Podem ser definidos como atores que iniciam a interação. Por exemplo, um condutor pode ser um controller que recebe um input de um usuário e redireciona a aplicação utilizando uma Port(interface).
  - Conduzidos: (também conhecido como atores secundários): São parte da camada de infrasctructure da aplicação. Podem ser utilizados para adicionar funcionalidades para o domínio da aplicação. Exemplos: Banco de dados, APIs externas.

![Untitled Diagram drawio](https://github.com/rickwalking/projeto-lanchonete/assets/25574889/12ddab40-97ec-4157-a1ae-803d258654ea)

  * Esquema de Domain Driven Design (DDD) no [Miro](https://miro.com/welcomeonboard/TG9pRTJMU1BNb2d4WUZvdE9PVHd1cEZudmpaczNhdDNMOVVmeDE0S0VOZkVDSmFDSG5uaU0waUZzdFV5Q1h5aXwzNDU4NzY0NTU1MDkxMDI0MTAxfDI=?share_link_id=171801921364)

## PROJETO

Este projeto está pronto para ser executado em um ambiente Docker. Por este motivo, será necessária apenas a instalação do Docker, não sendo necessária a instalação manual do projeto. Também não será necessária a instalação manual do banco de dados.

Caso não tenha o Docker instalado, siga as instruções para seu sistema operacional na [documentação oficial do Docker](https://docs.docker.com/get-docker/).

### PARA EXECUTAR EM AMBIENTE DE DESENVOLVIMENTO:

* Clone este repositório em seu computador;
* Entre no diretório local onde o repositório foi clonado;
* Altere o arquivo .env.example para .env 
* Preencha os campos dentro arquivo
* Utilize o comando ` docker compose up --build` para "build" e subir o servidor local e expor a porta 3000 em `localhost`. Além de `dev` também subirá o serviço `db` com o banco de dados de desenvolvimento.

**IMPORTANTE:** Esta API está programada para ser acessada a partir de `http://localhost:3000` e o banco de dados utiliza a porta `5432`. Certifique-se de que não existam outros recursos ocupando as portas acima.

Para desativar os serviços, execute o comando `docker-compose down`.
