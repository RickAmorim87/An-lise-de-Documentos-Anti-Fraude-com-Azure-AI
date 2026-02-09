# 📄 Análise de Documentos Anti-Fraude com Azure AI

## 🚀 Visão Geral

Este projeto apresenta uma solução inteligente para **análise automatizada de documentos** utilizando **Azure AI** e **Azure OpenAI**, com o objetivo de detectar fraudes, validar autenticidade e aumentar a segurança no processamento de documentos empresariais.

A aplicação utiliza recursos de Inteligência Artificial para extrair informações, analisar contexto semântico e identificar padrões suspeitos, reduzindo riscos operacionais e aumentando a confiabilidade dos processos.

---

## 🧠 Arquitetura da Solução

---

## ⚙️ Tecnologias Utilizadas

- Azure AI Document Intelligence
- Azure OpenAI Service
- Processamento de Linguagem Natural (NLP)
- Python
- API REST
- Serviços Cognitivos do Azure
- JSON para estruturação de dados

---

## 📌 Funcionalidades

✅ Extração automática de dados estruturados e não estruturados  
✅ Análise semântica e contextual dos documentos  
✅ Identificação de padrões suspeitos  
✅ Validação de autenticidade documental  
✅ Automação de processos de verificação  
✅ Processamento escalável em nuvem  

---

## 📂 Estrutura do Projeto


---

## 🔧 Pré-requisitos

- Conta no Microsoft Azure
- Azure OpenAI habilitado
- Azure Document Intelligence configurado
- Python 3.9+

---

## ▶️ Como Executar o Projeto

### 1️⃣ Clonar o Repositório

