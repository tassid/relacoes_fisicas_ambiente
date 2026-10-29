# Estimativa da radiação solar global, do saldo de radiação e da evapotranspiração de referência a partir da temperatura do ar no Sudoeste do Paraná

**Autores:** Tassiane Anzolin; **[preencher: demais integrantes do grupo]**

Programa de Pós-Graduação em Tecnologias Computacionais para o Agronegócio (PPGTCA), UTFPR, Câmpus Santa Helena
Disciplina: Relações Físicas do Ambiente Agrícola · Projeto 1 · 09/11/2026

> **Como usar este arquivo.** O texto está completo. Os valores numéricos dependem da execução do notebook [`codigos_python/projeto_01_sudoeste.ipynb`](../codigos_python/projeto_01_sudoeste.ipynb) no Colab. Cada trecho marcado com **[preencher: ...]** deve ser trocado pelo valor correspondente. As tabelas saem prontas em `resultados/tabelas_relatorio.md` (no Drive, pasta `RFAA_P1_sudoeste`), gerado na última seção do notebook, e as figuras em `figuras/`. Copiem as figuras para `relatorio/figuras/` para que apareçam aqui. Apaguem este aviso antes de entregar.

---

## Resumo

A radiação solar global (Qg) é a principal entrada de energia nos modelos de evapotranspiração, mas é medida em poucas estações, enquanto a temperatura do ar é medida em quase todas. Este trabalho avaliou, para Pato Branco, Francisco Beltrão e Dois Vizinhos (PR), se é possível estimar a evapotranspiração de referência (ETo) usando apenas a temperatura, e em que etapa dessa cadeia o erro se origina. Com dados diários do BR-DWGD (Xavier et al., 2022) de 2001 a 2025, foram calibrados (anos ímpares) e validados (anos pares) cinco modelos empíricos de Qg baseados na amplitude térmica (Fernandes et al., 2018), quatro modelos de saldo de radiação (Rn) e o método de Priestley-Taylor (Fietz & Fisch, 2009), tendo como referência o Rn calculado pela FAO-56 e a ETo Penman-Monteith. O melhor modelo de Qg foi **[preencher: modelo]**, com RMSE de **[preencher]** MJ m⁻² d⁻¹ e índice c de **[preencher]** na validação. A cadeia que usa apenas temperatura estimou a ETo com RMSE de **[preencher]** mm d⁻¹, contra **[preencher]** mm d⁻¹ quando se usa a Qg do BR-DWGD e **[preencher]** mm d⁻¹ com Hargreaves-Samani. **[preencher: frase com a conclusão principal, por exemplo: "A estimativa de Qg responde por cerca de X% do erro quadrático da cadeia, e os coeficientes calibrados são transferíveis entre os três municípios com perda inferior a Y%."]**

**Palavras-chave:** amplitude térmica; Priestley-Taylor; saldo de radiação; BR-DWGD; Hargreaves.

---

## 1. Introdução

A evapotranspiração de referência (ETo) é a base do manejo da irrigação, do balanço hídrico climatológico e dos modelos de produtividade, como o da Zona Agroecológica da FAO. O método padrão, Penman-Monteith FAO-56 (Allen et al., 1998), exige radiação solar, temperatura, umidade relativa e velocidade do vento. Na prática, porém, a radiação solar global (Qg) é medida em uma rede muito menos densa que a de temperatura, e séries longas de Qg medida são raras.

Uma alternativa é estimar Qg pela **amplitude térmica diária**. O raciocínio é físico: em dias de céu limpo, a superfície recebe muita radiação de onda curta durante o dia, elevando a Tmax, e perde muita radiação de onda longa à noite, derrubando a Tmin. Em dias nublados ocorre o contrário. Assim, ΔT = Tmax − Tmin funciona como um indicador da transmissividade atmosférica. Fernandes et al. (2018) calibraram para Goiás cinco modelos com essa base: Hargreaves (HG), Bristow-Campbell (BC), Donatelli-Campbell (CD), Donatelli-Bellocchi (DB) e uma combinação dos dois últimos (DCBB).

