## Imperial AI/ML Professional Certificate
### Andrew Thomas-Woolf (752)
November 2024 cohort

# Portfolio Project


#### Changelog
2024-11-26: Initial description from req act 24.4 used as template <br/>
2024-12-04: Update for progress on chosen data set <br/>
2024-12-11: Update for analysis at submission date <br/>


# PROJECT DESCRIPTION
My chosen industry-specific challenge is to identify short-term trading opportunities in a high-frequency trading (HFT) context using normalised market data. This project will focus on the preliminary identification of opportunities, leaving their validation to subsequent steps such as simulation runs or live trading with small risk budgets.

The objective is to design and evaluate a framework capable of producing actionable forecasts of short-term price movements. The system must also be, or be easily made to be, modular and extensible, allowing for the incorporation of additional data sources, features, or targets as needed.

## Why have I chosen this task?
The project aligns closely with my day-to-day activities in systematic trading, specifically within the realm of high-frequency trading to fast medium-frequency trading. It represents an important step towards building a module for inclusion in a framework for short-term price forecasting, integrating into larger infrastructure.

Key alignments include:

- Practical Relevance: The project mirrors the challenges I encounter in my day-to-day work, ensuring that the insights gained have immediate applicability.
- Skills Development: By combining machine learning, optimisation, and efficient data processing, the project will build upon my existing expertise while also exploring new methodologies.
- Future Scalability: The framework is to be designed with scalability in mind, enabling application across multiple venues, asset classes, and data sources.

Through this project, I have deepened my knowledge of machine learning and optimisation while contributing to long-term infrastructure that supports ongoing research around trading activities.

# NON-TECHNICAL DESCRIPTION
The model attempts to capture the intuition a single security, Solana in the project's instance, should in general trade for approximately the same price across different markets, whether purchased in dollars, dollar stablecoins, or financed on a rolling basis. 

These kind of price formation effects occur on a relatively short timescale: the returns included in the model range from 1 millisecond out to 5 seconds prior. The prediction horizon extends out only 15 seconds into the future.

In addition, revealed supply and demand as observed in the order books of different markets in Solana should represent supply and demand able to affect the future price path of Solana as a security broadly, regardless of in which market that supply or demand is revealed. The model seeks to capture the anticipated price impact of revealed supply and demand on the future price Solana.

When evaluating model performance, we considered the following: regression metrics, most specifically r-squared, are useful for evaluating models, but in practical use, one 'cannot eat r-squared'. We need to be able to monetise returns to pay our bills. Accordingly, we evaluated the various models using other, alternative rules in addition to r-squared metrics. A simplest trading rule is to model owning a security when the signal (model prediction) is positive, and model selling short a security when the signal (model prediction) is negative. This gives us a signal-signed return metric: what do we earn if we use the signal to buy or sell such that we're always either 1 unit long, or 1 unit short? This treats the output as a classification: a slightly more advanced evaluation of a models anticipated trading performance would be the signal-weight realised market returns. Here we assume that the stronger our upward(/download) model price forecast, the more securities we will buy(/sell). Also of interest is the risk-normalsed average return of this model: how much do we earn on average per unit of risk (measured as the standard deviation of earned returns)?

All these together were relevant in evaluating and selecting a particular model for use.

# DATA
The dataset consists of preprocessed data for a single security for a period of 4 24 hour trading days running over 2024-10-05 04:01:15.962069 to 2024-10-10 03:51:56.716628 (data is timestamped to local time, Zurich). A recording rollover occurs at ~4am each day leading to ~10 minutes of missing data per day.

The dataset contains the order book states of the SOLUSD, SOLUSDT and SOLF0:USTF0 markets all sampled together every 15+(raondom uniform 0..1) seconds. For each entry, we observe the order book imbalance at the top ten depth levels for each security, and the backward and forward returns for these instruments at various trailing window and forward forecast horizons.

This dataset is intentionally narrow, and generalisation to other instruments will require independent validation. This is acceptable as:
 - in production use, the model will be regularly refit on a rolling basis; and
 - from a research perspective, any insights gained can be explored against other securities to determine the degree of generalisation.

The Analysis.ipynb file does an automated download of the dataset to the local machine. The dataset can be downloaded directly using the link https://drive.google.com/file/d/1NWjfxIDQ1K1shKIWCfmFLhj7AlT8dWg_/view?usp=drive_link

# RESULTS
We compared the performance of:
 - a simple linear regression model
 - an ElasticNet model, using Optuna for hyperparameter tuning; and
 - a LightGBM gradient-boosted decision tree model, using Optuna for hyperparameter tuning.
 
The chosen model for use on a more comprehensive dataset is the LightGBM model using Optuna for hyperparameter tuning.

The decision was made not only on typical ML prediction performance metrics, but also but also with reference to domain-relevant metrics, such as prediction-weighted realised performance, and risk-normalised performance.

See Analysis.ipynb notebook contained herein for further details.
