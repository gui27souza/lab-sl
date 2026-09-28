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