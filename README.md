# Skoob Bookshelf Exporter

Exporta todos os livros da sua estante do Skoob para uma planilha Excel (`.xlsx`), com as colunas: **título, autor, editora, nota, % lido e URL da capa**.

> Criado pela comunidade como alternativa após a descontinuação da API oficial do Skoob.

---

## Pré-requisitos

- Python 3.8 ou superior → https://www.python.org/downloads/

---

## Instalação

**1. Instale as dependências:**
```bash
pip install -r requirements.txt
python -m playwright install chromium
```

---

## Como usar

### Passo 1 — Exporte os cookies do Skoob

O script precisa dos seus cookies de sessão para acessar sua estante. Nenhuma senha é armazenada.

1. Instale uma extensão de exportação de cookies no seu navegador — qualquer uma das opções abaixo funciona:
   - **EditThisCookie**: https://www.editthiscookie.com
   - **Cookie-Editor**: https://cookie-editor.com
2. Faça login no Skoob normalmente
3. Navegue até a sua estante
4. Abra a extensão → clique em **Export** (EditThisCookie) ou **Export → Export as JSON** (Cookie-Editor)
5. Cole o conteúdo em um arquivo chamado `skoob_cookies.json` na mesma pasta do script

### Passo 2 — Configure o seu perfil

Abra o arquivo `skoob_scraper.py` e altere a URL na linha 8 para a URL da **sua** estante:

```python
BASE_URL = "https://www.skoob.com.br/pt/user/SEU_ID_AQUI/bookshelf"
```

Para encontrar sua URL: acesse o Skoob, vá em **Minha Estante** e copie o endereço da barra do navegador.

### Passo 3 — Execute

```bash
python skoob_scraper.py
```

O script vai percorrer todas as páginas da sua estante automaticamente e salvar o arquivo `skoob_books.xlsx` na mesma pasta.

**Exemplo de saída no terminal:**
```
Loaded 11 cookies
Scraping page 1: .../bookshelf
  → Found 30 books
Scraping page 2: .../bookshelf?page=2
  → Found 30 books
Scraping page 3: .../bookshelf?page=3
  → Found 17 books
Last page reached (got 17 < 30). Done.

Total books collected: 77
Saved to skoob_books.xlsx
```

---

## Resultado

A planilha gerada contém as seguintes colunas:

| # | Título | Autor | Editora | Nota | % Lido | URL da Capa |
|---|--------|-------|---------|------|--------|-------------|

---

## Problemas comuns

**"Found 0 books" / planilha vazia**
Os cookies expiraram. Repita o Passo 1 para exportar novos cookies.

**Timeout ao carregar a página**
Sua conexão pode estar lenta. Tente executar novamente — o script já faz retry automático em caso de falha.

**Número de livros menor do que o esperado**
O script para ao encontrar uma página com menos de 30 livros. Se o total da sua estante for múltiplo exato de 30, uma página extra vazia pode ser requisitada — isso é normal e não afeta o resultado.

---

## Observações

- Os cookies de sessão ficam **apenas no seu computador** e são usados somente para autenticar a requisição ao Skoob, exatamente como seu navegador faria.
- Os cookies expiram após algumas semanas. Quando isso acontecer, basta exportar novamente.
- Este projeto não é afiliado ao Skoob.
