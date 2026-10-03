# Semana 4

### Nota Off-Topic Semi-Relevante

- Na última semana fiz uma troca de SO para desenvolvimento, embarquei na jornada do Arch Linux.
- Isso não muda quase nada, mas a nota serve para evitar a estranheza se pintar alguma confusão entre eu estar usando ubuntu e depois começar a falar do arch do nada.
- Já validei e conseugi buildar e rodar o jogo tranquilamente.

## 17/9

- Retomando os andamentos do projeto após a semana da pátria (7/9 a 13/9), que não foi necessária fazer um registro semanal.
- A ideia dessa semana é mergulhar mais a fundo no código em si, explorar a codebase e registrar o que eu conseguir aprender dela.

- Comecei pedindo uma geral ao antigravity sobre o codebase, já que ele consegue ver arquivo por arquivo, e temos mais de 900 somados. Para não "sujar" minhas notas pessoais, pedi para ele jogar as análises iniciais no `AI_NOTES.md`

- Ele me deu uma visão geral muito boa da codebase, muito pelo fato do código em c++ apesar de ser muito verboso, está bem organizado em módulos. Além disso, passou algumas visões sobre o ciclo de vida da execução.
- Além disso, acho que o mais legal foi a conexão de alguns pontos sobre as good-first-issues que eu havia citado, sobre a escolha aleatória dos ícones de perfil de usuário e sobre a notificação ao liberar uma dificuldade especial.

- Puxando a hint que o agy deixou para mim sobre a issue de escolha de icon do profile, fui explorar os arquivos `src/config/player_profile` (`.cpp` e `.hpp`).
- Surpreendentemente, apesar de eu ainda estranhar um pouco a verbosidade complexa do c++, consegui entender alguns pontos interessantes da classe `PlayerProfile`, que trás vários dados justamente relacionados a justamente... os profiles duhr.
- Um dos métodos relacionados é o `void PlayerProfile::addIcon()`, que faz a escolha pseudo-aletória do ícone do usuário, sem uma opção de escolher outro!
- Além disso, dei uma explorada extra na classe `PlayerProfile`, entendendo seus campos, fazendo ligações com o que eu vi dando uma jogada no jogo:
  - Vi o campo referente a cor do carro que o usuário escolheu ao profile, guardando um float, referindo ao Hue do HSV!
  - Achei alguns campos ponteiros interessantes:
    ```c++
      /** The complete challenge state. */
      StoryModeStatus *m_story_mode_status;

      /** The complete achievement data. */
      AchievementsStatus *m_achievements_status;

      /** The favorite tracks selected by this player. */
      FavoriteStatus *m_favorite_track_status;

      /** The favorite karts selected by this player. */
      FavoriteStatus *m_favorite_kart_status;
    ```

## 19/9

- Pra começar o dia, e quem sabe finalmente colocar a mão na massa no código, busquei entender melhor um pouco da dinâmica de commits e acompanhamento do desenvolvimento conforme o repositório remoto fosse recebendo atualizações.
- O primeiro passo foi realizar um fork do repositório na minha conta pessoal, mas apontando a origem para o repositório original do STK, de forma que eu possa ir realizando meu desenvolvimento local me mantendo atualizado com o estado do repo, corrigindo os possíveis conflitos que surgirem e afins.
- Um ponto interesante de atenção, é que meu fork não pôde trazer apenas a master, pois como já documentado aqui, o repo do STK tem algumas jogadas com as branhces, principalmente de testar o CI e da branch do STK Evolution (`BalanceSTK2`), as quais é importante eu manter a atenção.
- Além disso, para facilitar meu acompanhamento, fiz um 'cheat sheet' de comandos do git. Apesar de eu conhecer todos e não ser tão iniciante no git, o ponto principal é eu usar o sheet para me organizar mentalmente dos comandos.
- Abrindo parênteses para um insight legal que eu tive:
  > Sempre me questionei o porquê do git pedir para declarar explicitamente algumas referências remotas, como o ponteiro para tal branch e etc, mas com essa dinâmica de "Fork e PR", tive o click na minha cabeça e saquei, isso facilita muito a contribuição open source, de forma que os contribuidores consigam manter seus códigos atualizados sem mexer no repo original!!

- Com esse setup e aprendizados feitos, agora estou focando em puramente ler os arquivos das classes `PlayerProfile` e `PlayerManager`.

- Na descrição principal do `PlayerProfile`, descobri que localmente os profiles são salvos em um arquivo chamado `players.xml`, e usando uma combinação de comandos simples, achei aqui na minha máquina, já que eu já tinha dado uma brincada no jogo!
  - `code $(find -name "players.xml")`
  - abriu o arquivo `./.config/supertuxkart/config-0.10/players.xml` no meu vs code
  - tem muuuitos atributos e dados nesse xml
  - os que me chamaram atenção foram:
    - players -> player -> icon-filename
    - players -> player -> achievements (com certeza relacionado à outra issue aberta sobre notificação ao liberar um modo especial)

## 20/9

- Seguindo a linha de ontem, foquei a leitura dos arquivos `PlayerProfile` e `PlayerManager` (`.cpp` e `.hpp`), buscando entender a dinâmica entre essas classes e vínculos de cada uma, o que comportam, o que é responsabilidade de cada uma.
- Notei bem claramente também que elas passam por um processo de inicialização/load bem definido, já que comportam ponteiros para outras classes não simples.
- Além disso, vi bem na prática como o código consome e lida com o XML de player que descobri ontem, que o `PlayerManager` carrega ela, gerando um `PlayerProfile` para cada nó de player no xml.

- Analisando o código, alguns pontos ficaram mais claros para mim:
  - Seja local, seja online, só é possível jogar com 1 player principal na mesma máquina
  - Dessa forma, por vez, apenas um único profile está selecionado no runtime
  - Para multiplayer local, é possível sim jogar com tela dividida, mas os outros jogadores são apenas Guest Players (e as classes citadas acima lidam muito bem com isso, aliás! setam parâmetros "dummy" para instâncias guest)
  - É possível sim usar ícone perzonalizados, mas sob algumas condições específicas e mais técnicas que precisam de um conhecimento de como o código lida com os ícones, ou seja, impossível para o jogador comum
  - O código lida de forma bem pragmática com os ícones, sejam normais ou personalizados, pois tem um fallback bem robusto, levando para um ícone default de "?" caso dê qualquer problema no carregamento da imagem

- Acho que para a próxima semana, após eu ter melhor fundamentado de fatos essas classes, e outras correlatas, além de pegar exatamente a sacada de como elas se comportam em runtime, a ideia principal é focar nos requisitos reais para que eu consiga completar a issue 1771, de escolher o ícone do player.

- Um adendo sobre o uso do agy a partir do dia 17/9:
  - O intuito é unicamente me apoiar no entendimento da extensa codebase do jogo, visto que escolhi um projeto difícil com tecnologias que tenho interesse de aprender ao longo do desenvolvimento da disciplina e da minha contribuição.
  - Deixo explícito aqui que o código de conduta do SuperTux Kart proíbe completamente o uso de LLMs para geração de código, comentários e commits, mas permite o uso consciente para análises da codebase em [SuperTuxKart’s policy on AI-generated code](https://supertuxkart.net/How_to_contribute_code#supertuxkarts-policy-on-ai-generated-code):

    > There are a few limited usages of LLMs that are acceptable, chiefly: Helping to understand the structure of existing code. As SuperTuxKart’s code is quite complex, it might be occasionally helpful to use a LLM to assist you in analyzing how an unfamiliar portion of the code works. In this case, it is still important to manually confirm what is going on, as the goal is to acquire genuine understanding.
