# Como atualizar os dados

O painel foi desenhado para ser atualizado **sem mexer no código**. Cada tema vive em um JSON na pasta `data/`, com 3 estatísticas cada.

## Estrutura de um arquivo de tema

Cada JSON em `data/` (ex.: `saude.json`) tem essa estrutura:

```json
{
  "id": "saude",
  "tema": "Saúde",
  "ordem": 3,
  "capituloGuia": "Capítulo 5 — Saúde",
  "fontesRelacionadas": ["...", "..."],
  "indicadores": [
    { "id": "...", "titulo": "...", "valor": ..., ... },
    { "id": "...", "titulo": "...", "valor": ..., ... },
    { "id": "...", "titulo": "...", "valor": ..., ... }
  ]
}
```

**Importante:** a **primeira estatística do array** é o que aparece no card da página inicial (a "estatística-destaque" do tema). Os demais aparecem como abas no modal de detalhe.

## Atualizar uma estatística existente

### Exemplo: novos dados do Atlas da Violência saem em maio

1. Abra `data/seguranca.json`
2. No `indicadores[0]` (homicídios), atualize:
   - `valor`: nova taxa
   - `serie`: adicione o novo ano
   - `fonte.ultimaAtualizacao`: novo texto
   - se necessário, `destaque` e `totalAbsoluto`
3. Commit e push

## Atualizar a estatística-destaque do tema

Se quiser que outra estatística apareça no card da home (por exemplo, durante o mês das mulheres, destacar Feminicídios em vez de Homicídios), basta **reordenar o array** `indicadores` — o primeiro item passa a ser o destaque.

## Adicionar uma 4ª estatística a um tema

Adicione um novo objeto ao array `indicadores` do JSON do tema. As abas se ajustam automaticamente. Estrutura do objeto:

```json
{
  "id": "identificador-curto",
  "titulo": "Título do indicador",
  "indicador": "Descrição do que está sendo medido",
  "valor": 12.4,
  "unidade": "unidade do valor",
  "destaque": "Frase curta em itálico",
  "totalAbsoluto": "Contexto adicional (opcional)",
  "tendencia": "queda | alta | estavel",
  "formatoValor": "inteiro",
  "serie": [
    {"ano": 2020, "valor": 14.2},
    {"ano": 2021, "valor": 13.5}
  ],
  "rotuloSerieY": "Texto do eixo Y",
  "recorte": {
    "titulo": "Título do recorte",
    "valores": [
      {"rotulo": "Categoria", "valor": 100, "max": 200}
    ]
  },
  "fonte": {
    "nome": "Nome completo da fonte",
    "produtor": "Órgão",
    "periodicidade": "Anual",
    "ultimaAtualizacao": "Mês/Ano (dados de referência: ano)",
    "url": "https://..."
  },
  "notaMetodologica": "Como o indicador é calculado e suas limitações."
}
```

### Campos opcionais
`recorte`, `totalAbsoluto`, `formatoValor`, `tendencia`, `rotuloSerieY`

### Sobre `formatoValor`
- `"inteiro"` para números grandes (ex.: 1.279.498) — sem casas decimais
- Omita para taxas e percentuais — uma casa decimal

## Adicionar um novo tema

1. Crie um arquivo `data/<id>.json` no mesmo formato dos existentes
2. Adicione o nome do arquivo à lista `temas` em `data/manifesto.json`:
   ```json
   "temas": [
     ...,
     "novo-tema.json"
   ]
   ```
3. Atualize `meta.atualizado` no manifesto

## Validação antes do commit

```bash
for f in data/*.json; do python -m json.tool "$f" > /dev/null && echo "OK: $f" || echo "ERRO: $f"; done
```

Ou cole o conteúdo em https://jsonlint.com.

## Calendário recomendado de atualizações

| Fonte | Frequência | Mês típico |
|---|---|---|
| Atlas da Violência (homicídios, MVCI) | Anual | Maio/Junho |
| Anuário FBSP (feminicídios) | Anual | Julho |
| IDEB | Bienal | Agosto (ímpares) |
| PNAD Educação (analfabetismo) | Anual | Junho |
| Mortalidade infantil/materna (IBGE/MS) | Anual | Agosto |
| Cobertura vacinal (SI-PNI) | Mensal | Todo mês |
| Bolsa Família (MDS) | Mensal | Todo mês |
| BPC | Mensal | Todo mês |
| CadÚnico | Mensal | Todo mês |
| SINISA (saneamento) | Anual | Março |
| PNAD Domicílios (lixo, energia) | Anual | Setembro |
| Novo CAGED | Mensal | ~25 dias após mês ref. |
| PNAD Trabalho | Trimestral/Anual | Fev (anual) |
| PRODES (Amazônia, Cerrado) | Anual | Outubro/Novembro |
| SEEG (emissões) | Anual | Novembro |

---

Em caso de dúvidas, abra uma issue no repositório ou contate a equipe do CLEAR.
