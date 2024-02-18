##  First, the algorithm basis 

   According to the algorithm principle of KDTree, when the number k of neighborhood points is 1, a one-to-one mapping relationship is established between the search points and the neighborhood points. At this time, the searched neighborhood point is the point with the smallest distance from the search point and the neighborhood point. 

>  An application of KD tree: pcl :: Kd Tree FLANN 

##  Code implementation 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309574161097
  ```  


--------------------------------------------------------------------------------

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



--------------------------------------------------------------------------------

##  I. Introduction to improvement 

   The VoxelGrid class of PCL creates a three-dimensional voxel grid through the input point cloud data, and uses the center of gravity of the points in each voxel to approximate the other points in the voxel, so that all points in the voxel are finally represented by a center of gravity point. The center of gravity point is not necessarily the point in the original point cloud, and it loses the fine characteristics of the original point cloud. Replacing the center point of the voxel with the point closest to the center of gravity of the voxel in the original point cloud data can improve the expression accuracy of the point cloud data. 

##  References 

>  [1] Tao Sijie, Bai Ruilin. A point cloud registration method based on key point optimization after downsampling [J]. Computer Application Research, 2021, 38 (03): 904-907. 

##  Code implementation 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309574287658
  ```  


--------------------------------------------------------------------------------

#  Introduction to use 

   Knowing the index of a point, to extract the point from the original point cloud data, an index extractor is required to achieve it, and the application range of the index extractor is very wide. 

##  1. Main function 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309574259419
  ```  
>  Provides a pointer to the index vector representing the input data. Note: The index here, the structure of indices is a simple vector of type int. 

##  2. Define the index 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309574259419
  ```  
>  Vector class of type int 

 Or: 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309574259419
  ```  
>  A null smart pointer to the vector class of type int 

 To point a smart pointer to a defined points_indices, you need: 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309574259419
  ```  
 Or: 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309574259419
  ```  
##  3. The use of indexes can be as follows 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309574259419
  ```  
##  4. References 

>  [1] doc：Extracting indices from a PointCloud [2] PCL：pcl::ExtractIndices 

#  Code example 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309574259419
  ```  
#  III. Display of results 

 ![avatar]( 20210207091319697.png) 



--------------------------------------------------------------------------------

 index space sampling 

#  First, the code implementation 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309574297578
  ```  
#  III. Display of results 

 ![avatar]( 20210407113346466.png) 



--------------------------------------------------------------------------------

 + 1. Algorithm principle 

 + 2. Code implementation 

 + 3. Results display 

#  First, the principle of the algorithm 

>  It is the ICP algorithm, which adds some sao operations to the code to control the number of iterations and display of the algorithm with the mouse and keyboard. How to operate it has been explained in detail in the code. For the specific theory of the algorithm, please refer to: PCL ICP algorithm realizes point cloud fine registration 

#  Code implementation 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309574123833
  ```  
#  III. Display of results 

 ![avatar]( 20200624205052353.png) 



--------------------------------------------------------------------------------

#  First, the principle of the algorithm 

 ![avatar]( 20210307124845583.png) 

   Internal Shape Descriptor (ISS) is a method to represent solid geometry. The algorithm contains rich geometric feature information and can complete high-quality point cloud registration. Set a point in the point cloud, and the specific process of extracting key points is as follows: 1. Establish a local coordinate system for each point in the point cloud and set a search radius for all points;  

 2. Determine that each point in the point cloud is centered and is all points within the radius area, and calculate the weights of these points, the expression of which is: 3. Calculate the covariance matrix for each point: 4. Calculate the eigenvalues {} of the covariance matrix for each point and arrange them in order from largest to smallest; 5. Select the key points according to the following rules. The significance feature is the smallest eigenvalue: The value of usually does not exceed 1. The basic principle is to avoid detecting key points at points where similar diffusion occurs along the main direction. Since a repeatable standard reference frame cannot be established, it is difficult for the subsequent description stage to produce an effect. In the remaining points, the significance depends on the size of the smallest eigenvalue. Non-maximum suppression () is performed on the significant features within the set radius, and the points with large changes in the main direction are reserved as key points. 

>  PCL :: ISS Key Point 3D class implements ISS Key Point Detection Algorithm 

 References 

>  [1] Zhong Y. Intrinsic shape signatures: A shape descriptor for 3D object recognition [C]//IEEE International Conference on Computer Vision Workshops. IEEE, 2010. [2] Li Renzhong, Yang Man, Tian Yu, Liu Yangyang, Zhang Yanluan. Point cloud registration algorithm based on ISS feature points combined with improved ICP [J]. Advances in Laser and Optoelectronics, 2017, 54 (11): 312-319. [3] Ge Baozhen, Zhou Tianyu, Chen Lei, Tian Qingguo. Point cloud splicing method based on improved ISS feature points and artificial bee colony algorithm [J]. Journal of Tianjin University (Natural Science and Engineering Technology Edition), 2016, 49 (12): 1296-1302. [4] Li Yuxiang, Guo Jiming, Pan Shangyi, Lu Lili, Lu Zhuxing, Zhang Di. A point cloud registration algorithm based on IS-SHOT features [J]. Bulletin of Surveying and Mapping, 2020 (04): 21-26. 

#  Code implementation 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309574157312
  ```  
