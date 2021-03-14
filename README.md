<h1 align="center">Ignite Desafio 02 - Trabalhando com middlewares</h1>

<p align="center">
  <a href="#-Projeto">Desafio</a>&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;
  <a href="#-Rotas">Rotas</a>&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;
  <a href="#-Testes-de-repositórios">Testes de repositórios</a>&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;
  <a href="#-Testes-de-likes">Testes de likes</a>&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;
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

Esse middleware recebe o `username` de dentro do header e o `id` de um *todo* de dentro de `request.params`. Valida o usuário, verifica se que o `id` é um uuid e também valida se esse `id` pertence a um *todo* do usuário informado.

Com todas as validações passando, o *todo* encontrado é passado para o `request` assim como o usuário encontrado também e a função next é chamada..

### findUserById

Esse middleware possui um funcionamento semelhante ao middleware `checksExistsUserAccount` mas a busca pelo usuário é feita através do `id` de um usuário passado por parâmetro na rota. Caso o usuário tenha sido encontrado, o mesmo é repassado para dentro do `request.user` e a função `next` é chamada.

## 🔸 Testes dos middlewares

- Should be able to find user by username in header and pass it to request.user

Para que esse teste passe, deve-se permitir que o middleware `checksExistsUserAccount` receba um `username` pelo header do request e caso um usuário com o mesmo `username` exista, ele deve ser colocado dentro de `request.user` e, ao final, deve retornar a chamada da função `next`.

- Should not be able to find a non existing user by username in header

Para que esse teste passe, no middleware `checksExistsUserAccount` deve-se retornar uma resposta com status `404` caso o `username` passado pelo header da requisição não pertença a nenhum usuário. Pode também retornar uma mensagem de erro mas isso é opcional.



<br/><br/>

## 🔸 Testes de likes

- Should be able to give a like to the repository

Para que esse teste passe, deve ser possível incrementar a quantidade de likes em `1` a cada chamada na rota **POST** `/repositories/:id/like`. É utilizado o `id` passado por parâmetro na rota para realizar essa ação.

- Should not be able to give a like to a non existing repository

Para que esse teste passe, deve-se validar que um repositório existe antes de incrementar a quantidade de likes. Caso não exista, é retornado um status `404` com uma mensagem de erro no formato `{ error: "Mensagem do erro" }`.
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