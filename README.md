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
