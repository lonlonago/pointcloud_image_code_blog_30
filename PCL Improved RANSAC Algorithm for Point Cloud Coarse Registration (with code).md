 directory 

 + 1. 1. Overview of Algorithms 2. Overview of RANSAC Rough Registration 3. References

 + 2. Code implementation 

 + 3. Results display 

#  First, the principle of the algorithm 

##  1. Algorithm overview 

   The random sampling consistency algorithm is used for point cloud registration, and pcl :: SampleConsensusPrerejective this class inserts a simple but effective "pre-exclusion" step through the local invariant geometry constraint in the standard RANSAC pose estimation loop to avoid validating potentially incorrect pose assumptions. To robustly align the partial/occlusion model, the routine performs the fitting error evaluation using only inliers, i.e. points closer than the Euclidean threshold, set using setInlierFraction (). Use setSimilarityThreshold () in [0,1] to specify the number of pre-rejections or "greedy" for the algorithm, where 0 means disabled and 1 means maximum rejection. 

>  具体实现流程见论文：A. G. Buch, D. Kraft, J.-K. Kämäräinen, H. G. Petersen and N. Krüger. Pose Estimation using Local Structure-Specific Shape and Appearance Context. International Conference on Robotics and Automation (ICRA), 2013. 

##  2. Overview of RANSAC rough registration 

   Formally, the goal of point cloud registration is to estimate the transformation that minimizes the sum of the squared distances between each point on the source point cloud, and the corresponding point in the target point cloud: In the above equation, a homogeneous representation of the points is used to allow matrix vector multiplication. Attitude estimation is often solved with a robust and outlier-tolerant approach, such as RANSAC. A common way to handle this is based on feature correspondence, where iterations run the following: (1) Find the middle random sample point and its corresponding point in FPFH by describing the nearest neighbor match of the subFPFH with invariant features. (2) Use the sample correspondence to estimate the hypothesis transformation. (3) Apply the hypothesis transformation to the source point cloud. (4) Find the inner point by searching the spatial nearest neighbor between the transformed source point cloud and the target point cloud through the Euclidean distance threshold. If the number of inner points is too small, go back to step (1). (5) Re-estimate the hypothetical transformation using the inner point correspondence relationship. (6) Compute the distance between the inner point corresponding point pairs, as the final transformation if the distance reaches the minimum value so far. In many cases, the algorithm can be optimized by stopping if it falls below the predefined convergence threshold. Otherwise, the algorithm will run the specified number of iterations. After the first step of each iteration, the above RANSAC pose estimation process is modified by applying a low-level geometric constraint (for the corresponding points on the target object and the scene, according to the geometric consistency that the Euclidean distance between them in their respective spaces is constant). Specifically, the ratio between the side lengths of the virtual polygon formed by the object and the sample points on the scene model is checked. The points corresponding to the sampling of the feature are represented as the side lengths of the object polygon as follows:, the side lengths of the scene polygon are calculated in the same way. Then, the relative dissimilarity vector is calculated by the ratio between the side lengths of the polygon: if the two polygons match exactly, then = 0. Expect the maximum deviation to be below a certain threshold: Therefore, the modification to RANSAC is to insert the following steps in steps (1) and (2): Calculate the vector of dissimilarity between the side lengths of the sampled polygons, and if, return to step (1). This verification step is much less costly than implementing full geometric constraints using steps (2)~ (6), and the closer to 1, the fewer iterations; however, when noise is present, the risk of excluding the correct position is increased. Assuming that this step does not filter out the hypothetical postures of the correctly aligned model, the same probability of success can be expected, but in a much shorter time. It is clear that this assumption holds only for fairly precise geometric observations, so that the polygons are indeed equidistant. If the sensor used shows a large depth error or produces a large distortion effect, the threshold, taking into account these inaccuracies, needs to be set higher. Given the current quality of the sensor, we expect the scope of this problem to be limited. Use, thereby allowing for a 25% maximum edge length difference. Our modification of RANSAC is based on a simple criterion that false assumptions should be filtered out immediately, thus saving time to generate a greater number of hypothetical postures. The method can be considered hierarchical during the rejection of outer points phase, as the method introduces a preliminary low level of polygon-based rejection that does not require postures before the usual medial-based rejection phase. 

   Given the expected probability of success and the expected internal connection score, the formula for calculating the number of required RANSAC iterations is: in general, in step (1) sampling = 3 points, internal connection score estimate = 0.05, expected success rate = 0.99, given Technologies 37000, Euclidean internal connection threshold is set to 0.01 m, the required number of internal connection points is set to 50% of the total number of object model points. 

##  3. References 

>  [1] Fischler M A , Bolles R C . Random Sample Consensus: A Paradigm for Model Fitting with Applications to Image Analysis and Automated Cartography - ScienceDirect[J]. Readings in Computer Vision, 1987:726-740. [2] Chen C S , Hung Y P . RANSAC-based DARCES: a new approach to fast automatic registration of partially overlapping range images[J]. IEEE Transactions on Pattern Analysis and Machine Intelligence, 1999, 21(11): P.1229-1234. [3]A. G. Buch, D. Kraft, J.-K. Kämäräinen, H. G. Petersen and N. Krüger. Pose Estimation using Local Structure-Specific Shape and Appearance Context. International Conference on Robotics and Automation (ICRA), 2013. [4] 陆军,邵红旭,王伟,范哲君,夏桂华.基于关键点特征匹配的点云配准方法[J].北京理工大学学报,2020,40(04):409-415. 

#  Code implementation 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309574176421
  ```  
#  III. Display of results 

 ![avatar]( 20200930200752927.png) 

