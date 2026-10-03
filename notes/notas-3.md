# Semana 3

## 2/9

- Dei uma pausa na minha jornada do build no Windows, tanto pq aqui na faculdade só consigo usar o meu note com ubuntu, tanto quanto para focar um pouco na análise e entendimento do projeto.
- Hoje basicamente analisei documentação do site, acabei não indo a fundo em código.

## 3/9

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

## 6/9

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