#  III. Display of results 

 ![avatar]( 20210210113437893.png) 

#  IV. Test data 

 ![avatar]( 20210211141003388.png) 



--------------------------------------------------------------------------------

#  First, the principle of the algorithm 

##  1. Algorithm overview 

>  The K-4PCS method is mainly divided into two steps: (1) Utilize the VoxelGrid filter to downsample the point cloud, and then use the standard method (3D Harris or 3D DoG.) for 3D keypoint detection. (2) Use the keypoint set instead of the original point cloud for data matching through the 4PCS algorithm, which reduces the size of the search point set and improves the computational efficiency. 

##  2. References 

>  [1]THEILER P W, WEGNER J D, SCHINDLER K. Keypoint-based 4-points congruent sets - automated marker-less registration of laser scans[J]. ISPRS Journal of Photogrammetry and Remote Sensing, 2014,96(10):149-163. 

#  Code implementation 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309574110371
  ```  
#  III. Display of results 

 ![avatar]( 20210623200449655.png) 

>  The left is the initial position of the point cloud, and the right is the position of the point cloud after registration 

 ![avatar]( 20210211141100110.png) 



--------------------------------------------------------------------------------

  1. KD-tree optimized ICP

  2. Bidirectional KD-tree optimized ICP 

 ICP basic theory reference: PCL ICP algorithm realizes point cloud precision registration 

#  I. KD-ICP registration 

##  1. KD-tree optimized ICP 

   The nearest neighbor algorithm is realized through the constructed KD-tree, and the nearest neighbor search is carried out on the points after coarse registration. Taking the Euclidean distance as the judgment standard, the registration key points with the Euclidean distance greater than the threshold are eliminated, and the points with high registration accuracy are saved

   According to the above principle, the points with low registration accuracy can be quickly eliminated, and only the points with high registration quality can be retained. By eliminating the points with large registration errors and optimizing the quality of the remaining key points, the rough processing of the registration point cloud accuracy optimization is realized, which provides a basis for subsequent fine processing. 

##  2. Bidirectional KD-tree optimized ICP 

   After the initial registration, and have similar positions. During the registration calculation, the registration efficiency of the ICP algorithm is not high due to the many-to-one problem of the closest point pair. Therefore, the KD-tree ICP algorithm is improved by the method of bidirectional construction of KD-tree. 

   The advantage of bidirectional KD-tree is that it can determine the closest point pairs in one-to-one correspondence. Although the search time is twice that of KD-tree, the iteration time of the algorithm is greatly reduced, so the algorithm is especially suitable for registering point cloud models with large amounts of data. 

##  3. References 

>  [1] Huang Zheng. Research on 3D reconstruction method of scene in unstructured environment of mobile robot [D]. Chongqing University of Technology, 2019. [2] Zhengyou Zhang. Iterative point matching for registration of free-form curves. INRIA-RR-1658, 1992 [3] Cheng Xiaojun, Jia Dongfeng, Cheng Xiaolong. Massive point cloud data processing theory and technology [M]. Shanghai: Tongji University Press, 2014. P47-48 

#  III. Code implementation 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309574114897
  ```  
#  IV. Display of results 

 ![avatar]( 20210307193418103.png) 



--------------------------------------------------------------------------------

#  I. Overview 

 ![avatar]( 3fc17b29702a47ffab89b12666e7f2c4.png) 

   First of all, CloudCompare software should not support reading .bin data in KITTI data (anyway, the CloudCompare v 2.12 alpha I use is blind reading, how can a 1.8M file store more than a billion points?)  

   The main fields stored in the bin format point cloud in the kitti dataset are: x, y, z, and intensity, which correspond to PointXYZI in the PCL data structure. Therefore, just read the attribute information in the bin and put it into PointXYZI. This article gives a simple and easy-to-use implementation code. 

