# Roteiro da apresentação · Projeto 1 (Sudoeste do Paraná)

**Data:** 09/11/2026 · **Duração:** 15 a 20 min · **Slides:** 14 + reserva

As figuras são as que o notebook `codigos_python/projeto_01_sudoeste.ipynb` salva em `RFAA_P1_sudoeste/figuras/`. Os números vêm de `RFAA_P1_sudoeste/resultados/tabelas_relatorio.md` e são os mesmos do relatório (`relatorio/relatorio_p1_sudoeste.md`). Onde aparece **[valor]**, coloquem o número da tabela indicada.

**Regra para os slides:** uma ideia por slide, título que já diz o resultado (por exemplo, "O modelo BC estimou Qg com RMSE de 3,4 MJ" em vez de "Resultados de Qg") e no máximo uma tabela pequena ou uma figura.

---

## Divisão sugerida

| Bloco | Slides | Tempo | Quem apresenta |
|---|---|---|---|
| Abertura e área de estudo | 1 a 3 | 3 min | **[nome]** |
| Dados e validação | 4 a 6 | 4 min | **[nome]** |
| Radiação: Fernandes et al. | 7 a 9 | 4 min | **[nome]** |
| Saldo de radiação e ETo: Fietz & Fisch | 10 e 11 | 3 min | **[nome]** |
| Integração e conclusões | 12 a 14 | 4 min | **[nome]** |
| **Total** | | **18 min** | |

Ensaiem com cronômetro. Se passar de 20 min, cortem primeiro o slide 9 (transferência) e depois o 6 (balanço de radiação), que podem ir para a reserva.

---

## Abertura e área de estudo (3 min)

### Slide 1 · Título (20 s)
- **Na tela:** "Quanto erro carrego até a ETo se tiver só termômetro? Sudoeste do Paraná"; nomes do grupo; disciplina e data.
- **Falar:** apresentar o grupo e dizer a pergunta em uma frase.

### Slide 2 · A pergunta e por que ela importa (1 min)
- **Na tela:** a cadeia **temperatura → Qg → Rn → ETo** como diagrama, com os dois artigos sobre os elos (Fernandes et al., 2018: temperatura → Qg; Fietz & Fisch, 2009: Qg → Rn → ETo).
- **Falar:** a ETo precisa de radiação, mas a radiação é medida em poucas estações, enquanto a temperatura é medida em quase todas. Se a cadeia funcionar só com temperatura, dá para estimar a ETo em muito mais lugares. A pergunta é quanto erro chega ao fim e em que elo ele nasce.

### Slide 3 · Área de estudo (1 min 40 s)
- **Na tela:** mapa do Paraná com as três cidades (do trabalho da Aula 01) e a tabela de locais (Tabela 1): estação, coordenadas e altitude.
- **Falar:** Pato Branco, Francisco Beltrão e Dois Vizinhos; transição Cfa/Cfb; gradiente de altitude, com Pato Branco a mais alta; região de grãos, leite, aves e suínos. O gradiente permite testar se coeficientes calibrados numa cidade servem nas outras.

---

## Dados e validação (4 min)

### Slide 4 · Dados e estratégia de extração (1 min)
- **Na tela:** BR-DWGD (Xavier et al., 2022), 0,1°, diário, 2001–2025; variáveis usadas; esquema "25 anos × 3 cidades → extração ano a ano → CSV no Drive".
- **Falar:** o desafio da extração que o professor apontou: pedir tudo de uma vez estoura o limite do Earth Engine, por isso a série foi baixada ano a ano. Mencionar o fator de escala (`bruto · MULT + ADD`).

### Slide 5 · O BR-DWGD é confiável aqui? (1 min 30 s)
- **Na tela:** Figura 1 (`etapa1b_brdwgd_vs_inmet.png`) e uma tabela pequena com RMSE e viés de Tmax, Tmin e Qg (Tabela 3).
- **Falar:** a Qg do BR-DWGD é interpolada, não medida, por isso foi comparada com as estações do INMET. Tmax: RMSE **[valor]** °C, viés **[valor]**. Tmin: **[valor]**. Qg: **[valor]** MJ m⁻² d⁻¹. Concluir se a base serve de referência.
- **Se o INMET não tiver dado de radiação:** dizer isso claramente e mostrar só as temperaturas.

