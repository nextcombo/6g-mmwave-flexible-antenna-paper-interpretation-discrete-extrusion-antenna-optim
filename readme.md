# 6G mmWave Flexible Antenna Paper Interpretation: Discrete Extrusion Antenna Optimal Activation Three-Layer Tree Search Algorithm Full Analysis [Attached MATLAB Code]

6G Millimeter Wave Flexible Antenna Paper Interpretation: Full Explanation of the Discrete Pressure Antenna Optimal Activation Three-Layer Tree Search Algorithm
Introduction: From 5G millimeter wave to 6G terahertz communication, high-frequency communication has always faced two core challenges: significant path loss and severe shadowing. Traditional fixed array antennas are constrained by their physical location and cannot dynamically adjust the receiving aperture according to user distribution, greatly limiting system throughput in high-frequency scenarios. In recent years, the pinching antenna system (PASS) as the core solution for flexible reconfigurable antennas, is regarded by the industry as the key hardware architecture for 6G integrated communication and large-scale millimeter wave base stations.
There have been numerous studies on continuous adjustable pinching antennas, but at the engineering implementation level, mechanical, electrical, and magnetic control structures can only support limited discrete point activation, and continuous sliding schemes are almost unusable for commercial applications. Meanwhile, existing research on discrete pinching antenna activation mostly focuses on downlink transmission, with an open gap in optimal activation theory for maximizing multi-user rate and low complexity algorithms for uplink.
1. Research Background: Opportunities and existing research gaps in the flexible pinching antenna PASS
1.1 Flexible reconfigurable antennas: The core track of next-generation wireless communication
All elements of traditional phased array antennas are fixed on the substrate, with the array aperture and receiving position permanently unchanged. When users are concentrated on one side of the cell or when high-frequency signals are blocked by buildings, the fixed array cannot avoid deep fading, significantly reducing spatial multiplexing gain. To address this defect, the academic community proposed three flexible antenna solutions: fluid antennas, moveable antennas, and pinching antennas, which core logic is that the radiation point position of the antenna can be dynamically reconstructed to redefine the spatial characteristics of the wireless channel.
The pinching antenna system PASS is currently the most promising solution for engineering implementation, consisting of two main components:
1. Dielectric waveguide: laid on the side of the base station, extending horizontally to cover the entire service area, responsible for transmitting radio frequency signals;
Pinching Antenna (PA): A low-cost dielectric particle, mechanically pinched and electrically controlled to adhere at any point on the waveguide. The adhesive area forms a radiation / reception antenna, which remains non-radiating after release.
Akin to the piano keys, easier to understand: The medium waveguide is like the piano's long resonance board, and the compressed particles are the keys. Pressing the corresponding key (activating PA) will produce an antenna reception point at a specified location, which becomes inactive when released. At any given moment on a waveguide, only one compressed antenna can be activated, this is a rigid constraint from the hardware level of PASS.
Compared with fluid antennas and mechanical sliding arrays, PASS has three unique advantages:
1  
Large-scale reconfigurable aperture: The medium waveguide can extend dozens of meters, and the array's adjustable range is far beyond that of traditional fixed arrays with a few centimeters in size.
2  
Adapts to millimeter wave / terahertz high frequency: High-frequency line-of-sight propagation is the main mode, dynamically adjusting the receiving point can significantly reduce free space path loss and avoid occlusion.
3  
Low Hardware Costs: There are no expensive RF switches for medium particles or waveguides, making the cost of large-scale deployment significantly lower than traditional active phased arrays.
1.2 Four key shortcomings in current research (the core motivation of this paper) Existing PASS-related papers have covered scenarios such as uplink and downlink transmission, NOMA, and integrated sense-and-act. However, there is a significant issue of theoretical disconnect from engineering, which is the research gap that needs to be filled in this paper:
1. Shortcoming 1: A large number of literature assumes that PA can slide along the waveguide continuously, and in practical engineering research, researchers default to assuming that the squeezed antenna can select any real-valued coordinates on the waveguide, solving for the optimal position through continuous optimization. However, in reality, electromechanical rails and mechanical pressing devices can only preset a limited number of discrete positions, and each waveguide can only activate one of L preconfigured positions, making it impossible to directly solve the problem with continuous optimization. The issue of uplink antenna activation optimization under discrete point constraints lacks complete modeling and solution methods.
Weakness 2: Discrete PA activation research focuses on downlink, with virtually no studies in uplink multi-user scenarios. Upon reviewing existing literature, it is evident that discrete antenna selection and beamforming optimization are entirely dedicated to the downlink transmission from base stations; whereas, the uplink transmissions from user terminals and base station receptions, along with the problem of discrete PA activation for multi-user MIMO and rate maximization, receive almost no systematic research. The models for uplink and downlink channels, optimization objectives, and matrix update logic are completely different, making it impossible to directly reuse downlink algorithms for uplink.
Weakness 3: The existing discrete activation algorithms only pursue sub-optimal feasible solutions, lacking a global optimal benchmark. Most existing antenna selection algorithms are heuristics such as greedy and random search, which can only output approximate optimal solutions and cannot prove the global optimality of the solution. There is a lack of a benchmark algorithm that can output the optimal activation vector, leading to the inability to quantify the performance loss of various sub-optimal schemes and a lack of a unified performance metric within the field.
Weakness 4: Lack of Performance - The optimal brute-force search with adjustable complexity has exponentially increasing complexity when the number of M and candidate points L is increased, making it unusable for small-scale systems; the greedy algorithm has extremely low complexity but its performance loss is unstable. There is a lack of a compromise search scheme within the field that can freely adjust complexity and smoothly approximate the optimal solution.
1.3 Overview of Core Innovations in this Paper
1  
Greedy Search: GS only retains a single path at each layer, which has the lowest complexity. It avoids recursive inversion by using Sherman-Morrison matrix recursion, suitable for real-time lightweight deployment of base stations.
2  
Beam Search: Each layer retains B optimal local paths, with beam width B as tuning parameter, balancing the complexity of free-balanced algorithms with system and rate, bridging the performance gap between greedy and optimal algorithms.
3  
Branching Boundary Optimization BnB: Constructs a monotonic transformation objective function to achieve invalid subtree pruning without performance loss, significantly reducing the search space under the premise of no performance degradation. It outputs the global optimal activation vector as the performance benchmark for all algorithms.

