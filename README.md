# 🔐 RSA-Criptografia — Do Zero

**Implementação manual do algoritmo RSA em Python puro: geração de chaves, encriptação e desencriptação via linha de comando**

[![Python](https://img.shields.io/badge/Python-3.x-3776AB?style=flat&logo=python&logoColor=white)](https://python.org)
[![Dependências](https://img.shields.io/badge/dependências-nenhuma-brightgreen?style=flat)](.)
[![Matemática Discreta](https://img.shields.io/badge/disciplina-Matemática%20Discreta-blueviolet?style=flat)](.)
[![Licença](https://img.shields.io/badge/licença-MIT-green?style=flat)](LICENSE)

> Toda conexão HTTPS depende de criptografia assimétrica — mas a maioria dos tutoriais ensina apenas a importar uma biblioteca.
> Este projeto prova o domínio da matemática por baixo: implementa RSA 100% do zero, sem `cryptography`, sem `rsa`, sem atalhos.

---

## Índice

1. [Funcionalidades](#funcionalidades)
2. [Matemática por baixo](#matemática-por-baixo)
3. [Requisitos](#requisitos)
4. [Instalação e execução](#instalação-e-execução)
5. [Como usar](#como-usar)
6. [Arquitetura](#arquitetura)
7. [Contexto acadêmico](#contexto-acadêmico)
8. [Limitações conhecidas](#limitações-conhecidas)
9. [Licença](#licença)

---

## Funcionalidades

Extraídas diretamente do código-fonte:

- **Geração de chaves** — o usuário fornece dois primos `p` e `q` e o expoente `e`; o sistema valida a primalidade, calcula `n`, `φ(n)` e encontra `d` automaticamente. As chaves são salvas em arquivos `.txt`.
- **Encriptação** — lê a mensagem (letras A-Z e espaços) e a chave pública `(e, n)`, aplica `c = m^e mod n` para cada caractere e salva em `mensagem_encriptada.txt`.
- **Desencriptação** — lê a chave privada `(d, n)` e o arquivo encriptado, aplica `m = c^d mod n` e reconstrói o texto original em `mensagem_desencriptada.txt`.
- **Codificação alfabética própria** — o sistema mapeia A=2, B=3 … Z=27, espaço=28 (evita `m=0` e `m=1`, que produzem cifras triviais).
- **Menu interativo** com 4 opções: gerar chaves, encriptar, desencriptar, sair.

---

## Matemática por baixo

O diagrama abaixo resume o fluxo de geração de chaves e troca de mensagens:

![Arquitetura RSA](RSA-Arch.png)

### 1. Teste de primalidade — `primo(n)`

Verifica se `n` é primo por divisão tentativa até `√n`. Simples e correto para os primos pequenos usados nesta implementação educacional.

```python
def primo(n):
    for i in range(2, int(n**0.5) + 1):
        if n % i == 0:
            return False
    return True
```

### 2. Inverso multiplicativo modular — `inverso_modular(e, phi)`

Encontra `d` tal que `e * d ≡ 1 (mod φ(n))` por busca sequencial. É com esse `d` que a chave privada é gerada — ele só existe se `mdc(e, φ) = 1`, verificado com `math.gcd`.

```python
def inverso_modular(e, phi):
    for d in range(2, phi):
        if (e * d) % phi == 1:
            return d
```

### 3. Exponenciação modular rápida — `modular(base, exp, mod)`

O coração da criptografia RSA. Calcula `base^exp mod mod` usando o método de *square-and-multiply* — sem isso, elevar números grandes antes de extrair o módulo travaria qualquer computador.

```python
def modular(base, exp, mod):
    resultado = 1
    base = base % mod
    while exp > 0:
        if exp % 2 == 1:
            resultado = (resultado * base) % mod
        exp = exp >> 1
        base = (base * base) % mod
    return resultado
```

---

## Requisitos

- Python 3.x (qualquer versão moderna)
- Nenhuma dependência externa — usa apenas `math.gcd` da biblioteca padrão

---

## Instalação e execução

```bash
git clone https://github.com/Dom1ng0s/RSA-Criptografia.git
cd RSA-Criptografia
python "Código Final.py"
```

---

## Como usar

O programa funciona em três etapas sequenciais: gerar chaves → encriptar → desencriptar.

### Etapa 1 — Gerar o par de chaves

```
Escolha uma opção:
1 - Gerar chave pública e privada
2 - Encriptar
3 - Desencriptar
4 - Sair
Opção: 1

Digite um número primo p: 61
Digite um número primo q: 53
Digite o expoente e (relativamente primo a 3120): 17

✅ Chave pública salva em 'chave_publica.txt'
🔐 Chave privada salva em 'chave_privada.txt'
Chave privada: d = 2753, n = 3233
```

**Arquivos gerados:**
- `chave_publica.txt` → `17,3233`
- `chave_privada.txt` → `2753,3233`

### Etapa 2 — Encriptar uma mensagem

```
Opção: 2

Digite a mensagem (A-Z e espaços): OLA MUNDO
Digite a chave pública (formato: e,n): 17,3233

✅ Mensagem encriptada salva em 'mensagem_encriptada.txt'
```

**Conteúdo de `mensagem_encriptada.txt`:** `2024 13 2 28 2804 22 2015 394 16`

### Etapa 3 — Desencriptar

```
Opção: 3

Digite a chave privada (formato: d,n): 2753,3233

✅ Mensagem desencriptada salva em 'mensagem_desencriptada.txt'
📨 Mensagem original: OLA MUNDO
```

> **Nota:** a mensagem é convertida para maiúsculas automaticamente. Caracteres fora de A-Z e espaço são ignorados.

---

## Arquitetura

```
RSA-Criptografia/
├── Código Final.py     # código único com todo o sistema
├── RSA-Arch.png        # diagrama do fluxo matemático
├── chave_publica.txt   # gerado em tempo de execução
├── chave_privada.txt   # gerado em tempo de execução
├── mensagem_encriptada.txt    # gerado em tempo de execução
└── mensagem_desencriptada.txt # gerado em tempo de execução
```

**Funções principais:**

| Função | Responsabilidade |
|---|---|
| `primo(n)` | Teste de primalidade por divisão tentativa |
| `modular(base, exp, mod)` | Exponenciação modular rápida (*square-and-multiply*) |
| `inverso_modular(e, phi)` | Busca do inverso multiplicativo |
| `codificar(mensagem)` | Texto → lista de inteiros (A=2 … Z=27, espaço=28) |
| `decodificar(codes)` | Lista de inteiros → texto |
| `gerar_chave_publica()` | Orquestra geração e persistência das chaves |
| `encriptar()` | Aplica `m^e mod n` em cada código e salva |
| `desencriptar()` | Aplica `c^d mod n` em cada código cifrado |
| `menu()` | Loop interativo principal |

---

## Contexto Acadêmico

Trabalho final da disciplina de **Matemática Discreta** no curso de Ciência da Computação da **Universidade Federal de Alagoas (UFAL)**. Objetivo: provar domínio sobre os fundamentos matemáticos do RSA sem uso de bibliotecas de criptografia prontas.

---

## Limitações conhecidas

- **Escopo educacional** — os primos usados são pequenos (ex: p=61, q=53). Em produção, RSA usa primos de 1024+ bits. Esta implementação não é segura para uso real.
- **Inverso por busca** — `inverso_modular()` itera de 2 até `φ(n)`, o que é lento para valores grandes. O Algoritmo de Euclides Estendido seria a implementação eficiente.
- **Alfabeto restrito** — suporta apenas letras A-Z e espaços. Números, acentos e pontuação são descartados silenciosamente.
- **Bug de formato** — o arquivo `mensagem_encriptada.txt` é gravado com espaços (`" ".join(...)`) mas lido esperando vírgulas (`split(",")`). Para desencriptar, edite o arquivo substituindo espaços por vírgulas, ou reencripte após corrigir o código.
- **n deve ser > 28** — necessário porque o maior código do alfabeto é 28 (espaço). O sistema valida isso na geração de chaves.

---

## Licença

Distribuído sob a licença MIT. Veja o arquivo `LICENSE` para detalhes.

---

**Davi Domingos de Oliveira** — Estudante de CC · UFAL  
[![GitHub](https://img.shields.io/badge/GitHub-Dom1ng0s-181717?style=flat&logo=github)](https://github.com/Dom1ng0s)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-davidomingosdeoliveira-0077B5?style=flat&logo=linkedin)](https://www.linkedin.com/in/davidomingosdeoliveira/)