### Slide 6 · Balanço de radiação (1 min 30 s)
- **Na tela:** Figura 2 (`etapa2_climatologia_radiacao.png`).
- **Falar:** Qo varia com a latitude (26° S); Qg acompanha, mas com transmissividade menor no verão por causa das nuvens. O BOL quase não muda ao longo do ano, então pesa mais no inverno, e Rn cai proporcionalmente mais que Qg. Dizer que o albedo usado foi 0,23 (FAO-56), e por quê.

---

## Radiação a partir da temperatura · Fernandes et al. (4 min)

### Slide 7 · Por que a amplitude térmica indica a radiação (1 min)
- **Na tela:** Figura 3 (`etapa2_boxplot_amplitude.png`) e as 5 equações em versão resumida (HG, BC, CD, DB, DCBB).
- **Falar:** céu limpo dá dia quente e noite fria, ou seja, ΔT grande e Qg alta; céu nublado, o contrário. ΔT usa a Tmin do dia seguinte, como em Fernandes et al. Calibração nos anos ímpares e validação nos pares.

### Slide 8 · Qual modelo foi melhor (1 min 30 s)
- **Na tela:** Figura 4 (`etapa3_dispersao_qg.png`) ou uma tabela pequena com RMSE e c na validação por modelo (Tabela 6).
- **Falar:** o melhor foi **[modelo]**, com RMSE de **[valor]** MJ m⁻² d⁻¹ e c = **[valor]** (**[classe]**). Comparar com o desempenho que Fernandes et al. encontraram em Goiás. Comentar se o modelo com mais parâmetros valeu a pena e o desempenho nos meses secos e úmidos (Tabela 7).

### Slide 9 · Os coeficientes servem nas outras cidades? (1 min 30 s)
- **Na tela:** a tabela de transferência (Tabela 8), só com o melhor modelo.
- **Falar:** usando os coeficientes de uma cidade nas outras, o RMSE aumentou no máximo **[valor]**%. **[Conclusão: um conjunto de coeficientes serve para a região / Pato Branco precisa de calibração própria.]**

---

## Saldo de radiação e ETo · Fietz & Fisch (3 min)

### Slide 10 · Saldo de radiação (1 min 30 s)
- **Na tela:** Figura 6 (`etapa4_razao_rn_qg.png`) e os parâmetros do modelo (3) e do (4).
- **Falar:** Rn/Qg cai de **[valor]** no verão para **[valor]** no inverno, então o modelo proporcional (3) tem viés sazonal e o (4), com Qo, corrige parte disso. Lembrar que o Rn de referência é calculado (FAO-56), enquanto Fietz & Fisch mediram com saldo-radiômetro.

### Slide 11 · Priestley-Taylor e a equação local (1 min 30 s)
- **Na tela:** a equação local **ET₀ = [k] · W · Qg** de cada cidade, a tabela de α_PT seco e úmido (Tabela 11) e, se couber, a Figura 7 (W linear × exato).
- **Falar:** Priestley-Taylor reproduziu a Penman-Monteith com c = **[valor]**. O α_PT recalibrado ficou **[acima/abaixo]** de 1,26 nos meses secos: além da energia, o ar seco aumenta a demanda. A aproximação linear de W subestima o valor exato em Pato Branco, por causa da altitude.

---

## Integração e conclusões (4 min)

### Slide 12 · A resposta: quanto erro chega à ETo (2 min)
- **Na tela:** Figura 8 (`etapa5_dispersao_eto.png`) ou a Tabela 13 em destaque: RMSE de (a) Qg do BR-DWGD, (b) só temperatura e (c) Hargreaves-Samani.
- **Falar:** este é o slide principal. Só com termômetro, a ETo sai com RMSE de **[valor]** mm/dia. Com a Qg do BR-DWGD, **[valor]** mm/dia. A estimativa de Qg responde por **[valor]**% do erro, e o resto é do próprio Priestley-Taylor. Comparar com o Hargreaves-Samani.

