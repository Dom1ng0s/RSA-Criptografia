# Criptografia RSA em Python

Este repositório contém uma implementação em Python do algoritmo de criptografia RSA, permitindo a geração de chaves pública e privada, encriptação e desencriptação de mensagens.

## Funcionalidades

*   **Geração de Chaves:** Gera um par de chaves RSA (pública e privada) a partir de dois números primos fornecidos pelo usuário.
*   **Encriptação de Mensagens:** Encripta mensagens de texto usando a chave pública.
*   **Desencriptação de Mensagens:** Desencripta mensagens de texto usando a chave privada.

## Arquivos no Repositório

*   `CódigoFinal.py`: O código-fonte principal da aplicação Python, contendo todas as funções para geração de chaves, encriptação e desencriptação.
*   `ExplicaçãoCódigo.pdf`: Um documento PDF detalhando o funcionamento do código, explicando cada seção e as funções matemáticas envolvidas no algoritmo RSA.
*   `chave_privada.txt`: Arquivo gerado que armazena a chave privada (d, n).
*   `chave_publica.txt`: Arquivo gerado que armazena a chave pública (e, n).
*   `mensagem_encriptada.txt`: Arquivo gerado que armazena a mensagem encriptada.
*   `mensagem_desencriptada.txt`: Arquivo gerado que armazena a mensagem desencriptada.

## Como Usar

### Pré-requisitos

Certifique-se de ter o Python 3 instalado em seu sistema.

### Executando o Programa

1.  **Clone o repositório:**

    ```bash
    git clone <URL_DO_REPOSITORIO>
    cd <NOME_DO_REPOSITORIO>
    ```

2.  **Execute o script principal:**

    ```bash
    python3 CódigoFinal.py
    ```

3.  **Menu de Opções:**

    O programa apresentará um menu com as seguintes opções:

    *   `1 - Gerar chave pública e privada`: Solicita dois números primos (p e q) e um expoente 'e'. Gera e salva as chaves em `chave_publica.txt` e `chave_privada.txt`.
    *   `2 - Encriptar`: Solicita a mensagem a ser encriptada e a chave pública (e,n). A mensagem encriptada será salva em `mensagem_encriptada.txt`.
    *   `3 - Desencriptar`: Solicita a chave privada (d,n) e lê a mensagem encriptada de `mensagem_encriptada.txt`. A mensagem desencriptada será salva em `mensagem_desencriptada.txt` e exibida no console.
    *   `4 - Sair`: Encerra o programa.

## Estrutura do Código (`CódigoFinal.py`)

O código é dividido em seções lógicas para facilitar a compreensão:

*   **Codificar e Decodificar:** Funções para converter texto em números e vice-versa, utilizando um alfabeto personalizado que inclui letras de A-Z e espaço.
*   **Funções Matemáticas:** Implementações de `modular` (exponenciação modular), `inverso_modular` (cálculo do inverso modular para a chave privada) e `primo` (verificação de números primos).
*   **Geração de Chaves:** Função `gerar_chave_publica()` que orquestra a entrada dos números primos, validações e o cálculo e salvamento das chaves.
*   **Encriptação:** Função `encriptar()` que lida com a entrada da mensagem e da chave pública, codifica a mensagem e aplica a exponenciação modular para encriptar.
*   **Desencriptação:** Função `desencriptar()` que lê a chave privada e a mensagem encriptada, aplica a exponenciação modular inversa e decodifica a mensagem.
*   **Menu Principal:** A função `menu()` que fornece a interface de linha de comando para interagir com as funcionalidades do programa.

## Observações Importantes

*   O algoritmo RSA é sensível à escolha dos números primos. Para fins de demonstração, o programa permite a entrada manual.
*   A segurança da criptografia RSA depende do tamanho dos números primos escolhidos. Para aplicações reais, números muito maiores seriam necessários.
*   O mapeamento de caracteres para números (`A`=2, `B`=3, ..., `Z`=27, `espaço`=28) é uma simplificação para este exemplo.

## Contribuições

Contribuições são bem-vindas! Sinta-se à vontade para abrir issues ou pull requests para melhorias, correções de bugs ou novas funcionalidades.

