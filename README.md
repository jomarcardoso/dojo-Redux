# Redux mostrado de uma forma diferente

Há pouco tempo consegui entender como funciona o Redux, como tive muita dificudade para isso e todos ensinavam da mosma forma que nao achei intuitiva, resolvi também tentar repassar esse conhecimento, mas de uma forma diferente, algo mais gradual, “aprender aos poucos”.

## O que é

Redux é uma biblioteca para a criação de aplicações de controle de estado, ou seja, algo independente.

![image](https://user-images.githubusercontent.com/27368585/75840200-71e65c80-5da9-11ea-9990-eeac8f9177cb.png)

As flechas representam a comunicação que uma aplicação redux disponibiliza.

### Como funciona?

Você armazena nele os dados que achar necessário e toda atualização de qualquer um desses dados cria um novo estado.

### Exemplo

Vamos criar um Jogo da Velha, que assim como no Redux vamos armazenar um estado deste.

Estado(posições): [ 1: vazia, 2: vazia, 3: vazia, 4: vazia, 5: vazia, 6: vazia, 7: vazia, 8: vazia, 9: vazia ]

![image](https://user-images.githubusercontent.com/27368585/75840277-9f330a80-5da9-11ea-95c1-aa5d349f40f1.png)

No decorrer do jogo esse estado vai mudando.

Estado(posições): [ 1: O, 2: vazia, 3: vazia, 4: vazia, 5: vazia, 6: vazia, 7: vazia, 8: vazia, 9: vazia ]

![image](https://user-images.githubusercontent.com/27368585/75840349-c2f65080-5da9-11ea-8fdf-77b59b578d29.png)

Estado(posições): [ 1: O, 2: vazia, 3: vazia, 4: vazia, 5: X, 6: vazia, 7: vazia, 8: vazia, 9: vazia ]

![image](https://user-images.githubusercontent.com/27368585/75840361-cf7aa900-5da9-11ea-8d4e-f77e50ec40e2.png)

O estado de uma aplicação Redux é armazenado na “store”.

## Store

Toda interação nossa com o estado do Redux será feito na “Store” através dessas três funções:

`store.getState()` para ver o estado;

`store.dispatch()` para mudar o estado;

`store.subscribe()` para escutar a mudança do estado;

*** store.getState

Então seguindo nosso exemplo, o jogador A começa vendo como está o jogo:

`store.getState()`, que no início retornará: [ 1: vazia, 2: vazia, 3: vazia, 4: vazia, 5: vazia, 6: vazia, 7: vazia, 8: vazia, 9: vazia ]

![image](https://user-images.githubusercontent.com/27368585/75840452-ffc24780-5da9-11ea-9d24-14657e1e4980.png)

*** store.dispatch

Agora o Jogador A quer marcar um círculo na posição 1, para isso ele deve usar o `store.dispatch`

`store.dispatch(ação)`, a ação contém as informações necessárias de como alterar o estado, neste caso marcar "O" na posição 1.

*** store.subscribe

O Jogador B está esperando a hora dele de jogar, então ele fica “escutando” quando o Jogador A marcar uma posição e então alterar o estado, o B saberá.

`store.subscribe(() => se algo mudar faz isso daqui)` escuta o estado da store e faz algo se esse mudar. Geralmente após escutar é verificado o estado com store.getState() para saber como está.

![image](https://user-images.githubusercontent.com/27368585/75840482-16689e80-5daa-11ea-8758-f3842942cbb7.png)

E assim o jogo segue…

Até aqui vimos como interagir com uma aplicação Redux já funcionando, agora vamos falar um pouco sobre como implementar esta.

----

Sabemos que a “base” é o Store, mas para criá-lo precisamos primeiro difinir um estado inicial e quem contrala-o. Daqui em diante vamos no código.

```js
const initialState = new Array(9);
```

## Reducer

Reducers são os reponsáveis por definirem o estado inicial e alterar este conforme a instrução passada.

```js
const initialState = new Array(9);

function jogoDaVelha (state = initialState, action) {
  switch (action.type) {
    case 'MARCAR_POSICAO':
      state[action.posicao - 1] = action.sinal;
      return state;
    default:
      return state
  }
}
```

O Store espera do Reducer apenas que ele receba um estado e retorne um novo. A sintaxe acima segue os padrões mostrados pelo Redux, que são:

- O 1º parâmetro ser o estado, e no caso de ele ainda não existir no store definir um estado inicial.
- O 2º parâmetro ser a ação que definirá como alterar o estado e este ter um atributo chamado “type”.
- O “type” é usado no “switch” para saber como e que parte do estado alterar.
- Por fim se não encontrar ou não puder criar um novo estado ele retorna o anterior no “switch default”.

Agora temos o que é necessário para criar o "reducer" para controlar nossa "store".

```js
import { createStore } from 'redux';

const initialState = new Array(9);

function jogoDaVelha (state = initialState, action) {
  switch (action.type) {
    case 'MARCAR_POSICAO':
      state[action.posicao - 1] = action.sinal;
      return state;
      break;
  default:
    return state;
  }
}

const store = createStore(jogoDaVelha);
```

E é isso, temos nossa aplicação redux. Vamos jogar! 🤗

Começando criando os jogadores.

```js
class Jogador {
  constructor(sinal) {
    this.sinal = sinal;
  }
  
  marcar(posicao) {
    const action = { 
      type:'MARCAR_POSICAO', 
      posicao,
      sinal: this.sinal
    };
    
    store.dispatch(action);
  }
}

const jogador1 = new Jogador('O');
const jogador2 = new Jogador('X');
```

Fizemos a classe para criar os jogadores e estes só tem a capacidade de marcar uma posição, sem controle algum, só usar o `store.dispatch`.

Criamos uma ação que contém as informações necessárias para o reducer saber como proceder.

- type: conforme esperado lá no switch/case.
- posicao/sinal: para gerar um novo estado.

As jogadas então ficam assim:

```js
jogador1.marcar(1);
```

![image](https://user-images.githubusercontent.com/27368585/75840859-01d8d600-5dab-11ea-87f3-80f4a4b8301e.png)

`store.getState()`: [ 1: ‘O’, 2: vazia, 3: vazia, 4: vazia, 5: vazia, 6: vazia, 7: vazia, 8: vazia, 9: vazia ]

```js
jogador2.marcar(5);
```

![image](https://user-images.githubusercontent.com/27368585/75840905-28970c80-5dab-11ea-86f8-9d37e3b630a4.png)

`store.getState()`: [ 1: ‘O’, 2: vazia, 3: vazia, 4: vazia, 5: ‘X’, 6: vazia, 7: vazia, 8: vazia, 9: vazia ]

Beleza, o resultado é um array, mas que tal algo visual? Para fazer isso vamos ficar “escutando” as mudanças para mostrar na tela.

## Subscribe

Colocamos em prática o dispatch, um dos três métodos do store. Agora vamos “prestar atenção no jogo”? Perceber quando é a vez de cada jogador atuar usando o método "subscribe".

Para ilustrar melhor, agora a aplicação que controla os estados agora terá mais de uma atributo em seu estado. O estado do jogo agora é o `state.jogo` e último jogador é o `state.ultimoJogador`.

```js
import { createStore } from 'redux';

const initialState = { 
  jogo: new Array(9),
  ultimoJogador: null
}

function jogoDaVelha (state = initialState, action) {
  switch (action.type) {
    case 'MARCAR_POSICAO':
      state.jogo[action.posicao - 1] = action.jogador.sinal;
      state.ultimoJogador = action.jogador;
      return state;  
    default:
      return state;
  }
}

const store = createStore(jogoDaVelha);
```

Agora fazer algo bobo, um alert para avisar o jogador que o outro já jogou.

```js
class Jogador {
  constructor({ nome, sinal }) {
    this.nome = nome;
    this.sinal = sinal;
    
    this.prestarAtencao();
    this.estadoJogo = null;
  }
  
  marcar(posicao) {
    const { ultimoJogador } = store.getState();
    if (ultimoJogador === this) return;
    
    const action = { 
      type:'MARCAR_POSICAO', 
      posicao,
      jogador: this
    };    
    
    store.dispatch(action);
  }
  
  prestarAtencao() {
    store.subscribe(() => {
      const { ultimoJogador, jogo } = store.getState();
      
      if (jogo === estadoJogo) return;
      estadoJogo = jogo;
      
      if (ultimoJogador === jogador) return;      

      alert(`${ultimoJogador.nome} jogou`);
    });
  }
}

const jogador1 = new Jogador({ nome: 'jogador1', sinal: 'O' });
const jogador2 = new Jogador({ nome: 'jogador2', sinal: 'X' });
```

O subscribe aqui é como a visão dele, que está olhando para o estado esperando uma mudança.

Até aqui da para jogar uma partida completa de Jogo da Velha, mas acredito que algo esteja incomodando vocês ainda

Porque criamos um switch/case no Reducer se ele só tem uma opção? Bom, por padrão todos fazem assim, não vamos fugir dos padrões, mas só acrescentar mais um item para o negócio fazer mais sentido.

Para zerar o jogo, apagar tudo e começar de novo, é só adicionar uma nova opção no Reducer.

```js
import { createStore } from 'redux';

const initialState = { 
  jogo: new Array(9),
  ultimoJogador: null
}

function jogoDaVelha (state = initialState, action) {
  switch (action.type) {
    case 'MARCAR_POSICAO':
      state.jogo[action.posicao - 1] = action.jogador.sinal;
      state.ultimoJogador = action.jogador;
      return state;
    case 'RECOMECAR_JOGO':
      return initialState;
    default:
      return state;
  }
}

const store = createStore(jogoDaVelha);
```

E damos poder aos jogadores de poderem recomeçar a partida.

```js
estadoJogo = jogo;

class Jogador {
  constructor({ nome, sinal }) {
    this.nome = nome;
    this.sinal = sinal;
    
    this.prestarAtencao();
    this.estadoJogo = null;
  }
  
  marcar(posicao) {
    const { ultimoJogador } = store.getState();
    if (ultimoJogador === this) return;
    
    const action = { 
      type:'MARCAR_POSICAO', 
      posicao,
      jogador: this
    };    
    
    store.dispatch(action);
  }
  
  prestarAtencao() {
    store.subscribe(() => {
      const { ultimoJogador, jogo } = store.getState();
      
      if (jogo === estadoJogo) return;
      estadoJogo = jogo;
      
      if (ultimoJogador === jogador) return;      

      alert(`${ultimoJogador.nome} jogou`);
    });
  }
  
  zerarJogo() {
    const action = { type: 'ZERAR_JOGO' };
    store.dispatch(action);
  }
}

const jogador1 = new Jogador({ nome: 'jogador1', sinal: 'O' });
const jogador2 = new Jogador({ nome: 'jogador2', sinal: 'X' });
```

A qualquer momento o jogador pode colocar o jogo em seu estado inicial usando o `store.dispatch({ type: 'ZERAR_JOGO' })`.

Por enquanto é isso pessoal, espero ter ajudado. Desculpa interromper assim. Algum dia farei uma continuação para aprofundar melhor.
