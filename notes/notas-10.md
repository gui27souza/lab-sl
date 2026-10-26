# Semana 10

## 26/10

- Recebi uma resposta do meu amigo BR no Draft PR!
- https://github.com/supertuxkart/stk-code/pull/5907#issuecomment-6398405683

> Take a look here: <br>
>   https://github.com/supertuxkart/stk-code/blob/master/data%2Fgui%2Fdialogs%2Fandroid%2Fmultitouch_settings.stkgui <br>
> This uses a ribbon widget (to show the control methods for mobile)

- Olhando o `multitouch_settings.stkgui`, nao achei oq poderia me ajudar, pelo q entendi, tem alguns pontos:
    - preciso que meu elemento seja um ribbon_grid e nao um ribbon, para que possa ser dinamicamente populado
    - e outra coisa q percebi tmb, é q nenhum dialog usa um ribbon_grid, sempre telas completas, por isso esta sendo dificil encontrar uma solução

- Não quero propor um ajuste na engine, pois seria muito complexo, deixei uma resposta no draft PR:

> @SewerFox Thanks for the pointer! I took a look at `multitouch_settings.stkgui`, but it uses a static `<ribbon>` with pre-defined buttons declared directly in XML. <br>
> In our case, since the kart icons must be loaded dynamically (and we have lots of icons, requiring multiple rows/scrolling), I had to use `<ribbon_grid>` (`DynamicRibbonWidget`). <br>
> Exploring the codebase, I noticed that `<ribbon_grid>` is currently only used in full screens (like `karts.stkgui` or `tracks.stkgui`), and never inside a Dialog. That seems to be the root of the issue, in my view:
>   1. In modal dialogs, mouse clicks on children of a ribbon widget seem to be ignored by the engine (`event_handler.cpp` filters out `WTYPE_RIBBON` parents in modals, so only keyboard Enter activates the item). <br>
>   2. The layout calculation in dialog causes the ribbon_grid to misalign or clip its upper border unless weird percentage paddings are hardcoded. <br>
>
> Do you guys know of any recommended way or existing pattern to dynamically ￹display a list/grid of icons inside a dialog? I think it's better to stick to ￹standard GUI practices rather than touching core engine behavior if ￹possible. XD

- Vou pausar por aqui hj, ou até eu conseguir uma resposta
