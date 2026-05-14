# Painel CLEAR — Indicadores para Avaliação de Políticas Públicas

Painel institucional do **FGV CLEAR** com indicadores-chave de políticas públicas brasileiras: segurança, educação, saúde, assistência social, saneamento, mercado de trabalho e meio ambiente.

Acompanha o *Guia de Fontes de Dados para Avaliação de Políticas Públicas* — sétimo volume da série *Avaliação na Prática* publicada pelo CLEAR.

🔗 **Painel ao vivo:** https://caiodesouzacastro.github.io/painel-clear/

## Versão atual

**v2.0** — 7 temas, **21 indicadores** organizados em 3 por tema. Cada tema tem um indicador-destaque (que aparece no card da página inicial) e indicadores complementares acessíveis por abas no modal de detalhe.

## Estrutura

```
painel-clear/
├── index.html              # Painel (CSS e JS embutidos)
├── data/
│   ├── manifesto.json      # Metadados globais e lista de temas
│   ├── seguranca.json      # 3 indicadores
│   ├── educacao.json       # 3 indicadores
│   ├── saude.json          # 3 indicadores
│   ├── assistencia.json    # 3 indicadores
│   ├── saneamento.json     # 3 indicadores
│   ├── trabalho.json       # 3 indicadores
│   └── ambiente.json       # 3 indicadores
├── CONTRIBUTING.md         # Como atualizar dados
└── README.md
```

## Indicadores cobertos

### Segurança Pública (Atlas da Violência, Anuário FBSP)
- Taxa de homicídios — Atlas da Violência
- Homicídios ocultos (MVCI) — Atlas da Violência
- Feminicídios — Anuário FBSP

### Educação (INEP, IBGE)
- IDEB — INEP/SAEB
- Analfabetismo — PNAD Contínua/IBGE
- Conclusão do Ensino Médio — PNAD Contínua/IBGE

### Saúde (DATASUS, IBGE)
- Mortalidade infantil — SIM/Sinasc
- Mortalidade materna — SIM/Sinasc
- Cobertura vacinal (tríplice viral) — SI-PNI

### Assistência Social (MDS, INSS)
- Bolsa Família — folha mensal
- BPC — INSS/MDS
- CadÚnico — MDS

### Saneamento e Território (MCidades, IBGE)
- Coleta de esgoto — SINISA/SNIS
- Coleta de lixo — PNAD Contínua
- Acesso à energia — PNAD/MME

### Mercado de Trabalho (MTE, IBGE)
- Saldo de emprego formal — Novo CAGED
- Taxa de desocupação — PNAD Contínua
- Rendimento médio — PNAD Contínua

### Meio Ambiente (INPE, Observatório do Clima)
- Desmatamento Amazônia — PRODES
- Desmatamento Cerrado — PRODES
- Emissões GEE — SEEG

## Como atualizar

Ver **[CONTRIBUTING.md](CONTRIBUTING.md)** para o passo a passo. Em resumo: cada tema vive em um JSON em `data/`. Para atualizar um indicador, edite o JSON do tema correspondente, ajuste `valor`, adicione um ponto na `serie` e atualize `fonte.ultimaAtualizacao`. Commit e push — o painel é republicado automaticamente.

## Rodar localmente

```bash
python -m http.server 8000
```

Acesse `http://localhost:8000`. (Abrir o HTML direto com duplo clique não funciona porque o navegador bloqueia `fetch` em `file://`.)

## Decisões de escopo

- **Temas escolhidos**: áreas sociais com fontes oficiais consolidadas. Não inclui indicadores econômicos (PIB, inflação, mercado financeiro) que já são amplamente divulgados.
- **Profundidade**: 3 indicadores por tema. O primeiro é o destaque (aparece no card da home); os outros aparecem como abas no modal.
- **Atualização**: manual, com cadência alinhada à publicação de cada fonte. Calendário recomendado em [CONTRIBUTING.md](CONTRIBUTING.md).
- **Sobre os dados**: os números foram extraídos das fontes oficiais entre maio/2025 e fevereiro/2026. Algumas séries históricas têm valores aproximados — onde isso ocorre, a nota metodológica do indicador sinaliza.

## Licença

Conteúdo licenciado sob Creative Commons BY-NC 4.0. Código sob MIT.

---

**FGV CLEAR** · Centro de Aprendizado e Pesquisa em Avaliação e Resultados  
Escola de Economia de São Paulo da Fundação Getulio Vargas (FGV EESP)  
[fgvclear.org](https://fgvclear.org)
