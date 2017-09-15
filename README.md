# ModuleBintrayAndjCenter
將Module上傳到bintray並add to jCenter的過程紀錄


<h2>概述</h2>
目的是將寫好的Module上傳到jCenter給全世界的人使用

在build.gradle(Module)的dependencies裡，只要使用一句

<code>compile 'com.samuelhsieh:DeerActiobBar:1.0.0'</code>

就可以方便的從jCenter上載下來使用

<h2>Step 1. 註冊Bintray</h2>

[Bintray申請註冊](https://bintray.com/)

記住

這邊有一個坑就是

一定要選擇註冊OSS的(右邊 `Sign Up to an Open Source account`)

<img src="https://github.com/Samuel-Hsieh/ModuleBintrayAndjCenter/blob/master/png/07.png" width="75%"/>

<img src="https://github.com/Samuel-Hsieh/ModuleBintrayAndjCenter/blob/master/png/08.png" width="50%"/>

[註冊傳送門這裡](https://bintray.com/signup/oss)

如果選擇 Enterprise Edition Trial 會沒辦法Add to jCenter

<h2>Step 2. 創建一個屬於自己的repository</h2>

大概創一下

<img src="https://github.com/Samuel-Hsieh/ModuleBintrayAndjCenter/blob/master/png/01.png" width="400"/>

<h2>Step 3. 創建Repository裡的Package</h2>

大概創一下

<img src="https://github.com/Samuel-Hsieh/ModuleBintrayAndjCenter/blob/master/png/02.png" width="400"/>

<h2>Step 4. 配置Gradle Bintray的插件</h2>

讓我們回到Android Studio

在bulid.gradle(Project)配置如下

	dependencies {
		classpath 'com.android.tools.build:gradle:1.2.3'
		classpath 'com.jfrog.bintray.gradle:gradle-bintray-plugin:1.2'
 		classpath 'com.github.dcendents:android-maven-plugin:1.2'
	}

在bulid.gradle(Module)配置如下

	apply plugin: 'com.github.dcendents.android-maven'
	apply plugin: 'com.jfrog.bintray'
	
<h2>Step 5. 配置Bintray的Api.key和user在local.properties裡</h2>

打開Bintray網站

可以在這找到你的Key

<img src="https://github.com/Samuel-Hsieh/ModuleBintrayAndjCenter/blob/master/png/05.png" width="75%"/>

可以在這找到你的username (ex: medathsieh)

<img src="https://github.com/Samuel-Hsieh/ModuleBintrayAndjCenter/blob/master/png/04.png" width="400"/>

然後貼到local.properties裡

	bintray.user=YOUR_BINTRAY_USERNAME
	bintray.apikey=YOUR_BINTRAY_KEY
	
<h2>Step 6. 回到bulide.gradle(module) 配置上傳的Gradle腳本</h2>

可貼在bulide.gradle最下面

	version = "1.0.0"

	group = "com.samuelhsieh"
	install {
    repositories.mavenInstaller {
        	// This generates POM.xml with proper parameters
	        pom {
	            project {
	                packaging 'aar'
	                // Add your description here
	                name 'This is a actionBar support library that can easy to set title and title color and background.'
	                //描述
	                // Set your license
	                licenses {
	                    license {
	                        name 'The Apache Software License, Version 2.0'
	                        url 'http://www.apache.org/licenses/LICENSE-2.0.txt'
	                    }
	                }
	                developers {
	                    developer {
	                        id 'medathsieh'        
	                        name 'medathsieh'   //Bintary上的帳戶名稱
	                        email 'mydir11@gmail.com' //Bintary上的e-mail
	                    }
	                }
	            }
	        }
	  }
	}
	task sourcesJar(type: Jar) {
	    from android.sourceSets.main.java.srcDirs
	    classifier = 'sources'
	}
	artifacts {
	    archives sourcesJar
	}
	Properties properties = new Properties()
	properties.load(project.rootProject.file('local.properties').newDataInputStream())
	bintray {
	    user = properties.getProperty("bintray.user")
	    key = properties.getProperty("bintray.apikey")
	    configurations = ['archives']
	    pkg {
	        repo = "AndroidLib"       //Bintray上repository的名稱
	        name = "DeerActionBar"    //發佈出去的名稱
	        licenses = ["Apache-2.0"]
	        publish = true
	    }
	}

開頭的group = "com.samuelhsieh"

還有發佈出去的名稱name = "DeerActionBar"

最前面的version = "1.0.0"

決定他的樣子

<code>compile 'com.samuelhsieh:DeerActiobBar:1.0.0'</code>

<h2>Step 7. Android studio的Terminal打指令</h2>

	./gradlew install
	./gradlew bintrayUpload

沒問題的話會看到

BUILD SUCCESSFUL

<img src="https://github.com/Samuel-Hsieh/ModuleBintrayAndjCenter/blob/master/png/06.png" width="75%"/>

<h2>Step 8. Add to jCenter</h2>

回到Bintray找到你library上傳的Package，右下角有一個Add to jCenter

點進去後隨便輸入一下Comments然後就可以Send

聽說預計會在每天晚上的12:30審核，配合美國時間的樣子

這將就完成囉～！