Com Qg disponível, o saldo de radiação (Rn) pode ser estimado por relações simples, e a ETo pelo método de Priestley-Taylor (Priestley & Taylor, 1972), que depende essencialmente da energia disponível. Fietz & Fisch (2009) avaliaram essas relações para Dourados (MS). Se Rn for proporcional a Qg, Priestley-Taylor se reduz a ETo = k · W · Qg, uma equação que só precisa de temperatura (para W) e de Qg.

Este trabalho integra os dois artigos para responder: **se houver apenas termômetro, quanto erro chega à ETo, e em qual elo da cadeia temperatura → Qg → Rn → ETo esse erro nasce?** A análise foi feita para três municípios próximos do Sudoeste do Paraná, o que permite avaliar também se os coeficientes calibrados em um local podem ser usados nos outros.

## 2. Material e métodos

### 2.1 Área de estudo

Os locais são Pato Branco, Francisco Beltrão e Dois Vizinhos, no Terceiro Planalto Paranaense, na transição entre os climas Cfa (subtropical úmido com verão quente) e Cfb (subtropical úmido com verão ameno), ambos sem estação seca definida (Alvares et al., 2013). Os municípios são próximos entre si e formam um gradiente de altitude, com Pato Branco no ponto mais alto (Tabela 1). A região é uma das mais chuvosas do Paraná, tem agricultura de grãos feita em grande parte por produtores familiares e é uma importante bacia leiteira, o que torna a demanda hídrica relevante tanto para as lavouras quanto para as pastagens. Em cada um, o ponto de análise foi a posição da estação meteorológica do INMET, como exigido pelo projeto.

**Tabela 1.** Locais de estudo.

**[preencher: colar a Tabela 1 de `tabelas_relatorio.md`, com área, altitude média (SRTM) e chuva anual (CHIRPS)]**

### 2.2 Dados

Foram usados dados diários do BR-DWGD (*Brazilian Daily Weather Gridded Data*; Xavier et al., 2022), com resolução de 0,1° × 0,1°, de 01/01/2001 a 31/12/2025: precipitação, Tmax, Tmin, umidade relativa, vento a 2 m, Qg e ETo Penman-Monteith. A extração foi feita no Google Earth Engine (coleção `projects/ee-alexandrexavier/assets/BR-DWGD`). Como a série completa ultrapassa o limite de elementos de uma requisição `getRegion`, os dados foram extraídos **ano a ano** (25 requisições por local) e salvos em CSV. As bandas são armazenadas como inteiros; o valor físico foi obtido por `valor = bruto · MULT + ADD`, com os fatores lidos das propriedades de cada imagem.

Duas ressalvas condicionam a interpretação. Primeiro, a Qg do BR-DWGD **não é medida**: é interpolada a partir de estações, de modo que funciona aqui como referência e não como verdade de campo. Segundo, o BR-DWGD não contém Rn; o Rn de referência foi **calculado** pela FAO-56, enquanto Fietz & Fisch (2009) usaram saldo-radiômetro.

### 2.3 Controle de qualidade

Valores fora de faixas plausíveis foram tratados como falha: chuva de 0 a 300 mm; Tmax de −5 a 45 °C; Tmin de −10 a 35 °C; UR de 0 a 100%; vento de 0 a 20 m s⁻¹; Qg de 0 a 40 MJ m⁻² d⁻¹; ETo de 0 a 12 mm d⁻¹. Foram aplicadas também duas verificações físicas: dias com Tmin > Tmax tiveram as duas temperaturas descartadas, e dias com Qg maior que a irradiância extraterrestre (Qo) tiveram Qg descartada.

Os meses foram classificados como **secos** quando a chuva média mensal foi menor que a ETo média mensal, e como **úmidos** caso contrário.

### 2.4 Validação com a estação do INMET

