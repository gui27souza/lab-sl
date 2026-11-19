# Semana 13

## 19/11

- Tive um retorno no PR! o meu colega BR deixou um [comentário](https://github.com/supertuxkart/stk-code/pull/5907#issuecomment-7007695447):

> Hello Gui, I tested your code and the icon system works as promised, but about the UI ,I think using the same DynamicRibbon style of the user icons in the user selection screen would be better than using a badge to indicate the selected icon (to follow the same pattern of all screens). <br>
> Btw, you should add a scroll system to the dialog because there have a big chance of overflow if the player have a big amount of karts installed.

- O retorno tem uma dica sobre o uso do DynamicRibbon, especificamente sobre o uso de badges, mas não está claro p mim se isso resolveria meus outros problemas de UI, só testando para descobrir mesmo.
- Obviamente a segunda dica de scroll é bem útil também, com certeza vou olhar, não sabia que tinha a possibilidade do próprio usuário incluir novos personagens.

- Com isso, meu to-do atualizado é:
    - Refinar UI do UploadIconDialog
    - `PlayerProfile::setIconFromPath(const std::string& icon_path)` 
    - Revisitar DynamicRibbon no `IconChooseDialog` - tornar similar ao do UserScreen
    - Adicionar Scroll no `IconChooseDialog`

- Fazendo umas análises e sondagem nas mudanças sugeridas, somadas aos problemas de interface que ainda passo, decidi um plano de ação e já deixei uma [resposta no PR](https://github.com/supertuxkart/stk-code/pull/5907#issuecomment-7024619871):

> @SewerFox Hello! Thanks a lot for testing and for the feedback! XD <br> <br>
> That makes total sense. Just to give a bit of context: I used the badge mainly as a workaround for the dialog quirks. Since mouse clicks weren't locking the focus/selection on ribbon items inside the modal, the badge was the only way I found to give visual feedback that an icon was picked. <br> <br>
> Regarding the scroll system: when testing earlier, I tried <scrollable_ribbon>, but having all the kart icons in a single horizontal line didn't feel as pleasant for UX compared to seeing the grid of icons at once. If I'm not mistaken, <ribbon_grid> (DynamicRibbonWidget) already has pagination/scroll arrows built-in when there are many items, but the dialog event handling was getting in the way. <br> <br>
> Because of that, I was considering turning IconSelectionDialog into a full Screen instead. That would likely solve all these GUI issues at once: mouse selection, keyboard/arrow navigation, and the native grid pagination would work out of the box (just like in the kart selection screen). What do you think about this approach? <br> <br>
> Meanwhile, I'm almost done with the custom icon upload feature (which works independently of whether this is a Dialog or a Screen) and I plan to finish first. If handling the ribbon inside a modal proves too problematic, I can try converting it to a Screen. Sounds like a good plan?

- Ou seja, meu plano de ação é:
    - comentário postado no PR
    - Em paralelo a isso, finalizo a implementação do IconPathInputDialog, já q o pai sendo o dialog ou uma nova screen, o upload de icone continua o mesmo
    - Se eu receber uma resposta no PR, sigo com ela, se não receber, faço a tentativa de conversão para screen

- Outro ponto que eu notei é que preciso limitar o updload de ícone pro Windows/Linux, não ter a opção no mobile pelas dificuldades da coisa

- Meu colega BR já [respondeu](https://github.com/supertuxkart/stk-code/pull/5907#issuecomment-7025153734):

> Well, taking in account that there is a real change of some player have gazillion of installed karts (that's usual), the screen may be the most suitable option imo

- Atualizando o to-do:
    - Refinar UI do UploadIconDialog
    - `PlayerProfile::setIconFromPath(const std::string& icon_path)`
    - Limitar upload de ícones para Windows/Linux, remover mobile
    - Converter IconSelectionDialog para screen
