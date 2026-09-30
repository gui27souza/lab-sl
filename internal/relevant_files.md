- player_profile.cpp/.hpp

    - O cerne do backend sobre gerenciamento de perfil do user
    - Onde vai entrar meus métodos que atualizam de fato o ícone do profile

---

- player_manager.cpp/.hpp

    - Relevante pois carrega e salva o xml contendo os players e os dados

---

- user_screen.cpp/.hpp

    - tela inicial do gerenciador de perfis
    - adicionei o choose icon aqui

---

- register.stkgui
    - estrutura de dados da tela de registro, com os elementos e botões da tela

- register_screen.cpp/.hpp
    - faz a ligação da tela acima com código

---

- kart_color_slider_dialog.cpp/.hpp

    - Implementação de um dialog (tipo um modal ou popup) em cima da tela de gerenciamento de perfis
    - Implementa a interação e carregamento do dialog

- kart_color_slider_dialog.stkgui

    - Estrutura crua do modal em xml, com os elementos e botões de tela

