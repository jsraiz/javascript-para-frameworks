## [Destructuring](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Operators/Atribuicao_via_desestruturacao)
### Array
```js
  const [count, setCount] = useState(0);
```

### Object
```js
const state = { counter: 1, list: ['a', 'b'] };
 
const { list, counter } = state;
```

### Function
```js
function UserComponent({ name, email }) {
  return (
    <div className="user">
      <p><strong>{name}</strong> - {email}</p>
    </div>
  )
}
```

## Operador Spread
```js
const user = { name: 'Ayrton', age: 28 };
<UserComponent {...user} />
```
Desse jeito estamos espalhando o objeto `user` pra dentro de `UserComponent`, como se fossem os atributos de `UserComponent`. O jeito manual seria:
```js
<UserComponent name="Ayrton" age={28} />
```

## Operador Rest

### Em objeto com destructuring
Dessa forma capturamos a propriedade `name` e armazenamos a constante `name` enquanto todos as outras propriedades do objeto vão ser armazenada em rest graças aos `...`
```js
const { name, ...rest } = state;
```

### Em function
```js
function save(name, ...rest) {
  return axios.post('https://url', {
    name,
    email: rest[0],
    age: rest[1]
  })
  
}

// Utilizando:
save('Ayrton', 'ayrton@jsraiz.com', 28).then(console.log);
```
OBS: A palavra `rest` é só uma convensão, pode ser qualquer outro nome

## Arrow function
Utilize arrow function em momentos que você tem um retorno imediato, excelente em `map` para listar componentes para uma lista
```js
function Users(users) {
  return (
    <ul>
      {users.map(user => (
        <li><strong>{ user.name }</strong> - { user.email }</li>
      ))}
    </ul>
  )
}
```

## Template String
A forma ideal de elegante para concatenar string com variáveis/expressões
```js
function save({ name, email }) {
  return axios.post(`${urlBase}/users`, {
    name,
    email
  })
  
}
```

## Shorthand Object Assignment
```js
const name = 'Ayrton';
const email = 'ayrton@jsraiz.com';
const age = 28;

setUser({
  name,
  email,
  age
})

```

## ES6 Modulos [Import](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Statements/import) e [Export](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Statements/export)
Vindo do ES6, essas são as formas oficiais da linguagem de trabalhar com modularização.
```js
import react from 'react';
```
```js
export default AppComponent;
```

Com esses recursos conseguimos fazer importação e exportação dos nossos arquivos

## Operador rest

### [Map](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Global_Objects/Array/map) para iterar array

```js
const todoItems = todos.map((todo, index) =>
  <li key={index}>
    {todo.text}
  </li>
);
```

### Operador lógico `&&` que vai retornar o seu valor da direita caso a condição da esquerda seja `true`
> condicao && expressão

```js
function Mailbox(props) {
  const unreadMessages = props.unreadMessages;
  return (
    <div>
      <h1>Hello!</h1>
      {unreadMessages.length > 0 &&
        <h2>
          You have {unreadMessages.length} unread messages.
        </h2>
      }
    </div>
  );
}
```

### [Operador ternário](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Operators/Operador_Condicional) 
> condição ? expressão1 : expressão2
```js
function render() {
  return (
    <div>
      {isLoggedIn
        ? <LogoutButton onClick={handleLogoutClick} />
        : <LoginButton onClick={handleLoginClick} />
      }
    </div>
  );
}
```


### [Computed property name](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Operators/Inicializador_Objeto#Nomes_de_propriedades_computados)
Antes se quiséssemos adicionar uma propriedade em um objeto, precisávamos saber o nome antes. Agora podemos colocar entre colchetes uma expressão, o valor retornado vai ser a chave dessa propriedade.

Você vê muito isso principalmente em formulários, para definir e capturar do state o valor de cada campo.

```js
this.setState({
  [name]: value
});
```

### [High Order Functions e First-class Functions](https://developer.mozilla.org/pt-BR/docs/Glossario/Funcao-First-class)
No JavaScript funções são conhecidas como de primeira classe porque são tratadas como valor, com isso podemos armazenar function em variáveis/constantes. Podemos passar function como argumento de outra função e também função retornar function.

Uma function que retorna uma outra function é chamada de Higher-Order Function.

Isso dá muitos poderes no desenvolvimento JavaScript e vemos espalhado por todo o React, explorando o máximo first-class functions. Eis alguns exemplos mais direto na técnica:


`React.forwardRef` é uma function que recebe uma function como argumento e também retorna uma function/component

```js
const FancyButton = React.forwardRef((props, ref) => (
  <button ref={ref} className="FancyButton">
    {props.children}
  </button>
));
```

Inspirado em High Order Functions. O React se aproveita com um pattern (High Order Component) para um componente receber outro componente, retornando um novo componente customizado!

```js
const EnhancedComponent = higherOrderComponent(WrappedComponent);
```

Passando uma arrow function para a propriedade `render`. Dentro do componente `<DataProvider>` vai ser invocado essa function. Também é um exemplo de First-class functions
```js
<DataProvider render={data => (
  <h1>Hello {data.target}</h1>
)}/>
```

e é claro os próprios hooks:

```js
  const [count, setCount] = useState(0);
```
`useState` é uma function que um array que contém uma function `setCount`

```js
useEffect(() => {
  ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
  return () => {
    ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
  };
});
```
Mais um hook `useEffect` que recebe uma function como argumento e essa function retorna outra function (no exemplo acima são arrow functions)

## [ES6 Classes](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Classes)
Se você precisa trabalhar com classes em React, vamos detalhar aqui os tópicos importantes para aprender. Antes de ver o restante, precisamos saber como definir classes e seus métodos.


O `this` aqui é muito importante pois é através dele que você consegue invocar os métodos e obter as propriedades definidos na própria classe

```js
class NameForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {value: ''};

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({value: event.target.value});
  }

  handleSubmit(event) {
    alert('A name was submitted: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Name:
          <input type="text" value={this.state.value} onChange={this.handleChange} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```
### Palavra reservada [this](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Operators/this)
A palavra reservada `this` tem um valor dependendo do contexto o qual ela está sendo chamada.
Uma das formas de alterar o valor de this é quando utilizado classes, pois ela passa a apontar para a própria classe 
>
```jsx
<input type="text" value={this.state.value}></html>
```

### [Herança: extends](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Classes/extends)
```js
class Calculator extends React.Component {...}
```
e
```js
constructor(props) {
  super(props);
}
```
Dessa forma você consegue acessar métodos e propriedades públicas definido em `React.Component`

### [bind](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)
bind é um método de Function que permite invocar uma function e também alterar o this.
Você vai ver muito o bind sendo utilizado pra justamente alterar o valor de this, pois muitas vezes ele tem um valor indesejado por você naquele momento

```js
this.handleChange = this.handleChange.bind(this);
this.handleSubmit = this.handleSubmit.bind(this);
```
No exemplo acima você quer que a function `handleChange` e `handleSubmit` tenha o valor de `this` apontado para a própria classe e não para os eventos `change` e `submit` que o navegador invoca 