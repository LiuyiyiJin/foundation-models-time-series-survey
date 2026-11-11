# Why Time-Series Foundation Models Are Becoming Important

When people hear the words “foundation model,” they usually think about ChatGPT or other language models. I used to connect foundation models mostly with text. However, the same idea is now being used for time-series data, such as stock prices, electricity usage, traffic, weather, and sales records.

The survey paper [Foundation Models for Time Series: A Survey](https://arxiv.org/abs/2504.04011) explains how these models are being designed and compared. The paper does not introduce one completely new model. Instead, it organizes many existing models and explains the choices behind them. After reading it, I think time-series foundation models have a lot of potential, but they are not a perfect replacement for traditional models or smaller machine-learning systems.

## What makes time-series data different?

Time-series data is recorded over time. The order of the data matters. For example, today's electricity usage may depend on yesterday's usage, the day of the week, the weather, and seasonal patterns. A sales record also has a time order because weekends, holidays, and promotions can change customer behavior.

This makes time-series problems different from ordinary tabular prediction. A model cannot simply treat every row as independent. It needs to understand patterns such as trends, repeated cycles, sudden changes, and relationships between variables.

Older methods can work well for simple and stable data, but they may struggle when the data has long-term relationships or many variables. Neural networks helped with some of these problems, and Transformers made it easier to model relationships across long sequences.

## What is a foundation model for time series?

A foundation model is trained before it is used for a specific task. During pretraining, the model sees a large amount of data and learns general patterns. Later, the model can be adapted for a particular task, such as forecasting, anomaly detection, or classification.

This is different from training a new model from the beginning for every dataset. For example, a company may want to predict electricity demand. Instead of building a completely new model only from its own historical data, it may start with a pretrained time-series model and then adapt it to its own data.

The main advantage is flexibility. A pretrained model may already understand some common patterns, such as seasonality or changes in a trend. This can be useful when the new dataset is small or does not have many labeled examples.

## The most important design choices

One thing I liked about this survey is that it does not only list model names. It compares models using several design choices.

### 1. Patches or full sequences

Some models divide a long time series into smaller sections called patches. A patch is similar to a short window of consecutive observations. Instead of processing every single value as a separate token, the model processes groups of values.

This can make long sequences easier and faster to process. It is similar to looking at a video one short clip at a time instead of analyzing every frame separately. Patch-based models can learn local patterns inside each patch and then learn how different patches are connected.

The disadvantage is that the patch size matters. If patches are too large, small changes may be missed. If they are too small, the model may become slower and use more computation.

### 2. One variable or many variables

Some models focus on one time series at a time. This can work for a simple forecasting problem, such as predicting the next values of one stock or one sensor.

Other models handle multivariate data. For example, electricity demand may depend on temperature, humidity, time of day, and previous demand. A multivariate model tries to learn the relationship between these variables.

A model that performs well on one variable does not automatically work well when many variables are added. This is one reason model comparisons should include more than one type of dataset.

### 3. Point forecasts or probability distributions

Some models produce one predicted value. This is called a point forecast. Other models produce a range or probability distribution. These probabilistic predictions can show how uncertain the model is.

For business decisions, uncertainty can be just as important as the predicted value. If a store is planning inventory, it may want to know not only the expected demand but also the chance that demand will be much higher than expected.

Models such as MOIRAI and Lag-LLaMA are discussed as examples that can support probabilistic forecasting. Other models mainly focus on point forecasts. Neither approach is always better. The best choice depends on whether the application needs uncertainty information.

## Models discussed in the survey

The survey covers many different models, and they are not all designed for exactly the same purpose.

MOMENT uses masked time-series reconstruction. Some parts of the input are hidden, and the model learns to recover them. This helps it learn useful representations that can later be used for forecasting, anomaly detection, and classification.

MOIRAI focuses on flexible probabilistic forecasting and can work with multivariate series and different frequencies. This is useful because real-world data does not always arrive at the same frequency. A financial series, a weather series, and a sensor series may have very different time intervals.

TimesFM is mainly designed for forecasting and is an example of a large pretrained model. Tiny Time Mixers takes a different direction by focusing on a much lighter architecture. This shows an important point: a larger model is not automatically the best model for every user. A smaller model may be more practical when hardware, memory, or response time is limited.

TIME-LLM and LLMTime connect time-series forecasting with large language models. Their general idea is to represent time-series information in a way that a language model can process. This is interesting, but it also raises questions about whether a language model is always the right tool for numerical data.

## How should we compare these models?

The survey discusses common evaluation ideas such as MSE and MAE for point forecasting. It also discusses likelihood-based evaluation and prediction intervals for probabilistic forecasting.

However, accuracy should not be the only thing we look at. A model can have a good error score but still be difficult to use because it is expensive, slow, hard to interpret, or unable to handle multivariate data.

Other useful comparison points include zero-shot performance, few-shot performance, long-horizon forecasting, the number of parameters, the size of the pretraining data, and performance on tasks other than forecasting.

One important detail is that this survey does not run one new experiment with the same datasets and settings for every model. It summarizes experiments from the original model papers. Because of this, the results should not be treated as one perfectly controlled leaderboard.

## What about ablation studies?

The assignment asks us to pay attention to ablation studies. In this topic, useful ablation questions include whether patching improves performance, how patch size changes the result, and whether masked pretraining helps with tasks such as anomaly detection.

It is also important to separate the survey from the original model papers. The survey organizes the available evidence, but it does not prove that one architecture or one objective function is always the best. Results from different papers may use different datasets, data processing methods, and evaluation settings.

## My opinion

My main takeaway is that foundation models for time series are promising, but their value depends on the situation. They can save time by providing a starting point, especially when a dataset is small or when the same model needs to support several tasks.

At the same time, large models are not automatically reliable. They still depend on the quality of their pretraining data. A model trained on data that looks very different from a company's data may not transfer well. There are also concerns about computing cost, interpretability, long-term forecasting, and uncertainty.

For example, a model may work well on general electricity data but perform poorly for one city with unusual weather patterns. A company should still test the model on local data instead of trusting a zero-shot prediction immediately.

I also think smaller models deserve more attention. In a real project, a model that is slightly less accurate but much faster and easier to maintain may be a better choice.

## Final thoughts

The main contribution of this survey is its taxonomy. Instead of saying that one model is simply the best, it helps readers ask better questions: Does the model use patches? Does it support multiple variables? Does it provide uncertainty? What training objective does it use? How large is it, and how much data was used to train it?

These questions make it easier to choose a model for a real problem. Time-series foundation models may become an important part of machine learning, but they still need fair benchmarks, better explanations, efficient architectures, and careful testing across different domains.

In my view, the future is probably not one giant model replacing every smaller model. It is more likely to be a combination of pretrained models, domain-specific data, and practical engineering decisions.

## References

- Kottapalli, S. R. K., et al. [Foundation Models for Time Series: A Survey](https://arxiv.org/abs/2504.04011).
- Additional model papers and visual explanations will be added to this repository as the project develops.
