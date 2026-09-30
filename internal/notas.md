# Contexto e Diário - Lab de Software Livre

## O que é a disciplina
- O objetivo é vivenciar e contribuir de forma efetiva com um projeto de software livre/aberto.
- Carga horária de 120h no semestre (aprox. 8h semanais), avaliado por Diário Semanal (60%) e Qualidade/Impacto da contribuição (40%).
- Regras para o projeto: existir há mais de 6 meses e ter atividade recente nos últimos 6 meses.
- A contribuição não precisa ser só código (feature/fix); rola fazer testes, doc, usabilidade, tradução, etc.

## Meu Perfil
- Estudante de SI na USP, atualmente estagiário de Infra/Operações na Avenue.
- Base forte em Python, Java, C, C++, GoLang, e experiência com OpenGL, Sockets, e Padrões de Projeto.
- Quero usar a disciplina p/ aprender algo novo, com foco no que realmente me empolga: GameDev e Computação Gráfica.

## Semana 1

### 14/8 a 18/8
- Fizemos um brainstorm longo de possibilidades, cruzando meus interesses com meu CV.
- Passamos por opções como GIMP, GCC, DAWs (LMMS, Audacity), e mergulhamos forte no ecossistema de GameDev (Mindustry, CDDA, SRB2, Godot) e homebrew de Nintendo 3DS (Luma3DS, libctru).
- Filtramos os descartados (GCC por ser mto denso pro tempo da disciplina; CTGP-7 por não ser 100% open source; 3DSCraft e Lime por inatividade).
- Cheguei num Top 4 para apresentar ao professor, mesclando C/C++ e as áreas que curto:
  1. SuperTuxKart (Favorito)
  2. GIMP
  3. LMMS
  4. Godot
- Fiz um levantamento dos links importantes do SuperTuxKart (guia de contribuição, issues p/ iniciantes, fórum e reddit).
- Redigi e fechei o texto da minha primeira postagem de diário pro eDisciplinas, pedindo a validação do professor Daniel com foco no SuperTuxKart (e mantendo os outros 3 como backup). O texto ficou orgânico, direto e já mostra onde quero chegar.

### Semana 1 - Postagem

```
Olá professor Daniel, tudo bem?

Queria validar com você minha escolha de contribuição para a Disciplina de Laboratório de Software Livre.

Primeiro, só queria comentar que estou bem empolgado com a proposta da disciplina, tanto sobre contribuir com uma comunidade quanto me forçar a aprender algo novo nesse processo.
De primeiro momento, tentei fazer um levantamento de projetos em que eu poderia escolher, e acabei com uma lista de 19 possibilidades logo de primeira, precisei filtrar bastante até chegar em um número menor de opções haha.
Para contexto, tentei limitar minhas escolhas para ideias que me deixassem motivado, então as opções variaram entre jogos Open Source, motores de renderização Open Source ou ferramentas do tipo, DAWs Open Source ou simplesmente alguma ferramenta que eu gosto e uso.

Minha lista final ficou com 4 opções, em ordem de preferência:

1. SuperTuxKart - https://github.com/supertuxkart/stk-code
2. GIMP - https://gitlab.gnome.org/GNOME/gimp
3. LMMS - https://github.com/LMMS/lmms
4. Godot - https://github.com/godotengine/godot

Já verifiquei os repositórios e todos os projetos da lista cumprem os requisitos de terem mais de seis meses de existência e atividade recente.
Acho que firmo com certeza que quero trabalhar com o SuperTuxKart, um jogo Open Source de corrida, cuja proposta é rodar em qualquer lugar, com foco em C++ para o código fonte, Python para scripts relacionados ao Blender e PHP para o site e wiki.
Ainda assim, mantenho as outras opções como 'backup'.

Fiz um levantamento das principais fontes de conhecimento sobre o projeto para também entender como eu poderia fazer a minha contribuição:

- Guia de como contribuir com código - https://supertuxkart.net/How_to_contribute_code
- Fórum - https://forum.supertuxkart.net/
- Issues para iniciantes - https://github.com/supertuxkart/stk-code/issues?q=is%3Aopen+is%3Aissue+label%3A%22T%3Afor+beginners%22
- Reddit - https://www.reddit.com/r/SuperTuxKart/
- Dentre alguns outros links presentes na página principal do projeto, na aba de Comunidade https://supertuxkart.net/pt_BR/Community

De primeiro momento, minha ideia é realmente explorar a codebase e as wikis, brincar um pouco com ele na minha máquina para entender o funcionamento e coisas do tipo.

Poderia validar minha escolha para que eu possa iniciar os trabalhos?
```

## Semana 2

### 19/8

- No passo de 'onboarding' do projeto, no qual instalo as dependências dele, aprendi um novo comando, o `subversion`/`svn`, que representa uma forma alternativa ao git para versionamento e compartilhamento de código

- Ainda no onboarding, tive contato com essas dependências (com uma análise do Gemini do que é cada uma):
    - OpenAL: Motor de áudio 3D (para você ouvir de qual lado o casco vermelho está vindo).
    - Ogg: Formato de contêiner de arquivos de mídia (geralmente guarda as músicas do jogo).
    - Vorbis: O algoritmo de compressão de áudio (o "mp3" open source) que vai dentro do arquivo Ogg.
    - Freetype: Lê os arquivos de fontes (.ttf, etc) e desenha as letras na tela.
    - Harfbuzz: Trabalha junto com o Freetype para formatar e alinhar os textos corretamente (espaçamento, linguagens complexas).
    - libcurl: Faz requisições de rede (HTTP, etc). Usado para login, multiplayer ou baixar mods in-game.
    - libbluetooth: Gerencia conexões Bluetooth nativas (provavelmente para conectar controles sem fio nativamente).
    - openssl: Lida com criptografia (TLS/SSL). Garante que a comunicação de rede do curl seja segura.
    - libpng: Biblioteca para abrir, ler e renderizar imagens no formato PNG (muito usado para texturas e ícones).
    - zlib: O padrão universal de compressão de dados (o libpng usa ele por baixo dos panos para comprimir as imagens).
    - jpeg: Lê e processa imagens no formato JPG (geralmente para texturas pesadas que não precisam de transparência).
    - SDL2: O "faz-tudo" multiplataforma do GameDev! Ele cria a janela do sistema operacional, captura seu teclado/mouse/joystick e prepara o terreno para o OpenGL renderizar os gráficos.