#  Code implementation 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309574237507
  ```  
#  III. Display of results 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309574237507
  ```  
>  Keep the name of the original point cloud file and convert only the format. 



--------------------------------------------------------------------------------

#  First, the main code 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309574133576
  ```  
#  Code implementation 

##  1. Visualize two point clouds with a large window 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309574133576
  ```  
  result display  

##  2. Visualize two point clouds with two large windows 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309574133576
  ```  


--------------------------------------------------------------------------------

#  First, the principle of the algorithm 

   For the whole point cloud, construct the covariance matrix, and calculate the eigenvalues and eigenvectors of the covariance

#  Code implementation 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309574231469
  ```  
#  III. Display of results 

 ![avatar]( e397615fb98c4b918cbbda3ecde80b37.png) 



--------------------------------------------------------------------------------

##  First, the principle of the algorithm 

   PCL analyzes each point :: Local Maximum class and removes the locally largest points in the Z direction relative to their neighbors (found by a radius search). 

##  Code implementation 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309574124277
  ```  
##  III. Display of results 

 ![avatar]( 20210124184607149.png) 

 Four points, filtering out a maximum point.  



--------------------------------------------------------------------------------

#  First, the principle of the algorithm 

   Aleksey Golovinskiy et al. proposed a point cloud segmentation method based on minimum cut. In 3D point cloud data, a K-nearest neighbor graph is established according to an approximate position of the object, and the foreground constraint or optional background constraint is forced to be added, and then the minimum cut is found to complete the segmentation of the foreground and background. Object segmentation in 3D point clouds is challenging, the real-world data is relatively noisy, the foreground and background points are often entangled, and the point cloud data obtained by scanning is unevenly distributed. Existing point cloud segmentation methods mainly focus on extracting geometric data and local point cloud data, and rarely perform object segmentation. The method based on image cutting was originally used in image foreground and background segmentation, but now it is extended to process 3D point cloud data, but there is no information such as color or texture for reference, no smooth surface model and also contains noise points. Given the approximate position of an object, the purpose of the segmentation algorithm is to return the former attractions belonging to the object. In addition, in order to improve efficiency and accuracy, depending on the size of the object, a horizontal radius parameter belonging to the foreground range needs to be attached. 

#  Code implementation 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309574222195
  ```  
#  III. Display of results 

 ![avatar]( 2020072610270960.png) 



--------------------------------------------------------------------------------

 directory 

 + 1. MLS principle 

 + 1. Overview 2. Moving Least Squares 3. References

 + 2. Code implementation 

 + 3. Results display 

#  First, the principle of MLS 

##  1. Overview 

>  Original English paper: Surfaces Generated by Moving Least Squares Methods Related literature: [1] Zeng Qinghong, Lu Detang. Curve and surface fitting based on moving least squares method [J]. Journal of Engineering Graphics, 2004 (01): 84-89. [2] Wei Ya, Xiao Yong, Yan Chuang, Liu Yalin, Wang Linbing. Optimization of pavement 3D reconstruction data based on point cloud preprocessing [J]. Journal of Jilin University (Engineering Edition), 2020, 50 (03): 987-997.pcl :: Moving Least Squares massive point cloud data processing theory and technology [Cheng Xiaojun, Jia Dongfeng, Cheng Xiaolong edited] 2014 edition, this book has a detailed description of the principle of moving least squares. 

##  2. Moving least squares method 

   The moving least squares method fits a moving least squares surface to the neighborhood of each sampling point, and calculates the normal vector and curvature of the sampling point based on the information of the local surface. The calculation is usually divided into 3 steps: Step 1: Calculate the local approximate hyperplane. The local approximate hyperplane is, where, is the normal vector of the hyperplane, is, the neighborhood point of the point, so that the following nonlinear energy function should be minimized: Where, is a monotonically decreasing positive weight function, which is usually taken as, in order to estimate the global sampling space distance, can control the smoothness of the moving least squares surface, which can be dynamically taken as, where is the radius of the bounding sphere in the neighborhood of point K. Step 2: Least squares fitting. Use the local quadric surface to fit the local point cloud. Suppose it is the orthogonal projection to the top, its local two-dimensional coordinates, and its height above can make the following errors, minimum: Step 3: Get the point corresponding to the point on the moving least squares surface. Usually, the normal direction of the hyperplane estimated in the first step is used as the normal direction of the sampling point. Because the moving least squares estimation normal direction is obtained by nonlinear optimization, the method is more accurate. Comparing the covariance analysis method with the moving least squares method, the covariance analysis method is not as accurate as the moving least squares method, but the computational efficiency is greatly improved, and it is more suitable when the point cloud scale is large. The moving least squares method improves the estimation accuracy on the basis of sacrificing efficiency, and is more suitable when the point cloud scale is small or the estimation accuracy of the point cloud normal vector and curvature has higher requirements. 

