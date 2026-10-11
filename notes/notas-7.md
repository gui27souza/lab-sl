# Semana 7

## 5/10

- Iniciando a semana 7 atacando o próximo alvo óbvio: centralizar o grid de icons do dialog de seleção de ícones. Tentei fazer alguns tiros no escuro com o meu conhecimento enferrujado de css, mas não funcionou.
- Pedi ajuda do Gemini também para talvez me dar uma luz mas nenhuma solução ainda.
- Algo que tentei também foi talvez colocar uma bordinha vermelha no ribbon-grid para entender o comportamento dele com cada configuração, mas não é simples assim colocar uma borda pelo xml.

- Estou cogitando postar esses meus avanços lá na issue e pedir uma ajuda sobre a centralização do ribbon pro pessoal

- Antes de mandar lá, bati um pouquinho mais a cabeça, percebi que é uma jogada com a div principal e o alinhamento do ribbon grid, em `<div x="2%" y="20%" width="96%" height="80%" layout="vertical-row">`
- Até consegui manipular o grid, mas não ficou verdadeiramente alinhado, muito mais hardcoded, e não me garante nada que em telas de tamanhos diferentes funcionaria.
- Com isso, vou pedir ajuda pro pessoal lá na issue.

- Deixei um [comentário](https://github.com/supertuxkart/stk-code/issues/1771#issuecomment-6007067561) lá:

    > Hey guys, how are you doing?
    > I made some progress on this issue, but I'm having a hard time dealing with the GUI. To make it simpler as requested, I thought on keeping the pseudo-random default icon, and only allowing the player to choose the icon later, just like the kart color.
    > For what matters now, I basically:
    > 1. Created a new dialog specific for "Choose Icon", pretty much inspired by the `kart_color_slide_dialog` (all the .cpp, .hpp and .stkgui file). First step was to create a boilerplate dialog, empty with only the cancel/apply buttons.
    > 2. The next step was to create a DynamicRibbonWidget, so I can dynamically populate it with the actual icons
    > ```cpp
    > IconSelectionDialog::IconSelectionDialog(PlayerProfile* pp)
    >                     : ModalDialog(0.75f, 0.75f, MODAL_DIALOG_LOCATION_CENTER)
    > {
    >     loadFromFile("icon_selection_dialog.stkgui");
    >     m_player_profile = pp;
    > 
    >     m_buttons_widget = getWidget<RibbonWidget>("buttons");
    >     assert(m_buttons_widget);
    > 
    >     m_kart_icons = getWidget<DynamicRibbonWidget>("kart_icons");
    >     assert(m_kart_icons);
    > 
    >     m_kart_icons->updateItemDisplay();
    > }   // IconSelectionDialog
    > 
    > ```
    >
    > 3. Then, I populate the ribbon in the `beforeAddingWidgets` method:
    > ```cpp
    > void IconSelectionDialog::beforeAddingWidgets()
    > {
    > 
    >     m_kart_icons = getWidget<DynamicRibbonWidget>("kart_icons");
    >     assert(m_kart_icons);
    > 
    >     m_kart_icons->clearItems();
    >     for(unsigned int i=0; i<kart_properties_manager->getNumberOfKarts(); i++)
    >     {
    >         const KartProperties* kp = kart_properties_manager->getKartById(i);
    > 
    >         if (!kp) continue;
    > 
    >         m_kart_icons->addItem(
    >             kp->getName(),
    >             kp->getIdent(),
    >             kp->getAbsoluteIconFile(),
    >             0,
    >             IconButtonWidget::ICON_PATH_TYPE_ABSOLUTE
    >         );
    > 
    >     }
    > 
    > }   // beforeAddingWidgets
    > 
    > ```
    >
    > * This is the `icon_selection_dialog.stkgui`
    > ```xml
    > <?xml version="1.0" encoding="UTF-8"?>
    > <stkgui>
    >     <div x="2%" y="20%" width="96%" height="80%" layout="vertical-row">
    >         <spacer height="10" width="10"/>
    > 
    >         <ribbon_grid id="kart_icons"
    >             width="100%"
    >             height="100%"
    >             proportion="1"
    >             layout="horizontal-row"
    >             square_items="true"
    >             child_width="96"
    >             child_height="96"
    >         />
    > 
    >         <spacer height="10" width="10"/>
    > 
    >         <buttonbar id="buttons" height="20%" width="30%" align="center">
    >             <icon-button id="cancel" width="128" height="128" icon="gui/icons/red_x.png"
    >                 I18N="In the icon chooser dialog" text="Cancel" align="center"/>
    >             <icon-button id="apply" width="128" height="128" icon="gui/icons/green_check.png"
    >                 I18N="In the icon chooser dialog" text="Apply" align="center"/>
    >         </buttonbar>
    >     </div>
    > </stkgui>
    > 
    > ```
    > 
    > The thing is: I cant align the grid at all, i tried playing with the 'x' and 'y', both 'width' and 'height', but that's the best I could get, and I fear that even that I get the right values, it isn't the best way to deal with it, and also can't garantee that it will fit in every screen, I'm worried hardcoding values won't scale right.
    > How would you recommend to properly align it? Thanks in advance!

## 11/10

- Voltando aqui no domingo, registrando também alguns passos que dei na semana, mas esqueci de registrar

- Durante a semana, tentando entender como ligar o botão de ícone gerado dinamicamente ao código para eu adicionar o processamento do mesmo. Nesse processo, me surgiu o questionamento:

    > Pensando em UX, qual seria o formato ideal?
    > 1. O usuário clicar no ícone, deixando o botão em um estado pré-selecionado, e então o player confirma a seleção com o botão verde de apply
    > 2. Removo o botão de apply, mantendo apenas o de sair do dialog, e o usuário clica no ícone, já aplicando automaticamente

- Meu feeling como usuário foi que clicar direto fosse mais "ergonômico", então fiz uns ajustes no código pensando que seria a saída ideal; removi o botão de apply por exemplo
- Porém, fazendo uma análise nos outros dialogs, o modelo comum e padrão do stk é o 1: pré-seleção e confirmação!
- Então, fiz rollback nas minhas mudanças, a fim de manter o modelo padrão.

- Além disso, não tive retorno ainda sobre o comentário que deixei na issue sobre a melhor forma de centralizar o dynamic ribbon widget.

- Mais tarde no domingo, continuei quebrando a cabeça para tentar fazer o clique do mouse no ícone do grid funcionar e dar um feedback visual (com o OK_BADGE verde) antes do Apply.
- Passei por um looping bem frustrante:
  - Ao tentar ler `m_kart_icons->getSelectionIDString(...)` no Apply, a seleção vinha vazia porque ao clicar no Apply o foco do jogador saía da grade e ia pro próprio botão de Apply.
  - Resolvi guardar a seleção em uma variável membro (`m_selected_kart`).
  - Mas aí veio o perrengue real: clicar com o mouse físico nos ícones simplesmente NÃO disparava o processamento do clique. Apenas passar o mouse e apertar `Enter` no teclado funcionava e desenhava o selo verde.
  - Tentei implementar o `DynamicRibbonHoverListener`, mas ele disparava em cascata só de mover o mouse em cima dos ícones (hover), o que não faz sentido nenhum para clique.
  - Investigando a fundo o código da engine (`src/guiengine/event_handler.cpp`, linha 765), descobri o motivo:
    ```cpp
    if (ModalDialog::isADialogActive() && (parent == NULL || parent->m_type != GUIEngine::WTYPE_RIBBON))
    ```
    Ou seja, a engine do STK tem uma trava explícita que bloqueia o repasse de cliques de botões que sejam filhos diretos de um `WTYPE_RIBBON` quando estão dentro de um `ModalDialog`!
  - Basicamente, o `ribbon_grid` / `DynamicRibbonWidget` foi desenhado para telas completas (como a tela de escolha de karts), e não para dialogs modais. Ficar brigando contra a própria arquitetura da engine só traz dor de cabeça e comportamentos imprevisíveis.

- Diante disso, decidi dar um passo para trás e mudar a abordagem para a próxima semana:
  - Em vez de forçar um `ribbon_grid` dentro do modal, vou migrar para o padrão dos outros dialogs do STK: botões normais (`<icon-button>`) em containers `<div>` nativos.
  - Como são botões diretos sem estarem encapsulados em linhas de ribbon, a engine não vai barrar os cliques de mouse e o evento vai bater 100% limpo no `processEvent`.
  - Como vou ter que reformular boa parte dessa abordagem de GUI, decidi deletar minha postagem lá na issue do GitHub, evitando deixar uma dúvida que não vai mais fazer sentido com a nova direção técnica.
- Por ora, fecho a semana por aqui pois o cansaço mental bateu forte. O importante é que a causa raiz do problema foi identificada e o caminho para a semana 8 está traçado.
