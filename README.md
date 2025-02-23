# image-similarity-recommendations

Este projeto implementa um sistema de recomendação por similaridade visual utilizando técnicas de Deep Learning. A proposta é classificar imagens com base na aparência (cor, formato, textura, etc.) e indicar produtos similares para o usuário.

## Visão Geral

O sistema é dividido em duas partes principais:

1. **Treinamento e Construção do Índice**  
   - **Script:** `train_index.py`  
   - **Funcionalidades:**  
     - Organização dos dados: as imagens são organizadas em pastas de acordo com a categoria (utilizando informações do arquivo `styles.csv`).  
     - Treinamento do modelo: utiliza uma rede pré-treinada do TensorFlow Hub para extrair características (features) das imagens.  
     - Extração de vetores: os vetores de características são extraídos e salvos para cada imagem.  
     - Construção do índice: é utilizado o Annoy para construir um índice aproximado, facilitando a busca por similaridade.

2. **API de Recomendação**  
   - **Script:** `flask_app.py`  
   - **Funcionalidades:**  
     - Cria uma API usando Flask que recebe uma imagem enviada pelo usuário.  
     - Processa a imagem para extrair seu vetor de características.  
     - Busca no índice as imagens mais similares e retorna as recomendações, com as imagens codificadas em base64 e legendas informativas extraídas dos metadados.

## Materiais e Tecnologias Utilizadas

- **Dataset:** [fashion-product-images-small (Kaggle)](https://www.kaggle.com/paramaggarwal/fashion-product-images-small)
- **Metadados:** `styles.csv`
- **Deep Learning:** TensorFlow, TensorFlow Hub
- **Indexação:** Annoy
- **API Web:** Flask
- **Outras Bibliotecas:** NumPy, Pandas, Pillow, Matplotlib, entre outras.

## Pré-requisitos

- Python 3.x
- TensorFlow 2.x
- TensorFlow Hub
- Annoy
- Flask
- Pandas
- NumPy
- Pillow
- Matplotlib

*(Confira o arquivo `requirements.txt` para a lista completa de dependências.)*

## Instalação

1. **Clone o repositório:**

   ```bash
   git clone <URL-do-repositório>
   cd <nome-do-repositório>
   ```

2. **Instale as dependências:**

   ```bash
   pip install -r requirements.txt
   ```

## Uso

### 1. Treinamento e Construção do Índice

Execute o script `train_index.py` para:

- Organizar as imagens em pastas por categoria.
- Treinar o modelo e extrair os vetores de características.
- Construir e salvar o índice Annoy, além dos mapeamentos (nomes dos arquivos, vetores e IDs de produtos).

```bash
python train_index.py
```

> **Observação:** Os modelos e mapeamentos serão salvos no diretório definido (por exemplo, `/content/drive/MyDrive/ImgSim`). Certifique-se de ajustar os caminhos conforme o seu ambiente.

### 2. Iniciando a API de Recomendação

Após a execução do treinamento, inicie a API com:

```bash
python flask_app.py
```

A API estará disponível, por padrão, em `http://localhost:5000`. Para realizar uma recomendação, envie uma requisição POST para o endpoint `/fashion` contendo uma imagem.

### 3. Testando a API

Você pode testar a API usando ferramentas como o Postman ou um script em Python. Exemplo:

```python
import requests

url = 'http://localhost:5000/fashion'
files = {'image': open('caminho/para/sua/imagem.jpg', 'rb')}
response = requests.post(url, files=files)
print(response.json())
```

A resposta será um JSON contendo as imagens recomendadas (codificadas em base64) e suas respectivas legendas.

## Estrutura do Projeto

```
├── train_index.py         # Script para treinamento, extração de features e construção do índice Annoy
├── flask_app.py           # API Flask para recomendação por similaridade de imagens
├── requirements.txt       # Lista de dependências do projeto
├── README.md              # Este arquivo
└── (outros arquivos e pastas conforme necessário)
```

## Considerações Finais

- **Caminhos:** Ajuste os caminhos dos diretórios e arquivos (como `styles.csv` e os diretórios de imagens) conforme o seu ambiente (local, Colab, servidor, etc.).
- **Modelo:** O sistema utiliza um modelo pré-treinado disponível no TensorFlow Hub, o que facilita a extração de features sem necessidade de treinamento extenso.
- **Indexação:** A biblioteca Annoy é empregada para permitir buscas rápidas e escaláveis por similaridade, mesmo com um grande número de imagens.
