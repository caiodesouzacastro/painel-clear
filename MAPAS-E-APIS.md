# Mapas por UF e suas fontes (coleta via API)

Este documento registra **todos os mapas coropléticos por UF** do painel, a
**métrica** de cada um e a **fonte/endpoint de API** usado para coletar o dado.
Serve para dois fins: (1) auditar de onde veio cada número e (2) reproduzir a
coleta a qualquer momento (inclusive de forma automática — ver `CALENDARIO.md`
e a seção *Automação* no fim).

> **Como o painel decide desenhar um mapa:** se uma estatística tem um recorte com
> `"id": "uf"` e **20 ou mais estados**, o painel renderiza o mapa automaticamente.
> Entradas que não são UF (ex.: `"Brasil (média)"`) ficam na lista lateral, sem cor.

Estado atual: **8 das 9 áreas têm mapa.** Só **habitação** está pendente (motivo no fim).

---

## Quadro-resumo

| Área | Estatística (id) | Métrica do mapa | Fonte | Coleta |
|---|---|---|---|---|
| Segurança | `homicidios` | Taxa de homicídios /100 mil (2023) | Atlas da Violência (Ipea/FBSP) | Manual (PDF) |
| Segurança | `feminicidios` | Homicídios de mulheres /100 mil (2023) | Atlas da Violência (Ipea/FBSP) | Manual (PDF) |
| Trabalho | `desocupacao` | Taxa de desocupação % (2025) | IBGE/PNAD Contínua — SIDRA | **API** |
| Saúde | `mortalidade-infantil` | Óbitos <1 ano /1.000 nasc. (2024) | IBGE — Projeção da População 2024 | **API** (arquivo FTP) |
| Meio ambiente | `amazonia` | Desmatamento PRODES (km²) | INPE/TerraBrasilis | Manual |
| Meio ambiente | `cerrado` | Desmatamento PRODES (km²) | INPE/TerraBrasilis | Manual |
| Educação | `ideb` | IDEB anos iniciais — rede pública (2023) | INEP (via Exame) | Manual |
| Educação | `ideb-finais` | IDEB anos finais — rede total (2023) | INEP (via Exame) | Manual |
| Educação | `ideb-medio` | IDEB ensino médio — rede total (2023) | INEP (via Exame) | Manual |
| Saneamento | `lixo` | Domicílios com coleta de lixo % (Censo 2022) | IBGE — Censo 2022/SIDRA | **API** |
| Finanças | `receita-estadual-pc` | Receita corrente estadual per capita R$ (2024) | SICONFI/Tesouro Nacional | **API** |
| Assistência | `renda-media-uf` | Rendimento médio mensal 14+ R$ (Censo 2022) | IBGE — Censo 2022/SIDRA | **API** |
| Habitação | — | *(pendente — ver abaixo)* | — | — |

"**API**" = dá para coletar 100% por requisição automática. "Manual" = a fonte
publica em PDF/portal interativo; o número é conferido e atualizado à mão (ou com
ajuda de IA — ver `PROMPTS-IA.md`).

---

## Receitas de coleta (os mapas via API)

Todos os comandos abaixo são requisições HTTP públicas, sem token.

### Trabalho — desocupação por UF (PNAD Contínua / SIDRA)
```
https://apisidra.ibge.gov.br/values/t/4562/n3/all/v/4099/p/2025
```
- Tabela 4562 (PNAD Contínua anual), variável 4099 (taxa de desocupação), nível N3 (UF).
- Trocar `p/2025` pelo ano mais recente disponível.

### Saneamento — coleta de lixo por UF (Censo 2022 / SIDRA)
```
https://apisidra.ibge.gov.br/values/t/10345/n3/all/v/1013548/p/2022/c67/2520
```
- Variável 1013548 = **percentual** de domicílios; classe `c67/2520` = "Coletado".
- Para o valor do Brasil, troque `n3/all` por `n1/all`.

### Assistência — renda média por UF (Censo 2022 / SIDRA)
```
https://apisidra.ibge.gov.br/values/t/10299/n3/all/v/13502/p/2022/c2/6794/c629/32385/c58/95253
```
- Variável 13502 = rendimento médio mensal das pessoas de 14+; classes em "Total"
  (sexo 6794, condição 32385, idade 95253).

### Finanças — receita estadual per capita (SICONFI / Tesouro)
Duas etapas. Primeiro a lista de entes (traz população por estado):
```
https://apidatalake.tesouro.gov.br/ords/siconfi/tt/entes
```
Depois, para **cada** um dos 27 governos estaduais (código IBGE de 2 dígitos:
11=RO … 35=SP … 53=DF), a Declaração de Contas Anuais:
```
https://apidatalake.tesouro.gov.br/ords/siconfi/tt/dca?an_exercicio=2024&no_anexo=DCA-Anexo%20I-C&id_ente=35
```
- Pegar a conta `RO1.0.0.0.00.0.0` ("Receitas Correntes"), coluna "Receitas Brutas
  Realizadas", e **dividir pela população** do estado (campo `populacao` em `/entes`).
- Atenção: no `/entes`, entes estaduais vêm com `uf = "BR"`; identifique o estado
  pelo **código IBGE**, não por esse campo.

### Saúde — mortalidade infantil por UF (Projeção IBGE)
- Arquivo: `https://ftp.ibge.gov.br/Projecao_da_Populacao/Projecao_da_Populacao_2024/projecoes_2024_tab4_indicadores.xlsx`
- Coluna `TMI_T` (taxa de mortalidade infantil, ambos os sexos), por UF e ano.

---

## Habitação — por que ainda não tem mapa

A habitação é a única área sem mapa, e isso é **limitação de dado, não de código**:

- O **déficit habitacional** (Fundação João Pinheiro) não tem API aberta — o dado por
  UF está num hub interativo (cbic.org.br/hubdedados) que não é coletável por requisição.
- A **condição de ocupação** (domicílios próprios) do **Censo 2022** ainda **não foi
  publicada** no SIDRA.
- Os **aglomerados subnormais** no SIDRA são só do **Censo 2010** (16 anos — velho demais
  para o painel).
- Bolsa Família e afins (que poderiam ancorar um proxy) estão no MDS/Portal da
  Transparência, que **exigem token** de acesso.

**Quando o IBGE publicar** a condição de ocupação ou os aglomerados subnormais do
Censo 2022 no SIDRA, o mapa entra no mesmo padrão dos outros: puxa-se a tabela por
UF (`n3/all`), calcula-se o `%` (numerador ÷ total de domicílios — a tabela 10345,
classe `c67/10972` "Total", dá o total por UF) e adiciona-se uma estatística nova com
recorte `uf`. O mapa aparece sozinho.

---

## Automação (relatório trimestral)

A revisão recomendada é **trimestral** (ver `CALENDARIO.md`). Dos mapas acima, os
marcados "**API**" (desocupação, lixo, renda, receita estadual) podem ser
**re-coletados automaticamente**; os "Manual" (Atlas, IDEB, PRODES) seguem
dependendo de conferência humana porque a fonte só publica em PDF/portal.

O caminho de automação é um **GitHub Action agendado** que roda um script de coleta,
atualiza os JSONs das estatísticas "API" e gera um **relatório do que mudou**. A
proposta detalhada (e a decisão entre *publicar direto* ou *abrir um Pull Request para
você revisar*) está descrita junto com a entrega — ver conversa/`CALENDARIO.md`.
