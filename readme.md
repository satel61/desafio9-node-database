# Desafio 09: Relacionamentos com banco de dados no Node.js


## Sobre o desafio

Nesse desafio, você vai estar criando uma nova aplicação para aprender novas coisas e treinar o que você aprendeu até agora no Node.js junto ao TypeScript, incluindo o uso de banco de dados com o TypeORM, e relacionamentos ManyToMany!

Essa será uma aplicação que deve permitir a criação de clientes, produtos e pedidos, onde o cliente pode gerar novos pedidos de compra de certos produtos, como um pequeno e-commerce.


## Rotas da aplicação

__POST /customers__: A rota deve receber name e email dentro do corpo da requisição, sendo o name o nome do cliente a ser cadastrado. Ao cadastrar um novo cliente, ele deve ser armazenado dentro do seu banco de dados e deve ser retornado o cliente criado. Ao cadastrar no banco de dados, na tabela customers deverá possuir os campos name, email, created_at, updated_at.
Dica: Antes de criar um novo cliente, sempre verifique se já existe um cliente com o mesmo e-mail. Caso ela exista, retorne um erro.

__POST /products:__ Essa rota deve receber name, price e quantity dentro do corpo da requisição, sendo o name o nome do produto a ser cadastrado, price o valor unitário e quantity a quantidade existente em estoque do produto. Com esses dados devem ser criados no banco de dados um novo produto com os seguintes campos: name, price, quantity, created_at, updated_at.
__Dica 1__: Antes de criar um novo produto, sempre verifique se já existe um produto com o mesmo nome. Caso ela exista, retorne um erro.

__Dica 2:__ Para o campo price, você pode utilizar o type como decimal na sua migration, passando também as propriedades precision e scale.

__POST /orders/:__ Nessa rota você deve receber no corpo da requisição o customer_id e um array de products, contendo o id e a quantity que você deseja adicionar a um novo pedido. Aqui você deve cadastrar na tabela orders um novo pedido, que estará relacionado ao customer_id informado, created_at e updated_at . Já na tabela orders_products, você deve armazenar o product_id, order_id, price e quantity, created_at e updated_at.
__Dica 1:__ Nessa funcionalidade, você precisará fazer um relacionamento de N:N entre produtos e pedidos, onde vários produtos podem estar em vários pedidos, com isso você deve sempre armazenar o valor do produto no momento da compra e a quantidade pedida na tabela pivô com nome de orders_products, essa tabela vai ter os campos id, order_id, product_id, quantity, price, created_at e updated_at. Para esse tipo de relacionamento, você pode verificar na documentação do TypeORM sobre como fazer relacionamento muitos-para-muitos com propriedades customizadas.

__Dica 2:__ Além disso, você pode também utilizar o método de cascade do TypeORM, que irá adicionar na sua tabela order_products os produtos que você passar por parametro para a entidade de orders automaticamente, você pode saber mais sobre isso aqui: Opção de cascade

__Dica 3:__ A sua requisição do insomnia deve enviar um JSON com o formato parecido com esse:
```
{
  "customer_id": "e26f0f2a-3ac5-4c21-bd22-671119adf4e9",
  "products": [
    {
      "id": "ce0516f3-63ae-4048-9a8a-8b6662281efe",
      "quantity": 5
    },
    {
      "id": "82612f2b-3f31-40c6-803d-c2a95ef35e7c",
      "quantity": 7
    }
  ]
}
```
__Dica 4:__ Uma chamada a essa rota deve retornar os dados do cliente, produtos do pedido e id do pedido, num formato parecido com o seguinte:
```
{
  "id": "5cbc4aa2-b3dc-43f9-b121-44c1e416fa92",
  "created_at": "2020-05-11T07:09:48.767Z",
  "updated_at": "2020-05-11T07:09:48.767Z",
  "customer": {
    "id": "e26f0f2a-3ac5-4c21-bd22-671119adf4e9",
    "name": "Rocketseat",
    "email": "oi@rocketseat.com.br",
    "created_at": "2020-05-11T06:20:28.729Z",
    "updated_at": "2020-05-11T06:20:28.729Z"
  },
  "order_products": [
    {
      "product_id": "ce0516f3-63ae-4048-9a8a-8b6662281efe",
      "price": "1400.00",
      "quantity": 5,
      "order_id": "5cbc4aa2-b3dc-43f9-b121-44c1e416fa92",
      "id": "265b6cbd-3ab9-421c-b358-c2e2b5b3b542",
      "created_at": "2020-05-11T07:09:48.767Z",
      "updated_at": "2020-05-11T07:09:48.767Z"
    },
    {
      "product_id": "82612f2b-3f31-40c6-803d-c2a95ef35e7c",
      "price": "500.00",
      "quantity": 7,
      "order_id": "5cbc4aa2-b3dc-43f9-b121-44c1e416fa92",
      "id": "ae37bcd6-7be7-47b9-b277-afee35aab4e4",
      "created_at": "2020-05-11T07:09:48.767Z",
      "updated_at": "2020-05-11T07:09:48.767Z"
    }
  ]
}
```
__GET /orders/:id:__ Essa rota deve retornar as informações de um pedido específico, com todas as informações que podem ser recuperadas através dos relacionamentos entre a tabela orders, customers e orders_products.
Dica: Aqui você pode utilizar a opção eager do TypeORM ou passar a opção relations para o método findOne do TypeORM, informando os nomes das tabelas que você deseja buscar o relacionamento.

## Específicação dos testes

__Antes de rodar os testes, crie um banco de dados com o nome "gostack_desafio09_tests" para que todos os testes possam executar corretamente.__

__should be able to create a new customer:__ Para que esse teste passe, sua aplicação deve permitir que um cliente seja criado, e retorne um json com o cliente criado.

__should not be able to create a customer with one e-mail thats already registered:__ Para que esse teste passe, sua aplicação deve retornar um erro quando você tentar cadastrar um cliente com um e-mail que já esteja cadastrado no banco de dados.

__should be able to create a new product:__ Para que esse teste passe, sua aplicação deve permitir que um produto seja criado, e retorne um json com o produto criado.

__should not be able to create a duplicated product:__ Para que esse teste passe, sua aplicação deve retornar um erro quando você tentar cadastrar um produto com um nome que já esteja cadastrado no banco de dados.

__should be able to create a new order:__ Para que esse teste passe, sua aplicação deve permitir que um pedido seja criado, e retorne um json com o todos os dados do pedido criado.

__should not be able to create an order with a invalid customer:__ Para que esse teste passe, sua aplicação não deve permitir a criação de um novo pedido com um cliente que não existe no banco de dados, retornando um erro.

__should not be able to create an order with invalid products:__ Para que esse teste passe, sua aplicação não deve permitir a criação de um novo pedido com um produtos que não existem no banco de dados, retornando um erro caso um ou mais dos produtos enviados não exista no banco de dados.

__should not be able to create an order with products with insufficient quantities:__ Para que esse teste passe, sua aplicação não deve permitir a criação de um novo pedido com um produtos que não possuem quantidade disponível, retornando um erro caso um ou mais dos produtos enviados não possua a quantidade necessária.

__should be able to subtract an product total quantity when it is ordered:__ Para que esse teste passe, sua aplicação deve permitir que, quando um novo pedido for criado, seja alterada a quantidade total dos produtos baseado na quantidade pedida.

__should be able to list one specific order:__ Para que esse teste passe, você deve permitir que a rota orders/:id retorne um pedido, contendo todas as informações do pedido com o relacionamento de customer e order_products.