At the same time, the article thoroughly derives the computational and storage complexities of three types of algorithms, builds a 28GHz millimeter-wave line-of-sight simulation platform, quantifies the sum rate and delay of traditional fixed arrays, GS, BeS, and BnB, and fully verifies the performance gain of discrete squeeze antennas and the engineering practicality of three sets of algorithms.

System Model: Complete Mathematical Modeling of Uplink Multi-User Discrete Squeeze Antennas
This section strictly replicates the original derivation logic, dissecting from hardware deployment, internal and external channels, received signal, and uplink sum rate optimization objectives layer by layer. All formulas are accompanied by simple explanations in plain language, connecting the physical hardware to the complete link of mathematical optimization.

2.1 Base station and user hardware deployment geometric model: The base station side is deployed with M parallel dielectric waveguides, serving K single antenna users, all users distributed across a rectangular service area in the xy plane:
1  
User coordinates: The position vector of the kth user;
2  
Waveguide layout: M waveguides are arranged along the y axis at equal intervals, with a fixed height h, and the coordinates of the feeding points of the mth waveguide;
3  
Discrete point constraints: Each waveguide presets L fixed squeeze antenna candidate positions, corresponding coordinates;
4  
Activation vector definition: Represents the activation of the mth waveguide's selected candidate position, with only one PA activated per waveguide.
The total number of feasible solutions for the entire system is, when M=8 and L=16, the feasible vector reaches the level of 43 billion, making brute force exhaustive search completely unfeasible, which is the fundamental reason why this paper needs to design tree search pruning algorithms.
2.2 Dual-level cascading channel models (Free space channel + Waveguide internal transmission channel) The end-to-end channel of PASS consists of two cascading segments: the external free space line-of-sight channel from users to the squeeze antenna, and the internal transmission channel from the squeeze antenna to the feeding point of the dielectric waveguide. The total channel gain is the product of the two segments.
(1)External Visible Range Channel System: Facing millimeter wave high-frequency scenarios, line of sight (LoS) propagation dominates. Free space channel formula: Parameters in layman's terms:
- : The free space channel constant, c is the speed of light, carrier frequency, the higher the frequency, the smaller the η, corresponding to greater natural path loss for millimeter waves;
- : Wave number, λ is the free space wavelength;
- : Phase term, determined by the spatial distance between the user and PA;
- Denominator: Amplitude attenuation due to distance, the farther the distance, the lower the channel gain.

