# Todos-projetos-do-Senai
projeto colocando instrucao na agent ia:
from agno.agent import Agent
from agno.models.openai import OpenAIChat

api_key = ""

agent = Agent(
    model=OpenAIChat(
        id="meta/llama-3.1-70b-instruct",
        api_key="",
        base_url="https://integrate.api.nvidia.com/v1"
    ),instructions="""Voce e um assistente util atencioso que resposnde de forma didadica e clara sendo muito profissional .
    responda sempre de forma resumida e educada limite sua resposta no maximo em 3 linhas""",
    markdown=True
)

while True:
    pergunta = input("Digite sua pergunta: (ou 'sair para encerrar' ()")

    if pergunta.lower == "sair":
        print("saindo.....")
        break
    agent.print_response(pergunta, stream=True
    armazenando dados da ia no banco de dados local:
    from agno.agent import Agent
from agno.models.openai import OpenAIChat
from agno.db.sqlite import SqliteDb # Ajustado para o padrão comum da biblioteca

# O ideal é salvar a chave em uma variável de ambiente, evite deixar direto no código
api_key = "" 

agent = Agent(
    model=OpenAIChat(
        id="meta/llama-3.1-70b-instruct",
        api_key="",
        base_url="https://integrate.api.nvidia.com/v1"
    ),
    instructions="""Voce e um assistente util atencioso que responde de forma didatica e clara sendo muito profissional.
    responda sempre de forma resumida e educada limite sua resposta no maximo em 3 linhas""",
    # Corrigido: Geralmente usa-se 'storage' para persistência de sessão em bancos SQL
    add_history_to_context=True, # Faltava uma vírgula aqui
    num_history_runs=3,
    markdown=True
)

# Primeira interação (Inicia uma sessão automática)
agent.print_response("Eu estou trabalhando em um projeto de API em python")

# Segunda interação usando a mesma sessão (Corrigido o texto solto 'arume')
# Caso queira usar um ID manual, defina a string antes: dev_session = "sessao_dev"
agent.print_response("Qual framework eu devo usar?", session_id="dev_session")
extraindo dados da web com inteligencia artificial:
#exemplo de extração de dados de uma pagina web
import requests
from bs4 import BeautifulSoup

from agno.agent import Agent
from agno.models.openai import OpenAIChat

API_KEY =""

def etrair_texto(url:str)->str:
    headers={
        "User-Agent":"Mozzila/5.0"
    }

    response= requests.get(url,headers=headers)
    response.raise_for_status()

    soup = BeautifulSoup(response.text,"html.parser")

    for tag in soup(["script","style","nonscript"]):
        tag.decompose()
    
    texto = soup.get_text(separator=" ",strip=True)

    return texto[:2000]

url= "https://g1.globo.com/sp/sorocaba-jundiai/"

conteudo=etrair_texto(url)

agent = Agent(
    model=OpenAIChat(
        id="meta/llama-3.1-70b-instruct",
        api_key="",
        base_url="https://integrate.api.nvidia.com/v1"
    ),    
    markdown=True
)

agent.print_response(
    f"""Analise o conteudo abaixo extraido da pagina
    {conteudo}
    Tarefas:
    -Resuma o conteudo
    -Diga qual e o tema principal
    -Liste informações importantes""",
    stream=True
)
Criando assitente util:
from agno.agent import Agent
from agno.models.openai import OpenAIChat
from agno.db.sqlite import SqliteDb
API_KEY = "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"

agent = Agent(
    model=OpenAIChat(
        id="meta/llama-3.1-70b-instruct",
        api_key=API_KEY,
        base_url="https://integrate.api.nvidia.com/v1"
    ),
    instructions="""
    Você é um assistente útil e atencioso.
    Responda sempre de forma resumida e educada.
    Limite sua resposta a no máximo 3 linhas.
    """,
    db=SqliteDb(db_file="agente02.db"),
    num_history_messages=3,         # ✅ substitui as duas linhas acima
    markdown=True
)

while True:
    entrada = input("\nUsuário: ")

    if entrada.lower() == "sair":
        print("Encerrando...")
        break

    agent.print_response(
        entrada,
        session_id="dev_session",
        stream=True
    )
    armazenando dados no banco de dados:
    import sqlite3

conn = sqlite3.connect("agente82.db")
cursor = conn.cursor()

cursor.execute("SELECT name FROM sqlite_master WHERE type='table';")
print("Tabelas:")
for row in cursor.fetchall():
    print(row[0])

conn.close()
Título: Criando um Agente de Boas-Vindas
Contextualização: Você foi contratado para criar um chatbot simples que dê as boas-vindas aos usuários e se apresente. Ele deve ser capaz de responder a uma única pergunta sobre si mesmo.
from agno.agent import Agent
from agno.models.openai import OpenAIChat

API_KEY = "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"

agent = Agent(
    model=OpenAIChat(
        id="meta/llama-3.1-70b-instruct",
        api_key=API_KEY,
        base_url="https://integrate.api.nvidia.com/v1"
    ),
    instructions="""
    Você é um assistente virtual chamado Boas-Vindas Bot.
    Sempre comece a conversa se apresentando e dando boas-vindas ao usuário.
    Responda apenas UMA pergunta sobre você mesmo por conversa.
    Seja simpático, breve e objetivo — no máximo 3 linhas por resposta.
    """,
    markdown=True
)

# Boas-vindas automáticas ao iniciar
agent.print_response(
    "Apresente-se para o usuário.",
    stream=True
)

# Permite UMA pergunta do usuário
pergunta = input("\nUsuário: ")
agent.print_response(pergunta, stream=True)

print("\nObrigado por usar o Boas-Vindas Bot!")
Título: Personalizando a Persona do Chatbot
Contextualização: Modifique o chatbot interativo para que ele atue como um especialista em culinária, respondendo a perguntas sobre receitas e ingredientes de forma amigável e informativa.
Objetivo: Aplicar o conceito de instructions para moldar a persona de um agente de IA.
Requisitos: Python, Agno, API Key da NVIDIA, conhecimento do exemplo “Chatbot com Instruções de Comportamento”.
from agno.agent import Agent
from agno.models.openai import OpenAIChat

API_KEY = "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"

# Base de conhecimento predefinida
FAQ = """
PERGUNTAS E RESPOSTAS SOBRE A HISTÓRIA DO PYTHON:

P: Quem criou o Python?
R: Python foi criado por Guido van Rossum e lançado em 1991.

P: Por que se chama Python?
R: O nome foi inspirado no grupo de comédia britânico Monty Python, não na cobra.

P: Qual é a versão atual do Python?
R: A versão mais recente é o Python 3, sendo o Python 3.12 uma das mais usadas atualmente.

P: Onde o Python é usado?
R: Python é usado em ciência de dados, inteligência artificial, automação, web e muito mais.

P: Python é gratuito?
R: Sim, Python é open source e totalmente gratuito.
"""

agent = Agent(
    model=OpenAIChat(
        id="meta/llama-3.1-70b-instruct",
        api_key=API_KEY,
        base_url="https://integrate.api.nvidia.com/v1"
    ),
    instructions=f"""
    Você é um chatbot de FAQ sobre a história do Python.
    Responda APENAS com base nas informações abaixo. Não invente respostas.
    Se não souber a resposta, diga exatamente: "Não tenho essa informação no meu FAQ."
    Seja direto e use no máximo 3 linhas.

    {FAQ}
    """,
    markdown=True
)

print("🐍 FAQ Bot — História do Python! Digite 'sair' para encerrar.\n")
print("Perguntas disponíveis:")
print("  - Quem criou o Python?")
print("  - Por que se chama Python?")
print("  - Qual é a versão atual?")
print("  - Onde o Python é usado?")
print("  - Python é gratuito?\n")

while True:
    entrada = input("Usuário: ")

    if entrada.lower() == "sair":
        print("FAQ Bot: Até mais! 🐍")
        break

    agent.print_response(entrada, stream=True)
    Título: Construindo um Chatbot de FAQ
Contextualização: Crie um chatbot que responda a perguntas frequentes sobre um tópico específico (ex: história do Python, regras de um esporte). O chatbot deve ser capaz de lidar com um conjunto predefinido de perguntas e respostas, e informar quando não souber a resposta.
Objetivo: Desenvolver um chatbot funcional que gerencie um conjunto de conhecimentos e interaja com o usuário de forma controlada.
Requisitos: Python, Agno, API Key da NVIDIA, lógica condicional.
from agno.agent import Agent
from agno.models.openai import OpenAIChat

API_KEY = "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"

agent = Agent(
    model=OpenAIChat(
        id="meta/llama-3.1-70b-instruct",
        api_key=API_KEY,
        base_url="https://integrate.api.nvidia.com/v1"
    ),
    instructions="""
    Você é o Chef Bot, um especialista em culinária simpático e apaixonado por cozinha.
    Responda apenas perguntas sobre receitas, ingredientes e técnicas culinárias.
    Se perguntarem sobre outro assunto, redirecione gentilmente para o tema culinária.
    Use no máximo 4 linhas por resposta. Seja prático e direto.
    """,
    markdown=True
)

print("👨‍🍳 Chef Bot — especialista em culinária! Digite 'sair' para encerrar.\n")

while True:
    entrada = input("Usuário: ")

    if entrada.lower() == "sair":
        print("Chef Bot: Bom apetite! Até a próxima! 🍽️")
        break

    agent.print_response(entrada, stream=True)
    Ia completa:
    from pathlib import Path
import sqlite3
import pandas as pd
import yfinance as yf

from agno.agent import Agent
from agno.tools.pandas import PandasTools
from agno.models.openai import OpenAIChat
from agno.tools.sql import SQLTools
from agno.tools.yfinance import YFinanceTools

API_KEY ="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"

DB_PATH = Path(_file_).with_name("market_data.db")


def preparar_banco():
    tickers = ["NVDA", "AAPL", "MSFT"]
    linhas = []

    for ticker in tickers:
        historico = yf.Ticker(ticker).history(
            period="5d",
            interval="1d"  # corrigido
        )

        if historico.empty:
            continue

        historico = historico.reset_index()

        if "Date" in historico.columns:
            historico = historico.rename(columns={"Date": "data"})
        elif "Datetime" in historico.columns:
            historico = historico.rename(columns={"Datetime": "data"})

        historico["Ticker"] = ticker

        historico = historico.rename(
            columns={
                "Close": "fechamento",
                "Volume": "volume"
            }
        )

        historico = historico[
            ["data", "Ticker", "fechamento", "volume"]
        ]

        linhas.append(historico)

    # executa apenas após processar todos os tickers
    if linhas:
        df_historico = pd.concat(
            linhas,
            ignore_index=True
        )
    else:
        df_historico = pd.DataFrame(
            [{
                "data": pd.Timestamp.utcnow().normalize(),
                "Ticker": "NVDA",
                "fechamento": 0.0,
                "volume": 0
            }]
        )

    with sqlite3.connect(DB_PATH) as conn:
        df_historico.to_sql(
            "historico_precos",
            conn,
            if_exists="replace",
            index=False
        )


preparar_banco()

# Criando o agente

agent = Agent(
    model=OpenAIChat(
        id="meta/llama-3.3-70b-instruct",
        api_key=API_KEY,
        base_url="https://integrate.api.nvidia.com/v1"
    ),
    tools=[
        YFinanceTools(
            enable_stock_price=True,
            enable_analyst_recommendations=True,
            enable_stock_fundamentals=True
        ),
        PandasTools(),
        SQLTools(
            db_url=f"sqlite:///{DB_PATH.as_posix()}"
        ),
    ],
    description="""
    Você é um analista de investimentos que usa dados financeiros,
    SQL e análise de mercado para responder.
    """,
    instructions=[
        "use markdown nas respostas",
        "use tabelas quando possível",
        "consulte SQL para dados locais (historico_precos)"
    ],
    markdown=True
)

agent.print_response(
    """
    Use yfinance para obter o preço e recomendações da NVDA.
    Depois consulte o banco SQLite (historico_precos).
    """,
    stream=True
)
