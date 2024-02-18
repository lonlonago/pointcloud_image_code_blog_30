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