Como a Qg e as temperaturas do BR-DWGD são interpoladas, a base foi comparada com as medições da estação do INMET de cada município (**[preencher: códigos das estações e período com dados]**), obtidas no portal de dados históricos do INMET. Os dados horários foram convertidos para valores diários no horário local (UTC−3), aceitando apenas dias com pelo menos 20 horas de registro: chuva e radiação foram somadas, Tmax e Tmin correspondem ao máximo e ao mínimo do dia e a umidade relativa é a média diária. As séries passaram pelo mesmo controle de qualidade do BR-DWGD. A comparação usou as métricas descritas no item 2.9, em escala diária e mensal (média dos valores diários de meses com pelo menos 20 dias nas duas bases), com o INMET como observado.

### 2.5 Variáveis astronômicas e balanço de radiação

Para cada dia, a partir do número do dia do ano (NDA) e da latitude, foram calculados a declinação solar, o ângulo horário do nascer do Sol, o fotoperíodo (N), a correção da distância Terra-Sol e Qo, com a biblioteca `agrometeorologiapy`. O balanço de radiação seguiu a FAO-56 (Allen et al., 1998):

- pressão de saturação pela média de es(Tmax) e es(Tmin) (Tetens) e ea = es · UR/100;
- radiação de céu claro Qg,cs = (0,75 + 2 · 10⁻⁵ · z) · Qo, sendo z a altitude (m);
- balanço de ondas curtas BOC = Qg · (1 − r), com albedo r = 0,23 (grama de referência da FAO-56, coerente com a ETo de referência usada na comparação);
- balanço de ondas longas BOL = −σ · [(Tmax,K⁴ + Tmin,K⁴)/2] · (0,34 − 0,14 · √ea) · (1,35 · Qg/Qg,cs − 0,35), com Qg/Qg,cs limitada a 1;
- saldo de radiação Rn = BOC + BOL.

### 2.6 Modelos de Qg a partir da temperatura

Seguindo Fernandes et al. (2018), foram usadas as variáveis ΔTᵢ = Tmaxᵢ − (Tminᵢ + Tminᵢ₊₁)/2 (com a Tmin do dia seguinte), ΔTavg (média móvel centrada de 7 dias de ΔTᵢ) e Tavg = (Tmax + Tmin)/2. Os cinco modelos estão no Quadro 1.

**Quadro 1.** Modelos de Qg avaliados.

| Modelo | Equação | Parâmetros calibrados |
|---|---|---|
| HG | Qg = b · Qo · √ΔTᵢ + c | b, c |
| BC | Qg = 0,75 · [1 − exp(−b · ΔTᵢᶜ / ΔTavg)] · Qo | b, c |
| CD | Qg = 0,75 · [1 − exp(−b · f(Tavg) · ΔTᵢ² · exp(Tmin/Tnc))] · Qo | b, Tnc |
| DB | Qg = 0,75 · [1 + f(NDA)] · [1 − exp(−b · ΔTᵢ² / ΔTavg)] · Qo | b, C1, C2 |
| DCBB | Qg = 0,75 · [1 + f(NDA)] · [1 − exp(−b · ΔTᵢ² · exp(Tmin/Tnc) / ΔTavg)] · Qo | b, C1, C2, Tnc |

f(Tavg) = 0,017 · exp[exp(−0,053 · Tavg)]; f(NDA) = C1 · [sen(NDA · C2 · π/180) + cos(NDA · f₃ · π/180)], com f₃ = 1 − 1,9 · C3 + 3,83 · C3² e C3 = C2 − int(C2).

Os parâmetros foram ajustados por mínimos quadrados não lineares (`scipy.optimize.least_squares`, com limites) nos **anos ímpares** e avaliados nos **anos pares**. Como DB e DCBB são sensíveis ao chute inicial, cada ajuste partiu de vários chutes (40 para DB e DCBB, 10 para os demais), sorteados dentro dos limites com semente fixa, e foi mantido o de menor soma de quadrados. O melhor modelo de cada local foi o de maior eficiência de Nash-Sutcliffe (EF) na validação. Para avaliar a **transferibilidade**, os parâmetros de cada local foram aplicados aos outros dois.

### 2.7 Modelos de Rn e Priestley-Taylor

Foram calibrados quatro modelos de Rn contra o Rn FAO-56 (anos ímpares): (1) Rn = α · Tmax + β · Tmin; (2) Rn = α · (Tmax − Tmin)^β · Qo; (3) Rn = α · Qg; (4) Rn = α · Qg + β · Qo.