- Entendi como usar as flags do CMake para simplificar a compilação no Linux, desativando recursos desnecessários para o meu foco atual:
    - `-DBUILD_RECORDER=off`: Desliga a compilação do gravador de vídeos in-game, removendo a necessidade de instalar a dependência `libopenglrecorder`.
    - `-DNO_SHADERC=on`: Desliga o suporte ao Vulkan para a engine focar no OpenGL, o que evita o trabalho extra de compilar o `Shaderc` na mão.

- Consegui compilar e rodar sem problemas! Acho q em relação ao setup e como rodar localmente, o projeto está bem documentado
- Fiz o tutorial, o jogo é bem completinho, lembra realmente um Mario Kart

### 30/8

- Nessa segunda semana acabei não realizando nenhum avanço até o momento (domingo de manhã), pois estava esperando algum retorno do professor, mas fui surpreendido com a ausência desse retorno, então planejo avançar com algo hoje até a hora limite da postagem

- Com isso, decidi que hoje será um dia de sondagem e análise do projeto, para que eu possa fazer uma postagem de qualidade na semana 2, complementando o onboarding do projeto q fiz no dia 19/8, entendendo um pouco do build e dependências no linux

- Acabei quebrando demais a cabeça no build do sistema para Windows:
  - Primeiro, meu desafio inicial foi entender um pouco melhor o ecossistema do c++ no windows, com as ferramentas do CMake (Gerador de Build), o Visual Studio (IDE recomendada) e o MSVC (Compilador), e que o c++ não tem um gerenciador de pacote oficial.
  - Então, tentei seguir o passo a passo da documentação do repositório instruindo o build para windows usando CMake e Visual Studio, que está bem completo e direto também, mostrando 2 caminhos: o da última versão stable e o da versão em desenvolvimento.
  - O problema começou a surgir quando ao compilar e tentar rodar o projeto, estava ocorrendo um erro, alegando que uma das dependências necessárias (`libc++.dll`) não estava inclusa.
  - Com isso, comecei uma jornada para tentar rodar o jogo localmente no windows, seguindo a ideia de que não queria forçar a inclusão da dependência de forma externa pois idealmente, o pacote indicado das dependências deveria incluir tudo o que é necessário para a compilação e execução do jogo, e de fato, a dependência `libc++.dll` não é listada em lugar algum nem fornacida no arquivo compactado que deveria conter todas as deps.
  - Dessa forma, minha principal suspeita foi a versão do MSVC, pois na documentação de instalação, há uma nota sobre as versões do VS:
    ```
    *Note: To avoid confusion between releases and versions, refer to this table:*

    | Visual Studio Release | Version |
    | --------------------- | ------- |
    | Visual Studio 2019    | 16      |
    | Visual Studio 2017    | 15      |
    | Visual Studio 2015    | 14      |
    | Visual Studio 2013    | 13      |
    ```
  - Então, pelo `Visual Studio Installer`, fiz a instalação da v142, referente ao Visual Studio 2019 16.9, mas ainda assim obtive o mesmo erro. Estressei várias alternativas e combinação de comandos, aqui com ajuda de IA generativa para triagem e tentar alcançar uma possível solução, mas não funcionou também.
  - Dessa forma, vejo 2 possibilidades que posso seguir em paralelo:
    - Pedir ajuda na comunidade do jogo
    - Tentar fazer o build da última versão stable do jogo, com isso consigo entender melhor se eu estou errando em algo do meu lado na hora do build, ou se alguma dependência foi quebrada para a nova release. De qualquer forma, há um grande potencial em melhoria de documentações no repositório caso algum outro passo ou atenção no build seja necessário.

### Semana 2 - Postagem