##  3. References 

>  [1] Lancaster, P. and K. Salkauskas. “Surfaces generated by moving least squares methods.” Mathematics of Computation 37 (1981): 141-158. 

#  Code implementation 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309574126848
  ```  
#  III. Display of results 

 ![avatar]( 20210427180034477.png) 



--------------------------------------------------------------------------------

#  Introduction to the algorithm 

>  ModelOutlierRemoval filters noise in a point cloud based on the distance between the model and the points. Iterates over the entire input once, automatically filtering non-finite points and; setSampleConsensusModelPointer points outside the model specified by (), and thresholds specified by setThreholdFunctionPointer (). 

##  1. References 

>  [1] doc：Filtering a PointCloud using ModelOutlierRemoval [2] PCL：pcl::ModelOutlierRemoval 

#  Code example 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309574257926
  ```  
#  III. Display of results 

 ![avatar]( 20210211195635409.png) 



--------------------------------------------------------------------------------

#  I. Overview 

 ![avatar]( fa61ef3fc0e8432ca1f310232129a24a.png) 

   The main fields stored in the txt format point cloud in the modelnet40_normal dataset are: x, y, z, normal_x, normal_y, normal_z, which correspond to PointNormal in the PCL data structure. Therefore, just read the attribute information in txt and put it into PointNormal. This paper gives a simple and easy-to-use implementation code. 

#  Code implementation 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309574251527
  ```  
#  III. Display of results 

 ![avatar]( f36043b2dbcd41c78e18375616d6745e.png) 



--------------------------------------------------------------------------------

#  First, the principle of the algorithm 

##  1. Process overview 

 Marching Cubes algorithm flow: 

##  2. Detailed process 

 ![avatar]( 20210606091636648.jpg) 

   The moving cube algorithm, pcl :: Marching Cubes algorithm, was first proposed by Willian E. Lorensen and Harvey E. Cline in 1987 in the paper "Marching Cubes: A High Resolution 3D Surface Construction Algorithm". In simple terms, this 3D reconstruction algorithm is a divide and conquer algorithm. The basic unit of divide and conquer is a small cube (dividing a small cube is equivalent to resampling in three-dimensional space, and the number of cubes to be divided is determined by yourself). A small cube has 8 vertices, and each vertex has two states, Inside and Outside. Determine whether the 8 vertices of the small cube are inside the target organ. If a vertex is inside the object, then mark it with a 0; if the vertex is outside the object, mark it with a 1. In each case, some isosurfaces can be generated within the small cube. The isosurfaces are generally generated by interpolation, which can be understood as generating 0 or more triangles located inside the cube. So the distribution of isosurfaces in a small cube may have multiple combinations in total. After taking into account the two cases of Rotation and Invertion symmetry, all 256 possible cases can be covered with 15 basic cubes. In addition, there are some supplementary modes.  

 ![avatar]( 20210606091717804.png) 

 ![avatar]( 20210606091819640.png) 

    However, simply reconstructing the three dimensions from these cube patterns will create a missing point situation, which is due to ambiguity. As shown in Figure 3, in the 2D case, different choices of the two connection methods in the diagram will lead to completely different results on the same image. After observation, it can be seen that this ambiguity only occurs on: one surface, if the two endpoints of one diagonal are greater than the threshold, and the two endpoints of the other diagonal are less than the threshold. This kind of surface is called the ambiguity surface in the cube. The reason for ambiguity is to equate the supplementary mode with the corresponding basic mode, which leads to the interleaving of the Direct type and the Reverse type on the ambiguity surface. Putting this problem on 3D creates the Hole problem. In order to avoid the Hole problem, two extended Marching Cubes algorithms are proposed.  

 ![avatar]( 20210606091919907.png) 

   The first is to connect all ambiguities by type Direct. This results in 23 patterns (see Figure 4).  

 ![avatar]( 20210606091959241.png) 

   The other approach is just the opposite, with all ambiguities of type Reverse and 23 patterns. The results of the two methods are compared in Figure 5.  

#  Code implementation 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309574274242
  ```  