A ETo de Priestley-Taylor foi calculada como ETo = α_PT · W · Rn / λ, com α_PT = 1,26, λ = 2,45 MJ kg⁻¹ e fluxo de calor no solo desprezado na escala diária. O fator W = Δ/(Δ + γ) foi obtido pela aproximação linear usada por Fietz & Fisch (2009): W = 0,407 + 0,0145 · T para T ≤ 16 °C e W = 0,483 + 0,01 · T para T > 16 °C. Essa aproximação foi comparada com o W exato, calculado com a derivada da curva de Tetens e a constante psicrométrica da altitude de cada local. Combinando Priestley-Taylor com o modelo (3), obteve-se a equação local ETo = k · W · Qg, com k = α_PT · α₃ / λ. Por fim, α_PT foi recalibrado por regressão pela origem da ETo Penman-Monteith contra W · Rn / λ, separadamente para os meses secos e úmidos.

### 2.8 Integração das duas cadeias

Foram comparadas com a ETo Penman-Monteith do BR-DWGD, nos anos pares, três estimativas: (a) Priestley-Taylor com Qg do BR-DWGD e Rn FAO-56; (b) Priestley-Taylor com Qg estimada pelo melhor modelo de temperatura e Rn pelo modelo (3), ou seja, **apenas temperatura**; (c) Hargreaves-Samani (Hargreaves & Samani, 1985), também apenas com temperatura. A parcela do erro de (b) atribuída à estimativa de Qg foi calculada como (RMSE_b² − RMSE_a²)/RMSE_b².

### 2.9 Métricas de desempenho

As métricas foram implementadas como funções próprias e usadas em todas as comparações: RMSE, RMSE relativo (RRMSE), erro absoluto médio (MAE), viés médio, eficiência de Nash-Sutcliffe (EF; Nash & Sutcliffe, 1970), coeficiente de determinação (R²), índice de concordância d (Willmott, 1981) e índice de desempenho c = r · d, classificado segundo Camargo & Sentelhas (1997): ótimo (> 0,85), muito bom (0,76–0,85), bom (0,66–0,75), mediano (0,61–0,65), sofrível (0,51–0,60), mau (0,41–0,50) e péssimo (≤ 0,40). O r e o R² medem a precisão (dispersão em torno de uma reta), enquanto d mede a exatidão (afastamento da linha 1:1); por isso um modelo pode ter r elevado e ainda assim erros sistemáticos grandes.

## 3. Resultados e discussão

### 3.1 Base de dados e clima

O controle de qualidade descartou **[preencher]** dias em Pato Branco, **[preencher]** em Francisco Beltrão e **[preencher]** em Dois Vizinhos (Tabela 2), principalmente por **[preencher: causa mais frequente]**. **[preencher: comentar se as falhas se concentram em algum período ou variável.]**

**Tabela 2.** Dias descartados no controle de qualidade.

**[preencher: colar a Tabela 2]**

A comparação com as estações do INMET (Tabela 3, Figura 1) mostrou que o BR-DWGD reproduz bem as temperaturas, com RMSE diário de **[preencher]** °C para Tmax e **[preencher]** °C para Tmin e viés de **[preencher]** e **[preencher]** °C. Para Qg, o RMSE diário foi de **[preencher]** MJ m⁻² d⁻¹ (viés de **[preencher]**), e na escala mensal o índice d chegou a **[preencher]**. **[preencher: interpretar o viés; por exemplo, se o BR-DWGD superestima a Tmin, a amplitude térmica fica menor que a real, e os modelos de Qg calibrados com ele herdam esse efeito.]** A chuva diária teve concordância **[preencher]**, como esperado, porque o horário de fechamento do dia difere entre as bases e a chuva é muito localizada; na escala mensal, a concordância sobe para d = **[preencher]**. Esses resultados indicam que **[preencher: a base pode / não pode]** ser usada como referência nas etapas seguintes.

**Tabela 3.** Desempenho do BR-DWGD em relação às estações do INMET (INMET como observado).

