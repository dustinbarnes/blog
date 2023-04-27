> "If you are going to quantify one thing, quantify the cost of delay"  
> -- Donald Reinertsen, lean thought leader

To quantify the cost of delay is to answer the question: “What will be the cost per time unit if we delayed delivery?” You can then compare your answer with the estimate to deliver the feature with the highest cost of delay, and is cheapest to do, first. This is called Weighted Shortest Job First (WSJF) prioritization.

Cost of delay is not necessarily measured in terms of dollars. There are many ways to assess value and cost. Reputation or story points are two examples.

Quantifying cost of delay for any given feature can be challenging. Those who are good at it try to consider how cost of delay changes over time. They commonly use standards, such as the following.

- **Linear** — For every day we do not deliver, we lose some money. A common example of a linear cost of delay is money lost due to competitors already having a feature that you don’t.

- **Fixed date** — If we don’t deliver by a certain date, it’s too late. An example: let’s imagine you’re making New Year cards for 2019. If you don’t deliver them before the end of 2018, the cost of delay is very high. In fact, delivering afterward, in January or February, makes no difference — it is too late!

- **Intangible** — We can delay for now at minimal cost, but eventually it could become expensive. A good example is the cost of delay for fixing a few bugs or refactoring your code. You can skip today, but over time it will make other improvements more expensive and can cause the cost of delay to increase exponentially.

- **Expedite** — It must be done immediately or the cost of delay will grow radically. An example of an expedite feature is a severe bug that renders your product useless to all your customers.

The cost of delay categorization, when compared against cost of implementation, will often give you a good idea of what should be done first.