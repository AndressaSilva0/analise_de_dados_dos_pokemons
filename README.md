

# Análise de Dados de Pokémons com PokeAPI

## Sumário

1. [Resumo](#resumo)
2. [Introdução](#introdução)
3. [Instalação e Configuração do Ambiente](#instalação-e-configuração-do-ambiente)
   - [Instalação das Bibliotecas](#instalação-das-bibliotecas)
   - [Configuração do Ambiente Virtual (venv)](#configuração-do-ambiente-virtual-venv)
     - [Windows: PowerShell](#windows-powershell)
     - [Windows: CMD](#windows-cmd)
     - [Linux e macOS](#linux-e-macos)
4. [Configuração da API do Gemini](#configuração-da-api-do-gemini)
5. [Coleta de Dados da PokeAPI](#coleta-de-dados-da-pokeapi)
6. [Extração dos Dados](#extração-dos-dados)
7. [Análise dos Dados](#análise-dos-dados)
   - [Relação entre Altura e Peso dos Pokémons](#relação-entre-altura-e-peso-dos-pokémons)
   - [Quantidade de Pokémons por Tipo](#quantidade-de-pokémons-por-tipo)
   - [Base de Experiência Média por Tipo](#base-de-experiência-média-por-tipo)
   - [Distribuição de Altura e Peso dos Pokémons](#distribuição-de-altura-e-peso-dos-pokémons)
   - [Pokémons Raros](#pokémons-raros)
8. [Análise com o Gemini](#análise-com-o-gemini)
9. [Conclusão](#conclusão)

---

## Resumo

Este projeto tem como objetivo realizar uma análise detalhada dos dados de Pokémons fornecidos pela PokeAPI. A análise incluirá uma visão geral das características físicas dos Pokémons, como altura e peso, e suas distribuições, bem como uma análise mais aprofundada sobre a experiência base média por tipo e a raridade dos Pokémons. O projeto também utilizará a API do Gemini para gerar insights adicionais com base nos dados coletados.

---
## Introdução

Imagine uma API onde criaturas mágicas não apenas cativam nossas imaginações em jogos e animações, mas também nos ajude a criar projetos impressionantes com os seus dados. Neste projeto, mergulhei no mundo dos Pokémons utilizando a PokeAPI, uma rica fonte de informações sobre esses personagens. Meu objetivo? Revelar as histórias ocultas dentro dos dados de 50 Pokémons, explorando aspectos como altura, peso, tipo e base de experiência.

---

## Instalação e Configuração do Ambiente

### Instalação das Bibliotecas

Antes de iniciar a análise, é necessário instalar as bibliotecas que serão utilizadas para a coleta, manipulação e visualização dos dados. Essas bibliotecas estão listadas em um arquivo `requirements.txt` para facilitar a instalação.

### Configuração do Ambiente Virtual (venv)

Recomenda-se a criação de um ambiente virtual para isolar as dependências do projeto. Abaixo estão as instruções para criar e ativar o ambiente virtual, bem como instalar as dependências.

---

Obs: Se você preferir somente ativar o ambiente virtual desse projeto, você utiliza somente o código de ativação, mas caso queira criar um ambiente virtual do zero siga as instruções abaixo

---

#### Windows: PowerShell

1. **Criar o ambiente virtual**:
    ```powershell
    python -m venv venv
    ```

2. **Ativar o ambiente virtual**:
    ```powershell
    .\venv\Scripts\Activate.ps1
    ```

3. **Instalar as dependências**:
    ```powershell
    pip install -r requirements.txt
    ```

#### Windows: CMD

1. **Criar o ambiente virtual**:
    ```cmd
    python -m venv venv
    ```

2. **Ativar o ambiente virtual**:
    ```cmd
    .\venv\Scripts\activate.bat
    ```

3. **Instalar as dependências**:
    ```cmd
    pip install -r requirements.txt
    ```

#### Linux e macOS

1. **Criar o ambiente virtual**:
    ```bash
    python3 -m venv venv
    ```

2. **Ativar o ambiente virtual**:
    ```bash
    source venv/bin/activate
    ```

3. **Instalar as dependências**:
    ```bash
    pip install -r requirements.txt
    ```

---

## Configuração da API do Gemini

Antes de utilizar o modelo de IA generativa do Gemini, é necessário configurar a API e garantir que todas as permissões e variáveis de ambiente estejam devidamente configuradas.

---

## Configuração da API do Gemini

Antes de utilizar o modelo de IA generativa do Gemini, é necessário realizar a configuração adequada da API, garantindo que todas as permissões e variáveis de ambiente estejam devidamente configuradas. Para isso, siga o passo a passo abaixo.


### Passo 1: Criar uma chave para a API do Gemini

1. **Acesse a página de API do Gemini**: Para gerar sua chave de API, acesse o [Google AI Studio](https://aistudio.google.com/app/?pli=1).

2. **Gerar a chave**: Após fazer o login, localize a opção para **Gerar chave de API** e siga as instruções para criar sua chave exclusiva. E use com segurança, pois será necessária para acessar a API.

![api_key](https://github.com/user-attachments/assets/2916d6c0-e17c-4bae-859a-2c689b8c22d9)

### Passo 2: Criar o arquivo `.env` para armazenar a chave

Para garantir a segurança do seu código e evitar expor a chave da API diretamente no código-fonte, você deve armazená-la em um arquivo `.env` (arquivo de variáveis de ambiente).

1. **Criar o arquivo `.env`**:
   - Na pasta raiz do seu projeto, crie um arquivo chamado `.env`.

2. **Adicionar a chave da API no `.env`**:
   - No arquivo `.env`, insira sua chave no formato abaixo:
   
   ```bash
   GEMINI_API_KEY=sua_chave_aqui
   ```

### Passo 3: Instalar a biblioteca `python-dotenv`

Para que seu código Python consiga ler as variáveis de ambiente definidas no arquivo `.env`, é necessário instalar a biblioteca `python-dotenv`.

1. **Instale o `python-dotenv`** executando o seguinte comando no terminal:

   ```bash
   pip install python-dotenv
   ```

### Passo 4: Utilizar a chave da API no código

Com o arquivo `.env` configurado, você pode agora acessar a chave da API no seu código Python. Para isso, siga as instruções abaixo:

1. **Importar e carregar a variável de ambiente**:

   No início do seu script Python, adicione o seguinte código para carregar as variáveis do `.env`:

   ```python
   import os
   from dotenv import load_dotenv

   # Carrega as variáveis do .env para o seu código principal
   load_dotenv()

   # Obtem a chave da API do Gemini
   gemini_api_key = os.getenv('GEMINI_API_KEY')

   # Para testar se a sua chave foi configurada corretamente
   print(f'Sua chave da API: {gemini_api_key}')
   ```


---

## Coleta de Dados da PokeAPI

Os dados dos Pokémons serão coletados diretamente da PokeAPI. Abaixo estão alguns exemplos de dados que serão coletados para análise:

| Nome         | Altura (m) | Peso (kg) | Base de Experiência |
|--------------|-------------|-----------|---------------------|
| Bulbasaur    | 2.13           | 31.29        | 64                  |
| Ivysaur      | 3.05          | 58.95       | 142                 |
| Venusaur     | 6.10          | 453.50      | 236                 |
| Charmander   | 1.83           | 38.55        | 62                  |
| Charmeleon   | 3.35          | 86.17       | 142                 |

---

## Extração dos Dados

Os dados extraídos da PokeAPI serão transformados em um formato adequado para análise, como DataFrames. O processo de extração incluirá a seleção dos atributos mais relevantes, como nome, altura, peso, tipo e base de experiência.

---

## Análise dos Dados

### Relação entre Altura e Peso dos Pokémons

A relação entre altura e peso dos Pokémons será visualizada por meio de um gráfico de dispersão. Esse gráfico ajudará a entender se existe alguma correlação entre essas duas variáveis.

| Nome         | Altura (dm) | Peso (hg) |
|--------------|-------------|-----------|
| Bulbasaur    | 7           | 69        |
| Ivysaur      | 10          | 130       |
| Venusaur     | 20          | 1000      |
| Charmander   | 6           | 85        |
| Charmeleon   | 11          | 190       |

---
### Quantidade de Pokémons por Tipo

Será feita uma análise para identificar a quantidade de Pokémons existentes por tipo, representada por meio de um gráfico de barras.

| Tipo       | Quantidade |
|------------|------------|
| Grass      | 12         |
| Fire       | 10         |
| Water      | 14         |
| Electric   | 8          |
| Rock       | 6          |

### Base de Experiência Média por Tipo

Essa análise identificará a base de experiência média dos Pokémons de cada tipo, utilizando um gráfico de barras para melhor visualização.

| Tipo       | Base de Experiência Média |
|------------|---------------------------|
| Grass      | 150                       |
| Fire       | 160                       |
| Water      | 140                       |
| Electric   | 170                       |
| Rock       | 120                       |

### Distribuição de Altura e Peso dos Pokémons

A distribuição de altura e peso dos Pokémons será analisada por meio de histogramas, permitindo identificar padrões nos dados.

| Faixa de Altura (dm) | Quantidade de Pokémons |
|----------------------|------------------------|
| 0-5                  | 15                     |
| 6-10                 | 30                     |
| 11-15                | 25                     |
| 16-20                | 10                     |

| Faixa de Peso (hg) | Quantidade de Pokémons |
|--------------------|------------------------|
| 0-100              | 20                     |
| 101-200            | 30                     |
| 201-300            | 25                     |
| 301-400            | 5                      |

---
### Pokémons Raros

Será realizada uma análise para identificar Pokémons raros, considerando aqueles com **base de experiência muito alta**, acima de 200.

| Nome        | Base de Experiência |
|-------------|---------------------|
| venusaur   | 263                 |
| charizard   | 267                 |
| blastoise   | 265                 |


---

## Análise com o Gemini

Utilizei o modelo de IA generativa Gemini para criar insights adicionais a partir dos dados coletados e processados. Essa análise foi útil para identificar padrões e detelhes que não estão nos graficos anteriores

---

## Conclusão

Neste projeto, explorei uma variedade de análises sobre os Pokémons, desde características físicas como altura e peso até a base de experiência média por tipo. A utilização da API do Gemini trouxe insights valiosos que complementaram as análises tradicionais. As tabelas e gráficos gerados fornecem uma visão clara e detalhada dos dados dos Pokémons, permitindo uma compreensão mais profunda das suas características e distribuições.

