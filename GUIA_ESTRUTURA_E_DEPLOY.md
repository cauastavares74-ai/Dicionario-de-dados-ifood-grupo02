# Estrutura, validação e publicação — Dicionário de Dados iFood

## Estrutura exata do projeto

```text
dicionario-ifood-grupo2/
├── index.html
├── style.css
├── script.js
├── docs/
│   ├── Especificacao_Requisitos_Regras_Negocio_BD.pdf
│   ├── DDL_iFood_Grupo2_v1.sql
│   └── INVENTARIO_DDL.md
└── assets/
    └── der.png
```

O `index.html` referencia o CSS por `<link rel="stylesheet" href="style.css">` e o JavaScript por `<script src="script.js"></script>`. O PDF e a imagem DER são referenciados pelos caminhos `docs/Especificacao_Requisitos_Regras_Negocio_BD.pdf` e `assets/der.png`. O SQL original está integralmente embutido no próprio `index.html` no bloco `<pre><code class="language-sql">`, pois abrir a página por `file://` não permite depender de `fetch()` para carregar o `.sql`. A cópia em `docs/` é disponibilizada para consulta/download.

## Uso local

1. Extraia o ZIP inteiro.
2. Mantenha `index.html`, `style.css`, `script.js`, `docs/` e `assets/` no mesmo nível indicado na árvore.
3. Abra `index.html` no navegador. Bootstrap, Font Awesome, Prism.js e Panzoom são carregados por CDN; por isso, os recursos visuais dessas bibliotecas exigem conexão com a internet. O conteúdo do dicionário e o SQL ficam no próprio projeto.
4. Pesquise pelo nome da tabela ou coluna. Use os filtros de privacidade para restringir os resultados.
5. Abra o DER para ampliar/reduzir pelo scroll, arrastar a imagem e usar os botões Zoom +, Zoom - e Reset.
6. Abra o DDL para consultar o script completo, copiar ou baixar. A cópia automática depende da permissão de área de transferência do navegador; em contexto `file://`, pode ser necessário copiar manualmente.

## Publicação no GitHub Pages

1. Entre em sua conta do GitHub e crie um repositório **público**, por exemplo `dicionario-ifood-grupo2`.
2. Envie o **conteúdo** desta pasta à raiz do repositório (não crie uma pasta externa adicional com os arquivos dentro).
3. Abra o repositório e acesse **Settings > Pages**.
4. Em **Build and deployment**, escolha **Source: GitHub Actions**.
5. Na opção de workflow, selecione **Static HTML**.
6. Clique em **Configure**, confira o workflow sugerido e depois em **Commit changes**.
7. Na guia **Actions**, acompanhe o workflow. Quando terminar com sucesso, volte a **Settings > Pages** e acesse o endereço publicado.

## Atenção à diferenciação entre maiúsculas e minúsculas

O ambiente de publicação do GitHub Pages diferencia letras maiúsculas e minúsculas. Os caminhos do HTML precisam coincidir exatamente com os nomes reais. Por exemplo, `assets/der.png` não é igual a `Assets/DER.png`; `docs/Especificacao_Requisitos_Regras_Negocio_BD.pdf` não é igual a `docs/especificacao_requisitos_regras_negocio_bd.pdf`. Não renomeie arquivos sem atualizar todas as referências no HTML.

## Situação da validação

Esta entrega passou por conferências estáticas de extração, estrutura, contagem e correspondência do SQL embutido. O DDL **não foi executado** em um servidor MySQL nesta entrega; portanto, criação real das tabelas, procedures, functions, triggers, views e event, além do comportamento transacional/concorrrente, permanecem sem validação de runtime. O site deve permanecer como prévia local até a resolução das pendências de coerência listadas na seção Auditoria dos artefatos e a revisão dos artefatos de origem.

## Classificação de privacidade

As etiquetas `[DP]` e `[N/A]` são classificações contextuais preliminares para governança. Não foram identificadas no esquema colunas que correspondam às categorias legais de dados pessoais sensíveis descritas na LGPD; por isso, nenhuma coluna foi etiquetada `[DPS]`. A ausência de `[DPS]` não elimina a necessidade de controles: CPF, telefone, endereço, credenciais e registros de transação vinculáveis a uma pessoa exigem proteção apropriada. A classificação deve ser aprovada pelo responsável de dados do grupo.
