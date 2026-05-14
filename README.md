# Painel CLEAR — Indicadores para Avaliação de Políticas Públicas

Painel institucional do **FGV CLEAR** com indicadores-chave de políticas públicas brasileiras: segurança, educação, saúde, assistência social, saneamento, mercado de trabalho e meio ambiente.

Acompanha o *Guia de Fontes de Dados para Avaliação de Políticas Públicas* — sétimo volume da série *Avaliação na Prática* publicada pelo CLEAR.

🔗 **Painel ao vivo:** https://caiodesouzacastro.github.io/painel-clear/

---

## Objetivo

Tornar visíveis indicadores de fontes brasileiras consolidadas — IDEB, Atlas da Violência, SINISA, PRODES, Bolsa Família, Novo CAGED, SIM — em formato simples e atualizável, conectando cada indicador ao capítulo correspondente do Guia onde a fonte é discutida em profundidade.

O recorte temático prioriza áreas em que o CLEAR possui expertise e contribuição substantiva: monitoramento, avaliação de impacto e uso de evidências em políticas sociais.

## Como funciona

O painel é uma página estática (HTML + CSS + JavaScript) servida via GitHub Pages. **Não há banco de dados, backend ou dependências externas além das fontes de tipografia** — toda a inteligência roda no navegador do usuário.

Os dados de cada indicador ficam em arquivos JSON separados na pasta `data/`. Para atualizar um indicador basta editar o arquivo correspondente — o painel é reconstruído automaticamente a cada acesso.

## Estrutura do repositório

```
painel-clear/
├── index.html              # Painel (HTML + CSS + JS embutidos)
├── data/
│   ├── manifesto.json      # Lista de indicadores e metadados globais
│   ├── seguranca.json      # Atlas da Violência (Ipea/FBSP)
│   ├── educacao.json       # IDEB (INEP)
│   ├── saude.json          # Mortalidade infantil (IBGE/SIM)
│   ├── assistencia.json    # Bolsa Família (MDS)
│   ├── saneamento.json     # SINISA/SNIS (Ministério das Cidades)
│   ├── trabalho.json       # Novo CAGED (MTE)
│   └── ambiente.json       # PRODES (INPE)
├── CONTRIBUTING.md         # Como atualizar dados
└── README.md
```

## Como atualizar dados

Ver **[CONTRIBUTING.md](CONTRIBUTING.md)** para o passo a passo. Em resumo:

1. Edite o arquivo JSON do indicador (ex.: `data/seguranca.json`)
2. Atualize `valor`, adicione o novo ponto à `serie` e ajuste `fonte.ultimaAtualizacao`
3. *Commit* e *push* — o painel é republicado automaticamente

Para **adicionar um novo indicador**: crie um novo `.json` em `data/`, adicione-o à lista em `manifesto.json`, e pronto.

## Como rodar localmente

O painel usa `fetch` para carregar os JSONs, o que não funciona com `file://`. Rode um servidor local simples:

```bash
# Python 3
python3 -m http.server 8000

# Node
npx serve

# VS Code: extensão "Live Server"
```

Acesse `http://localhost:8000`.

## Como publicar no GitHub Pages

1. *Push* deste repositório
2. Settings → Pages → Source: `main` branch / `/ (root)`
3. Aguarde 1-2 minutos — disponível em `https://<usuario>.github.io/painel-clear/`

## Indicadores cobertos

| Tema | Indicador | Fonte | Periodicidade |
|---|---|---|---|
| Segurança | Taxa de homicídios | Atlas da Violência (Ipea/FBSP) | Anual |
| Educação | IDEB | INEP/SAEB | Bienal |
| Saúde | Mortalidade infantil | IBGE / SIM-Sinasc | Anual |
| Assistência social | Famílias no Bolsa Família | MDS | Mensal |
| Saneamento | Cobertura de esgoto | SINISA/SNIS | Anual |
| Mercado de trabalho | Saldo de emprego formal | Novo CAGED | Mensal |
| Meio ambiente | Desmatamento Amazônia | PRODES/INPE | Anual |

## Decisões de escopo

- **Temas escolhidos:** áreas sociais com fontes oficiais consolidadas. Não inclui indicadores econômicos (PIB, inflação, mercado financeiro) que já são amplamente divulgados por outras instituições.
- **Profundidade:** um indicador-síntese por tema, com série histórica e recortes. Para análises detalhadas, o painel remete ao capítulo correspondente do Guia.
- **Atualização:** manual, com cadência alinhada à publicação de cada fonte.

## Licença

Conteúdo licenciado sob Creative Commons BY-NC 4.0. Código sob MIT.

---

**FGV CLEAR** · Centro de Aprendizado e Pesquisa em Avaliação e Resultados
Escola de Economia de São Paulo da Fundação Getulio Vargas (FGV EESP)
[fgvclear.org](https://fgvclear.org)
