## Selenium运行遇到的问题 ##

1. selenium 和 browser 版本不兼容   
	question：  

	---
 		Webdriver Unable to connect to host 127.0.0.1 on port 7055 after 45000 ms  

	--- 
  	slove：  
  	firefox47+ 需要 selenium 3.x

2. how to use webdriver.gecko.driver on selenium3.0  
	 question1：

	---  		
		The path to the driver executable must be set by the webdriver.gecko.driver system property    

	---  	

	slove：  
  	add webdriver.gecko.driver 配置，在网上看了下，需要这么改，添加进代码
	
	```System.setProperty("webdriver.gecko.driver", "pathTogeckodriver");```  
	```	DesiredCapabilities capabilities = DesiredCapabilities.firefox();```  
	```	capabilities.setCapability("marionette", true);```  
	```	WebDriver dr = new FirefoxDriver(capabilities);```
	

	question2：
	
	---
		pathTogeckodriver找不到，找了很久都没有找到叫哪个driver叫这个名字：pathTogeckodriver 

	--- 

	slove:  
	再仔细一看 path to geckodriver，就是geckodriver的路径，赶紧下载了*geckodriver.exe*

	question3：
	
	---
		[Child 13280] ###!!! ABORT: Aborting on channel error.: file c:/builds/moz2_slave/m-rel-w32-00000000000000000000/build/src/ipc/glue/MessageChannel.cpp, line 2056
		[NPAPI 2196] ###!!! ABORT: Aborting on channel error.: file c:/builds/moz2_slave/m-rel-w32-00000000000000000000/build/src/ipc/glue/MessageChannel.cpp, line 2056
		Jan 15, 2017 10:58:04 PM org.openqa.selenium.os.UnixProcess destroy
		SEVERE: Unable to kill process with PID 13140

    ---

	slove:  
	(1)如果google / stack 都没有答案的话，看看有github上的[issue list](https://github.com/mozilla/geckodriver/issues?page=1&q=is%3Aissue+is%3Aopen)  
	①如果是issue，就想别的办法实现②如果不是issue，就有解决办法  
	原则：哪种方法快且复用性高，就用哪种

	(2)最后确定这是个issue，问题出在

	---
		Calling driver.quit() fails on "Aborting on channel error."

	---
	Final slove 0.1:  
	file issue on the github [issue](https://github.com/mozilla/geckodriver/issues/387)  
	Then write java code to close the browser.

3. continue to add
