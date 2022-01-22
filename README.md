<h1 align="center">
  <img alt="KenzieHub" title="KenzieHub" src="https://kenzie.com.br/images/logoblue.svg" width="100px" />
</h1>

<h1 align="center">
  Hamburgueria - API
</h1>

<p align = "center">
Este é o backend de Hamburgueria.
</p>

<p align="center">
  <a href="#endpoints">Endpoints</a>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</p>

## **Endpoints**

O url base da API é https://hamburgueria-dmsatiro.herokuapp.com/

## Rotas que não precisam de autenticação

<h2 align ='center'> Listando produtos </h2>

`GET /products - FORMATO DA RESPOSTA - STATUS 200`

```json
[[
  {
    "id": 1,
    "name": "Hamburguer",
    "category": "Sanduíches",
    "price": 14,
    "img": "https://i.ibb.co/fpVHnZL/hamburguer.png"
  },
  {
    "id": 2,
    "name": "X-Burguer",
    "category": "Sanduíches",
    "price": 16,
    "img": "https://i.ibb.co/djbw6LV/x-burgue.png"
  },
  {
    "id": 3,
    "name": "Big Kenzie",
    "category": "Sanduíches",
    "price": 18,
    "img": "https://i.ibb.co/FYBKCwn/big-kenzie.png"
  },
  {
    "id": 4,
    "name": "Fanta Guaraná",
    "category": "Bebidas",
    "price": 5,
    "img": "https://i.ibb.co/cCjqmPM/fanta-guarana.png"
  },
  {
    "id": 5,
    "name": "Coca Cola",
    "category": "Bebidas",
    "price": 7,
    "img": "https://i.ibb.co/fxCGP7k/coca-cola.png"
  },
  {
    "id": 6,
    "name": "McShake Ovomaltine",
    "category": "Bebidas",
    "price": 10,
    "img": "https://i.ibb.co/QNb3DJJ/milkshake-ovomaltine.png"
  }
]
```

Podemos utilizar os query params para filtrar a lista. Pode-se filtrar por nome ou categoria, por exemplo.
`GET /products?name=Hamburguer - FORMATO DA RESPOSTA - STATUS 200`
Pode-se utilizar uma correspondência aproximada da busca com "like"
`GET /products?name_like=hamb - FORMATO DA RESPOSTA - STATUS 200`

<h2 align ='center'> Criação de usuário </h2>
`POST /register - FORMATO DA REQUISIÇÃO`
```json
{
  "name": "eduardo",
  "email": "eduardo@yahoo.com",
  "password": "123456"
}
```
Caso dê tudo certo, a resposta será assim:
`POST /users - FORMATO DA RESPOSTA - STATUS 201`
```json
{
  "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6Im1vbmljYUB5YWhvby5jb20uYnIiLCJpYXQiOjE2NDE5MDAxNjIsImV4cCI6MTY0MTkwMzc2Miwic3ViIjoiMiJ9.sS9466Xd8_n30fU7gThr7xtnWQcAGi0F9M_Hh02Ya6I",
  "user": {
    "email": "eduardo@yahoo.com",
    "name": "Eduardo",
    "id": 2
  }
}
```
1. Os campos email e password são obrigatórios para a requisição.
2. A API já retorna o token no momento do cadastro, podendo-se dispensar o usuário voltar para a página de login.
<h2 align ='center'> Possíveis erros </h2>
Caso você acabe errando e mandando algum campo errado, a resposta de erro será assim:
Necessário ter email e senha.
`POST /register - `
` FORMATO DA RESPOSTA - STATUS 400`
```json
"Email and password are required"
```
Necessário formato de email válido.
`POST /register - `
` FORMATO DA RESPOSTA - STATUS 400`
```json
"Email format is invalid"
```
Email já cadastrado:
`POST /register - `
` FORMATO DA RESPOSTA - STATUS 400`
```json
"Email already exists"
```
<h2 align = "center"> Login </h2>
`POST /login - FORMATO DA REQUISIÇÃO`
```json
{
  "email": "eduardo@yahoo.com",
  "password": "123456"
}
```
Caso dê tudo certo, a resposta será assim:
`POST /login - FORMATO DA RESPOSTA - STATUS 201`
```json
{
  "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6Im1vbmljYUB5YWhvby5jb20uYnIiLCJpYXQiOjE2NDE5MDQyMTAsImV4cCI6MTY0MTkwNzgxMCwic3ViIjoiMiJ9.VlXStsfP2m3PxELxON0aYh2bAq6IRL7FEymeo0Vvg_I",
  "user": {
    "email": "eduardo@yahoo.com",
    "name": "Eduardo",
    "id": 2
  }
}
```
## Rotas que necessitam de autorização
Rotas que necessitam de autorização deve ser informado no cabeçalho da requisição o campo "Authorization", dessa forma:
> Authorization: Bearer {token}
> Após o usuário estar logado, ele deve conseguir cadastrar cometários é os veículos que possui.
<h2 align ='center'> Adicionar produtos ao carrinho e fazer alerações</h2>
`POST /cart - FORMATO DA REQUISIÇÃO`
```json
{
  "name": "Hamburguer",
  "category": "Sanduíches",
  "price": 14,
  "img": "https://i.ibb.co/fpVHnZL/hamburguer.png",
  "Quantidade": 1,
  "userId": "4"
}
```
1. No momento da criação a quantidade sempre deverá ser 1 caso não exista um produto com o mesmo nome.
2. É necessário informar o campo userId, pois somente o usuário que criou terá acesso para fazer alterações:
`PATCH /cart/:product_id - FORMATO DA REQUISIÇÃO`
```json
{
  "Quantidade": 2,
  "userId": "4"
}
```
3. Caso não informe o userId tanto na criação quanto na alteração respectivamente os seguintes erros:
```json
"Private resource creation: request body must have a reference to the owner id"
```
```json
"Cannot read property 'userId' of undefined"
```
Também é possível excluir utilizando este endpoint, desde que seja um produto do cart do próprio usuário:
`DELETE /cart/:product_id`
```
Não é necessário um corpo da requisição.
```
<h2 align ='center'> Atualizando os dados do perfil </h2>
Precisamos estar logados, com o token no cabeçalho da requisição. Estes endpoints são para atualizar seus dados do usuário como senha e mais informações.
Endpoint para atualizar um dado específico do usuário. É possível fazer alteração por outro email válido que não esteja sendo utilizado ou da senha:
`PATCH /users/:user_id - FORMATO DA REQUISIÇÃO`
```json
{
  "password": 20
}
```
Nesse endpoint é possível modificar o perfil do usuário em sua totalidade, incluindo ou excluindo propriedades, ou seja, o que não for incluído na requisição será deletado, com exceção do email e senha que são obrigatórios.
`PUT /users/:user_id - FORMATO DA REQUISIÇÃO`
```json
{
  "email": "edauardo@yahoo.com",
  "password": "123456",
  "new_property": "new_property"
}
