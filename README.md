##  Fase 5: O Motor Temporal e Otimização Quantitativa (Projeto 2)

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

