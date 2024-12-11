# LGBM Short-term SOLUSD Price Return Model Card

This model is designed to predict the 15 seconds price returns of the SOLUSD security traded on BitFinex.

By examing features such as recent returns on the security itself and returns of tightly-coupled markets, namely the stablecoin-denominated SOLUSDT market and the perpetual future SOLF0:USTF0 market, and various imbalances within the order books of all of these securities, we seek to anticipate future price changes as a combination of correlated security performance and anticipated future supply and demand as revealed in the relevant order books.

# MODEL DESCRIPTION
Model inputs include:
 - The recent returns of the security itself (SOLUSD, identifable as 'cash' in the feature-naming convention), its stablecoin-denominated counter-party (SOLUSDT, identifable as 'stable' in the feature-naming convention) and the related perpetual future (SOLF0USTF0, identifiable as 'perp' in the feature-naming convention). These returns cover the periods of 1, 2, 3, 5, 10, 20, 50, 100, 1000 and 5000 milliseconds into the past.
 - The size-weighted imbalance, down to ten depth levels, between the revealed demand (bid) and supply (offer) in the order books of the same three instruments.
 
The model output is:
  - The forecast 15 second return of SOLUSD
  
# PERFORMANCE

The realised R-square of the LGBM model in hold-out test was approximately 46bps (0.46%). This compares favourable with a baseline linear regression model performance of 26bps (0.26%) on the same train/val and test sets, but would be considered low even for financial market prediction tasks.

Similarly, the signal-weighted return (normalised such that the average magnitude of the signal is 1) for the LGMB model was 3417bps with an annualised Sharpe ratio of 1.25, compared to the basline linear regression model's 2788bps with an annualised Sharpe ratio of 1.04.

# LIMITATIONS

 - Generalisation:
    - the model is trained only on SOLUSD and its related SOLUSDT and SOLF0:USTF0 markets on a single trading venue, BitFinex, ergo use with other security sets and with other venues is not provided for.
    - the model would not be expected to retain performance significantly beyond the training period in October 2024, as financial markets are highly non-stationary and continuously evolving.
 - Feature choice:
    - the model makes use of a very limited set of input features, that should not be considered sufficient for trading with live capital.
    - while the chosen features represent long-standing empirically validated inputs, there is no guarantee of their utility broadly and on an on-going basis.
 - Limited absolute magnitude of forecasts:
    - the absolute magnitude of forecasts generated is small, and should not be used to trade where transaction costs will be incurred without careful evaluation.
 
# TRADE-OFFS
 - Interpretability:
    - the LGBM model offers improved predictive performance but at a loss of simple interpretability: no obvious model coefficients can be manually inspected, but tools (tree-explainers) can be used to discern some details about the model.
 - Inference time:
    - the LGBM model involves extensive compare-and-branch decisions, depending on the depth of the trees and the number of boosting rounds. Typically performance of GBDT is measured in the 10's to 100's of milliseconds, whereas linear model multiple-and-add computations can be done well within 1 millisecond.
    