(2) Transmission Channel within a Guided Waveguide: Radio frequency signals from the squeezed antenna travel along the dielectric waveguide to the feed point, experiencing both dielectric attenuation and phase shift due to guided wave propagation: Parameters in layman's terms:
- : Dielectric waveguide attenuation coefficient, unit dB/m, the further away from the feed point, the greater the signal attenuation;
- : Guided wave wavelength, effective refractive index of the dielectric waveguide;
- Exponential term: Extra phase rotation due to internal transmission within the waveguide.

(3) Complete End-to-End Channel: When selecting the mth guide at point p, the total channel for user k to base station m receiving channel: Channel matrix definition:, where each element represents a user channel selected at a certain point on the guide.
2.3 Uplink Received Signal Model and System and Rate Expression (1) User Transmitted Signal: User k transmits a signal with power, normalized complex Gaussian information symbol, and users' symbols are independent of each other.
(2) Base Station Received Vector:
- User power diagonal matrix;
- User symbol vector;
- Noise, additive white Gaussian noise (AWGN) at the base station receiver channel.
Normalized equivalent channel matrix for simplified calculation: normalizes the noise to have unit variance, eliminating the interference of noise power on optimization solution.
(3) Uplink Multi-User MIMO and Rate Core Formula Base Stations Utilize Optimal Multi-User Joint Decoding, Total System Spectral Efficiency (Rate, in bps/Hz): The core formula's physical significance is that the determinant represents the spatial multiplexing freedom of a MIMO system. The stronger the orthogonality of the channel matrix H, and the higher the channel gain, the larger the numerical value of the determinant, leading to a higher overall transmission rate. The optimization objective essentially involves selecting a set of discrete PA points to maximize the numerical value of the determinant.
2.4 Discrete Extrusion Antenna Activation Optimization Mathematical Problems Integrating Hardware Discrete Point Constraints, the complete optimization problem is written in the standard integer combination optimization form: interpretation of constraints: each waveguide m can only select one from L preset points, all variables are discrete integers, which belongs to an NP-hard combinatorial optimization problem.
To facilitate tree-based search deduction, define the channel row vector corresponding to the ℓth candidate position under a single waveguide m: Each waveguide corresponds to a set of candidate row vectors. The optimization problem is equivalent to selecting one row from each of M sets and forming matrix H, maximizing it, which essentially is a multi-subset single-row selection combination optimization problem.
III. Optimization Theory Based on Hierarchical Tree Search (Full Text Core Theory Chapter) The innovative approach in this paper: Mapping the discrete antenna selection combination optimization problem into an M-layer tree search structure, achieving rapid local path rate calculation through matrix determinant recursion and matrix rank one update. On this basis, three sets of algorithms - GS, BeS, BnB - are derived. This section thoroughly dissects three core mathematical tools: tree search fundamental theory, the recursive calculation of determinants, and Sherman-Morrison matrix updates.
3.1 Definition of Hierarchical Search Tree Structure 1
Number of layers: A total of M layers, with layer number m corresponding to the mth medium waveguide.
2  
Root node: 0 layer, no waveguide PA selected, initial and rate, initial matrix;
3  
Middle node: The nth layer node corresponds to the local activation vectors of the first n waveguides, known as the local path.
4  
Leaves nodes: The complete path of the Mth layer, corresponding to a set of feasible activation vectors, with the objective function being the total uplink and downlink rate of the system.
Branching rules: Any node at the nth layer extends L branches downwards, corresponding to L candidate positions for the n+1th waveguide.
For example, with M=2 and L=3, there are two layers of trees, with 3 1st-layer nodes branching out from the root, each of which then branches into 3 leaves, totaling 9 sets of feasible solutions, perfectly matching.
3.2 Local Path and Rate Recursion Formula (Sylvester's Inverse Matrix Equality Derivation) Assume that the local channel matrix for the first n waveguides is selected. The local sum rate: If, on top of the local path for the nth layer, a candidate row vector is chosen for the n+1th waveguide, resulting in an n+1-dimensional matrix.
Utilize Sylvester's Inverse Matrix Equality to perform algebraic transformations, splitting out new rate increment terms: including the auxiliary inverse matrix.
Define the single branch rate increment (the rate increase brought by adding a new waveguide): This recursion formula is the foundational building block for three tree search algorithms, possessing two major engineering values:
1  
No need to recalculate high-order determinants every time; based solely on the parent node's local rate addition, significantly reducing computational load;
2  
The increment can directly measure the value of a potential PA point location: The larger the increment, the more it represents a new spatial degree of freedom, leading to higher system rate gains.
3.3 Sherman-Morrison rank-one matrix update (Avoiding redundant matrix inversion) When the optimal branch for the local path is selected, it is necessary to update the inverse matrix used for the next layer. Directly inverting this would result in exponentially increasing computational complexity and power consumption as the number of users K increases.
This paper adopts the Sherman-Morrison first-order correction formula, updating only through parent nodes without direct inversion operations: The update operation only includes vector inner product and matrix vector multiplication, reducing complexity to a key mathematical tool for the low-complexity implementation of the entire algorithm.
3.4 Tree search algorithm unified execution logic all three types of algorithms share the same basic traversal process, the only difference is in each layer node retention and pruning strategy:
1  
Initialize root node: ,,;
2  
Traverse m=1~M, for each surviving local path on each layer, traverse all L candidate positions in the current waveguide.
3  
Calculate the rate increment of each branch, update the local rate and the local inverse matrix G;
4  
Filter / crop local paths according to algorithm rules, retaining nodes that will enter the next layer.
5  
Upon reaching the leaf nodes on M layer, record the activation vector corresponding to the global maximum and rate.
IV. Complete Derivation and Detailed Explanation of Three-Class Tree Search Algorithms
Underlying logic: The incremental representation represents the new degrees of freedom that this PA point can supplement under the current remaining channel space. The greedy algorithm selects the point with the highest current gain at each step, aiming for immediate maximization of returns.
4.1.2 Complete Algorithm Process (Original Algorithm 1) 1
Initialization: Zero the activation vector, initialize the inverse matrix, and set the initial local rate;
2  
Loop through each waveguide m=1 to M: (1) Calculate the rate increment for each candidate position of L in the current layer's parent node; (2) Select the candidate index with the maximum increment; (3) Update the cumulative rate; (4) Use the Sherman-Morrison formula to update the inverse matrix.
3  
All waveguide traversals are completed, outputting the complete activation vector and total uplink and rate.
4.1.3 Analysis of Advantages and Disadvantages:
1  
Low complexity: Each layer only traverses L candidate nodes, total access nodes, overall computational complexity, storage needs only 1 matrix, memory overhead.
2  
Real-time, can be deployed directly at the edge of base stations for real-time scheduling;
3  
The output solution can be used as an initial feasible solution for branch and bound BnB, tightening the pruning threshold significantly reduces the number of nodes searched by the optimal algorithm.
Deficiency: Local optimal does not imply global optimal. Selecting a point with a very large increment may occupy the unique orthogonal channel dimension, leading to subsequent waveguides without effective spatial gain, ultimately resulting in lower global and rate than the optimal value. Simulation shows that the performance of GS is usually about 3% lower than the optimal value, and the loss will be enlarged when the user distribution is extremely uneven.
4.2 Beam Search (Beam Search): A Balanced Solution with Tunable Performance and Complexity
When B=1, BeS completely degenerates into greedy GS.
When B is large enough, it equates to a full-scale exhaustive search, outputting the optimal solution.
The B-value can freely balance the power loss and system rate, making it the most flexible compromise algorithm for engineering implementation.
4.2.2 Complete Algorithm Process (Original Algorithm 2) 1
Initialize the path set, storing format (local activation vector, local inverse matrix G, local rate R);
2  
Iteratively looping m=1~M: (1) Initialize an empty candidate path pool C; (2) Traverse all surviving paths from the previous layer; (3) Traverse all L candidate points in the current waveguide.
Based on path-specific computation branches for incremental updates;
Generate new local vectors and update the local rate;
Update the path-specific inverse matrix;
Store the new path into the candidate pool C; (4) Sort all paths in C in descending order and keep the top ones for the surviving paths set at this layer.
3  
After the traversal, select the vector with the maximum total rate in the complete path set of M-layer as the final activation scheme.
4.2.3 Complexity and Engineering Characteristics 1
Computational complexity: Each layer can expand up to the number of child nodes, and the total complexity increases linearly with B.
2  
Storage overhead: Each surviving path is stored independently in the inverse matrix, memory;
3  
Parameter tuning advantages: Millimeter-wave base stations can dynamically adjust B according to computational load, increasing B during idle periods to approach optimal performance and reducing B during high loads to minimize computation latency. Simulations show that when Δ=1m and B=8, the BeS and rate can achieve over 99% of the optimal BnB performance, far surpassing brute force and BnB in terms of computational power.
4.3 Branch and Bound (BnB): A global optimal benchmark algorithm, greedy and beam search are heuristic suboptimal algorithms that cannot guarantee the global optimality of solutions. Branch and bound achieves strict global optimality by transforming the objective function monotonically and predicting the upper bound on subtrees, while preserving the optimal solution. It outputs a strictly global optimal activation vector as the performance benchmark for all algorithms in this paper.
4.3.1 Core Challenge: The traditional local rate cannot be used for pruning. The original local and rate increase monotonically with the number of layers, n. In the maximization problem, if a parent node's local rate is lower than the current optimal complete solution, it does not imply that a child node cannot surpass the optimal solution and cannot be directly pruned. This paper innovatively constructs a decreasing transformation objective function to achieve reliable pruning.
4.3.2 Derivation of Upper Bound and Transformed Objective Functions
Maximum rate increment for a single waveguide m: Represents the theoretical upper limit of rate increment provided by any candidate point in the mth waveguide, which can be estimated only once and is fully reused throughout the process.
2  
Transformed local objective function: Key Lemma (Lemma 1): For any local path, the transformed objective function after adding an arbitrary branch satisfies: The core logic proof: Since it is positive definite and bounded, the quadratic form satisfies: Substituting the increment formula gives, thus: The transformed objective function strictly monotonically increases along any branch, providing theoretical support for pruning.
3  
Optimal solution equivalence: The total offset of the complete path is a fixed constant, maximizing the original rate is equivalent to maximizing the transformation rate;
4.3.3 Pruning rules record the current optimal transformation rate found for complete paths; if a local path satisfies, all descendants of that node's leaves will not exceed this rate, it is certain that they cannot surpass the current optimal solution, and the entire subtree is directly pruned without further traversal downwards.
4.3.4 Complete branch bounding algorithm flow (original Algorithm 3) 1
Preprocessing: Traverse all waveguides, precompute,
2
Initialize optimal feasible solution: Run greedy GS to obtain initial activation vector, calculate corresponding transformation optimal value, initial optimal solution;
3
Depth-first search traversal of the tree, each layer nodes expand subnodes in descending order by increment Δ (priority access to high gain branches, faster update Γ, enhance pruning effect);
4
For intermediate nodes at depth n<M: (1) Calculate the current transformation target; (2) If, prune the subtree, stop traversal downwards; (3) If not pruned, update, continue depth-first branching;
5
Reach leaf complete paths (n=M): (1) Calculate the complete path transformation rate; (2) If, update global optimal threshold and optimal activation vector;
6
All accessible nodes traversed, output global optimal.
4.3.5 Complexity characteristics 1
Worst complexity: No subtree can be pruned, equivalent to exhaustive search, exponential complexity;
2
Actual engineering complexity: Dependent on GS providing good initial solutions to tighten Γ, most low gain subtrees are pruned, actual visit nodes, complexity;
3
Localization: Only as a performance benchmark, not suitable for real-time base station scheduling, used to quantify GS, BeS rates of loss.
Section Five: Comprehensive Comparison Analysis of Three Types of Algorithms Based on Four Dimensions: Calculation Complexity, Memory Overhead, Applicable Scenarios, Performance
Algorithm  
Calculation complexity  
Storage overhead  
Optimality
Core Features
Suitable Scenes
Greedy GS
Suboptimal, Loss of about 3%
Low latency and single-path traversal
Real-time online dispatching by base stations, lightweight edge devices
Beamforming BeS
Nearly optimal, B=8 Loss < 1%
Scalable performance complexity, compromise solution
Medium-power mmWave base stations with dynamic load dispatching
Branch and bound BnB
Worst case, actual
Strict global optimization
Output performance upper bound benchmark, extensive pruning
Offline simulation, algorithm performance benchmarking, small-scale scenarios
Supplementary Key Conclusions:
1.
Matrix updates are the core computational power source for all algorithms; the dominant terms are all users K, which is the first factor affecting latency;
2.
The complexity of BeS grows linearly with beam width B, and the number of candidate points M and the waveguide derivative M are linearly added;
3.
The performance of BnB highly depends on the quality of the initial solution, and the output of Greedy GS is a key prerequisite for efficient pruning in BnB, two algorithms can be combined.
VI. Numerical Simulation Results and Engineering Conclusons (Original Chapter 4) 6.1 Simulation Baseline Parameters:
Number of waveguides M=4, user count K=4; Carrier frequency 28GHz mmWave;
Effective refractive index of the dielectric waveguide, attenuation coefficient; Waveguide height h=3m;
User emission power, noise power;
User uniform distribution 30m×10m rectangular area, waveguide fully covering x-axis;
Comparison benchmark: traditional centrally located uniform half-wavelength fixed antenna array;
All results averaged over 1000 random user deployments to eliminate channel randomness impact.
6.2 Simulation Experiment 1: Impact of transmission power and service area size on throughput rate (Figure 3)
Power Characteristics: As the user-emitted power increases, all schemes and rates rise in sync; The PASS discrete activation system significantly outperforms traditional fixed arrays throughout its operation, dynamically adjusting the PA position to compensate for path loss; The GS curve closely follows the optimal curve of BnB, with a very small gap. The BeS gradually approaches the optimal value as B increases.
2  
Service Area Size: The larger the service area's horizontal size, the rate of traditional fixed arrays plummets, making it impossible to compensate for long-distance user path loss; PASS can deploy PA receiving points at the remote end, with a rate decay amplitude much smaller than that of fixed arrays, and the greater the area, the more evident the flexible antenna gain becomes.
Core Conclusion: In high-frequency wide coverage scenarios, discrete squeezed antennas have an irreplaceable performance advantage over traditional arrays.
6.3 Simulation Experiment 2: The Impact of Number L of Candidate Points on Rate (Figure 4)
Rate dimension: The larger L is, the more densely available points are, leading to a monotonic increase in system and rate; At only L=4 with a small number of points, the BnB optimal rate of 21bps/Hz far exceeds the fixed array's 17bps/Hz; At L=151 with high density points, the rate only slightly increases to 24bps/Hz.
2  
Latency dimension: After increasing L, the running time of three types of algorithms increases synchronously, with the BnB growing most rapidly; A small number of discrete points can capture the vast majority of PASS flexible gains, eliminating the need for ultra-high density tracks, reducing hardware costs.
Engineering Insight: Base station waveguides do not need to deploy hundreds of points; leaving 4~8 discrete points per waveguide is sufficient to balance hardware cost and communication rate.
6.4 Simulation Experiment 3: The Impact of Beamwidth B on BeS Performance and Latency (Figure 5)
Rate performance: At a point-to-point distance of Δ=5m, B=3 already reaches the optimal value of 99%; At a point-to-point distance of Δ=1m, B=8 approximates the optimal; GS always maintains a stable optimal value of about 97%.
Time delay performance: As B increases, the number of BeS traversal paths rises linearly, and the running time increases; with a larger Δ, a small B can achieve high gain with lower computational overhead. In extreme cases, the delay of wide-beam BeS can even exceed that of BnB.
Field deployment suggestions: The actual deployment can adaptively set the beam width based on the density of waveguide points, using smaller B for low-density orbits to further reduce computational overhead.
Unified simulation summary 1
Compared to traditional fixed receiving arrays, discrete squeeze antenna systems have significant gains in uplink multi-user and rate, further enhancing their advantages in broad coverage of millimeter wave scenarios.
2
Greedy GS has the lowest complexity and controllable performance loss, suitable for real-time scheduling in commercial base stations.
3
BeS provides a smooth and adjustable trade-off between performance complexity, making it the optimal engineering solution for 6G mmWave base station.
4
Branch and bound BnB serves as the optimal benchmark for algorithm performance evaluation, but is not suitable for online real-time computation.
Conclusion and 6G Engineering Landing Outlook 7.1 Comprehensive Work Review (Corresponding to the original conclusion)
1
Modeling level: For engineering discrete point-to-point hardware constraints, this paper first completes the mathematical model of an uplink multi-user squeeze antenna system, transforming the problem of maximizing uplink and rate into a M-layer combinatorial tree search problem, filling the gap in theoretical research on uplink discrete PA activation.
2
Theoretical tool innovation: Based on the Sylvester determinant identity, we derive a local rate recursion, combined with Sherman-Morrison rank one update to realize matrix inversion without repetition; designing a monotonic transformation objective function, constructing a branch and bound pruning theory that proves global optimality.
3
Algorithm layered design: Proposing gradient-based three sets of algorithms, covering low complexity suboptimal, adjustable compromise, and global optimality across all scenarios, fully deriving the computational and storage complexities of each algorithm.
4
Simulation Verification: Quantitative Proof of Discrete PASS Exceeding Traditional Arrays in 28GHz Millimeter Wave Line-of-Sight Simulation Platform
7.2 Research Limitations and Future Exploration Directions
1. The current model assumes perfect line-of-sight propagation, which can be extended to non-line-of-sight multipath channels in the future;
2. A single waveguide activates only one PA, which can be expanded to architectures that simultaneously activate multiple squeezed particles;
3. This paper focuses on centralized base station algorithms, which can be further researched in distributed lightweight antenna selection in the future;
4. Combining NOMA, integrated communication and sensing, and energy efficiency optimization, expand multi-objective tree search algorithms.
7.3 Industrial Value for 6G Industry Landing
Targeting 6G millimeter and terahertz macro-micro base stations, media waveguide squeezed antennas are core candidate hardware solutions for flexible reconfigurable arrays. The three tree search activation algorithms provided in this paper have solved the real-time optimization challenges under discrete point locations: a lightweight greedy algorithm adapts to massive low-cost edge base stations, beam search is used for high-performance macro stations, branch bounding is used for pre-deployment simulation calibration, completely connecting "hardware architecture - mathematical optimization - engineering algorithm" full chain, which has important theoretical and engineering reference significance for the next generation high-frequency mobile communication network design.
End Interaction
This paper is a pioneering work in uplink optimal activation for squeezed antennas by China Mobile 6G Research Institute and University of London Queen Mary, fully solving the core optimization problem of discrete squeezed antenna engineering landing. If you are interested in flexible antennas, MIMO antenna selection, tree search optimization algorithms, and millimeter wave base station deployment, please leave a message for exchange. We will continue to push forward with PASS system downlink optimization and integrated communication and sensing related papers for in-depth interpretation.

## Images

[6G Millimeter Wave Flexible Antenna Paper Interpretation: Discrete Extrusion Antenna Optimal Activation Three-Layer Tree Search Algorithm Full Analysis with MATLAB Code](cover.jpg)

<!-- PAYMENT_BLOCK -->

## Get this dataset

- Payment link (Stripe): [Buy now](https://buy.stripe.com/3cs8yP7sY87d0vu9AB)
- Payment QR code: ![Payment QR code](pay.png)

<!-- /PAYMENT_BLOCK -->
