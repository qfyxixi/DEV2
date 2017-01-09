## JVM Optimize ##


- print JVM config :-XX  
	``` java -XX:+PrintCommandLineFlags -version ``` 
 


	It will show message:

---

	-XX:InitialHeapSize=129263744 -XX:MaxHeapSize=2068219904 -XX:+PrintCommandLineFl
	ags -XX:+UseCompressedOops -XX:-UseLargePagesIndividualAllocation -XX:+UseParall
	elGC
	java version "1.7.0_99"
	Java(TM) SE Runtime Environment (build 1.7.0_99-b04)
	Java HotSpot(TM) 64-Bit Server VM (build 24.95-b01, mixed mode)

---

- config heap  
	``` java -Xmx3550m -Xms3550m -Xss128k -XX:NewRatio=4 -XX:SurvivorRatio=4 -XX:MaxPermSize=16m -XX:MaxTenuringThreshold=0 ```  
	1)-Xmx: set JVM maxsize  

	2)-Xms: set JVM initial size  
	
	3)-Xmn: set young size  

	4)-Xss: set heap size for thread  

	5)-XX:NewRatio=4:set value=young size(Eden+2 Survivor):old size.Young accounts for 1/5 of heap
 
 
	6)-XX:SurvivorRatio=4:set value=Eden:SurvivorRatio.SurvivorRatio accounts for 1/6 of young
  
	7)-XX:MaxPermSize=16m:set perm max size  

	8)-XX:MaxTenuringThreshold:set max surviving period of garbage    
	  	①if -XX:MaxTenuringThreshold=0  
	Young generation of objects without Survivor area, directly into the old generation  
	It can improve efficiency for the older generation of more applications  
	②if -XX:MaxTenuringThreshold with large value  
	The younger generation of objects in the Survivor area will be repeated many times, which can increase the survival of the younger generation of the object.  

- Recycling option  
	**Parallel collector is mainly aimed at reaching a certain throughput**  
	1)Configuring parallel collectors for younger generation  
	``` java -Xmx3800m -Xms3800m -Xmn2g -Xss128k -XX:+UseParallelGC -XX:ParallelGCThreads=20```

	-XX:+UseParallelGC：Select parallel collector for young  
	-XX:ParallelGCThreads：Set number of threads in parallel collector  

	2)Configuring parallel collectors for old generation   
	``` java -XX:+UseParallelOldGC ```  
	-XX:+UseParallelOldGC:Select parallel collector for old
  
	3)config max time of young garbage collection  

	``` java -Xmx3550m -Xms3550m -Xmn2g -Xss128k -XX:+UseParallelGC  -XX:MaxGCPauseMillis=100 ```  
	-XX:MaxGCPauseMillis=100：  
	If this time is not met, JVM automatically adjusts the young generation size to meet this value  

	**Concurrent collector is mainly to ensure the response time of the system and reduce the time of garbage collection**  
	1)Set concurrent collection for old  
	``` java -XX:+UseConcMarkSweepGC -Xmn2g ```  
	-XX:NewRatio will become invalid.  
	
	2)Memory space for compression, sorting  

	``` java -Xmx3550m -Xms3550m -Xmn2g -Xss128k -XX:+UseConcMarkSweepGC -XX:CMSFullGCsBeforeCompaction=5 -XX:+UseCMSCompactAtFullCollection ```
	-XX:CMSFullGCsBeforeCompaction: Compress and organize memory space after finish GC run time
	-XX:+UseCMSCompactAtFullCollection: Open the compression of the old generation  

- summary  
	1）young  
	①response time prior
	as large as limit  
	The frequency of young generation collection is the smallest. It help to reduce the arrival of the older generation of objects 
 
	②throughput prior  
	as large as limit.It adapt to ≥8CPU

	2）old  
	①response time prior  
	It need to refence:  
	Concurrent garbage collection information  
	Collection times of persistent generation  
	Traditional GC information  
	The proportion of time spent on the recovery of young and old generations 
    
	②throughput prior  
	large young + small old
	




