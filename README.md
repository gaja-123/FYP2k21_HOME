# JLOPSO I: Algorithm Workflow

The Jump Local Optima Particle Swarm Optimization I (JLOPSO I) is an advanced PSO-based algorithm designed for feature selection on high-dimensional data. It improves upon standard PSO by splitting the problem into manageable sub-problems, adaptively adjusting swarm sizes, and incorporating a local search protocol to escape local optima.

For implementation details, refer to the source code (📘 [JLOPSO 1.ipynb](source%20code/JLOPSO%201.ipynb) – Core implementation notebook for Jump Local Optima Particle Swarm Optimization I (JLOPSO I), updated in 2021) in this repository. This workflow enhances efficiency on high-dimensional datasets by promoting diversity and escaping local optima.


## Overall JLOPSO I Feature Selection Process

![JLOPSO I Feature Selection Process](images/Adaptive%20Particle%20swarm%20optimization%20for%20Feature%20Selection%20on%20High%20Dimensional%20Data%20%2C%20a.k.a%20Jump%20Local%20Optima%20Particle%20Swarm%20Optimization%20I%20-%20visual%20selection%20(4).png)  
![JLOPSO I Feature Selection Process](images/Adaptive%20Particle%20swarm%20optimization%20for%20Feature%20Selection%20on%20High%20Dimensional%20Data%20%2C%20a.k.a%20Jump%20Local%20Optima%20Particle%20Swarm%20Optimization%20I%20-%20visual%20selection%20(5).png)  
*(High-level diagram: Sequential steps from feature importance evaluation using SU, feature removal, subspace division, sub-swarm initialization, and iterative optimization with fitness evaluation and local search.)*

The complete step-by-step workflow is as follows:

## Step 1: Feature Space Division (Sub-Problem Splitting)

1. **Calculate Feature Importance**: The algorithm first evaluates the importance of every feature by calculating its Symmetrical Uncertainty (SU) with respect to the class labels. SU measures the correlation between a feature and the class.
2. **Remove Irrelevant Features**: Features with an SU value below a certain threshold (e.g., SU > 0) are considered weak or irrelevant and are removed from the feature set.
3. **Sort and Divide**: The remaining features are sorted in descending order based on their SU values. This sorted set is then uniformly divided into a predefined number of M non-overlapping feature subspaces. This "divide-and-conquer" strategy splits the high-dimensional problem into smaller, low-dimensional sub-problems.

![Feature Space Division Process](images/Adaptive%20Particle%20swarm%20optimization%20for%20Feature%20Selection%20on%20High%20Dimensional%20Data%20%2C%20a.k.a%20Jump%20Local%20Optima%20Particle%20Swarm%20Optimization%20I%20-%20visual%20selection.png)  
*(Diagram: A funnel-shaped flow from calculating SU, evaluating correlations, sorting features, removing irrelevant ones, and dividing into subspaces.)*

## Step 2: Sub-Swarm Initialization and Sizing

1. **Calculate Subspace Importance**: The importance of each feature subspace (FI) is calculated as the average SU of all features within it.
2. **Set Initial Sub-Swarm Sizes**: The initial size (i.e., number of particles) for each sub-swarm is determined proportionally to the importance of its corresponding feature subspace.
3. **Bound Sub-Swarm Sizes**: To manage computational resources, the size of each sub-swarm is constrained within a predefined upper and lower bound.
4. **Initialize Particles**: For each sub-swarm, particles are initialized with random positions. Each particle's best-known position (pbest) is set to its initial position, and the swarm's global best (gbest) is determined.

![Sub-Swarm Initialization Process](images/Adaptive%20Particle%20swarm%20optimization%20for%20Feature%20Selection%20on%20High%20Dimensional%20Data%20%2C%20a.k.a%20Jump%20Local%20Optima%20Particle%20Swarm%20Optimization%20I%20-%20visual%20selection%20(1).png)  
*(Diagram: Stacked blocks showing determination of subspace significance, allocation of particles based on importance, constraining sizes, and positioning particles randomly with initial best positions.)*

## Step 3: Iterative Optimization

The algorithm iterates until a stopping criterion is met. In each iteration, the following steps are performed for every sub-swarm:

