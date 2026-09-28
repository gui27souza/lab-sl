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