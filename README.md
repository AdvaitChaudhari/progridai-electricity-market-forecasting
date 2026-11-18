# Electricity price forecasting – Spanish day-ahead market

Next-hour electricity price forecast for Spain (2015–2018), comparing an LSTM, a 1D-CNN and a
persistence baseline.

**Data:** the public *Hourly energy demand, generation, prices and weather – Spain* dataset
(ENTSO-E / REE market data plus OpenWeather data for five cities; I use Madrid).

**Approach**
- join energy and weather data on the hourly UTC timestamp
- features: price, total load, onshore wind, solar generation, temperature, humidity, wind speed, hour (sin/cos), weekend flag
- 24-hour look-back windows, chronological 70/15/15 split, scaling fitted on train only
- LSTM vs 1D-CNN, both with early stopping; persistence as the baseline

**Results (test period, €/MWh)**

| Model | MAE | RMSE |
|---|---|---|
| Persistence | 1.97 | 2.88 |
| **LSTM** | **1.72** | **2.28** |
| 1D-CNN | 2.75 | 3.52 |

**Things I noticed**
- The LSTM beats persistence by about 13 % MAE. The 1D-CNN does *worse* than persistence, so the convolution over 24 h
  doesn't capture the price dynamics as well as the recurrent model.
- Hourly price is strongly autocorrelated, so persistence is already a decent baseline.
- High wind output pushes prices down (merit-order effect), which shows up clearly in the correlations.

**Next steps:** a full 24-hour day-ahead forecast in one shot, and gas/CO₂ prices as inputs.

Files: `Electricity_Price_Prediction.ipynb`, `energy_dataset.csv`, `weather_features.csv`, report: `Gr_C_Energy_Price_Forecast.pdf`

*University group project (Group C), M.Sc. Smart Energy Systems.*
