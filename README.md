1. Introduction
1.1 Supervised vs. Unsupervised Learning
Supervised learning trains a model on labeled data each input has a known correct output. The model learns by minimizing the difference between its predictions and the true labels. Examples: image classification, spam detection, price prediction.
Unlabeled data is used in unsupervised learning. The model must find hidden structure on its own, there are no right answers. Examples include dimensionality reduction (PCA) and consumer segmentation (clustering).

	Supervised	Unsupervised
Data	Labeled (input + correct output)	Unlabeled (input only)
Goal	Predict output for new inputs	Discover patterns/groupings
Examples	Classification, Regression	Clustering (K-Means), PCA
Evaluation	Clear — loss/accuracy	Harder to measure objectively

1.2 Loss Functions
A loss function measures how wrong the model's predictions are. During training, the optimizer adjusts weights to minimize this value. A lower loss means better predictions.
•	Mean Squared Error (MSE): Used for regression. Penalizes large errors more heavily.
•	Cross-Entropy: Used for classification. Measures difference between predicted probability and true label.
In TensorFlow Playground, both training loss and test loss are shown in real time, allowing you to see if the model is learning or diverging.

1.3 Parameter Tuning
Parameter tuning is the process of adjusting settings that control how a model learns, in order to improve its performance.
•	Learning Rate: How large each weight update step is. Too high = unstable; too low = very slow.
•	Neuron Count: More neurons = more capacity to learn complex patterns.
•	Hidden Layers: Deeper networks can learn hierarchical features, but are harder to train.
•	Activation Function: Introduces non-linearity (Tanh, ReLU) so the model can learn curved boundaries.
•	Regularization: Penalizes large weights (L1/L2) to reduce overfitting.

1.4 Overfitting and Underfitting
Overfitting: The model learns the training data too well, including its noise, and performs poorly on new data. Training loss is very low but test loss is much higher.
•	Underfitting: The model is too simple to capture the data patterns. Both training and test loss remain high.
•	Overfitting fix: Add regularization, reduce model complexity, or use more training data.
•	Underfitting fix: Increase model complexity, add more neurons/layers, or use non-linear activations.
 
2. Simulation Experiments
All experiments were performed in TensorFlow Playground using the XOR dataset, 50/50 train/test split, LR = 0.03, and Tanh activation unless stated otherwise.

2.1 Experiment A — Network Architecture (Hidden Layers & Neurons)
No Hidden Layers (0 layers) Underfitting
With 0 hidden layers and only raw X1, X2 inputs, the model is a simple linear classifier. It cannot learn the non-linear XOR boundary. Both training and test loss stayed very high even after 153 epochs, confirming classic underfitting.

 

Figure: 0 Hidden Layers — Linear boundary fails on XOR dataset. Test loss 0.001 only due to accidental linear separation, Training loss 0.000. Epoch 153.

1 Hidden Layer, 1 Neuron — Underfitting
With only 1 neuron in the hidden layer, the model still lacks the capacity to form a meaningful non-linear boundary. After 757 epochs, test loss = 0.399 and Training loss = 0.378 — both high, confirming the model is underfitting and cannot capture the XOR pattern.

 

Figure: 1 Hidden Layer, 1 Neuron — Both losses ~0.38–0.40. Model cannot capture the XOR pattern. Epoch 757.

1 Hidden Layer, 2 Neurons — Weak Fit
Adding a second neuron provides slightly more capacity. A curved but imprecise boundary begins to form. After 438 epochs, Test loss = 0.211, Training loss = 0.180 — still not accurate enough, but better than 1 neuron.

 

Figure: 1 Hidden Layer, 2 Neurons — Diagonal-ish boundary. Test loss 0.211, Training loss 0.180. Epoch 438.

1 Hidden Layer, 4 Neurons — Adequate Fit (Baseline)
Four neurons provide adequate capacity to learn the XOR pattern. A recognizable rectangular decision boundary emerges. After 1,692 epochs, Test loss = 0.140, Training loss = 0.069. This is the standard baseline configuration.

 

Figure: 1 Hidden Layer, 4 Neurons — Baseline adequate fit. Test loss 0.140, Training loss 0.069. Epoch 1692.

1 Hidden Layer, 8 Neurons — Good Fit
Eight neurons give the model significantly more representational power. The XOR boundary becomes sharp and accurate. After just 493 epochs, Test loss = 0.021, Training loss = 0.004 — a major improvement over 4 neurons and converged much faster.

 