**[preencher: colar a Tabela 3]**

![BR-DWGD versus INMET](figuras/etapa1b_brdwgd_vs_inmet.png)
*Figura 1. Tmax, Tmin e Qg diárias do BR-DWGD × estação do INMET.*

Os três municípios têm chuva bem distribuída ao longo do ano (Tabela 4), compatível com os climas Cfa e Cfb. Pelo critério adotado, os meses secos foram **[preencher: meses]**, quando a demanda atmosférica supera a chuva média. A classificação de Köppen obtida pela própria série foi **[preencher]**. **[preencher: comentar a diferença entre os locais; o "a" de Cfa exige mês mais quente acima de 22 °C, então Pato Branco, mais alta, tende a ficar em Cfb, e Dois Vizinhos e Francisco Beltrão em Cfa.]**

**Tabela 4.** Climatologia mensal de chuva, ETo e temperatura média (2001–2025).

**[preencher: colar a Tabela 4]**

### 3.2 Balanço de radiação

![Climatologia mensal do balanço de radiação](figuras/etapa2_climatologia_radiacao.png)
*Figura 2. Climatologia mensal de Qo, Qg, BOC, BOL e Rn em Pato Branco, Francisco Beltrão e Dois Vizinhos (2001–2025).*

Qo varia de **[preencher]** MJ m⁻² d⁻¹ em junho a **[preencher]** MJ m⁻² d⁻¹ em dezembro, efeito da latitude (cerca de 26° S, ao sul do Trópico de Capricórnio, o que dá uma variação anual de Qo maior que no norte do estado). Qg acompanha Qo, mas com transmissividade (Qg/Qo) de **[preencher]** no verão e **[preencher]** no inverno: no verão, a nebulosidade da estação chuvosa reduz a fração de Qo que chega à superfície. O BOL varia pouco ao longo do ano (**[preencher]** a **[preencher]** MJ m⁻² d⁻¹) e é mais negativo no inverno, quando o ar mais seco devolve menos radiação de onda longa à superfície. Como resultado, o Rn cai proporcionalmente mais que Qg no inverno.

Os três locais têm balanços muito parecidos, como esperado pela proximidade. A principal diferença é **[preencher: por exemplo, BOL ligeiramente mais negativo em Pato Branco, pelas temperaturas mais baixas e pelo ar menos úmido em altitude]**.

![Amplitude térmica diária por mês](figuras/etapa2_boxplot_amplitude.png)
*Figura 3. Distribuição mensal da amplitude térmica diária (Tmax − Tmin).*

A amplitude térmica é maior em **[preencher: meses]** (mediana de **[preencher]** °C) e menor em **[preencher]**. Os meses de inverno, mais secos e com mais dias de céu limpo, combinam aquecimento diurno com forte resfriamento noturno, o que amplia ΔT. Essa variação sazonal é a informação que os modelos de Fernandes et al. (2018) usam para estimar Qg.

### 3.3 Qg a partir da temperatura

**Tabela 5.** Parâmetros calibrados dos modelos de Qg.

**[preencher: colar a Tabela 5]**

**Tabela 6.** Desempenho dos modelos de Qg na validação (anos pares).

**[preencher: colar a Tabela 6]**

![Qg estimada versus Qg do BR-DWGD](figuras/etapa3_dispersao_qg.png)
*Figura 4. Qg estimada × Qg do BR-DWGD na validação, por modelo e local.*

Na validação, o melhor modelo foi **[preencher]** em Pato Branco, **[preencher]** em Francisco Beltrão e **[preencher]** em Dois Vizinhos, com RMSE entre **[preencher]** e **[preencher]** MJ m⁻² d⁻¹ (RRMSE de **[preencher]**%). As métricas da validação ficaram próximas às da calibração, o que indica que **[preencher: não houve sobreajuste / houve sobreajuste em algum modelo]**. O HG, que usa ΔT de forma linear em √ΔT, teve o pior desempenho **[preencher: confirmar]**; os modelos exponenciais (BC, CD, DB, DCBB) representam melhor a saturação de Qg em dias de céu limpo, quando um aumento de ΔT não pode elevar Qg acima de cerca de 75% de Qo.

