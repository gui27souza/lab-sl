# Semana 12

## 11/11

- Pra começar a semana, vou atacar primeiro a abertura oficial do primeiro PR, com um leve refinamento e documentando os débitos técnicos, pois aí talvez eu tenha uma resposta enquanto trabalho na segunda contribuição.

- Os refinamentos que planejo fazer são aplicar os novos conhecimentos sobre o front-end e interface com stkgui

- Depois de dar uma re-lida, nem precisu de ajuste algum, já tá bem alinhado com oq estava pensando em ajustar, edssa forma, abri o PR oficialmente, marcado como "Ready for Review", com um [comentário](https://github.com/supertuxkart/stk-code/pull/5907#issuecomment-6809080619):

    > Since the grid alignment is fixed, but still need some guidance on how to proceed, I'm sending this PR as ready for review, but also aware of what is still not ready on the Icon Selection Dialog:
    > - Mouse selection: Hovering with the mouse highlights the item, but clicking doesn't trigger the selection action like pressing Enter on the keyboard does.
    > - Arrow key navigation: Focus/arrow navigation across grid rows within the modal dialog doesn't work.
    > ---
    > Even though, it's possible now to choose an icon directly from the user_screen:
    > - Click `Choose Icon`
    > - On the dialog, hover the one you want and click enter (a check badge will appear)
    > - Click `Apply`
    > - Icon changed!

## 14/11

- Ainda sem retorno no PR
- Hoje foquei na implementação do `IconPathInputDialog::validatePath()` e da renderização do ícone após a validação

- O primeiro passo foi encontrar a melhor opção para um objeto na interface no qual eu pudesse mudar dinâmicamente, com o intuito de renderizar o ícone a ser escolhido após a validação do path.
- Análisando as documentações pelo doxygen, encontrei o `IconButtonWidget`, que tem o método `setImage`, se encaixou como uma luva pro propósito.

- Antes de por a mão na massa com o validate path, fiz uma pequena limpeza no código, pois haviam 2 variáveis de propósito similar no Dialog:
    - `bool m_is_valid_icon_path` - uma flag que marcava se o path do ícone estava válidado ou não
    - `std::string m_icon_path` - o path do ícone em si
- Fiquei com a sensação de que eu poderia melhorar essa validação, assim como o PlayerProfile fazia com o gerenciamento de caminho de ícone: apenas armazenava o path se estivesse válido, ou seja:
    - se `icon_path.empty()` - não é válido
    - se `!icon_path.empty()` - ícon válido
- Apliquei essa mudança no meu código do Dialog e ficou muito mais limpo!

- Após isso, iniciei a implementação de fato da validação do input de path para o ícone, e a grosso modo, o passo a passo para o método final ficou como:
    - Converter o texto de input para `std::string`, pois é mais simpels de manipular
    - Fazer as validações do path e ícone:
        - input vazio
        - se o arquivo existe - há um singleton disponível para uso chamado `file_manager`, ele é um util justamente p lidar com arquivos
        - se o arquivo é um `png` - há um módulo `StringUtils` também com um método pronto já de validação de extensão de arquivo
        - se a imagem é de fato suportada e não está corrompida - também há um singleton referente a driver de vídeo com operações úteis com imagens, e o método usado aqui foi o de criar uma imagem na memória RAM com base no arquivo existente
        - se a imagem tem proporção próxima de 1:1 - se a imagem foi criada com sucesso no passo anterior, podemos pegar a altura e largura em pixels da mesma, e fazendo uma razão entre elas, conseguimos o valor do ratio (claro né XD). Para o ratio, deixei uma gordura com valores válidos de 0.9 a 1.1, pois não preciso checar qual das duas dimensões é maior e garanto que imagens por exemplo de 511 x 512 pixels sejam válidas
    - Apenas se passar por todos esses gates, o método retorna o path para ser armazenado em definitivo no campo `m_icon_path`!
    - E em todos os casos de path inválido, é feito um feedback ao usuário!

- Após passar por tudo isso então, o `m_icon_path` é armazenado de fato, garantindo um caminho real e válido pro icon, e então o botão de `Apply` no dialog é liberado!

- Adicionalmente, fiz um método `resetState()` no dialog com o intuito de facilitar a tratativa em casos de paths invalidos e etc, garantindo sempre o estado inicial esperado ao checar um novo ícone!

- Para finalizar o dia, fazendo uma leve revisão com o antigravity, ele m e sugestionou envolver os textos de feedback ao usuário com um wrapper que permite a tradução do texto! O wrapper é literalmente um underscore e vem de `#include "utils/translation.hpp"`, fica assim:
    - `_("Please, provide an icon file path.")`
- Dessa forma, o processo de tradução por outros contribuidores fica automatizado, fica marcado como tradução necessária em uma outra plataforma do stk e quem quiser pode ajudar!

---

- No fim das contas, meu código do novo Dialog está basicamente pronto

- Validação do input de path
```cpp
std::string IconPathInputDialog::validatePath()
{
    std::string path = StringUtils::wideToUtf8(
        getWidget<TextBoxWidget>("icon_path_textbox")->getText().c_str()
    );
    if (path.empty()) {
        m_icon_path_output_widget->setText(_("Please, provide an icon file path."));
        return "";
    }
    if (!file_manager->fileExists(path)) {
        m_icon_path_output_widget->setText(_("File does not exist."));
        return "";
    }
    if (StringUtils::toLowerCase(StringUtils::getExtension(path)) != "png") {
        m_icon_path_output_widget->setText(_("Invalid file type, 'png' expected."));
        return "";
    }
    video::IVideoDriver* driver = irr_driver->getVideoDriver();
    video::IImage* img = driver->createImageFromFile(path.c_str());
    if (!img) {
        m_icon_path_output_widget->setText(_("Unsupported or corrupted image."));
        return "";
    }
    core::dimension2d<u32> dim = img->getDimension();
    img->drop();
    float ratio = 0;
    if (dim.Width > 0 && dim.Height > 0) {
        ratio = (float) dim.Width / (float) dim.Height;
    }
    if (ratio < 0.9 || ratio > 1.1) {
        m_icon_path_output_widget->setText(_("Invalid image ratio, must be near 1:1."));
        return "";
    }
    return path;
}   // validatePath
```

- Processamento das interações

```cpp
GUIEngine::EventPropagation
            IconPathInputDialog::processEvent(const std::string& eventSource)
{
    if (eventSource == "path_input_buttons")
    {
        const std::string& selection = m_path_input_buttons_widget->
                                    getSelectionIDString(PLAYER_ID_GAME_MASTER);
        if (selection == "validate") {
            resetState();
            m_icon_path_output_widget->setText("");
            m_icon_path = validatePath();

            if (!m_icon_path.empty()) {
                m_icon_render->setImage(m_icon_path, IconButtonWidget::ICON_PATH_TYPE_ABSOLUTE);
                m_icon_render->setActive(true);
                m_icon_path_output_widget->setText(_("Valid icon!"));
                m_apply_button->setActive(true);
            }
            else
                resetState();
        }
    }
    if (eventSource == "buttons")
    {
        const std::string& selection = m_buttons_widget->
                                    getSelectionIDString(PLAYER_ID_GAME_MASTER);
        if (selection == "apply")
        {
            if (!m_icon_path.empty())
            {
                m_player_profile->setIconFromPath(m_icon_path);
                m_parent_screen->loadProfilesIcons();
            }
            ModalDialog::dismiss();
            return GUIEngine::EVENT_BLOCK;
        }
        else if (selection == "cancel")
        {
            ModalDialog::dismiss();
            return GUIEngine::EVENT_BLOCK;
        }
    }
    return GUIEngine::EVENT_LET;
}   // processEvent
```

---

- Os próximos passos serão focados em:
    - refinar a interface do dialog, ainda não estou 100% satisfeito (melhorar as dimensões do ícone renderizar após validar, entre outros)
    - criar o método `PlayerProfile::setIconFromPath(const std::string& icon_path)` para persistir a escolha do usuário de fato
