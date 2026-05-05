The script begins by importing several essential machine learning utilities from
scikitlearn, along with numerical and datahandling libraries such as NumPy and pandas. Because
NSLKDD contains both categorical features (protocol types, services, flags) and numerical
network statistics (counts, rates, durations), preprocessing is essential for the models to learn
effectively. The script prompts the user to upload their CSV files via Google Colab, reads them
into memory, and automatically detects the correct label column. It then converts all attack labels
into a binary form in which “normal” traffic is given the label 0 and all forms of malicious traffic
are assigned the label 1. This transformation allows the models to frame intrusion detection as a
binary classification problem which is ideal for computing metrics such as precision, recall, and
ROCAUC.
Once the label conversion is complete, the program constructs preprocessing pipelines for
both numerical and categorical features. Numerical columns undergo imputation to fill missing
values and are then standardized so that all features share a similar scale. Categorical fields are
one hot encoded, transforming text based protocol names and service identifiers into numerical
vectors that the learning algorithms can interpret. The resulting processed feature matrix is then
split into training and test sets using a stratified approach, ensuring that both sets preserve the
original class distribution. This is important because the NSLKDD dataset is imbalanced, with
far fewer normal samples than attack samples.
The next phase of the script trains multiple machine learning models. These include
Logistic Regression, Linear Support Vector Machine (SVM), Decision Tree, XGBoost (if
available), and a Stacking Ensemble composed of several of the base models. Each classifier is
trained on identical training data, and each model is evaluated on the test data using accuracy,
precision, recall, F1score, and AUC. These metrics offer different perspectives on model
performance. Accuracy measures the overall correctness of the classifier, but in intrusion
detection, precision and recall are significantly more important. Precision captures how many
flagged intrusions were actually attacks, where recall represents how many true attacks the
system successfully detected. F1 is a harmonic mean that balances both. ROCAUC provides an
aggregate measure of performance across all threshold choices and is often the most informative
metric for binary classification under class imbalance.
The results of this project are shown both numerically and in the ROC plot. The summary
table reports that Logistic Regression and SVM both achieved an accuracy of approximately 0.73
and nearly identical precision, recall, and F1 scores. This consistency suggests that both linear
models learned similar decision boundaries within the featuretransformed space. Decision Tree
performed substantially better across metrics, with an accuracy of 0.875 and a recall of 0.757.
This suggests that the nonlinear splitting behavior of the tree structure is particularly well suited
to capturing the complex interactions between categorical and numerical variables in the
NSLKDD dataset.
XGBoost delivered high performance with an accuracy of approximately 0.879 and an
F1score near 0.847. This is expected, as gradientboosted trees often excel at structured tabular
data such as network logs. The Stacking Ensemble, which integrates predictions from Logistic
Regression, SVM, Decision Tree, and optionally XGBoost, produced accuracy and F1 results
very close to those of the best individual models. Its F1score of about 0.844 and AUC of 0.873
show that the ensemble was able to combine the strengths of simpler and more complex models
to achieve balanced and reliable performance.
The ROC curves show the tradeoff between the true positive rate and the false positive
rate for each classifier across every possible decision threshold. In the plot, Logistic Regression
and SVM follow nearly identical ROC curves, reflecting their similar linear separating planes.
Both achieve an AUC of approximately 0.788, meaning they capture useful structure in the data
but fail to capture deeper nonlinear patterns.
DecisionTree shows a noticeably
more curved and elevated ROC line,
corresponding to an AUC of roughly
0.864. This indicates greater capability in
distinguishing attack from normal traffic
across various thresholds. XGBoost and
the Stacking Ensemble achieve the
strongest ROC curves in the figure, with
AUC values near 0.872 and 0.873
respectively. These models hug the upper
left portion of the graph, representing a
high truepositive rate achieved alongside
a relatively low falsepositive rate. This is
exactly the behavior desired in an
intrusion detection context: maximizing
attack detection while minimizing false
alarms that could overwhelm analysts.
The nearly overlapping ROC curves of XGBoost and the Stacking Ensemble demonstrate
the effectiveness of treebased and hybrid methods for cybersecurity classification tasks. Their
superior performance indicates that nonlinear, boosted, or combined classifiers capture the
nuances of network traffic more effectively than linear or shallow models. The results also
highlight that even simple models like Logistic Regression perform reasonably well, which is
encouraging for lowresource or realtime deployment scenarios.
Overall, the script serves as a comprehensive demonstration of how different machine
learning methods behave on a realistic intrusion detection dataset. It shows the full process from
raw data to trained models, and most importantly, the results both numerical and graphics offer
clear insight into the strengths and weaknesses of each approach. The ROC plot is central to
interpreting these findings, revealing the superior discriminative power of nonlinear models and
ensembles. This comparison not only guides model selection for practical cybersecurity
applications but also illustrates how statistical learning can meaningfully enhance network
security monitoring systems.
