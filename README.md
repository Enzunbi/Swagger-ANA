# Swagger-ANA: Interface Hidro Webservice 🌊

Este repositório fornece uma biblioteca em Python para a automação do download e processamento de dados hidrometeorológicos através do **Hidro Webservice**, a nova API baseada em Swagger da Agência Nacional de Águas e Saneamento Básico (ANA).

O projeto transforma as requisições complexas da API em dados estruturados (Pandas DataFrames), prontos para aplicação em estudos de engenharia e recursos hídricos.

## 🏗️ Estrutura do Repositório

O projeto é modular para facilitar a manutenção e escalabilidade:

* `ANA_Swagger_Autenticacao.py`: Lógica de login e geração de tokens.
* `ANA_Swagger_Base_GET.py`: Métodos base para comunicação HTTP com o Swagger.
* `ANA_Swagger_Aplicacoes.py`: Funções de alto nível e ferramentas espaciais (Bacias/Mapas).
* `ANA_Swagger_Processamento.py`: Tratamento de dados brutos e exportação.
* `ANA_Swagger_Download.py`: Orquestração de downloads em lote por período.

###1. Autenticação e Segurança (ANA_Swagger_Autenticacao.py)

O módulo `ANA_Swagger_Autenticacao.py` é o componente responsável pela gestão de acesso e segurança na comunicação com o Hidro Webservice. Ele implementa a função `gerar_token_ana`, que automatiza o processo de login enviando as credenciais do usuário via cabeçalhos HTTP para o endpoint de OAuth da Agência Nacional de Águas. Além de extrair o `tokenautenticacao` necessário para validar todas as requisições subsequentes de consulta e download, o script realiza o tratamento da string de validade retornada pela API, convertendo-a em um objeto `datetime` nativo do Python para permitir o controle programático da expiração da sessão. Vale ressaltar que o identificador e a senha necessários para o funcionamento deste módulo devem ser solicitados oficialmente através do portal do Hidroweb.

Aqui está o conteúdo formatado para o seu README.md, seguindo exatamente o estilo de texto corrido e técnico dos tópicos anteriores:

###2. Comunicação Base com o Swagger (ANA_Swagger_Base_GET.py)

O módulo ANA_Swagger_Base_GET.py funciona como o motor de requisições do projeto, encapsulando a complexidade das chamadas HTTP na classe Base_API. Ele padroniza a comunicação com os diversos endpoints da ANA, gerenciando automaticamente os cabeçalhos de autenticação Bearer e os parâmetros de consulta necessários para cada operação. Uma característica fundamental deste módulo é a implementação de validações rigorosas antes do envio dos dados: o código verifica se os intervalos de datas respeitam o limite de 366 dias para séries históricas e 30 dias para dados telemétricos, além de validar o formato das strings e os tipos de filtros permitidos, como DATA_LEITURA ou DATA_ULTIMA_ATUALIZACAO. O módulo também trata respostas de erro específicas, como o status 401 para tokens expirados, e converte os retornos da API diretamente em dicionários JSON, servindo de base para o processamento de dados subsequente.

Abaixo estão listados todos os endpoints da ANA já implementados através deste módulo:
* HidroSerieChuva
* HidroSerieCota
* HidroSerieVazao
* HidroSerieCurvaDescarga
* HidroSeriePerfilTransversal
* HidroSerieQA
* HidroSerieResumoDescarga
* HidroSerieSedimentos
* HidroSerieDados:
* HidroInventarioEstacoes
* HidroinfoanaSerieTelemetricaDetalhada
* HidroinfoanaSerieTelemetricaAdotada

---

## 🚀 Como Usar

### Pré-requisitos
```bash
pip install requests pandas geopandas matplotlib contextily
