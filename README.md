# Dicionário de Dados — Sistema de Gestão de Pedidos e Entregas iFood

Projeto acadêmico do Grupo 2 — Fundação Santo André.

## Arquivos

O pacote inclui o site, os dois documentos de referência, o DDL original para consulta, o inventário extraído e um guia de estrutura/publicação.

```text
dicionario-ifood-grupo2/
├── index.html
├── style.css
├── script.js
├── docs/
│   ├── Especificacao_Requisitos_Regras_Negocio_BD.pdf
│   ├── DDL_iFood_Grupo2_v1.sql
│   └── INVENTARIO_DDL.md
├── GUIA_ESTRUTURA_E_DEPLOY.md
├── RELATORIO_VALIDACAO_E_AUDITORIA.md
└── assets/
    └── der.png
```

O SQL original está embutido integralmente em `index.html` para que o modal do DDL funcione também quando o arquivo é aberto localmente por `file://`. A cópia `.sql` em `docs/` é fornecida para download/consulta e não é carregada por `fetch()`.

## Conteúdo derivado do DDL

- 22 tabelas; 185 colunas.
- 5 views; 65 triggers; 6 procedures; 3 functions; 1 event.
- O dicionário extrai tipos físicos, nulabilidade, PK/FK, UNIQUE, DEFAULT, AUTO_INCREMENT, GENERATED e CHECKs do arquivo DDL fornecido.
- A matriz RBAC foi transcrita da seção 3.1 da especificação (páginas 20–21). O controle apresentado é por tabela, pois a fonte não define permissões por coluna.
- As classificações LGPD e linhagens são metadados interpretativos de governança; devem ser aprovadas pelo responsável de dados do grupo.

## Importante — antes do deploy público

A auditoria incorporada ao site identificou pendências de alinhamento entre o DDL, o DER e a especificação. A versão é uma prévia local e não deve ser publicada como versão final até que:

1. A coluna técnica `endereco_cliente.id_cliente_principal_ativo` seja documentada também no DER e na seção 6.1, ou o DDL seja ajustado.
2. A regra de exatamente um endereço principal ativo tenha implementação transacional completa ou delimitação explícita da responsabilidade da camada de aplicação.
3. A cobertura de auditoria de `endereco_cliente` e `solicitacao_titular` seja revisada.
4. A estratégia de provisionamento RBAC/GRANTs e os controles operacionais de retenção, backup, TLS, cifragem e retries sejam documentados.
5. O DDL seja executado e validado em MySQL 8.0.36+ em instalação limpa.

A especificação original não apresentou, na extração textual das 40 páginas, placeholders entre colchetes, instruções de template ou “Orientação:”. O histórico possui cinco entradas (0.1, 0.2, 0.3, 1.0 e 1.1). Após implementar as correções, recomenda-se registrar nova revisão (por exemplo, 1.2 — 18/11/2026), preservando o histórico existente.

## Como visualizar localmente

1. Extraia a pasta do ZIP, mantendo os nomes exatamente como estão.
2. Abra `index.html` no navegador. O dicionário, a pesquisa e os modais funcionam localmente; as bibliotecas externas dependem de conexão à internet por CDN.
3. O botão **Requisitos PDF** abre o arquivo em `docs/`.
4. O botão **Abrir DER** abre `assets/der.png` com zoom, rolagem e arraste.
5. O botão **Abrir DDL** mostra o script SQL completo embutido, com destaque de sintaxe e números de linha.

## Guia completo

Consulte `GUIA_ESTRUTURA_E_DEPLOY.md` e `RELATORIO_VALIDACAO_E_AUDITORIA.md` para a estrutura de diretórios, caminhos das tags `link` e `script`, uso local e instruções detalhadas do GitHub Pages.

## Guia completo

Consulte `GUIA_ESTRUTURA_E_DEPLOY.md` e `RELATORIO_VALIDACAO_E_AUDITORIA.md` para a estrutura de diretórios, caminhos das tags `link` e `script`, uso local e instruções detalhadas do GitHub Pages.

## Deploy futuro no GitHub Pages

1. Entre em sua conta GitHub e crie um repositório público, por exemplo `dicionario-ifood-grupo2`.
2. Envie o conteúdo da pasta para a raiz do repositório. Não envie a pasta externa que contém o projeto como uma camada extra.
3. No repositório, abra **Settings > Pages**.
4. Em **Build and deployment**, defina **Source: GitHub Actions**.
5. Selecione o workflow **Static HTML**, clique em **Configure**, depois em **Commit changes**.
6. Aguarde a execução do workflow e abra o endereço publicado indicado em **Settings > Pages**.

### Atenção à diferenciação entre maiúsculas e minúsculas

O GitHub Pages roda em ambiente Linux. `assets/der.png` é diferente de `Assets/DER.png`; `docs/Especificacao_Requisitos_Regras_Negocio_BD.pdf` deve manter exatamente a capitalização usada no HTML. Não renomeie arquivos sem atualizar os caminhos correspondentes em `index.html`.

## Limitações de validação

A contagem e os metadados foram extraídos estaticamente do DDL fornecido. Este pacote não afirma que o DDL foi executado com sucesso no MySQL. A validação real de instalação, rotinas, triggers, views, eventos e comportamento concorrente permanece pendente.
