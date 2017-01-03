## heap and stack ##
### java ###
1. heap: dynamic allocation for object,the space will free when it is no reference 
2. stack:local variable
### c ###
1. heap:use malloc to apply,use free to free space manually
2. stack:system allocate/free  
	1）compiling： variable or function   
	2）running：Parameter transfer
3. malloc must Initialization,Free pointer must be set to NULL

### c++ ###
1. heap:use new to apply,use delete to free 
2. stack:system allocate/free
	1）compiling： variable or function   
	2）running：Parameter transfer