Figure: 1 Hidden Layer, 8 Neurons — Sharp XOR boundary. Test loss 0.021, Training loss 0.004. Epoch 493.

 
2.2 Experiment B — Increasing Network Depth (2 Hidden Layers)
2 Hidden Layers, 2+1 Neurons — Insufficient Deep Network
Adding a second hidden layer but keeping neuron counts very low (2 then 1) still produces a poor result. The network is technically deep but under-parameterized. Test loss = 0.222, Training loss = 0.179 after 679 epochs — worse than 1 layer with 8 neurons.

 

Figure: 2 Hidden Layers, 2+1 Neurons — Under-parameterized deep network. Test loss 0.222, Training loss 0.179. Epoch 679.

2 Hidden Layers, 2+2 Neurons — Struggling
Slightly more capacity with 2+2 neurons across two layers. Still underperforming. Test loss = 0.298, Training loss = 0.265 after 760 epochs. Network depth alone does not compensate for insufficient neurons per layer.

 
Figure: 2 Hidden Layers, 2+2 Neurons — Still struggling to define XOR boundary. Test loss 0.298, Training loss 0.265. Epoch 760.

2 Hidden Layers, 3+2 Neurons — Excellent Fit
Increasing to 3 neurons in layer 1 and 2 in layer 2 produces an excellent result. The XOR boundary is sharp and correct. Test loss = 0.007, Training loss = 0.002 after just 238 epochs — the fastest and most accurate result across all experiments.

 

Figure: 2 Hidden Layers, 3+2 Neurons — Excellent XOR fit. Test loss 0.007, Training loss 0.002. Epoch 238.

2 Hidden Layers, 4+2 Neurons — Best Overall
The 4+2 two-layer architecture achieves the best balance of accuracy and generalization. Test loss = 0.012, Training loss = 0.001 at epoch 340. Adding the second layer with adequate neurons allows the model to learn hierarchical features more efficiently than a single wider layer.

 
Figure: 2 Hidden Layers, 4+2 Neurons — Best overall result. Test loss 0.012, Training loss 0.001. Epoch 340.

3. Conclusion
The TensorFlow Playground experiments visually confirmed how architecture choices and parameter settings directly control model performance. The key findings are summarized below:

Configuration	Result	Key Observation
0 hidden layers	Failed	Cannot learn non-linear patterns — needs hidden layers
1 layer, 1 neuron	Underfitting	Loss ~0.40 — too simple, no meaningful boundary formed
1 layer, 2 neurons	Weak	Some learning but loss still ~0.21 — insufficient capacity
1 layer, 4 neurons	Adequate	Baseline fit — recognizable boundary, loss ~0.14/0.07
1 layer, 8 neurons	Good	Sharp boundary, fast convergence — loss drops to 0.021
2 layers, 2+1 neurons	Poor	Deep but under-parameterized — depth alone not enough
2 layers, 3+2 neurons	Excellent	Fastest convergence (238 epochs), test loss 0.007
2 layers, 4+2 neurons	Best	Best generalization — test loss 0.012, training 0.001

The most important insight from these experiments is that both depth (number of layers) and width (neurons per layer) must be considered together. A deep network with too few neurons underperforms a shallow but wider network. The sweet spot 2 layers with 3–4 neurons in the first layer and 2 in the second consistently produced the best results on the XOR classification task.
Underfitting was clearly observed when the model had 0 or 1 neurons in the hidden layer: both training and test loss remained high, and the decision boundary was too simple. Overfitting risk increases when using very high neuron counts without regularization, which would show as a large gap between training and test loss.
These experiments confirm that supervised learning's power lies in having a measurable loss signal guiding training. Parameter tuning especially architecture design, is an iterative process of balancing model capacity against the complexity of the task.
















References

1.	Google Brain / TensorFlow Team. (n.d.). TensorFlow Playground. TensorFlow. https://playground.tensorflow.org
2.	IBM. (2024). Supervised vs. Unsupervised Learning: What's the Difference? IBM Think Blog. https://www.ibm.com/think/topics/supervised-vs-unsupervised-learning
3.	IBM. (2024). Underfitting and Overfitting in Machine Learning. IBM Think Blog. https://www.ibm.com/think/topics/underfitting
4.	3Blue1Brown. (2017). But what is a Neural Network? [Video]. YouTube. https://www.youtube.com/watch?v=aircAruvnKk
5.	StatQuest with Josh Starmer. (2019). Machine Learning Fundamentals: The Loss Function Explained [Video]. YouTube. https://www.youtube.com/watch?v=OtdnFHUOvRo
6.	Geron, A. (2022). Hands-On Machine Learning with Scikit-Learn, Keras, and TensorFlow (3rd ed.). O'Reilly Media.
7.	Goodfellow, I., Bengio, Y., & Courville, A. (2016). Deep Learning. MIT Press. https://www.deeplearningbook.org
8.	Anthropic. (2024). Claude AI — Generative AI Platform for Concept Explanation. Anthropic. https://www.anthropic.com