#  III. Display of results 

 ![avatar]( 20200717100244351.png) 



--------------------------------------------------------------------------------

#  First, the principle of the algorithm 

##  1. Registration principle 

   The basic principle of continuous registration of multiple point clouds is to transform all point clouds into the same coordinate system, that is, the world coordinate system. Generally, we use the coordinate system where the first point cloud is located as the world coordinate system. Suppose we collect point clouds from different angles for the same object in a certain order, respectively, and there is a certain overlap area between any two adjacent point clouds, then the specific process of continuous registration of multiple point clouds is as follows: (1) First, register the point cloud and the point cloud as the target point cloud, register the point cloud to the coordinate system where it is located, and remember that the transformation matrix obtained by this registration is, then the point cloud after registration becomes; (2) Register the original and, for the target point cloud, register the point cloud to the coordinate system where it is located, and record the transformation matrix obtained by this registration as, then the transformation can be used and converted to the coordinate system where it is located at this time, and obtain; (3) And so on, any first The point cloud can be transformed into the coordinate system of the point cloud by the equation (1), and the point cloud together constitutes a complete three-dimensional point cloud model of the object.  

##  2. Process overview 

   The code uses a non-linearly optimized ICP algorithm to incrementally pair-register a series of point clouds. The idea is to transform all point clouds so that they are all in a unified coordinate system with the first point cloud. Find the best transformation between each coherent overlapping point cloud and accumulate these transformations to all point clouds. Point clouds capable of performing the ICP algorithm need to be roughly pre-matched, and one point cloud needs to overlap with another. 

##  3. References 

>  [1] doc：How to incrementally register pairs of clouds 

#  Code implementation 

>  Note: This code was compiled in PCL 1.11.1, and there may be several errors in other versions of PCL. 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309574118159
  ```  
#  III. Parameter debugging 

 ![avatar]( 20210413211325755.png) 

 Enter the point cloud to be registered in the command line arguments. For example, to register capture001.pcd and capture002.pcd, enter in the command line arguments:  

#  IV. Display of results 

 ![avatar]( 20210413211431497.png) 

    Run the code, if you can't see the result, you need to press R to reset the camera. By adjusting the angle, you can observe that there are two groups of red and green point clouds displayed on the left side of the window, red is the source point cloud, and green is the target point cloud. As shown in Figure 1, this picture shows that only the source and target point clouds are displayed on the left side of the window before registration, and different colors are used.  

 ![avatar]( 20210413211643269.gif) 

   The command line prompts that the configuration criterion needs to continue to be executed. Press q. After pressing q, you will see a continuously adjusted point cloud on the right side of the window. In fact, it is the output of the intermediate result of the iteration during the registration process, as shown in Figure 2. Before the number of iterations is less than the set number of times, the right side will continue to refresh the latest registration result until it converges.  

 ![avatar]( 20210413211746398.png) 

   After 30 registration iterations, the entire registration process is completed, and the final display is shown in Figure 3. There are obvious differences between the left and right in the figure. The right shows the registered display. Generally, the red part and the green part are smaller than the distance between the red and green corresponding to the unregistered point cloud on the left. Press q again to exit the program, and you will see the stored 1.pcd file in the same directory. This file is the point cloud of the first and second input point clouds registered in the same coordinate system as the first input point cloud.  

 ![avatar]( 20210413211839272.png) 

   Another example of implementation is if you need to register capture001.pcd, capture002.pcd, capture002.pcd, etc., just enter the data to be registered into the command line parameters: the whole process is the same as the above two inputs, the program registers the two adjacent ones (the first and the second, the second and the third...), and converts the final point clouds into the same coordinate system as the first input point cloud, and stores all the output point cloud files to the hard disk (1.pcd, 2.pcd...). It should be noted here that all point clouds are transformed and finally registered with the first input point cloud in a unified coordinate system, which completes the registration of all point clouds.  



--------------------------------------------------------------------------------

#  First, the principle of the algorithm 

    PCL :: FastBilateralFilterOMP realize multi-threaded fast bilateral filtering of smooth depth information in organized point clouds. 

#  Code implementation 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309574270909
  ```  
#  III. Display of results 

 ![avatar]( 20210427215239494.png) 



--------------------------------------------------------------------------------

