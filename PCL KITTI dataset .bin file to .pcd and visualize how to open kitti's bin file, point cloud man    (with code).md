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

