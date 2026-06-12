# Atualizar com ajuda da IA — Prompts prontos

Este guia é o caminho **rápido** para manter o painel: em vez de editar o JSON na
mão, você cola um prompt pronto numa IA **com acesso à internet** (como o Claude),
ela busca o dado novo na fonte oficial e devolve o trecho pronto para colar no GitHub.

> **Regra de ouro — sempre conferir.** A IA pode errar ou confundir números. Ela
> ajuda a buscar e a formatar, mas **a responsabilidade pelo número é sua**: antes
> de publicar, compare o valor com a página oficial da fonte (o link está em
> `fonte.url`). Número errado num painel institucional da FGV é um problema sério.

---

## Como funciona o ciclo

1. Abra o arquivo da área no GitHub (ex.: `data/seguranca.json`) e **copie** o objeto
   do indicador que vai atualizar (ou o arquivo inteiro, se preferir).
2. Cole o prompt escolhido abaixo **+ o objeto/arquivo** na IA.
3. A IA busca o dado na fonte, mostra o valor encontrado (com período e link) e
   devolve o JSON atualizado.
4. **Confira** o número contra a fonte.
5. Cole de volta no GitHub, salve (*commit*) e confira o site numa **janela anônima**.

Faça **uma alteração por vez** e nunca mude o `id` de um indicador.

---

## Prompt 0 — Conferir o que está vencido (use a cada revisão)

> Você é assistente de manutenção do Painel CLEAR. Vou colar os arquivos `data/*.json`.
> Para cada indicador, compare `fonte.ultimaAtualizacao` e `fonte.periodicidade` com a
> data de hoje e me diga **quais indicadores provavelmente já têm dado novo disponível**
> (ou seja, passou tempo suficiente desde a última atualização para a fonte ter divulgado
> de novo). Liste em formato: Área › Indicador › última atualização › periodicidade ›
> "provavelmente vencido?" (sim/não). Não invente datas de divulgação; baseie-se só na
> periodicidade. No fim, sugira quais conferir primeiro.

*(Isto substitui qualquer "marca" no site: a verificação fica aqui, sem mudar o painel público.)*

---

## Prompt 1 — Atualizar o valor de um indicador (com busca na fonte)

> Você vai atualizar **um** indicador do Painel CLEAR. Abaixo está o objeto JSON dele.
> 1. Acesse a fonte oficial em `fonte.url` (e, se precisar, busque na web pelo nome em `fonte.nome`).
> 2. Encontre o **valor mais recente** da métrica descrita em `indicador`/`unidade`.
> 3. Me diga, **antes do JSON**: o valor encontrado, o período de referência e o link exato de onde tirou.
> 4. Atualize no objeto: `valor`, adicione `{ "ano": ANO, "valor": VALOR }` ao fim de `serie`,
>    recalcule `tendencia` comparando os dois últimos pontos da série (subiu = `"alta"`,
>    caiu = `"queda"`, praticamente igual = `"estavel"`) e ajuste `fonte.ultimaAtualizacao`
>    para o formato "Mês/Ano (dados de ...)".
> 5. Não altere `id`, `titulo` nem a estrutura. Devolva **só** o objeto do indicador completo, pronto para colar.
>
> [cole aqui o objeto do indicador]

---

## Prompt 2 — Atualizar só a série histórica

> Adicione o ponto mais recente à `serie` deste indicador do Painel CLEAR, mantendo a
> ordem cronológica e o formato `{ "ano": ..., "valor": ... }`. Busque o valor na fonte
> (`fonte.url`), me informe valor + período + link antes do JSON, recalcule `tendencia`
> pelos dois últimos pontos e atualize `fonte.ultimaAtualizacao`. Devolva só o objeto.
>
> [cole aqui o objeto do indicador]

---

## Prompt 3 — Atualizar recortes (UF, sexo, raça, idade, renda)

> Atualize os `recortes` deste indicador com os dados mais recentes da fonte. Mantenha a
> mesma estrutura de cada recorte (chaves, categorias e o campo `max`) e ajuste só os
> valores. Se uma categoria nova for necessária, siga o padrão das existentes. Busque na
> fonte, informe os valores e o link antes do JSON, e devolva só o objeto do indicador.
>
> [cole aqui o objeto do indicador]

---

## Prompt 4 — Adicionar um indicador novo

> Quero adicionar um indicador ao tema "[NOME DO TEMA]" da área "[ÁREA]" do Painel CLEAR.
> Métrica: [descreva]. Fonte: [nome + link]. Use exatamente a estrutura do "Modelo completo
> de indicador" do `MANUTENCAO.md` (campos `id`, `titulo`, `indicador`, `valor`, `unidade`,
> `destaque`, `tendencia`, `serie`, `rotuloSerieY`, `recortes`, `fonte`, `notaMetodologica`).
> Busque os números na fonte, mostre de onde tirou, e me devolva o objeto pronto + me diga
> em qual arquivo e tema colar e que a contagem do cabeçalho precisa ser ajustada.

---

## Prompt 5 — Gerar um "texto simples" do indicador (produto)

> Com base neste indicador do Painel CLEAR, escreva um parágrafo curto (3–4 frases) em
> português claro, para um público amplo: o que o indicador mede, o valor atual, como
> mudou em relação aos pontos anteriores da `serie`, e a fonte. Sem jargão, sem opinião,
> tom institucional do FGV CLEAR. Não invente números além dos que estão no objeto.
>
> [cole aqui o objeto do indicador]

---

## Exemplo real (Emprego Formal — Novo CAGED)

Objeto (resumido) colado na IA:

```json
{
  "id": "emprego-formal",
  "titulo": "Emprego Formal",
  "indicador": "Saldo de empregos formais (admissões - desligamentos)",
  "valor": 1279498,
  "unidade": "empregos formais (saldo de 2025)",
  "tendencia": "alta",
  "serie": [
    { "ano": 2023, "valor": 1480067 },
    { "ano": 2024, "valor": 1677575 },
    { "ano": 2025, "valor": 1279498 }
  ],
  "fonte": {
    "nome": "Novo CAGED — Cadastro Geral de Empregados e Desempregados",
    "periodicidade": "Mensal",
    "ultimaAtualizacao": "Janeiro/2026 (fechamento de 2025)",
    "url": "https://www.gov.br/trabalho-e-emprego/pt-br/assuntos/estatisticas-trabalho/novo-caged"
  }
}
```

Resposta esperada da IA (antes do JSON): *"Na página do Novo CAGED, o saldo acumulado
de [ano] é X (referência: [período]; link: …)."* — e depois o objeto com `valor`, `serie`,
`tendencia` e `ultimaAtualizacao` já atualizados. Você confere o número na página e cola.
