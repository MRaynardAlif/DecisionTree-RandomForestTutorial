# DecisionTree-RandomForestTutorial
A Tutorial for Decision Tree and Random Forest Model Training

Question:
#  How was the result of both models?
Performance        : 
Both models achieved near-perfect benchmark scores on the test set (99% vs 98% accuracy), successfully classifying the vast majority of themtest samples.

Feature Hierarchy  : 
The Random Forest prioritized the electrical signature (Voltage) as its most critical feature, whereas the single Decision Tree prioritized the environmental signature (Temperature Indoor).

Confusion Parity   : 
Regardless of the architecture (ensemble vs. single tree) or the primary features used, both models encountered the exact same boundary overlaps (In this specific case: Normal/Trouble 3 and Abnormal/Maintenance 1).

# The Hyperparameters Used?
max_depth: None (Used in both models)
This controls the maximum number of levels (or "questions") the tree is allowed to ask before making a final classification. The value 'None' means the tree has no depth limit. It will continue splitting the data over and over until every single "leaf" at the end of the branches is perfectly pure (containing only one class. 

min_samples_split: 2 (Used in both models)
This is the minimum number of data points required in a node before the algorithm is allowed to attempt another split. The value '2' means, as long as a node contains at least 2 data points, the tree will try to find a feature to split them apart. This is the lowest, most aggressive setting possible.

criterion: 'entropy' (Decision Tree)
This is the mathematical function used to measure the quality of a split. Entropy measures the level of impurity or "chaos" in a group of data.

min_samples_leaf: 1 (Random Forest)
This is the minimum number of data points that must end up in a final leaf node after a split is made. The value '1' means a split is legally allowed to happen even if it isolates a single, solitary data point into its own leaf.

n_estimators: 300 (Random Forest)
This is the total number of individual Decision Trees generated to create the "forest" ensemble. The value '300' means the algorithm built 300 separate, fully unconstrained trees, giving each one a slightly different, random subset of your data and features, and then averaged their final votes.

# Top Performer?
In this specific case, the Decision Tree is acting as a fragile, single point of failure that aggressively memorizes the loudest signal. The Random Forest is the clear winner because it achieves slightly higher peak accuracy while maintaining a much safer, distributed approach to evaluating the AC unit's telemetry. The key point is:

1.  Raw Accuracy
On a purely objective level, the Random Forest edges out the Decision Tree on the test set. It achieved a 0.99 (99%) across accuracy, macro average, and weighted average, whereas the Decision Tree plateaued at 0.98 (98%). While a 1% difference might seem small, across a dataset of ~169,000 test samples, that represents nearly 1,700 fewer classification errors.

2.  Stability
The 5-Fold Cross-Validation charts reveal a massive difference in stability. The single Decision Tree is highly volatile. Depending on how the data was split, its accuracy dropped as low as ~79%. The Random Forest, by averaging 300 different trees, acted as a shock absorber. Its lowest cross-validation dip was only ~83.6%. This means the Random Forest is much more reliable.

3.  Feature Robustness
This is the most critical advantage. When deploying models trained on synthetic or highly controlled data into physical environments, bridging that "domain gap" requires robust feature utilization.
    - The Decision Tree is fragile. It places almost 28% of its decision-making weight on a single feature (Temperature Indoor (°C)). In a physical setting, if that one sensor drifts, fails, or encounters unexpected ambient conditions, the model's logic breaks down.
    - The Random Forest relies on a balanced blend of electrical signatures (Voltage) and derived diagnostic parameters (Delta and Alpha). By distributing the importance across multiple physical and electrical dimensions, it is inherently more resistant to single-sensor failures or real-world noise.

# What is Bagging and Random Feature Selection?
1.  Bagging (Bootstrap Aggregating) is the process of creating multiple different datasets from your original training data by randomly sampling it with replacement. Each individual tree in the forest is trained on one of these unique "bootstrapped" datasets.
   
2.  Normally, when a Decision Tree evaluates how to split data at a node, it looks at all available features and greedily picks the absolute best one. In a Random Forest, each time a tree needs to make a split, it is only allowed to choose from a random subset of the features.

# When Decision Tree is a better choice?
If the goal is to push the highest possible accuracy to a cloud server, use a Random Forest. However, if the goal is to build an explainable, lightweight rule set that can be hardcoded directly onto a circuit board, use a Decision Tree.

