# Calendário de Atualização — Painel CLEAR

Este documento define **com que frequência** o painel precisa ser revisado e
**quais fontes** costumam divulgar dados novos em cada época do ano.

> Os meses são **indicativos**, baseados na última divulgação observada de cada
> fonte. Calendários oficiais mudam de ano para ano — sempre reconfirme na página
> da fonte (o link está no campo `fonte.url` de cada estatística no `.json`).

---

## Ritmo recomendado: revisão trimestral

A grande maioria das estatísticas é **anual** e cai em meses previsíveis. Por isso,
**quatro revisões por ano** cobrem o painel inteiro sem esforço desnecessário:

- **Janeiro · Abril · Julho · Outubro** (primeira semana de cada um).

Como o painel é de **monitoramento** (e não de tempo real), as séries mensais e
trimestrais (Bolsa Família, CAGED, PNAD trimestral) podem ser atualizadas **junto**
na revisão trimestral. Só vale um toque mensal extra se você quiser essas poucas
séries sempre no mês corrente.

---

## O que conferir em cada trimestre

### 1º trimestre — janeiro a março
| Estatística / fonte | Mês típico |
|---|---|
| Finanças municipais — FINBRA/Siconfi e Anuário Multi Cidades (FNP) | janeiro |
| Minha Casa Minha Vida — Ministério das Cidades | janeiro |
| Saneamento — SNIS/SINISA | março |
| PNAD Contínua trimestral (4º tri + média anual): desocupação, subutilização, rendimento, informalidade | fevereiro |

### 2º trimestre — abril a junho
| Estatística / fonte | Mês típico |
|---|---|
| Homicídios — Atlas da Violência (Ipea/FBSP) | maio |
| Déficit Habitacional — Fundação João Pinheiro | abril |
| Educação — PNAD (conclusão do EM, analfabetismo) | junho |
| PNAD Contínua trimestral (1º tri) | — |

### 3º trimestre — julho a setembro
| Estatística / fonte | Mês típico |
|---|---|
| Feminicídios e Anuário Brasileiro de Segurança Pública (FBSP) | julho |
| Saúde — IBGE projeções / SIM / Sinasc | agosto |
| PNAD domicílios: coleta de lixo, energia elétrica, moradia | setembro |
| IDEB / SAEB (INEP) — **só em ano de divulgação** (bienal) | — |
| PNAD Contínua trimestral (2º tri) | — |

### 4º trimestre — outubro a dezembro
| Estatística / fonte | Mês típico |
|---|---|
| Desmatamento — INPE PRODES (Amazônia e Cerrado) | outubro/novembro |
| Emissões de GEE — SEEG | nov/dez |
| Resíduos sólidos — ABRELPE | — |
| Sistema prisional — SISDEPEN (semestral) | dezembro |
| PNAD Contínua trimestral (3º tri) | — |

### Conferir em **toda** revisão (séries mensais/trimestrais)
- Bolsa Família, BPC, CadÚnico — MDS (mensal)
- Emprego formal — Novo CAGED (mensal)
- Desocupação, subutilização, rendimento, informalidade — PNAD Contínua (trimestral)

---

## Checklist de uma revisão trimestral

1. Abra o painel numa **janela anônima** e veja a data de "Última atualização" no topo.
2. Rode o prompt **"Conferir o que está vencido"** (ver `PROMPTS-IA.md`) para listar as estatísticas cuja `ultimaAtualizacao` já passou da periodicidade esperada.
3. Para cada estatística atrasada: confirme na página oficial se saiu dado novo.
4. Se saiu: atualize `valor`, `serie`, `tendencia` e `fonte.ultimaAtualizacao` (use os prompts ou o passo a passo manual do `MANUTENCAO.md`).
5. Atualize, se for o caso, a "Última atualização" do cabeçalho e a contagem de estatísticas.
6. Publique e **confira em janela anônima** (por causa do cache do GitHub Pages).

---

## Referência rápida: periodicidade por estatística

| Área | Estatística | Fonte | Periodicidade |
|---|---|---|---|
| Segurança | Homicídios; Homicídios ocultos | Atlas da Violência (Ipea/FBSP) | Anual (mai) |
| Segurança | Feminicídios | Anuário Bras. de Seg. Pública (FBSP) | Anual (jul) |
| Segurança | População carcerária; Déficit prisional | SISDEPEN | Semestral (jun/dez) |
| Saúde | Mortalidade infantil; materna; pré-natal | IBGE / DATASUS (SIM, Sinasc) | Anual |
| Saúde | Cobertura vacinal; BCG; ESF | SI-PNI / e-Gestor APS | Mensal (consolidado anual) |
| Educação | Qualidade (SAEB); IDEB anos finais e EM | INEP | Bienal |
| Educação | Conclusão EM; analfabetismo | PNAD Contínua (IBGE) | Anual (jun) |
| Educação | Abandono; distorção idade-série | INEP — Censo Escolar | Anual |
| Trabalho | Emprego formal | Novo CAGED | Mensal |
| Trabalho | Desocupação; subutilização; rendimento; informalidade | PNAD Contínua | Trimestral (fev/mai/ago/nov) |
| Assistência | Bolsa Família; BPC; CadÚnico | MDS | Mensal |
| Saneamento | Saneamento básico | SNIS/SINISA | Anual (mar) |
| Saneamento | Coleta de lixo; energia elétrica | PNAD Contínua | Anual (set) |
| Saneamento | Resíduos sólidos | ABRELPE | Anual |
| Habitação | Déficit habitacional | Fundação João Pinheiro | Anual (abr) |
| Habitação | Minha Casa Minha Vida | Ministério das Cidades | Mensal/anual (jan) |
| Habitação | Casa própria; aluguel | PNAD Contínua | Anual (set) |
| Finanças | FPM | Tesouro Nacional | Decendial (anual em jan) |
| Finanças | IPTU; ISS | FNP / FINBRA-Siconfi | Anual (jan) |
| Finanças | Dependência de transferências | FINBRA / Siconfi | Anual |
| Finanças | Despesa com pessoal | RGF (LRF) | Quadrimestral (anual) |
| Meio ambiente | Desmatamento Amazônia e Cerrado | INPE PRODES | Anual (out/nov) |
| Meio ambiente | Emissões de GEE | SEEG | Anual |
| Meio ambiente | Estresse hídrico | Conjuntura dos Recursos Hídricos (ANA) | Anual |
