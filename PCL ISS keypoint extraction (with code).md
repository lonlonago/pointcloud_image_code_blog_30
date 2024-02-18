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

