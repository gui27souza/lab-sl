# Semana 6

## 28/9

- Começando a semana 6, fiz um breve levantamento de arquivos relevantes que valem a pena eu manter registrado em `relevant_files.md`

- Primeiro passo foi mover o botão de choose icon e seu trigger de processamento para o `user_screen` .cpp e .stkgui
- Simplesmente pois é mais simples e implementável seguir com a escolha do icon assim que o profile estiver meio pronto, assim como o kart color. Dessa forma, posso seguir com o default o pseudo-aleatório já existente, e o usuário vai lá e escolhe outro! Simples assim

- O próximo passo é criar um Dialog `icon_selection_dialog` assim como o `kart_color_slider_dialog`, para isso, preciso entender um pouco da sintaxe desses modais, e depois ligar a escolha do icone com um método que seta isso no player profile!

## 29/9

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

## 1/10

- O foco hoje foi entender como renderizar os ícones de cada personagem, pois justamente o próximo passo para minha implementação é gerar os botões clicáveis e gerar a fiação com o código de persistência da escolha no PlayerProfile
- Descobri o já esperado:
  - o ideal é renderizar os ícones dinamicamente, ou seja, para isso preciso de um DynamicRibbonWidget, ou seja, uma caixinha que se expande dinamicamente, perfeito para meu caso 
  - também encontrei uma implementação parcial já existente em `user_screen.cpp`
  - mas essa implementação não é em um dialog, e a implementação de um Dialog para uma Screen é um pouco diferente, por isso preciso entender um pouco melhor
    - como usar um dynamic ribbon reservado para os icons
    - como integrar com getNumberOfKarts() e getKartById(n) do kart_properties_manager, que vai me prover os IDs dos ícones dos personagens e o caminho do .png de cada ícone

- Deixei um campo DynamicRibbonWidget reservado no meu IconSelectionDialog, mas ainda estou batendo cabeça em como incluir o campo na classe, parece que não é só declarar no hpp e inicializar no construtor o_O

## 2/10

- Esse foi o dia de quebrar a cabeça com os imports meio confusos do c++ e entender de fato o pipeline de funcionamento da renderização de um dialog, pois descobri que alguams coisas ocorrem debaixo dos panos ao carregar uma tela no formato de `.stkgui`
- Sobre os imports, é besteira mas foi algo que me consumiu um pouquinho de tempo. Como muita coisa tava sendo importada de tabela e outras não, ficou um pouco confuso e precisei dar uma limpa nos imports dos meus arquivos e fazer uma seleção mais granular do que importar, principalmente em relação aos widgets que preciso usar.

- Sobre o pipeline de carregamento do `.stkgui` e mais especificamente sobre o dialog, entendi o seguinte:
  - Meio que cada `.stkgui` tem uma classe correspondente, que ao ser inicializada com seu construtor, precisa obviamente carregar o `.xml` (que ainda é o `.stkgui`)
  - Nesse carregamento do arquivo, internamente ele tem um passo anterior de `beforeAddingWidgets`, ou seja, um pré-passo preparatório para adicionar aluma lógica necessária ao dialog
    - É bem aqui que entra o carregamento dinâmico dos ícones dos personagens disponíveis, já detalho melhor como fiz isso
  - Ou seja, o passo a passo do carregamento dos ícones ficou:
    1. Contruir uma instância da classe
    2. No construtor, carrega o arquivo
    3. Antes de renderizar, lida com a lógica de busca dinâmica de personagens disponíveis e ícones
    4. Só então rederizo tudo isso

- E sobre como eu consigo todos os ícones de forma dinâmica, isso foi mais fácil de entender:
  - Existe um singleton `KartPropertiesManager` que provê várias informações úteis (aqui podemos considerar kart = personagem), como o número de karts existentes, o nome e identificador de cada um e o mais importante, o *path do icon* de cada um!
  - Então, ali no `beforeAddingWidgets`, faço o uso do singleton de karts para gerar um widget dinamicamente conforme existirem karts!

```cpp
void IconSelectionDialog::beforeAddingWidgets()
{

    m_kart_icons = getWidget<DynamicRibbonWidget>("kart_icons");
    assert(m_kart_icons);

    m_kart_icons->clearItems();
    for(unsigned int i=0; i<kart_properties_manager->getNumberOfKarts(); i++)
    {
        const KartProperties* kp = kart_properties_manager->getKartById(i);

        if (!kp) continue;

        m_kart_icons->addItem(
            kp->getName(),
            kp->getIdent(),
            kp->getAbsoluteIconFile(),
            0,
            IconButtonWidget::ICON_PATH_TYPE_ABSOLUTE
        );

    }

}
```

- E para mais contexto, esse é o estado do meu xml `icon_selection_dialog.stkgui`:

> O `befforeAddindWidgets` pega os karts e insere dinamicamente no container `ribbon_grid`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<stkgui>
    <div x="2%" y="0%" width="96%" height="95%" layout="vertical-row">
        <spacer height="20" width="10"/>
        <div layout="horizontal-row" width="100%" proportion="1" align="center">
            <ribbon_grid id="kart_icons" height="100%" width="100%" align="center"
                        square_items="true" child_width="96" child_height="96" />
        </div>
        <spacer height="10" width="10"/>
        <buttonbar id="buttons" height="20%" width="30%" align="center">
            <icon-button id="cancel" width="128" height="128" icon="gui/icons/red_x.png"
                I18N="In the kart color slider dialog" text="Cancel" align="center"/>
            <icon-button id="apply" width="128" height="128" icon="gui/icons/green_check.png"
                I18N="In the kart color slider dialog" text="Apply" align="center"/>
        </buttonbar>
    </div>
</stkgui>
```

- E com isso, finalmente meu dialog ganha vida!

<p align="center">
   <img width="40%" src="../img/2-10-26/icon_dialog_unaligned.png">
</p>

- Claramente ainda está bem desalinhado, então tenho claro meu próximo passo: brincar o frontend usando xml
