# Previsão de Inflação com ARIMA e LSTM

## 📋 Descrição

Este projeto implementa e compara três métodos de previsão de séries temporais para prever o **Core CPI (Core Consumer Price Index)** americano, um indicador chave da inflação. O projeto utiliza dados macroeconômicos mensais de 1994 a 2020 para treinar e avaliar modelos de machine learning.

## 🎯 Objetivo

Prever o índice Core CPI para novembro de 2021 e comparar o desempenho de três abordagens diferentes :
1. **ARIMA (1,1,1)** - Modelo estatístico clássico
2. **LSTM Univariado** - Rede neural recorrente com uma única variável
3. **LSTM Multivariado** - Rede neural recorrente com múltiplas variáveis macroeconômicas

## 📊 Dados

### Fonte
- **Arquivo** : `macro_monthly.csv`
- **Fonte** : FRED (Federal Reserve Economic Data) - dados públicos
- **Período** : 1994-2020 (após limpeza)
- **Observações** : 334 meses
- **Indicadores** : 11 variáveis macroeconômicas

### Variáveis Incluídas
- `unrate` : Taxa de desemprego
- `psr` : Taxa de poupança pessoal
- `m2` : Massa monetária M2
- `dspic` : Renda disponível pessoal
- `pce` : Despesas de consumo pessoal
- `reer` : Taxa de câmbio efetiva real
- `ir` : Rendimento de títulos de 10 anos
- `ffer` : Taxa da Fed (Federal Funds Rate)
- `tcs` : Gastos com construção
- `indpro` : Índice de produção industrial
- `ccpi` : Core CPI (variável alvo)

## 🔬 Métodos Implementados

### 1. ARIMA (1,1,1)
- **Tipo** : Modelo estatístico clássico
- **Parâmetros** : p=1, d=1, q=1
- **Transformação** : Logarítmica para estabilizar variância
- **Resultado** : MSE = 20.44

### 2. LSTM Univariado
- **Tipo** : Rede neural recorrente
- **Input** : Apenas Core CPI (12 timesteps)
- **Arquitetura** : 1 camada LSTM (64 unidades) + Dense(1)
- **Normalização** : Min-Max [0,1]
- **Resultado** : MSE = 1.97 ⭐ (melhor desempenho)

### 3. LSTM Multivariado
- **Tipo** : Rede neural recorrente
- **Input** : 11 variáveis macroeconômicas (12 timesteps)
- **Arquitetura** : 2 camadas LSTM (100 unidades cada) + Dropout(0.2) + Dense(1)
- **Seleção de features** : Teste de causalidade de Granger
- **Regularização** : Dropout 20%, EarlyStopping (patience=50)
- **Resultado** : MSE = 20.72

## 📈 Resultados

| Modelo | MSE | Erro Médio | Ranking |
|--------|-----|------------|---------|
| ARIMA | 20.44 | +3.15 | 2/3 |
| **LSTM Univariado** | **1.97** | +0.85 | **1/3** ⭐ |
| LSTM Multivariado | 20.72 | -4.13 | 3/3 |

### Previsão Final
- **Modelo vencedor** : LSTM Univariado
- **Core CPI previsto (Nov 2021)** : 281.55
- **Variação YoY** : +4.35%

## 🛠️ Requisitos

### Python
- Python 3.9+

### Bibliotecas Principais
```
pandas
numpy
matplotlib
plotly
statsmodels
scikit-learn
tensorflow==2.20.0
keras
```

### Instalação
```bash
pip install pandas numpy matplotlib plotly statsmodels scikit-learn tensorflow==2.20.0
```

## 🚀 Como Usar

### 1. Preparar o Ambiente
```bash
# Clonar o repositório
git clone [URL_DO_REPOSITORIO]
cd ciencasDados

# Instalar dependências
pip install -r requirements.txt
```

