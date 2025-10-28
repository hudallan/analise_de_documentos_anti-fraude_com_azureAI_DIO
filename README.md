# Verificador de Identidade Anti-Fraude com Azure AI

Este projeto implementa uma aplicação web interativa para a verificação de identidade e análise anti-fraude de documentos.

A aplicação utiliza um conjunto de serviços de Inteligência Artificial do Azure para processar o upload de um documento de identidade (ex: CNH, Passaporte) e uma "selfie" do usuário. O sistema então extrai os dados, valida a autenticidade (liveness) da selfie e compara o rosto da selfie com o rosto no documento, simulando um processo de "Know Your Customer" (KYC) e anti-fraude.

🎯 **Funcionalidades e Objetivo**

O objetivo é demonstrar um fluxo de verificação de identidade robusto, detectando tentativas de fraude (como usar a foto de outra pessoa ou uma foto de uma foto).

O aplicativo faz o seguinte:

  * **Processa Múltiplos Inputs:** Permite ao usuário carregar dois arquivos:
    1.  Uma imagem do documento de identidade (frente).
    2.  Uma "selfie" tirada no momento.
  * **Extração de IA (Documento):** Envia o documento para o **Azure Document Intelligence** (modelo `prebuilt-idDocument`) para extrair dados estruturados (Nome, Data de Nascimento, Nº do Documento).
  * **Extração de IA (Liveness):** Envia a selfie para o **Azure AI Vision (Face API)** para realizar uma "verificação de vivacidade" (liveness check), garantindo que o usuário é uma pessoa real e presente, e não uma foto de uma tela ou papel.
  * **Verificação de IA (Face Match):** Compara o rosto da selfie (validado como "ao vivo") com o rosto extraído da foto no documento de identidade, gerando uma pontuação de confiança na correspondência.
  * **Exibe o Resultado:** Apresenta os dados extraídos do documento e um veredito final (ex: "Verificação Aprovada" ou "Falha na Verificação Anti-Fraude"), juntamente com as pontuações de liveness e correspondência.

✨ **Tecnologias Utilizadas**

| Tecnologia | Finalidade |
| :--- | :--- |
| Python 3.x | Linguagem principal do projeto. |
| Streamlit | Framework para construção da interface web e dashboard. |
| Azure Document Intelligence | Serviço de IA para extrair dados de documentos de identidade. |
| Azure AI Vision (Face API) | Serviço de IA para verificação de liveness e correspondência facial. |
| python-dotenv | Gerenciamento seguro das chaves de acesso. |

⚙️ **Como Rodar o Projeto Localmente**

Siga estas instruções para configurar e executar a aplicação em sua máquina local.

**1. Pré-requisitos**

  * Python 3.8+ instalado.
  * Credenciais ativas para **Azure Document Intelligence** (KEY e ENDPOINT).
  * Credenciais ativas para **Azure AI Vision / Face API** (KEY e ENDPOINT). *Importante: O recurso Face API pode exigir uma solicitação de ativação especial no Azure.*

**2. Configuração e Execução**

Clone o repositório para sua máquina local e navegue até a pasta:

```bash
git clone [URL_DO_SEU_NOVO_REPOSITORIO]
cd [NOME_DA_PASTA_DO_PROJETO]
```

**\# Ambiente e Dependências**

Crie e ative um ambiente virtual (venv) e instale as bibliotecas necessárias:

```bash
# Cria o ambiente virtual
python -m venv venv

# Ativa o ambiente (Windows PowerShell)
.\venv\Scripts\activate

# Instala todas as dependências (crie um requirements.txt)
# O requirements.txt deve conter:
# streamlit
# azure-ai-documentintelligence
# azure-cognitiveservices-vision-face
# python-dotenv
pip install -r requirements.txt
```

**\# Configuração das Chaves de Acesso**

Renomeie o arquivo `env.example` para `.env`.

Preencha o arquivo `.env` com suas credenciais reais dos **dois** serviços do Azure:

```bash
# Conteúdo do seu arquivo .env

# Credenciais do Document Intelligence (para ler o RG/CNH)
DOC_INTEL_ENDPOINT="SUA_URL_DO_DOC_INTELLIGENCE"
DOC_INTEL_KEY="SUA_CHAVE_DO_DOC_INTELLIGENCE"

# Credenciais do Face API (para liveness e comparação)
FACE_API_ENDPOINT="SUA_URL_DO_SERVICO_FACE"
FACE_API_KEY="SUA_CHAVE_DO_SERVICO_FACE"
```

**\# Execução do Aplicativo**

Com o ambiente ativado e as chaves configuradas, execute o Streamlit a partir da raiz do projeto:

```bash
streamlit run src/app.py
```

O aplicativo será aberto automaticamente no seu navegador, geralmente em `http://localhost:8501`.