### Slide 13 · Quando o erro é maior (1 min)
- **Na tela:** Figura 9 (`etapa5_rmse_mensal.png`).
- **Falar:** o erro é maior em **[meses]**, quando **[interpretação]**. Para uso mensal, como o balanço hídrico do P2, o erro é menor que no diário.

### Slide 14 · Conclusões e próximos passos (1 min)
- **Na tela:** 3 conclusões em uma linha cada e uma linha de ligação com o P2 e o P3.
- **Falar:**
  1. O melhor modelo de Qg por temperatura foi **[modelo]**, com desempenho **[classe]**.
  2. Só com termômetro, a ETo diária tem erro de **[valor]** mm/dia, e **[valor]**% desse erro vem da Qg.
  3. **[Recomendação: equação local ou Hargreaves-Samani para a região.]**
  - **Próximos passos:** essa ETo entra no balanço hídrico do P2 e, depois, na penalização da produtividade no P3.

---

## Slides de reserva (só se perguntarem)

- **R1.** Controle de qualidade: dias descartados por motivo (Tabela 2).
- **R2.** Climatologia de chuva, ETo e temperatura e os meses secos (Tabela 4).
- **R3.** Parâmetros calibrados de todos os modelos (Tabelas 5 e 9).
- **R4.** Dispersão dos 4 modelos de Rn (Figura 5).
- **R5.** Equações completas dos modelos de Qg e das métricas.

---

## Perguntas que o professor deve fazer

| Pergunta | Como responder |
|---|---|
| A estação do INMET está a menos de 10 km? | O ponto de análise é a própria posição da estação (Tabela 1), e o pixel do BR-DWGD tem cerca de 11 km. Se alguma estação estiver longe da cidade, dizer e justificar. |
| A Qg do Xavier é medida? | Não, é interpolada. Por isso foi feita a validação com o INMET (slide 5). |
| Como vocês contornaram o limite do Earth Engine? | Extração ano a ano, salva em CSV no Drive, lida de volta nas execuções seguintes. |
| Por que albedo 0,23 e não 0,25? | 0,23 é o da grama de referência da FAO-56, que é a mesma referência da ETo Penman-Monteith usada na comparação. |
| Um modelo com r = 0,95 é bom? | Não necessariamente: r mede precisão, não exatidão. Por isso usamos d e c (Willmott; Camargo & Sentelhas). |
| Por que a Tmin do dia seguinte no ΔT? | A Tmin ocorre ao amanhecer e reflete a perda de onda longa da noite que segue o dia avaliado, como em Fernandes et al. (2018). |
| Os modelos DB e DCBB convergiram? | Dizer quantos chutes iniciais foram usados (40) e se algum parâmetro ficou no limite (por exemplo, Tnc). |
| O Rn de vocês é comparável ao de Fietz & Fisch? | Não diretamente: o deles foi medido por saldo-radiômetro; o nosso é calculado pela FAO-56. |
| Por que o α_PT muda entre seco e úmido? | Nos meses secos o déficit de pressão de vapor aumenta a demanda além do que a energia explica (componente aerodinâmico). |
| As métricas foram implementadas por vocês? | Sim, como funções próprias no notebook, como o enunciado exige. Mostrar a célula, se pedirem. |

---

## Checklist antes de apresentar

- [ ] Notebook rodado do começo ao fim no Colab, sem erro, com a versão do `main`
- [ ] Todos os campos **[valor]** e **[preencher]** trocados por números (slides e relatório)
- [ ] Figuras em boa resolução (as do notebook saem com 200 dpi)
- [ ] Títulos dos slides dizendo o resultado, não só o tema
- [ ] Ensaio cronometrado: entre 15 e 20 min
- [ ] Notebook aberto no Colab durante a apresentação, para mostrar se pedirem
- [ ] Notebook, relatório e slides enviados até 09/11/2026
