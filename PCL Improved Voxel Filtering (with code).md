##  I. Introduction to improvement 

   The VoxelGrid class of PCL creates a three-dimensional voxel grid through the input point cloud data, and uses the center of gravity of the points in each voxel to approximate the other points in the voxel, so that all points in the voxel are finally represented by a center of gravity point. The center of gravity point is not necessarily the point in the original point cloud, and it loses the fine characteristics of the original point cloud. Replacing the center point of the voxel with the point closest to the center of gravity of the voxel in the original point cloud data can improve the expression accuracy of the point cloud data. 

##  References 

>  [1] Tao Sijie, Bai Ruilin. A point cloud registration method based on key point optimization after downsampling [J]. Computer Application Research, 2021, 38 (03): 904-907. 

##  Code implementation 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309574287658
  ```  