Os modelos com mais parâmetros (DB e DCBB) **[preencher: superaram / não superaram]** os mais simples. **[preencher: se não superaram, discutir que o termo sazonal f(NDA) acrescenta pouco onde a sazonalidade já está em Qo e ΔTavg; se algum parâmetro bateu no limite, como Tnc, registrar e discutir que o termo exp(Tmin/Tnc) ficou praticamente constante, ou seja, o efeito da Tmin foi pequeno.]**

Nos dois gráficos de dispersão, os maiores erros ocorrem em **[preencher: dias de Qg baixa/alta]**: **[preencher: por exemplo, os modelos superestimam Qg em dias nublados com ΔT moderado, típicos de frentes frias, quando a nuvem reduz a radiação mas a advecção de ar mantém a amplitude.]**

**Tabela 7.** Índice c diário dos modelos de Qg por período (série completa).

**[preencher: colar a Tabela 7]**

Em escala diária, o desempenho foi **[preencher: melhor / pior]** nos meses secos (c de **[preencher]**) do que nos úmidos (c de **[preencher]**). **[preencher: interpretação; por exemplo, nos meses úmidos a nebulosidade varia de um dia para o outro sem relação estreita com ΔT, enquanto nos meses secos a relação entre céu limpo e amplitude é mais forte.]** Na escala mensal, o índice d sobe para **[preencher]**, porque os erros diários de sinais opostos se compensam; para aplicações mensais, como o balanço hídrico climatológico, a estimativa por temperatura é, portanto, mais confiável que para o manejo diário da irrigação.

**Tabela 8.** RMSE (MJ m⁻² d⁻¹) na validação com coeficientes calibrados em um local e aplicados nos outros.

**[preencher: colar a Tabela 8]**

A troca de coeficientes entre os municípios aumentou o RMSE em no máximo **[preencher]** MJ m⁻² d⁻¹ (**[preencher]**%). **[preencher: se a perda for pequena, concluir que um único conjunto de coeficientes serve para a região; se Pato Branco se destacar, discutir o efeito da altitude sobre a amplitude térmica e a umidade.]**

### 3.4 Saldo de radiação e Priestley-Taylor

**Tabela 9.** Parâmetros dos modelos de Rn.

**[preencher: colar a Tabela 9]**

**Tabela 10.** Desempenho dos modelos de Rn na validação.

**[preencher: colar a Tabela 10]**

![Rn estimado versus Rn FAO-56](figuras/etapa4_dispersao_rn.png)
*Figura 5. Rn estimado × Rn FAO-56 na validação, por modelo e local.*

O modelo (4), que combina Qg e Qo, foi o mais exato (c = **[preencher]**), seguido do (3) (c = **[preencher]**). Os modelos baseados apenas na temperatura, (1) e (2), tiveram desempenho **[preencher]**: sem informação direta de Qg, eles não distinguem bem dias de céu limpo e nublado. O coeficiente α do modelo (3) foi de **[preencher]** a **[preencher]**, isto é, cerca de **[preencher]**% de Qg se converte em Rn. **[preencher: comparar com o valor de Fietz & Fisch (2009) para Dourados e discutir a diferença: lá o Rn foi medido por saldo-radiômetro, aqui foi calculado pela FAO-56 com albedo 0,23.]**

![Razão Rn/Qg mensal](figuras/etapa4_razao_rn_qg.png)
*Figura 6. Razão mensal Rn/Qg.*

A razão Rn/Qg não é constante: varia de **[preencher]** no verão a **[preencher]** no inverno (Figura 6). Como o BOC é sempre 77% de Qg e o BOL quase não diminui no inverno, a perda de onda longa pesa mais quando Qg é menor. Isso explica o viés sazonal do modelo (3), que tende a **superestimar Rn no inverno e subestimá-lo no verão**, e por que o modelo (4), com o termo em Qo, representa melhor essa variação.

![Fator W](figuras/etapa4_fator_w.png)
*Figura 7. Fator W pela aproximação linear de Fietz & Fisch (2009) e pela expressão exata Δ/(Δ + γ) em cada altitude.*

