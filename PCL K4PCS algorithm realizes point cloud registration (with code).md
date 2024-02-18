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

