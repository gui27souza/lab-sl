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