```
Olá professor Daniel, tudo bem?

Registrando aqui minha atualização para a semana 2. Apenas relembrando que minha escolha de projeto foi o SuperTuxKart, anexando os links de referência que citei na primeira postagem, ainda aguardo sua validação sobre minha escolha:
- GitHub - https://github.com/supertuxkart/stk-code
- Guia de como contribuir com código - https://supertuxkart.net/How_to_contribute_code
- Fórum - https://forum.supertuxkart.net/
- Issues para iniciantes - https://github.com/supertuxkart/stk-code/issues?q=is%3Aopen+is%3Aissue+label%3A%22T%3Afor+beginners%22
- Reddit - https://www.reddit.com/r/SuperTuxKart/
- Dentre alguns outros links presentes na página principal do projeto, na aba de Comunidade https://supertuxkart.net/pt_BR/Community

Senti que a melhor forma de eu iniciar a exploração do projeto foi começar como um mero usuário: jogar o jogo. Não baixando diretamente o executável, mas sim fazendo o build na minha máquina.
Vale ressaltar que a proposta do projeto é poder rodar em vários tipos de dispositivos diferentes: Linux, MacOS, Windows, Android e até no Nintendo Switch.

No passo de 'onboarding' do projeto, no qual instalo as dependências dele, aprendi um novo comando, o `subversion`/`svn`, que representa uma forma alternativa ao git para versionamento e compartilhamento de código.
O código principal está no GitHub, em [stk-code](https://github.com/supertuxkart/stk-code), e os assets, estão no SubVersion, em [stk-assets](https://svn.code.sf.net/p/supertuxkart/code/stk-assets).

Primeiro, comecei com a versão do Linux. Aqui, já tive contato com inúmeras novas bibliotecas e dependências usadas, o que me deu um gostinho de como a parte técnica do projeto funciona:
  - OpenAL: Motor de áudio 3D
  - Ogg: Formato de contêiner de arquivos de mídia
  - Vorbis: O algoritmo de compressão de áudio (o "mp3" open source) que vai dentro do arquivo Ogg
  - Freetype: Lê os arquivos de fontes (.ttf, etc) e desenha as letras na tela
  - Harfbuzz: Trabalha junto com o Freetype para formatar e alinhar os textos corretamente
  - libcurl: Faz requisições de rede (HTTP, etc). Usado para login, multiplayer ou baixar mods in-game.
  - libbluetooth: Gerencia conexões Bluetooth nativas (para conectar controles sem fio nativamente, por exemplo)
  - openssl: Lida com criptografia (TLS/SSL). Garante que a comunicação de rede do curl seja segura
  - libpng: Biblioteca para abrir, ler e renderizar imagens no formato PNG
  - zlib: O padrão universal de compressão de dados
  - jpeg: Lê e processa imagens no formato JPG (geralmente para texturas pesadas que não precisam de transparência)
  - SDL2: O canivete-suiço multiplataforma do GameDev! Ele cria a janela do sistema operacional, captura seu teclado/mouse/joystick e prepara o terreno para o OpenGL renderizar os gráficos.

Entendi também sobre como usar as flags do CMake para simplificar a compilação no Linux, desativando recursos desnecessários para o meu foco atual:
    - `-DBUILD_RECORDER=off`: Desliga a compilação do gravador de vídeos in-game, removendo a necessidade de instalar a dependência `libopenglrecorder`.
    - `-DNO_SHADERC=on`: Desliga o suporte ao Vulkan para a engine focar no OpenGL, o que evita o trabalho extra de compilar o `Shaderc` na mão.
Essas flags estão bem documentadas, sendo recomendadas mesmo no passo a passo, mas quis entender o porquê de adicioná-las, visto que ao tentar executar sem elas no meu ambiente linux, não funcionou muito bem.
Então seguindo o passo a passo a risca para o Linux, consegui compilar e rodar sem problemas, em cerca de 4 comandos a mágica aconteceu! Nessa seção, o projeto está muito bem documentado. Fiz o tutorial do jogo, e umas partidinhas, me diverti, o jogo é bem completo e complexo para sua proposta de ser leve (lembra realmente um Mario Kart!) e rodar em qualquer lugar.

Dando sequência na semana, fui tentar então fazer o build no ambiente do Windows. Aqui tive meus primeiros entraves. Meu desafio inicial foi entender um pouco melhor o ecossistema do C++ no windows, com as ferramentas do CMake (Gerador de Build), o Visual Studio (IDE recomendada) e o MSVC (Compilador), e que o C++ não tem um gerenciador de pacote oficial.
Então, tentei seguir o passo a passo da documentação do repositório instruindo o build para windows usando CMake e Visual Studio, que está bem completo e direto também, mostrando 2 caminhos: o da última versão stable e o da versão em desenvolvimento.

O problema começou a surgir quando ao compilar e tentar rodar o projeto, estava ocorrendo um erro, alegando que uma das dependências necessárias (`libc++.dll`) não estava inclusa.
Com isso, comecei uma jornada para tentar rodar o jogo localmente no windows, seguindo a ideia de que não queria forçar a inclusão da dependência de forma externa pois idealmente, o pacote indicado das dependências deveria incluir tudo o que é necessário para a compilação e execução do jogo, e de fato, a dependência `libc++.dll` não é listada em lugar algum nem fornecida no arquivo compactado que deveria conter todas as deps.
  - Dessa forma, minha principal suspeita foi a versão do MSVC, pois na documentação de instalação, há uma nota sobre as versões do VS:
    ```
    *Note: To avoid confusion between releases and versions, refer to this table:*

    | Visual Studio Release | Version |
    | --------------------- | ------- |
    | Visual Studio 2019    | 16      |
    | Visual Studio 2017    | 15      |
    | Visual Studio 2015    | 14      |
    | Visual Studio 2013    | 13      |
    ```
Então, pelo `Visual Studio Installer`, fiz a instalação da v142, referente ao Visual Studio 2019 16.9, mas ainda assim obtive o mesmo erro. Estressei várias alternativas e combinação de comandos, aqui com ajuda de IA generativa para triagem e tentar alcançar uma possível solução, mas não funcionou também.
Dessa forma, vejo 2 possibilidades que posso seguir em paralelo:
  - Pedir ajuda na comunidade do jogo
  - Tentar fazer o build da última versão stable do jogo, com isso consigo entender melhor se eu estou errando em algo do meu lado na hora do build, ou se alguma dependência foi quebrada para a nova release. De qualquer forma, há um grande potencial em melhoria de documentações no repositório caso algum outro passo ou atenção no build seja necessário.

Por ora, minha semana 2 termina nesse estágio travado da compilação, como escrevi acima, meus próximos passos são fazer um primeiro contato com a comunidade e pedir uma ajuda sobre a compilação no windows, além de ter registrado uma oportunidade de primeira pequena contribuição de melhorias de documentação, caso de fato haja algum entrave no build da versão em desenvolvimento.
```

## Semana 3

### 2/9

- Dei uma pausa na minha jornada do build no Windows, tanto pq aqui na faculdade só consigo usar o meu note com ubuntu, tanto quanto para focar um pouco na análise e entendimento do projeto.
- Hoje basicamente analisei documentação do site, acabei não indo a fundo em código.

### 3/9

- Dando uma bizoiada no repositório, notei que há uma branch de uma atualização massiva do jogo, basicamente uma completa breaking change, com nome de `Super Tux Kart Evolution`
- Então fuçando nas branches, descobri coisas legais:
  - Por algum motivo, tem uma branch guardada chamada `add-dirt-to-karts`, em que a última atualização foi a 8 anos atrás, provavelmente estão usando de arquivo p algum código interessante, foi tentar investigar o motivo depois.

