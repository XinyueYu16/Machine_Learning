Methods of Error Analysis

- Predicted vs. Residuals vs. True Values Plot:
    - Many of the times, we see a relationship between residuals and the true values, that is **when the true values get higher, the resisuals go up as well**. One possible explanation for this is that **a Polynomial feature** should be considered into the model(as shown in _Applied Predictive Modeling_ p21 "**Introducing some nonlinearity in the model**").
        - When I was making the footfall prediction for mall volume, this relationship is extremely obvious. At that time we used **spline at different intervals of mall-origin distances**, which is close to the **MARS(Multivariate Adaptive Regression Spline)** suggested by *APM p22*. But **MARS** could automatically search for the best place for **Knot**, what you need to hypertune is only **the maximum number of knots** (actually even the degree of interaction) you want on your predictor data.
        - [MARS](https://en.wikipedia.org/wiki/Multivariate_adaptive_regression_spline)
    - Or more simply, there is a undiscovered element should be put into the model.