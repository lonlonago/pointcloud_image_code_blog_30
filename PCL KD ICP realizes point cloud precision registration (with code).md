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