### 2. Preparar os Dados
- Coloque o arquivo `macro_monthly.csv` no diretório raiz do projeto
- O arquivo deve conter as colunas : DATE, unrate, psr, m2, dspic, pce, reer, ir, ffer, tcs, indpro, ccpi

### 3. Executar o Notebook
```bash
jupyter notebook forecasting-inflation-with-arima-and-lstm.ipynb
```

### 4. Estrutura do Notebook
O notebook está organizado em três seções principais :

#### I. Import Libraries and Data Loading
- Importação de bibliotecas
- Carregamento e limpeza dos dados
- Configuração de seeds para reprodutibilidade

#### II. Data Understanding
- Análise de tendências macroeconômicas
- Análise sazonal do Core CPI (por mês e trimestre)
- Visualizações exploratórias

#### III. Forecast
- **ARIMA** : Implementação e avaliação
- **LSTM Univariado** : Treinamento e previsão
- **LSTM Multivariado** : Seleção de features, treinamento e análise de importância

## 🔧 Configuração e Reprodutibilidade

### Seeds
```python
np.random.seed(234)
tf.random.set_seed(234)
```

### Split Train/Test
- **Treino** : 322 observações (1994-2020)
- **Teste** : 12 observações (últimos 12 meses)

### Normalização
- **LSTM** : MinMaxScaler (0, 1)
- **ARIMA** : Transformação logarítmica

## 📝 Estrutura do Projeto

```
ciencasDados/
│
├── forecasting-inflation-with-arima-and-lstm.ipynb
├── macro_monthly.csv
├── README.md
│
├── PRESENTATION_CODE_DETAILLEE_PT.md
├── SCRIPT_PRESENTATION_ORAL_PT.md
├── SLIDES_PRESENTATION_PT.md
│
├── TEXTO_SLIDE_GESTAO_DADOS_PT.md
├── TEXTO_SLIDE_CONFIGURACAO_DADOS_PT.md
└── TEXTO_SLIDE_MODELAGEM_ANALISE_PT.md
```

## 🔍 Conceitos Chave

### Windowing
Transformação da série temporal em janelas deslizantes de 12 meses para alimentar os modelos LSTM.

### Estacionariedade
ARIMA requer séries estacionárias (média e variância constantes), obtida através de diferenciação.

### Seleção de Features
Teste de causalidade de Granger identifica quais variáveis macroeconômicas realmente causam o Core CPI.

### Regularização
Dropout e EarlyStopping evitam sobreajuste, especialmente importante para o modelo multivariado.

## 📚 Referências

- **ARIMA/SARIMA vs LSTM** : [Data Science Central](https://www.datasciencecentral.com/profiles/blogs/arima-sarima-vs-lstm-with-ensemble-learning-insights-for-time-ser)
- **Limitações do ARIMA** : Petrica et al., (2016) - "Limitation of ARIMA models in financial and monetary economics"
- **LSTM para Séries Temporais** : [Machine Learning Mastery](https://machinelearningmastery.com/how-to-develop-lstm-models-for-time-series-forecasting/)
- **FRED** : [Federal Reserve Economic Data](https://fred.stlouisfed.org/)

## 👥 Contribuições

Contribuições são bem-vindas! Por favor :
1. Faça um fork do projeto
2. Crie uma branch para sua feature (`git checkout -b feature/AmazingFeature`)
3. Commit suas mudanças (`git commit -m 'Add some AmazingFeature'`)
4. Push para a branch (`git push origin feature/AmazingFeature`)
5. Abra um Pull Request

## 📄 Licença

Este projeto está sob licença MIT. Veja o arquivo `LICENSE` para mais detalhes.

## 🙏 Agradecimentos

- FRED (Federal Reserve Economic Data) pelos dados públicos
- Comunidade open source pelas bibliotecas utilizadas

## 📧 Contato

Para questões ou sugestões, abra uma issue no repositório.

---

**Nota** : Este projeto foi desenvolvido para fins educacionais e de pesquisa. Os resultados não devem ser usados para decisões financeiras sem análise adicional.