- E aqui que tive uma descoberta legal:

  - Na semana passada, por conta do problema de build que estava tendo no Windows olhando nas docs do site, mais em específico na [página de FAQ](https://supertuxkart.net/pt_BR/FAQ), achei uma especificamente um tópico sobre "The Git version won't compile. What should I do?", e a resposta é que: 

  > "This happens sometimes; the developers should be aware of that and it should be fixed soon. If GitHub Actions says that the current Git version compiles, but it doesn’t do so for you, then probably something is wrong with your compiler setup. (Check if you have all dependencies, re-run CMake, …)"

  - Ainda na semana passada, lembro de dar um check nos Actions do repositório, para a branch master, muitas versões estavam com falha no build, principalmente a do windows!

  - Somado a isso, hoje, fuçando nas branches como falei, descobri uma branch chamada `TestCI`, na qual são feitos ajustes no repositório e testes no GitHub Actions do Build no CI!! E aqui o ápice: o commit mais recente chamava `Switch to LLVM for Windows x86 cross-compilation `, algo claramente relacionado ao problema que eu estava tendo no build do windows, das dependências e falta do libc++.dll !!!
  - E hoje mesmo o ajuste foi para a master

  - Assim que possível, vou re-testar o build.
  - Hoje aprendi um pouco ainda mais sobre o processo de build e como funciona o controle disso na infraestrutura do repositório!

- Achei uma issue interessante marcada para beginners -> [Allow players to pick the kart icon for their profile #1771](https://github.com/supertuxkart/stk-code/issues/1771)
  - A proposta dela é permitir que o jogador possa alterar o icon de perfil de cada 'profile' criada no jogo. No jogo atual, o icon é escolhido de forma aleatória e não é possível customizar. Também propõe que se o profile for Online, seja possível fazer o upload de um icon.
  - Aliás, ela está marcada como milestone para a nova versão citada do `STK Evolution`.

- Achei também outra issue interessante para begginners -> [Unlock notification when the SuperTux difficulty is unlocked #4116](https://github.com/supertuxkart/stk-code/issues/4116)
  - A issue está relacionada a um pop-up quando uma dificuldade especial de corrida for desbloqueada.
  - Também marcada como milestone para a nova versão `STK Evolution`

### 6/9

- Hoje, com o windows em mãos, tentei realizar uma nova tentativa de build, compilação e execução do stk, visto que aparentemente, alguma correção no processo para windows havia ocorrido.

- O primeiro passo foi atualizar meu repositório local com os novos commits feito pela comunidade, e re-tentar o processo de ponta a ponta, limpei as dependências e binários antigos, para a nova tentativa, e baixei as dependências atualizadas.
- Segui o passo a passo instruído pela documentação e tive o mesmo problema, a `libc++.dll` não foi encontrada no passo da execução.
- O passo seguinte foi ajustar qualquer ponto de disparidade em relação a documentação, tentando seguir absolutamente à risca agora, busquei o visual studio recomendado para o processo (Visual Studio 2019 16) via CMake GUI, também não funcionou.

- Como alternativa não muito ideal então, tentei forçar a inclusão das dependências faltantes:
  - Primeiro, injetei manualmente `libc++.dll` e `libunwind.dll` extraídas da distribuição llvm-mingw. Ocorreram então novos erros de ponto de entrada (_ZNSt...) na `OpenAL32.dll` decorrentes do conflito entre assinaturas C++ do Clang (`libc++`) e do GCC (`libstdc++`).
  - Então, fiz o download e extração do runtime WinLibs (`GCC 16.2.0 UCRT Win64`) para fornecimento da `libstdc++-6.dll` e `libwinpthread-1.dll`, mas ocorreram erros de incompatibilidade de arquiteturas ao misturar DLLs de 32-bit (como libgcc_s_dw2-1.dll) com executáveis de 64-bit.

- Seguindo minhas tentativas, o próximo passo foi tentar migrar meu ambiente em que estava realizando o processo para o MSYS2 CLANG64 + Ninja, com o intuito de isolar de forma completa a toolchain necessária no LLVM/Clang.
  - Instalei as dependências necessárias então, e fiz a execução do cmake pelo próprio terminal do MSYS2, onde também houveram erros devido à busca de DLLs do sistema do MSYS2 (como `libcurl-4.dll`) fora do PATH do Windows Explorer. Identifiquei que isso ocorria pois o projeto ainda estava vinculando libs estáticas do visual studio. Limpei novamente meu ambiente, removendo qualquer resquício das tentativas anteriores.
  - Na nova tentativa, o build e compilação ocorreram normalmente, gerando o executável, mas agora, ao tentar executá-lo, siplesmente nada acontecia.

- Dessa forma, com esses tantos entraves, decidi dar uma mudada de foco do andamento do projeto, vou focar temporariamente apenas no linux para a próxima semana, pelo menos até eu encontrar alguma ajuda na comunidade sobre o assunto e não ficar travado em algo que deveria ser simples, podendo focar de fato em alguma contribuição.

### Semana 3 - Postagem

Olá professor Daniel, tudo bem?

Registrando aqui minha atualização para a semana 3. Nessa semana, segui com minha exploração de documentações do projeto, indo um pouco mais a fundo na arquitetura do repositório (suas branches e issues), e alguns comportamentos do GitHub Actions, além de também seguir com a jornada de execução do projeto no ambiente do windows.

Sobre o progesso dessa semana, comecei dando uma olhada no repositório, notei que há uma branch de uma atualização massiva do jogo, basicamente uma completa breaking change, com nome de `Super Tux Kart Evolution`, fui ler sobre essa grande atualização no fórum do projeto, e aprendi que é uma atualização que está em andamento a mais de um ano, que realmente consiste em reformular o jogo em muitos aspectos, com várias milestones a serem cumpridas, com algumas issues marcadas para 'begginners' para completar esse milestone, me chamando atenção as seguintes:

- [Allow players to pick the kart icon for their profile #1771](https://github.com/supertuxkart/stk-code/issues/1771)
  - A proposta dela é permitir que o jogador possa alterar o icon de perfil de cada 'profile' criada no jogo.
  - Validei no ambiente do linux e no jogo atual é possível criar perfis locais, nos quais o icon é escolhido de forma aleatória e não é possível customizar. Também propõe que se o profile for Online (no sentido de que perfis locais tem uma flag de online ou não, permitindo que o jogador use-o para jogar no modo multiplayer), seja possível fazer o upload de um icon.

- [Unlock notification when the SuperTux difficulty is unlocked #4116](https://github.com/supertuxkart/stk-code/issues/4116)
  - A issue está relacionada a um pop-up quando uma dificuldade especial de corrida for desbloqueada, que ocorre após você fazer alguns objetivos dentro do "modo história"

Seguindo minha análise do repositório, junto da minha investigação sobre conseguir rodar no windows, tive descobertas:

  - Na semana passada, por conta do problema de build que estava tendo no Windows olhando nas docs do site, mais em específico na [página de FAQ](https://supertuxkart.net/pt_BR/FAQ), achei uma especificamente um tópico sobre "The Git version won't compile. What should I do?", e a resposta é que:
  > "This happens sometimes; the developers should be aware of that and it should be fixed soon. If GitHub Actions says that the current Git version compiles, but it doesn’t do so for you, then probably something is wrong with your compiler setup. (Check if you have all dependencies, re-run CMake, …)"
  - E ainda na semana passada, lembro de dar um check nos Actions do repositório, para a branch master, muitas versões estavam com falha no build, principalmente a do windows

  - Somado a isso, analisando as branches como falei, descobri uma branch chamada `TestCI`, na qual são feitos ajustes no repositório e testes no GitHub Actions do Build no CI, na qual o commit mais recente chamava `Switch to LLVM for Windows x86 cross-compilation `, algoque me chamou atenção por possivelmente estar relacionado ao problema que eu estava tendo no do windows, relacionado às dependências, e no mesmo dia em que vi essas mudanças, o PR estava aberto contra a master.

  - Então, com o windows em mãos, tentei realizar uma nova tentativa de build, compilação e execução do stk, visto que aparentemente, alguma correção no processo para windows havia ocorrido.

  - O primeiro passo foi atualizar meu repositório local com os novos commits feito pela comunidade, e re-tentar o processo de ponta a ponta, limpei as dependências e binários antigos, para a nova tentativa, e baixei as dependências atualizadas.
  - Segui o passo a passo instruído pela documentação e tive o mesmo problema, a `libc++.dll` não foi encontrada no passo da execução.
  - O passo seguinte foi ajustar qualquer ponto de disparidade em relação a documentação, tentando seguir absolutamente à risca agora, busquei o visual studio recomendado para o processo (Visual Studio 2019 16) via CMake GUI, também não funcionou.

Aqui iniciei uma jornada de tentar conseguir executar o projeto a todo custo, mesmo que fosse via uma alternativa não muito ideal: tentar forçar a execução incluindo manualmente das dependências faltantes:
  - Primeiro, injetei manualmente `libc++.dll` e `libunwind.dll` extraídas da distribuição llvm-mingw. Ocorreram então novos erros de ponto de entrada (_ZNSt...) na `OpenAL32.dll` decorrentes do conflito entre assinaturas C++ do Clang (`libc++`) e do GCC (`libstdc++`).
  - Então, fiz o download e extração do runtime WinLibs (`GCC 16.2.0 UCRT Win64`) para fornecimento da `libstdc++-6.dll` e `libwinpthread-1.dll`, mas ocorreram erros de incompatibilidade de arquiteturas ao misturar DLLs de 32-bit (como libgcc_s_dw2-1.dll) com executáveis de 64-bit.

Seguindo minhas tentativas, o próximo passo foi tentar migrar meu ambiente em que estava realizando o processo para o `MSYS2 CLANG64` + `Ninja`, com o intuito de isolar de forma completa a toolchain necessária no LLVM/Clang.
  - Instalei as dependências necessárias então, e fiz a execução do cmake pelo próprio terminal do MSYS2, onde também houveram erros devido à busca de DLLs do sistema do MSYS2 (como `libcurl-4.dll`) fora do PATH do Windows Explorer. Identifiquei que isso ocorria pois o projeto ainda estava vinculando libs estáticas do visual studio. Limpei novamente meu ambiente, removendo qualquer resquício das tentativas anteriores.
  - Na nova tentativa, o build e compilação ocorreram normalmente, gerando o executável, mas agora, ao tentar executá-lo, siplesmente nada acontecia.

Dessa forma, com esses tantos entraves, decidi dar uma mudada de foco do andamento do projeto, vou focar temporariamente apenas no linux para a próxima semana, pelo menos até eu encontrar alguma ajuda na comunidade sobre o assunto e não ficar travado em algo que deveria ser simples, e isso já tomou tempo demais. Tenho um sentimento ambíguo de frustração, não sei se estou fazendo algo de errado e falta algo na minha máquina para permitir a execução, ou se realmente é um gap de documentação/infraestrutura existente no projeto (que me permitiria uma contribuição sobre, como falei da possibilidade na semana 2).
A ideia de próximos passos é realmente análisar o código, e brincar um pouco no ambiente que já sei que funciona, podendo focar de fato em alguma contribuição.

## Semana 4

### Nota Off-Topic Semi-Relevante

- Na última semana fiz uma troca de SO para desenvolvimento, embarquei na jornada do Arch Linux.
- Isso não muda quase nada, mas a nota serve para evitar a estranheza se pintar alguma confusão entre eu estar usando ubuntu e depois começar a falar do arch do nada.
- Já validei e conseugi buildar e rodar o jogo tranquilamente.

### 17/9

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

### 19/9

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

### 20/9

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

### Semana 4 - Postagem

Olá professor Daniel, tudo bem?

Registrando aqui minha atualização referente à Semana 4.

Após a pausa da semana da pátria, decidi mudar a abordagem em relação aos gargalos de compilação que tive no Windows nas semanas anteriores: deixei o ambiente Windows de lado temporariamente e concentrei 100% dos esforços no Linux (já que eu já tinha testado essa parte de compilação e build nesse ambiente, sem problemas). O objetivo desta semana foi mergulhar a fundo na arquitetura do código (`stk-code/src`) e estruturar o fluxo prático de contribuição, além de entender melhor a dinâmica do git em relação a contribuições e deixar explicitamente registrado o código de conduta esperado pelo projeto sobre LLMs.

Principais avanços e aprendizados da semana:

1. Estruturação do Fluxo Git & Fork:
  - Configurei o modelo de contribuição open source via Fork: meu repositório pessoal como `origin` e o repositório oficial como `upstream`.
  - Tive um insight muito bacana sobre por que o Git exige declarar explicitamente referências remotas (que era algo que eu sempre me questionava ao iniciar um novo repo ou coisas do tipo): essa dinâmica de Fork e PR distribuído permite manter o código local sincronizado com a evolução do projeto via `fetch` e `rebase` contínuos, sem interferir na integridade do repositório
original, e como isso se alinha muito com a essência da filosofia open source (e permitir que ela exista).
  - Criei minha branch de trabalho (`feat/1771-profile-icon`, apenas relembrando: a contribuição é sobre permitir que o jogador escolha uma foto para seu profile local no jogo, cuja escolha hoje é pseudo-aleatória) apontando para a branch `upstream/BalanceSTK2` (branch de desenvolvimento do STK Evolution, na qual a issue #1771 está alocada como milestone) e montei um cheat sheet para me guiar nos comandos nesse jogo de manter meus avanços a par do estado do repositório.
  - Já assumo que provavelmente essa será minha contribuição, pois as coisas estão se encaixando muito bem mentalmente para essa feature, como relato a seguir.

2. Deep Dive nas Classes `PlayerProfile` e `PlayerManager`:
  - Dediquei bastante tempo lendo a implementação dessas classes (`.hpp` e `.cpp`), entendendo suas responsabilidades: o `PlayerManager` atua gerenciando a lista de perfis e orquestrando o carregamento a partir do XML, enquanto o `PlayerProfile` encapsula os dados e o progresso do jogador.
  - Compreendi a dinâmica de runtime: apenas um perfil principal é ativo por máquina (no caso de multiplayer em tela dividida, os outros jogadores entram como "Guest Players", recebendo dados dummy de forma elegante para não corromper as estatísticas do jogador principal).
  - Entendi a mecânica do avatar: o método `addIcon()` escolhe um kart pseudo-aleatório no primeiro boot e copia a imagem para a pasta do usuário. Se o arquivo estiver ausente ou corrompido, há um fallback bem robusto que desenha um ícone de "?" na UI.

3. Engenharia Reversa dos Dados em Disco:
  - Achar o `players.xml` pelo Linux foi bem tranquilo, esse é um ponto no qual estou me divertindo bastante também, o ecossistema facilita muito. Inspecionei diretamente os arquivos gerados em tempo de execução na pasta `~/.config/supertuxkart/config-0.10/`.
  - No arquivo `players.xml`, localizei exatamente as tags que refletem o código C++:
    - O `icon-filename="1.png"` apontando para a imagem do meu avatar físico salvo no mesmo diretório
    - O `default-kart-color="0"`, confirmando o uso de um float para o matiz (Hue do HSV), que o shader de renderização do kart usa diretamente sem duplicar texturas
    - E a tag `<unlock_supertux solved="none" .../>` dentro de `<story-mode>`, que confirma o gatilho da outra issue que mapeei (#4116, notificação ao liberar a dificuldade SuperTux)
  - Algo interessante que notei é que é possível sim usar ícones personalizados, mas sob algumas condições específicas e mais técnicas que exigem conhecimento de como o código lida com os arquivos, ou seja, impossível para o jogador comum.

    4. Transparência e Código de Conduta de IA:
      - Para me apoiar na leitura de uma codebase tão extensa em C++, consultei previamente as diretrizes oficiais da comunidade do SuperTuxKart sobre o uso de IA (SuperTuxKart’s policy on AI-generated code, em https://supertuxkart.net/How_to_contribute_code#supertuxkarts-policy-on-ai-generated-code).
      - O projeto proíbe terminantemente código gerado por LLMs em PRs, mas permite expressamente o uso consciente de IA como ferramenta de apoio para análise e entendimento estrutural da codebase
      - Mantenho esse rigor ético: o uso é estritamente analítico/tutor, e toda a lógica e codificação seguem com autoria 100% própria e compreendida na ponta dos dedos. Nessa semana, alternei entre o Gemini e o Antigravity para me apoiar nessas análises

Estou com os próximos passos semi-mapeados: com a arquitetura das classes de perfil e os arquivos locais compreendidos, para a próxima semana o objetivo é desenhar os requisitos concretos para a Issue #1771 e quem sabe iniciar as primeiras alterações práticas no código.

## Semana 5

### 23/9

- Começando a quinta semana, com pretenções de finalmente por a mão em código, me surgiu uma breve insegurança sobre como dar seguimento na issue #1771 em específico, meu medo foi: "E se alguém atuar na issue ao decorrer do semestre, no meio do meu desenvolvimento?".
- Com isso, me senti na liberdade de ir clamar a issue para que eu me tornasse o assignee, e não correr o risco acima, então deixei um [comentário](https://github.com/supertuxkart/stk-code/issues/1771#issuecomment-5799971261) lá:
  > Hello, I'm an Information Systems student at the University of São Paulo (USP) taking a Free Software Lab course, and I’d like to tackle this issue for my contribution. <br>
  > I aim to start with local profiles, building on top of the STK Evolution branch, allowing the player to choose a kart icon as their avatar in the profile settings or upload one that they chooses. <br>
  > If no one is actively working on it, could you please assign it to me? Thanks! <br>

- E em cerca de 5 minutos, um contribuinte do projeto deixou uma [resposta](https://github.com/supertuxkart/stk-code/issues/1771#issuecomment-5800055301) (inusitada XD) sobre:
  > Well, I just do my code by myself (without any assignment), and do a pull request and pray to the leader developer accepts ;) <br>
  > But that's good (depending of the improvement level) talk about this in forum.supertuxkart.net. <br>
  > PS: Sou Brasileiro também :) (mais daora ainda que também sou de São Paulo kkkk) <br>

- Com isso, posso concluir que querendo ou não, o caminho está aberto para minha contribuição haha!

- Decidi fracionar meus objetivos em pequenas entregas, para formar melhor as ideias de como seguir e tornar os PRs mais aceitáveis
- Objetivos fracionados:
  1. Na hora de criação de perfil (estritamente) permitir que o jogador escolha um ícone dos já existentes
  2. Na hora de criação de perfil (estritamente), adicionar a função de fazer o upload de ícone, além dos já existentes
  3. A qualquer hora, poder editar o ícone de perfil (já existente ou adicionar um)

### 24/9

- Sobre o objetivo fracionado, cheguei nas seguintes análises com apoio do agy para conseguir cumprir o primeiro passo:

  - Expor a lista de karts/ícones
    - O STK já possui o singleton kart_properties_manager (de kart_properties_manager.hpp)
    - Esse singleton já tem a lista completa de todos os karts do jogo getAllKarts() e getNumberOfKarts()

  - Método no PlayerProfile
    - Criar um método explícito como void setIconFromKart(const std::string& kart_ident)
    - Ele busca a textura do kart escolhido via kart_properties_manager, copia para ~/.config/supertuxkart/config-0.10/<unique_id>.png e atualiza m_icon_filename

  - Desacoplar o addIcon()
    - Fazer com que o addIcon() só execute o cálculo pseudo-aleatório antigo se nenhum ícone explícito for passado, mantendo retrocompatibilidade

  - Tela de escolha do Ícone
    - Para não complicar tanto, a ideia é "apenas" criar uma tela Popup, igual a de escolha da cor do kart na tela de register_screen, e não uma nova tela completa.
    - Com isso, posso criar um KartIconSelectionDialog, um modal com uma gradezinha dos karts para clicar e confirmar

### 26/9

- Recebi o [aval](https://github.com/supertuxkart/stk-code/issues/1771#issuecomment-5840414548) do próprio [líder do projeto do STK](https://github.com/Alayan-stk-2) para seguir com a feature XD, apenas registrando aqui, pois agora não tenho mais para onde fugir, bora pro código:
  > If you want to implement the feature, that's welcome.
  > Please make sure to test your code well and to not overcomplicate things!

- Descobri também da pior forma que a branch `BalanceSTK2` não é o lugar mais ideal para trabalhar, mas sim a própria `master`, ajustei meu fork aqui.
- Tive erros de execução por falta de assets e outras dependências, então voltei pro `master`, que não é necessariamente a branch segura da ultima versao stable lançada, mas sim um checkpoint de desenvolvimento estavel!

- Com o build comp exec atualizado, fiz uns testes criando uns perfis de teste para ver o pseudo-aleatório de escolha de ícones em ação e testes de sobrescrita dos png dos ícones com id do player
- Além disso, fiz testes de inclusão de ícone quebrado e personalizado, funcionaram bem! Mas não são tranquilos para o user comum, como já tinha levantado

- O primeiro passo de código em que andei foi na GUI, na tela de deregistro de perfil, onde eu simplesmente adicionei um botão de `Choose Icon` XD
<p align="center">
   <img width="40%" src="img/26-9-26/choose_icon_tests.png">
</p>

- Para entender também como o botão se ligava com o código, fiz um simples trigger no `register_screen.cpp` de printar no terminal "Clicou em Choose Icon", e funcionou normalmente!

```cpp
// ...
    else if (name=="options")
    {
        const std::string button = m_options_widget
                                 ->getSelectionIDString(PLAYER_ID_GAME_MASTER);
        if(button=="next")
        {
            doRegister();
        }
        else if(button=="cancel")
        {
            StateManager::get()->popMenu();
            onEscapePressed();
        }
        else if (button == "choose_icon")
        {
            Log::info("RegisterScreen", ">>> CLICOU NO CHOOSE ICON! <<<");
        }
// ...
```

### Semana 5 - Postagem

Olá professor Daniel, tudo bem?

Nessa semana 5, consegui realizar alguns avanços e interações interessantes.

No início da semana, me surgiu a breve insegurança sobre como dar seguimento na issue #1771 em específico, meu medo foi: "E se alguém atuar na issue ao decorrer do semestre, no meio do meu desenvolvimento?". Com isso, me senti na liberdade de ir clamar a issue para que eu me tornasse o assignee, e não correr o risco que falei acima, então deixei um [comentário](https://github.com/supertuxkart/stk-code/issues/1771#issuecomment-5799971261):
  > Hello, I'm an Information Systems student at the University of São Paulo (USP) taking a Free Software Lab course, and I’d like to tackle this issue for my contribution. <br>
  > I aim to start with local profiles, building on top of the STK Evolution branch, allowing the player to choose a kart icon as their avatar in the profile settings or upload one that they chooses. <br>
  > If no one is actively working on it, could you please assign it to me? Thanks! <br>

Nessa postagem, recebi 2 boas respostas (uma bem inusitada):
  - Um dos contribuinte frequentes do projeto deixou uma [resposta](https://github.com/supertuxkart/stk-code/issues/1771#issuecomment-5800055301):
    > Well, I just do my code by myself (without any assignment), and do a pull request and pray to the leader developer accepts ;) <br>
    > But that's good (depending of the improvement level) talk about this in forum.supertuxkart.net. <br>
    > PS: Sou Brasileiro também :) (mais daora ainda que também sou de São Paulo kkkk) <br>
  - E o próprio líder do projeto respondeu:
    > If you want to implement the feature, that's welcome.
    > Please make sure to test your code well and to not overcomplicate things!

Com isso, pude concluir que o caminho está aberto para minha contribuição, então decidi fracionar meus objetivos em pequenas entregas, para formar melhor as ideias de como seguir e tornar os PRs mais aceitáveis:
  1. Na hora de criação de perfil (estritamente) permitir que o jogador escolha um ícone dos já existentes
  2. Na hora de criação de perfil (estritamente), adicionar a função de fazer o upload de ícone, além dos já existentes
  3. A qualquer hora, poder editar o ícone de perfil (já existente ou adicionar um)

E sobre a primeira parte em específico, cheguei nas seguintes conclusões analisando o código e comportamento do jogo:

  - Expor a lista de karts/ícones
    - O STK já possui o singleton kart_properties_manager (de kart_properties_manager.hpp)
    - Esse singleton já tem a lista completa de todos os karts do jogo getAllKarts() e getNumberOfKarts()

  - Método no PlayerProfile
    - Criar um método explícito como void setIconFromKart(const std::string& kart_ident)
    - Ele busca a textura do kart escolhido via kart_properties_manager, copia para ~/.config/supertuxkart/config-0.10/<unique_id>.png e atualiza m_icon_filename

  - Desacoplar o addIcon()
    - Fazer com que o addIcon() só execute o cálculo pseudo-aleatório antigo se nenhum ícone explícito for passado, mantendo retrocompatibilidade

  - Tela de escolha do Ícone
    - Para não complicar tanto, a ideia é "apenas" criar uma tela Popup, igual a de escolha da cor do kart na tela de register_screen, e não uma nova tela completa.
    - Com isso, posso criar um KartIconSelectionDialog, um modal com uma gradezinha dos karts para clicar e confirmar

Essa semana descobri também da pior forma que a branch `BalanceSTK2` não é o lugar mais ideal para trabalhar, mas sim a própria `master`. Tive erros de execução por falta de assets e outras dependências, então voltei pro `master`, que não é necessariamente a branch segura da ultima versao stable lançada, mas sim um checkpoint de desenvolvimento estavel! Então ajustei meu fork local para poder retomar os avanços.

Fiz uns testes criando uns perfis de teste para ver o pseudo-aleatório de escolha de ícones em ação e testes de sobrescrita dos png dos ícones com id do player, e além disso, fiz testes de inclusão de ícone quebrado e personalizado, funcionaram bem! Mas não são tranquilos para o user comum, como já tinha levantado.
Os testes incluíram:
- Criação de perfis com ícones pseudo-aleatórios
- Sobrescrita manual dos ícones em `~/.config/supertuxkart/config-0.10/<unique_id>.png`
- Inclusão de ícones personalizados e ícones quebrados ou ausentes

O primeiro passo de código em que andei foi na GUI, na tela de deregistro de perfil (`register.stkgui`, um tipo de xml especial dedicado para telas do jogo), onde eu simplesmente adicionei um botão de `Choose Icon`
<p align="center">
   <img width="40%" src="img/26-9-26/choose_icon_tests.png">
</p>

Para entender também como o botão se ligava com o código, fiz um simples trigger no `register_screen.cpp` de printar no terminal "Clicou em Choose Icon", e funcionou!

```cpp
// ...
    else if (name=="options")
    {
        const std::string button = m_options_widget
                                 ->getSelectionIDString(PLAYER_ID_GAME_MASTER);
        if(button=="next")
        {
            doRegister();
        }
        else if(button=="cancel")
        {
            StateManager::get()->popMenu();
            onEscapePressed();
        }
        else if (button == "choose_icon")
        {
            Log::info("RegisterScreen", ">>> CLICOU NO CHOOSE ICON! <<<");
        }
// ...
```

Com esses passos, fecho a semana 5, com o intuito de seguir com os avanços visuais que me permitam ver minhas implementações funcionando!

## Semana 6

### 28/9

- Começando a semana 6, fiz um breve levantamento de arquivos relevantes que valem a pena eu manter registrado em `relevant_files.md`

- Primeiro passo foi mover o botão de choose icon e seu trigger de processamento para o `user_screen` .cpp e .stkgui
- Simplesmente pois é mais simples e implementável seguir com a escolha do icon assim que o profile estiver meio pronto, assim como o kart color. Dessa forma, posso seguir com o default o pseudo-aleatório já existente, e o usuário vai lá e escolhe outro! Simples assim

- O próximo passo é criar um Dialog `icon_selection_dialog` assim como o `kart_color_slider_dialog`, para isso, preciso entender um pouco da sintaxe desses modais, e depois ligar a escolha do icone com um método que seta isso no player profile!

### 29/9

- Para começar o dia, fiz um simples teste de integração:
  - Basicamente, copiei a implementação de `kart_color_slider_dialog` no `icon_selection_dialog`, ajustando as referências, imports e nomenclaturas
  - A ideia era ver se a integração do novo botão de `Choose Icon` replicava o comportamento do seletor de cor de kart, de forma idêntica mesmo
  - funcionou!

- O passo seguinte foi fazer um dialog bem boilerplate, com uma implementação mínima, mas agora referente ao verdadeiro IconSelectionDialog
- Algo que está sendo bem útil nessa implementação, e no qual eu vou me basear muito é o já citado `kart_color_slider_dialog`, pois vai seguir uma lógica muito parecida, já que ele também pega e salva um dado em um PlayerProfile, meio caminho dessa implementação eu posso puxar dele!

- Fiz meu primeiro commit na minha branch XD, pois é algo bem seguro e que abre esaço pro resto da minha implementação
  - `data/gui/dialogs/icon_selection_dialog.stkgui`
  - `data/gui/screens/user_screen.stkgui`
  - `.../dialogs/icon_selection_dialog.cpp`
  - `.../dialogs/icon_selection_dialog.hpp`
  - `src/states_screens/options/user_screen.cpp`

  -  É basicamente o que eu comentei acima (título e descrição do commit abaixo):
    -  Add 'Choose Icon' button to user screen
    -  The button triggers an empty dialog, with a boilerplate dialog (IconSelectionDialog), which will be implemented next