1. **Construct Solution & Evaluate Fitness**:
   - For each particle, a complete solution is constructed using an elite combination strategy.
   - This solution is decoded into a feature subset.
   - The *fitness* of the subset is evaluated using a K-Nearest Neighbors (KNN) classifier with Leave-One-Out Cross-Validation (LOOCV). The resulting classification accuracy serves as the fitness value.

2. **Update Best Positions**: If a particle finds a position with better fitness, its pbest is updated. If any particle's pbest is better than the current gbest of the sub-swarm, the gbest is updated.

3. **Local Search Protocol (Local Search I)**:
   - This strategy is invoked if the global best solution (gbest) has not improved for a set number of iterations.
   - It explores the neighborhood of the current gbest by randomly adding potentially relevant features and removing potentially redundant ones to escape local optima. If a better solution is found, the gbest is updated.

4. **Adaptive Sub-Swarm Size Adjustment**:
   - Periodically, the algorithm calculates the *relative convergence and divergence* of each sub-swarm.
   - Based on these metrics, particles may be automatically removed from a converging (less diverse) sub-swarm or new particles added to a diverging (more diverse) sub-swarm.
   - This dynamic adjustment maintains diversity and reduces unnecessary computation.

![Iterative Optimization Cycle](images/Adaptive%20Particle%20swarm%20optimization%20for%20Feature%20Selection%20on%20High%20Dimensional%20Data%20%2C%20a.k.a%20Jump%20Local%20Optima%20Particle%20Swarm%20Optimization%20I%20-%20visual%20selection%20(2).png)  
*(Diagram: A circular flow with nodes for constructing/evaluating solutions, updating best positions, local search, and adjusting sub-swarm size to maintain diversity and efficiency.)*

## Step 4: Final Output and Evaluation

1. **Return Feature Set**: Once the stopping criterion is met, the final gbest from the optimization process represents the selected optimal feature subset.
2. **Validate Performance**: The effectiveness of the selected feature subset is validated using key metrics: number of selected features and classification accuracy. The performance is typically compared against other algorithms like standard PSO, GA, and HHO on multiple classifiers (e.g., KNN and SVM) to ensure robustness.

![Feature Selection and Validation Process](images/Adaptive%20Particle%20swarm%20optimization%20for%20Feature%20Selection%20on%20High%20Dimensional%20Data%20%2C%20a.k.a%20Jump%20Local%20Optima%20Particle%20Swarm%20Optimization%20I%20-%20visual%20selection%20(3).png)  
*(Diagram: A pyramid flow from returning the optimal feature set, validating performance, comparing algorithms, and ensuring robustness across classifiers.)*

## Overall JLOPSO I Feature Selection Process

![JLOPSO I Feature Selection Process](images/Adaptive%20Particle%20swarm%20optimization%20for%20Feature%20Selection%20on%20High%20Dimensional%20Data%20%2C%20a.k.a%20Jump%20Local%20Optima%20Particle%20Swarm%20Optimization%20I%20-%20visual%20selection%20(4).png)  
![JLOPSO I Feature Selection Process](images/Adaptive%20Particle%20swarm%20optimization%20for%20Feature%20Selection%20on%20High%20Dimensional%20Data%20%2C%20a.k.a%20Jump%20Local%20Optima%20Particle%20Swarm%20Optimization%20I%20-%20visual%20selection%20(5).png)  
*(High-level diagram: Sequential steps from feature importance evaluation using SU, feature removal, subspace division, sub-swarm initialization, and iterative optimization with fitness evaluation and local search.)*

For implementation details, refer to the source code (📘 [JLOPSO 1.ipynb](source%20code/JLOPSO%201.ipynb) – Core implementation notebook for Jump Local Optima Particle Swarm Optimization I (JLOPSO I), updated in 2021) in this repository. This workflow enhances efficiency on high-dimensional datasets by promoting diversity and escaping local optima.

🧑‍💻 Authorship & Collaboration This project, titled “Adaptive Particle Swarm Optimization for Feature Selection on High-Dimensional Data”, was carried out by Gajarajan V Y under the guidance of G. Manikandan. The work was completed in collaboration with Chelampalem Sampreeth and Rekkarekula Romeyo, whose contributions supported the development, experimentation, and validation of the proposed algorithm
