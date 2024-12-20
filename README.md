# rabbitMQ-API-pedido

# Sobre a API

Este microsserviço faz parte do projeto de mensageria utilizando o rabbitMQ, e se comunica com mais dois microsserviços: o de notificação do pedido (https://github.com/kauannr/rabbitMQ-API-notificacao) e o de processamento do pedido (https://github.com/kauannr/rabbitMQ-API-processamento).
A funcionalidade dessa API, como o nome sugere, é a criação de pedidos de um cliente, contendo os itens do pedido, produto, valor de cada produto, a quantidade, o subtotal e o email para notificar o cliente.

# O que faz no rabbitMQ
A API está configurada para criar uma exchange no rabbitMQ para poder enviar o pedido em forma de mensagem para as filas que estarão vinculadas a essa exchange (no caso, a fila da API de notificação e a fila da API de processamento). Após a requisição retornar o status 201 created, o pedido é enviado para as duas APIs e elas executarão suas lógicas internas com esse pedido

# Exemplo de Json na requisição
```json
{
  "cliente": "TESTE",
  "itensDoPedido": [
    {
      "produto": {
        "nome": "Fone de ouvido sansumg",
        "valor": 56.0
      },
      "quantidade": 2,
      "subTotal": 0.0
    },
    {
      "produto": {
        "nome": "Fone de ouvido xiomi",
        "valor": 56.0
      },
      "quantidade": 1
    }
  ],
  "emailParaNotificar": "teste1@gmail.com"
}
```
# Exemplo de Json de resposta
```json
{
  "id": "7318164b-4643-4b94-98a9-a9ec19f2f41b",
  "cliente": "TESTE",
  "itensDoPedido": [
    {
      "id": "7e34b553-cced-4141-9e4c-be76941a5124",
      "produto": {
        "id": "be40e25f-cf64-4d29-9604-1d4fdfae7ed7",
        "nome": "Fone de ouvido sansumg",
        "valor": 56.0
      },
      "quantidade": 2,
      "subTotal": 112.0
    },
    {
      "id": "8158cfdf-7884-4953-92e7-a8737183649f",
      "produto": {
        "id": "b006f27a-fa74-46dd-a97c-2da9c442f123",
        "nome": "Fone de ouvido xiomi",
        "valor": 56.0
      },
      "quantidade": 1,
      "subTotal": 56.0
    }
  ],
  "emailParaNotificar": "teste1@gmail.com",
  "valorTotal": 168.0,
  "instantePedido": "2024-12-17 01:12:33",
  "statusPedido": "APROVADO"
}
```
