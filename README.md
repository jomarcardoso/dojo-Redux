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

Então seguindo nosso exemplo, o jogador A começa vendo como está o jogo:

`store.getState()`, que no início retornará: [ 1: vazia, 2: vazia, 3: vazia, 4: vazia, 5: vazia, 6: vazia, 7: vazia, 8: vazia, 9: vazia ]

![image](https://user-images.githubusercontent.com/27368585/75840452-ffc24780-5da9-11ea-9d24-14657e1e4980.png)

Agora o Jogador A marcará um círculo na posição 1, para isso ele usa o store.dispatch

store.dispatch(ação), a ação contém as informações necessárias de como alterar o estado, neste caso marcar O na posição 1.

O Jogador B está esperando a hora dele de jogar, então ele fica “escutando” quando o Jogador A marcar uma posição e então alterar o estado, o B saberá.

store.subscribe(fazer algo) escuta o estado da store e faz algo se esse mudar. Geralmente após escutar é verificado o estado com store.getState() para saber como está.

![image](https://user-images.githubusercontent.com/27368585/75840482-16689e80-5daa-11ea-8758-f3842942cbb7.png)

E assim o jogo segue…

Até aqui vimos como interagir com uma aplicação Redux já funcionando, agora vamos falar um pouco sobre como implementar esta.

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