```bash
git clone https://github.com/seu-usuario/analise-documentos-azure-ai
cd analise-documentos-azure-ai

2️⃣ Criar Ambiente Virtual
python -m venv venv
source venv/bin/activate

No Windows:
venv\Scripts\activate
3️⃣ Instalar Dependências
pip install -r requirements.txt
4️⃣ Configurar Variáveis de Ambiente
AZURE_DOCUMENT_ENDPOINT=seu-endpoint
AZURE_DOCUMENT_KEY=sua-chave
AZURE_OPENAI_ENDPOINT=seu-endpoint
AZURE_OPENAI_KEY=sua-chave
5️⃣ Executar a Aplicação
python src/document_analysis.py

🧪 Exemplo de Uso

A aplicação permite enviar documentos como:

RG

CNH

Contratos

Notas fiscais

Comprovantes financeiros

Após o processamento, o sistema retorna:

Dados extraídos

Índice de confiabilidade

Alertas de possíveis fraudes

📊 Benefícios da Solução

Redução de erros manuais

Aumento da segurança operacional

Automatização de validações

Escalabilidade em nuvem

Melhor tomada de decisão baseada em IA

🔐 Segurança

Este projeto segue boas práticas de segurança:

Uso de variáveis de ambiente

Proteção de credenciais

Processamento seguro em cloud

🌎 Possíveis Aplicações

Bancos e Fintechs

Seguradoras

Validação documental corporativa

Compliance e auditoria

Processos KYC (Know Your Customer)

📈 Melhorias Futuras

Interface Web

Integração com bancos de dados

Dashboard analítico

Treinamento de modelos customizados

Pipeline de automação com Azure Functions

👨‍💻 Autor

Rick Soares

Especialista em Inteligência Artificial | Cloud | Automação Inteligente
⭐ Contribuição

Sinta-se à vontade para abrir Issues ou Pull Requests.

📜 Licença

Este projeto está sob a licença MIT.

---



📦 Estrutura do Projeto

anti-fraude-azure-ai
│
├── src
│   ├── document_analysis.py
│   ├── openai_validation.py
│   └── config.py
│
├── .env.example
├── requirements.txt
└── main.py

📄 requirements.txt

python-dotenv
openai
azure-ai-formrecognizer

⚙️ config.py

Responsável por carregar variáveis do Azure.
import os
from dotenv import load_dotenv

load_dotenv()

AZURE_DOCUMENT_ENDPOINT = os.getenv("AZURE_DOCUMENT_ENDPOINT")
AZURE_DOCUMENT_KEY = os.getenv("AZURE_DOCUMENT_KEY")

AZURE_OPENAI_ENDPOINT = os.getenv("AZURE_OPENAI_ENDPOINT")
AZURE_OPENAI_KEY = os.getenv("AZURE_OPENAI_KEY")
AZURE_OPENAI_DEPLOYMENT = os.getenv("AZURE_OPENAI_DEPLOYMENT")

import os
from dotenv import load_dotenv

load_dotenv()

AZURE_DOCUMENT_ENDPOINT = os.getenv("AZURE_DOCUMENT_ENDPOINT")
AZURE_DOCUMENT_KEY = os.getenv("AZURE_DOCUMENT_KEY")

AZURE_OPENAI_ENDPOINT = os.getenv("AZURE_OPENAI_ENDPOINT")
AZURE_OPENAI_KEY = os.getenv("AZURE_OPENAI_KEY")
AZURE_OPENAI_DEPLOYMENT = os.getenv("AZURE_OPENAI_DEPLOYMENT")

📄 document_analysis.py
from azure.ai.formrecognizer import DocumentAnalysisClient
from azure.core.credentials import AzureKeyCredential
from config import AZURE_DOCUMENT_ENDPOINT, AZURE_DOCUMENT_KEY


def analyze_document(file_path):
    client = DocumentAnalysisClient(
        endpoint=AZURE_DOCUMENT_ENDPOINT,
        credential=AzureKeyCredential(AZURE_DOCUMENT_KEY)
    )

    with open(file_path, "rb") as f:
        poller = client.begin_analyze_document(
            "prebuilt-document",
            document=f
        )

    result = poller.result()

    extracted_text = []

    for page in result.pages:
        for line in page.lines:
            extracted_text.append(line.content)

    return "\n".join(extracted_text)


🤖 openai_validation.py

Responsável por validar possível fraude com IA.
from openai import AzureOpenAI
from config import (
    AZURE_OPENAI_ENDPOINT,
    AZURE_OPENAI_KEY,
    AZURE_OPENAI_DEPLOYMENT
)

client = AzureOpenAI(
    api_key=AZURE_OPENAI_KEY,
    azure_endpoint=AZURE_OPENAI_ENDPOINT,
    api_version="2024-02-01"
)


def validate_fraud(document_text):

    prompt = f"""
    Analise o texto abaixo e identifique possíveis indícios de fraude.
    Retorne nível de risco (Baixo, Médio ou Alto) e justificativa.

    Documento:
    {document_text}
    """

    response = client.chat.completions.create(
        model=AZURE_OPENAI_DEPLOYMENT,
        messages=[
            {"role": "system", "content": "Você é um especialista em análise antifraude."},
            {"role": "user", "content": prompt}
        ],
        temperature=0.2
    )

    return response.choices[0].message.content

🚀 main.py
Arquivo principal da aplicação.
from src.document_analysis import analyze_document
from src.openai_validation import validate_fraud

def main():

    file_path = "samples/documento_exemplo.pdf"

    print("📄 Extraindo dados do documento...")
    document_text = analyze_document(file_path)

    print("\n🧠 Analisando possível fraude...")
    fraud_result = validate_fraud(document_text)

    print("\n===== RESULTADO =====")
    print(fraud_result)


if __name__ == "__main__":
    main()
🔐 .env.example

AZURE_DOCUMENT_ENDPOINT=
AZURE_DOCUMENT_KEY=

AZURE_OPENAI_ENDPOINT=
AZURE_OPENAI_KEY=
AZURE_OPENAI_DEPLOYMENT=
▶️ Como Executar
1️⃣ Criar ambiente virtual
python -m venv venv
Windows:
venv\Scripts\activate
Linux/Mac:
source venv/bin/activate
2️⃣ Instalar dependências
pip install -r requirements.txt
3️⃣ Configurar .env
Copiar .env.example para .env e preencher com dados do Azure.
4️⃣ Executar
python main.py

🧪 Resultado Esperado
📄 Extraindo dados do documento...
🧠 Analisando possível fraude...

===== RESULTADO =====
Nível de risco: Médio
Justificativa: ...


