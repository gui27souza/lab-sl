# Semana 2

## 19/8

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

## 30/8

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
