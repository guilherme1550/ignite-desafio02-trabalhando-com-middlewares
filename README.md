<h1 align="center">Ignite Desafio 02 - Trabalhando com middlewares</h1>

<p align="center">
  <a href="#-Projeto">Desafio</a>&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;
  <a href="#-Middlewares-da-aplicacão">Middlewares da aplicacão</a>&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;
  <a href="#-Testes-dos-middlewares">Testes dos middlewares</a>&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;
  <a href="#-Tecnologias">Tecnologias</a>&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;
  <a href="#-Como-executar">Como executar</a>
</p>
<br/>

## 📙 Desafio

Neste desafio, temos uma aplicacão em Node.js, onde foi possível trabalhar mais a fundo com middlewares no Express.

Esta aplicacão gerencia tarefas(ou todos), onde é permitido a criacão de um usuário com `name` e `username`, bem como fazer o CRUD de *todos*:

- Criar um novo *todo*;
- Listar todos os *todos*;
- Alterar o `title` e `deadline` de um *todo* existente;
- Marcar um *todo* como feito;
- Excluir um *todo*;

Todas essas informacões é para cada usuário em específico. Cada usuário possuí um plano grátis onde é possível cadastrar até dez *todos* ou um plano Pro que permite criar *todos* ilimitados, sendo utilizado middlewares para realizar as validações necessárias.

Não foi necessário realizar alterações nos testes, nem nas rotas, apenas nos middlewares que estão no arquivo 📃 index.js.
<br/><br/>

## 🔸 Middlewares da aplicacão

### checksExistsUserAccount

Esse middleware é responsável por receber o `username` do usuário pelo header e validar se existe ou não um usuário com o `username` passado. Caso exista, o usuário é repassado para o `request` e a função `next` é chamada.

### checksCreateTodosUserAvailability

Esse middleware recebe o usuário já dentro do `request` e chama a função `next` apenas se esse usuário ainda estiver no plano grátis e ainda não possuir 10 *todos* cadastrados ou se ele já estiver com o plano Pro ativado. 

### checksTodoExists

Esse middleware recebe o `username` de dentro do header e o `id` de um *todo* de dentro de `request.params`. Valida o usuário, verifica se o `id` é um uuid e também valida se esse `id` pertence a um *todo* do usuário informado.

Com todas as validações passando, o *todo* encontrado é passado para o `request` assim como o usuário encontrado também e a função next é chamada.

### findUserById

Esse middleware possui um funcionamento semelhante ao middleware `checksExistsUserAccount` mas a busca pelo usuário é feita através do `id` de um usuário passado por parâmetro na rota. Caso o usuário tenha sido encontrado, o mesmo é repassado para dentro do `request.user` e a função `next` é chamada.

## 🔸 Testes dos middlewares

- Should be able to find user by username in header and pass it to request.user

Para que esse teste passe, deve-se permitir que o middleware `checksExistsUserAccount` receba um `username` pelo header do request e caso um usuário com o mesmo `username` exista, ele deve ser colocado dentro de `request.user` e, ao final, deve retornar a chamada da função `next`.

- Should not be able to find a non existing user by username in header

Para que esse teste passe, no middleware `checksExistsUserAccount` deve-se retornar uma resposta com status `404` caso o `username` passado pelo header da requisição não pertença a nenhum usuário. Pode também retornar uma mensagem de erro mas isso é opcional.

- Should be able to let user create a new todo when is in free plan and have less than ten todos

Para que esse teste passe, deve-se permitir que o middleware `checksCreateTodosUserAvailability` receba o objeto `user` (considerando que o objeto já exista) da `request` e chame a função `next` somente no caso do usuário estar no **plano grátis e ainda não possuir 10 *todos* cadastrados** ou se ele **já estiver com o plano Pro ativado**.

- Should not be able to let user create a new todo when is not Pro and already have ten todos

Para que esse teste passe, no middleware `checksCreateTodosUserAvailability` deve-se retornar uma resposta com status `403` caso o usuário recebido pela requisição esteja no **plano grátis** e **já tenha 10 *todos* cadastrados**. Pode também retornar uma mensagem de erro mas isso é opcional.

- Should be able to let user create infinite new todos when is in Pro plan

Para que esse teste passe, deve-se permitir que o middleware `checksCreateTodosUserAvailability` receba o objeto `user` (considere sempre que o objeto existe) da `request` e chame a função `next` caso o usuário já esteja com o plano Pro.

- Should be able to put user and todo in request when both exits

Para que esse teste passe, o middleware `checksTodoExists` deve receber o `username` de dentro do header e o `id` de um *todo* de dentro de `request.params`. Deve-se validar que o usuário exista, validar que o `id` seja um uuid e também validar que esse `id` pertence a um *todo* do usuário informado.

Com todas as validações passando, o *todo* encontrado deve ser passado para o `request` assim como o usuário encontrado também e a função `next` deve ser chamada.

É importante que seja colocado dentro de `request.user` o usuário encontrado e dentro de `request.todo` o *todo* encontrado.

- Should not be able to put user and todo in request when user does not exists

Para que esse teste passe, no middleware `checksTodoExists` deve-se retornar uma resposta com status `404` caso não exista um usuário com o `username` passado pelo header da requisição.

- Should not be able to put user and todo in request when todo id is not uuid

Para que esse teste passe, no middleware `checksTodoExists` deve-se retornar uma resposta com status `400` caso o `id` do *todo* passado pelos parâmetros da requisição não seja um UUID válido (por exemplo `1234abcd`).

- Should not be able to put user and todo in request when todo does not exists

Para que esse teste passe, no middleware `checksTodoExists` deve-se retornar uma resposta com status `404` caso o `id` do *todo* passado pelos parâmetros da requisição não pertença a nenhum *todo* do usuário encontrado.

- Should be able to find user by id route param and pass it to request.user

Para que esse teste passe, o middleware `findUserById` deve receber o `id` de um usuário de dentro do `request.params`. Deve-se validar que o usuário exista, repassar ele para `request.user` e retornar a chamada da função `next`.

- Should not be able to pass user to request.user when it does not exists

Para que esse teste passe, no middleware `findUserById` deve-se retornar uma resposta com status `404` caso o `id` do usuário passado pelos parâmetros da requisição não pertença a nenhum usuário cadastrado.

---
Todos os demais testes são os mesmos testes encontrados no desafio 01 com algumas (ou nenhuma) mudanças.
<br/><br/>

## 💻 Tecnologias

Essa aplicacão foi desenvolvido com as seguintes tecnologias:

- [Express](https://expressjs.com/pt-br/)
- [Jest](https://jestjs.io/)
<br/><br/>

## 🔸 Como executar

- Clone o repositório
- Instale as dependências com `yarn`
- Rode os testes com `yarn test`

---

<p align="center">🚀 Desenvolvido durante o bootcamp da Rockeseat Ignite - Trilha Nodejs 🚀<p>