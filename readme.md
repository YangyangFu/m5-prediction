# M5 competition


## Data Pipeline

### Features
We categorize the features of a timeserie prediction problem into three aspects:
1. global covariates: covariates that are indepedent of the time series themselves, such as time features, time series covariates (e.g., to predict time series $y_1$, we might use $y_2$ as a covariate). 
    - `time_features`: time features based on time series data time index. 
    - `cat_cov_global`: categorical global features other than time features such as external features like quarterly financial report status if we want to include. Here we dont have any such features in M5.
    - `num_cov_global`: numerical global features other than time features such as Dowjones index if we want to include. Here we dont have any such features in M5.
2. local covariates: covariates that are dependent on the time series themselves, such as the attributes of the time series (e.g., index of the series in a multi-variate prediction problem, store_id of the product prices etc). 
    - `cat_cov_local_variant`: local time-variant categorical covariates, whose raw data shape is $(N, M)$, such as the SNAP status of the state the store is located, etc.
    - `num_cov_local_variant`: local time-variant numerical covariates, whoe raw data shape is $(N,M)$,  such as the price of an item.
    - `cat_cov_local_invariant`: local time-invariant categorical covariates, whose raw data shape is $(M, L_c)$, where $M$ is number of time series, $L_c$ is the number of time-invariant categorical covariates. In M5, this could be the `id`, `store_id`, `state_id` for each time series.
    - `num_cov_local_invariant`: local time-invariant numerical covariates, whose raw data shape is $(M, L_n)$.

We design these features so that we can use time-windows to generate look-backs and prediction horizons of `time_features`, `cat_cov_global`, `num_cov_global`, `cat_cov_local_variant`, and `num_cov_local_variant`.

### Data Loader
The batch design of a data loader has two different approaches:
1. batch on time steps: e.g., the time series window time could then be $(B, M, L+H)$, where $B$ is a batch size, and the batch is taken on time step index, $M$ is the number of time series to be predicted, $L$ is the history length, $H$ is the prediction length.
2. batch on number of time series: the time series window could then be $(B, L+H)$, where $B$ is taken on $M$, thus $B <= M$.

