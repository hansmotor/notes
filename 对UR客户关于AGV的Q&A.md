## AGV Specification    AGV规格    

1.  What is the min clear aisle?     
    通道最小预留宽度是？
          
    (min clear aisle) = (length of AGV) + 0.1m    
    (length of AGV) = 1.1m     
    (通道最小预留宽度) = (AGV车长) + 0.1m    
    (AGV车长) = 1.1m     



2.  How about the balancing/stability of AGV if CoBot reach out and works at one side?       
    当协作机器人向外伸出并在一侧工作时， AGV 的平衡性/稳定性如何？    
            
    The AGV is heavy enough to keep stable when an Elfin robot mounted on it is reaching the farthest distance with 60% speed.         
    AGV本体比较重，机械臂以60%速度运动到最远距离时，AGV本体不会有任何晃动或抖动产生。   

  

3.  Is the AGV powered by battery or diesel?      
    AGV是用电池驱动还是使用内燃机？      

    All powered by battery.     
    都用电池驱动。      



4.  Guidance type? Magnetic tape, vision with QR?       
    导航方式有什么？ 比如磁带，或者使用二维码的视觉定位？      
         
    We can use magnetic tape or LIDAR only, or use both.       
    我们可以仅使用磁带或激光雷达， 也可以二者兼用。      



5.  Any remote control?      
    可以遥控吗？      
 
    Yes.      
    可以。     
 



## Floor Conditions Requirement   地面条件要求   

1.  What is the level difference?     
    能越过多高的坎？     

    The AGV is originally designed for moving on flat ground. It has four swivel casters at four corners, polyurethane tire, and small wheel diameter, which makes it not suitable for getting over obstacles and gaps. Crossing a 4mm high obstacle could cause a relatively large shake making an impact to the AGV and its wheel, which may damage the AGV. Fewer obstacle-crossing is recommended.       
    该款移动机器人设计之初是用于平地场合，四角有四个万向轮，轮胎材质为聚氨酯材料，轮径较小，不适合进行越障，对越障高度为4mm时便会产生较大振动，对AGV本身和车轮造成较大冲击，容易损坏机器人，尽量减少使用。      



2. What is the groove width?       
   能越过多宽的沟？       

   The largest groove width should be 5mm. Same as above, making the AGV to cross a groove is not suggested.      
   同上回回答，不适合跨越沟渠等，尽量减少该情况使用，最大跨越宽度应低于5mm。       



3.  What is the max. slope can climb?       
    最大爬坡斜率？       

    The maximum slope angle it can climb is 3 degrees.    
    最大爬坡高度为3°。     



4.  What is the flooring flatness per SQM?    
    每平方米的地面平整度要求？      

    The difference of the highest point and the lowest point should be 3mm per SQM.     
    每平方米平整度要低于3mm，即每平方米最高点和最低点的高度差小于3mm。      
    
    
    
5.  What is the coefficient of static friction?      
    地面静摩擦力要求？     

    This AGV is designed for indoor applications, working on cement, marble, epoxy floor paint or terrazzo floor. It requires a modest sliding frition, and the coefficient of rolling friction should be between 0.05 and 0.3.       
    该机器人用于室内环境，主要适应的地面为水泥、大理石、环氧地坪漆以及水磨石等，滑动摩擦力不可太小也不可过大，要求驱动轮胎与地面的滚动摩擦系数应大于0.05小于0.3.      