A aproximação linear de W diferiu do valor exato em média **[preencher]** em Pato Branco, **[preencher]** em Francisco Beltrão e **[preencher]** em Dois Vizinhos. A diferença cresce com a altitude porque a constante psicrométrica diminui com a pressão atmosférica, o que eleva o W exato; a aproximação linear, sem termo de altitude, **subestima** W nos locais mais altos.

**Tabela 11.** α_PT recalibrado por período.

**[preencher: colar a Tabela 11]**

O Priestley-Taylor com Rn FAO-56 reproduziu a ETo Penman-Monteith com c = **[preencher]** e RMSE de **[preencher]** mm d⁻¹, e a equação local ETo = **[preencher: k]** · W · Qg teve desempenho **[preencher: semelhante]**. O α_PT recalibrado foi **[preencher]** nos meses secos e **[preencher]** nos úmidos. Valores acima de 1,26 nos meses secos indicam que, além da energia disponível, o déficit de pressão de vapor do ar contribui para a demanda evaporativa, um componente aerodinâmico que o Priestley-Taylor original não representa. **[preencher: ajustar a frase ao resultado.]**

### 3.5 Cadeia integrada: quanto erro chega à ETo usando apenas temperatura

**Tabela 12.** ETo estimada × Penman-Monteith nos anos pares.

**[preencher: colar a Tabela 12]**

**Tabela 13.** Origem do erro da cadeia apenas com temperatura.

**[preencher: colar a Tabela 13]**

![ETo estimada versus Penman-Monteith](figuras/etapa5_dispersao_eto.png)
*Figura 8. ETo estimada pelos cenários (a), (b) e (c) × ETo Penman-Monteith.*

![RMSE mensal da ETo](figuras/etapa5_rmse_mensal.png)
*Figura 9. RMSE mensal da ETo nos três cenários.*

Usando apenas temperatura (cenário b), a ETo foi estimada com RMSE de **[preencher]** mm d⁻¹ e c = **[preencher]** (**[preencher: classe]**). Com a Qg do BR-DWGD (cenário a), o RMSE foi de **[preencher]** mm d⁻¹; esse é o erro que já existe sem estimar Qg e se deve ao próprio Priestley-Taylor (ausência do termo aerodinâmico), à aproximação de W e à relação Rn–Qg. A estimativa de Qg pela temperatura respondeu por **[preencher]**% do erro quadrático da cadeia (Tabela 13).

**[preencher: escolher a interpretação que corresponder ao resultado:]**
- *Se a parcela for positiva e alta:* o elo mais frágil é a estimativa de Qg; melhorar Qg (por exemplo, com dados de satélite) traria o maior ganho.
- *Se a parcela for pequena ou negativa:* a maior parte do erro está no próprio Priestley-Taylor; o erro de Qg chega a compensar parcialmente o viés do restante da cadeia, de modo que estimar Qg pela temperatura custa pouco.

Comparada ao Hargreaves-Samani (cenário c), que também usa apenas temperatura, a cadeia calibrada **[preencher: foi melhor / equivalente / pior]** (RMSE de **[preencher]** contra **[preencher]** mm d⁻¹). **[preencher: se melhor, a vantagem vem da calibração local; se equivalente, o Hargreaves-Samani, sem calibração, é mais simples para o mesmo resultado.]** O erro foi maior em **[preencher: meses]** (Figura 9), quando **[preencher: interpretação, por exemplo, a advecção de ar seco no fim do inverno eleva a ETo Penman-Monteith acima do que a energia disponível explica]**.

### 3.6 Limitações

- A Qg e a ETo de referência vêm do BR-DWGD, que é interpolado; os modelos foram avaliados contra uma referência que também tem erro. A validação com o INMET (Tabela 3) mede esse erro, mas cobre só o período em que as estações têm dados.
- O Rn de referência foi calculado pela FAO-56, e não medido; parte da concordância entre os modelos de Rn e a ETo vem de usarem as mesmas equações.
- Os coeficientes valem para o período e os locais estudados; a transferência para outras regiões do Paraná exige nova validação.

