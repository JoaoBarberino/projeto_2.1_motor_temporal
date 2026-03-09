##  O Motor Temporal e Otimização Quantitativa (Projeto 2)

Neste projeto, elevamos a Engenharia de Dados ao nível de **Séries Temporais** e **Análise Quantitativa**, resolvendo o problema crítico de *Data Leakage* (Vazamento de Dados) e enfrentando a eficiência financeira das casas de apostas.

### Extração e o "Escudo Temporal"
Para evitar que o modelo previsse o passado usando médias do futuro, construímos um Pipeline de ETL com Web Scraping (via *Wayback Machine*) para extrair o diário cronológico de jogos (Match Logs). 
* **Feature Engineering:** Criamos um Laço de Repetição Temporal que viaja rodada a rodada calculando a **Média Móvel de 5 jogos** para o Ataque (xG) e Defesa (xGA) de cada equipe, estritamente *antes* de a bola rolar.

### O Paradoxo da Acurácia vs. ROI
Focamos o *Random Forest* no mercado de Gols (Over/Under 2.5). O modelo alcançou uma acurácia excepcional, mas o Backtest Financeiro cruzado com as Odds da Bet365 revelou um **ROI negativo**. 
* **O Diagnóstico:** Acurácia não vence o *Juice* (margem de lucro da casa). O modelo atirava em todos os jogos com "falsa confiança", comprando *Odds* esmagadas sem Valor Esperado (EV+).

### Calibração de Probabilidades e Radar EV+
Para reverter o prejuízo, implementamos técnicas de fundos quantitativos:
1. **Calibração:** Usamos o `CalibratedClassifierCV` (Scikit-Learn) para corrigir a superconfiança do Random Forest, forçando a IA a gerar probabilidades matemáticas reais.
2. **Otimização de Threshold:** Construímos um "Radar Quantitativo" em Python que simulou o campeonato dezenas de vezes para encontrar a margem de segurança perfeita. 
* **O Resultado:** O radar detectou que ao exigir um **Filtro EV+ de 1.27** (só apostar quando nossa matemática for 27% superior à da casa), o modelo filtrou o ruído, encontrou 10 entradas cirúrgicas e entregou um **ROI de 52.30%**.

## Feature Engineering e a Barreira da Multicolinearidade

Após alcançar 52.30% de ROI com médias de xG (Expected Goals), a hipótese testada foi: *"Dar mais profundidade tática à máquina aumentará o volume de apostas lucrativas?"*

* **A Engenharia:** Expandimos o Motor Temporal extraindo dados do *football-data.co.uk*. Passamos de 4 variáveis para 12 dimensões táticas de Ataque e Defesa: `xG`, `xGA`, `Remates`, `Remates no Alvo`, `Cantos` e `Faltas`.
* **O Paradoxo dos Dados:** Ao rodar o novo modelo de 12 variáveis no Radar Quantitativo, o resultado foi **exatamente o mesmo**: 52.30% de ROI nas mesmas 10 entradas.
* **O Diagnóstico (Lição Aprendida):** Comprovamos na prática o efeito da **Multicolinearidade**. A métrica de xG já carrega implicitamente a informação de chutes e escanteios. O algoritmo (*Random Forest*) reconheceu que as novas colunas eram redundantes. Além disso, mapeamos o **Teto de Eficiência da Premier League**: a casa de apostas (Bet365) simplesmente não comete mais do que 10 erros grosseiros por temporada nesse mercado.

