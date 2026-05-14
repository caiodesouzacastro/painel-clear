# Como atualizar os dados

O painel foi desenhado para ser atualizado **sem mexer no código**. Cada indicador vive em um arquivo JSON na pasta `data/`.

## Atualizar um indicador existente

### Exemplo: novos dados do CAGED são divulgados em fevereiro

1. Abra `data/trabalho.json`
2. Atualize o campo `valor` com o novo número
3. Adicione o ano novo ao array `serie`:
   ```json
   "serie": [
     ...,
     {"ano": 2025, "valor": 1279498},
     {"ano": 2026, "valor": 1850000}
   ]
   ```
4. Atualize `fonte.ultimaAtualizacao` (ex.: `"Fevereiro/2026"`)
5. Se necessário, atualize `destaque` e `totalAbsoluto`
6. *Commit* e *push* — pronto

### Exemplo: o recorte por estado/setor mudou

Edite o bloco `recorte.valores`:

```json
"recorte": {
  "titulo": "Saldo por setor (2025)",
  "valores": [
    {"rotulo": "Serviços", "valor": 758355, "max": 800000},
    ...
  ]
}
```

- `max` define a escala da barra (use um valor maior ou igual ao maior `valor` do conjunto).

## Adicionar um novo indicador

1. Crie um arquivo `data/<id>.json` seguindo o esquema padrão (veja qualquer um dos existentes)
2. Adicione o nome do arquivo à lista em `data/manifesto.json`:
   ```json
   "indicadores": [
     ...,
     "novo-indicador.json"
   ]
   ```
3. Atualize `meta.atualizado` no manifesto

## Remover um indicador

Basta retirar o nome do arquivo da lista em `manifesto.json`. O JSON pode ser mantido no repositório para histórico.

## Esquema JSON de um indicador

```json
{
  "id": "identificador-curto",
  "tema": "Nome do Tema",
  "ordem": 1,
  "titulo": "Título do indicador",
  "indicador": "Descrição do que está sendo medido",
  "valor": 12.4,
  "unidade": "unidade do valor (por 100 mil hab., %, etc.)",
  "destaque": "Frase curta em itálico sobre o valor",
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
    "produtor": "Órgão/Ministério",
    "periodicidade": "Anual / Mensal / Bienal",
    "ultimaAtualizacao": "Mês/Ano (dados de referência: ano)",
    "url": "https://..."
  },
  "notaMetodologica": "Como o indicador é calculado e suas limitações.",
  "capituloGuia": "Capítulo X — Tema",
  "fontesRelacionadas": [
    "Outra fonte 1",
    "Outra fonte 2"
  ]
}
```

### Campos obrigatórios
`id`, `tema`, `titulo`, `indicador`, `valor`, `unidade`, `destaque`, `fonte`, `notaMetodologica`

### Campos opcionais
`recorte`, `totalAbsoluto`, `formatoValor`, `capituloGuia`, `fontesRelacionadas`, `ordem`, `tendencia`, `rotuloSerieY`

### Sobre `formatoValor`

- `"inteiro"` para números grandes (ex.: 1.279.498) — formata sem casas decimais
- Omita para taxas e percentuais — formata com uma casa decimal

## Validação rápida

Antes de fazer *commit*, valide o JSON:

```bash
# valida sintaxe de todos os arquivos
for f in data/*.json; do python3 -m json.tool "$f" > /dev/null && echo "OK: $f" || echo "ERRO: $f"; done
```

Ou cole o conteúdo em https://jsonlint.com.

## Calendário recomendado de atualizações

| Fonte | Frequência | Mês típico de divulgação |
|---|---|---|
| Atlas da Violência | Anual | Maio/Junho |
| IDEB | Bienal | Agosto (anos ímpares) |
| Mortalidade infantil (IBGE) | Anual | Agosto |
| Bolsa Família (MDS) | Mensal | Todo mês |
| SINISA | Anual | Março |
| Novo CAGED | Mensal | ~25 dias após o mês de referência |
| PRODES | Anual | Outubro/Novembro |

---

Em caso de dúvidas, abra uma *issue* no repositório ou contate a equipe do CLEAR.