## 4. Conclusões

1. Os modelos exponenciais de Qg a partir da amplitude térmica tiveram desempenho **[preencher: classe]** nos três municípios; o melhor foi **[preencher]**, com RMSE de **[preencher]** MJ m⁻² d⁻¹ na validação.
2. Os coeficientes calibrados em um município **[preencher: podem / não podem]** ser usados nos outros dois com perda de desempenho de até **[preencher]**%.
3. O saldo de radiação é mais bem estimado por Qg e Qo (modelo 4); a razão Rn/Qg cai do verão para o inverno, de **[preencher]** para **[preencher]**, o que torna o modelo proporcional (3) sazonalmente enviesado.
4. Usando apenas temperatura, a ETo diária pode ser estimada com RMSE de **[preencher]** mm d⁻¹; a estimativa de Qg responde por **[preencher]**% desse erro, e o restante vem do próprio método de Priestley-Taylor.
5. **[preencher: recomendação final, por exemplo: "Para locais do Sudoeste do Paraná que só têm termômetro, recomenda-se a equação local ETo = k · W · Qg com Qg pelo modelo X, ou o Hargreaves-Samani, que teve desempenho equivalente sem exigir calibração."]**

## Referências

ALLEN, R. G.; PEREIRA, L. S.; RAES, D.; SMITH, M. **Crop evapotranspiration**: guidelines for computing crop water requirements. Rome: FAO, 1998. (Irrigation and Drainage Paper, 56).

ALVARES, C. A.; STAPE, J. L.; SENTELHAS, P. C.; GONÇALVES, J. L. M.; SPAROVEK, G. Köppen's climate classification map for Brazil. **Meteorologische Zeitschrift**, v. 22, n. 6, p. 711-728, 2013.

BRISTOW, K. L.; CAMPBELL, G. S. On the relationship between incoming solar radiation and daily maximum and minimum temperature. **Agricultural and Forest Meteorology**, v. 31, n. 2, p. 159-166, 1984.

CAMARGO, A. P.; SENTELHAS, P. C. Avaliação do desempenho de diferentes métodos de estimativa da evapotranspiração potencial no Estado de São Paulo, Brasil. **Revista Brasileira de Agrometeorologia**, v. 5, n. 1, p. 89-97, 1997.

FERNANDES, D. S.; HEINEMANN, A. B.; AMORIM, A. O.; PAZ, R. L. F. Estimativa da radiação solar global com base em observações de temperatura para o estado de Goiás. **Revista Brasileira de Meteorologia**, v. 33, n. 3, p. 558-566, 2018.

FIETZ, C. R.; FISCH, G. F. Avaliação de modelos de estimativa do saldo de radiação e do método de Priestley-Taylor para a região de Dourados, MS. **Revista Brasileira de Engenharia Agrícola e Ambiental**, v. 13, n. 4, p. 449-453, 2009.

HARGREAVES, G. H.; SAMANI, Z. A. Reference crop evapotranspiration from temperature. **Applied Engineering in Agriculture**, v. 1, n. 2, p. 96-99, 1985.

NASH, J. E.; SUTCLIFFE, J. V. River flow forecasting through conceptual models part I: a discussion of principles. **Journal of Hydrology**, v. 10, n. 3, p. 282-290, 1970.

PEREIRA, A. R.; ANGELOCCI, L. R.; SENTELHAS, P. C. **Agrometeorologia**: fundamentos e aplicações práticas. Guaíba: Agropecuária, 2002.

PRIESTLEY, C. H. B.; TAYLOR, R. J. On the assessment of surface heat flux and evaporation using large-scale parameters. **Monthly Weather Review**, v. 100, n. 2, p. 81-92, 1972.

WILLMOTT, C. J. On the validation of models. **Physical Geography**, v. 2, n. 2, p. 184-194, 1981.

XAVIER, A. C.; SCANLON, B. R.; KING, C. W.; ALVES, A. I. New improved Brazilian daily weather gridded data (1961–2020). **International Journal of Climatology**, v. 42, n. 16, p. 8390-8404, 2022.
