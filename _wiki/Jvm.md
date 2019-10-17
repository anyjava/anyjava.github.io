---
layout  : wiki
title   : JVM 성능 튜닝 
summary : JVM 관련하여 GC 종류, thread dump 보는 방법등을 정리
date    : 2018-12-03 19:31:45 +0900
updated : 2019-10-17 09:20:06 +0900
tags    : devops, jvm, gc, thread
toc     : true
public  : true
parent  : DevOps
latex   : false
adsense : true
--- * TOC
{:toc}

# JVM (Java Virtual Machine) 성능 튜닝

## JVM Heap Area 의 구성
* new 연산자로 생성된 객체와 배열을 저장하는 공간입니다.
* GC 로 사용하지 않는 객체들은 메모리로 반환됩니다.
* Heap Area 종류
	* Permanent Generation: 생성된 객체들의 정보의 주소값이 저장된 공간
	* New Area
		* Eden: 객체들이 최초로 새성되는 공간
		* Survivor: Eden에서 참조되는 객체들이 저장되는 공간
		* 실제 통계로도 생성된 98% 객체가 곧바로 쓰레기 객체가 된다고 한다.

	* Old Area
		* New Area 에서 일정시간이상 참조되고 있는 객체들이 저장되는 공간 

## GC (Gabage Colector) 의 종류와 특징

대부분의 내용은 아래 참고 링크에서 발췌하였음,

* Serial GC
* Parallel GC
* CMS GC
	* 일반적으로 많이 사용함.
* G1 GC
	* 4G 이상의 힙 영역이 매우 큰 머신에서 설정
* ZGC

## GC 설정
* Heap Size = 운영체제 메모리 - 2GB 정도로 하되 최대 31GB까지만 사용한다.
	* (swappiness=0 or 1이라는 가정하에- 그렇지 않으면 Swap 발생으로 성능저하가 일어날 수 있어보임)
* 웹 애플리케이션은 운영체제 메모리중 2GB 정도를 남기고 사용한다.  (ElasticSearch 제외. 각 솔루션은 솔루션 가이드를 따른다.)
	* 단, 31GB 까지만 사용한다. 32GB가 넘어가게 되면 메모리 포인터의 크기가 64bit가 되면서 메모리 점유율이 크게 늘어난다.
	* 32GB미만은 32bit 포인터 사용. 

```
GC_OPTIONS="-XX:+UseG1GC -XX:MaxGCPauseMillis=200 -XX:+DisableExplicitGC"
GC_LOG_OPTIONS="-Xloggc:/var/logs/gc.log -verbose:gc -XX:+PrintGCDetails -XX:+PrintGCDateStamps -XX:+PrintGCTimeStamps -XX:+UseGCLogFileRotation -XX:NumberOfGCLogFiles=10 -XX:GCLogFileSize=512K"
```


## 운영시 JVM 상태 모니터링하는 명령어

* `$ sudo yum install java-1.8.0-openjdk-devel 를 통해 도구를 설치한다.`
	* `sudo -u tomcat jstack <pid>`
	* `sudo -u tomcat jstat -gcutil -h20 <pid> 1000`
	* `jstat -gccapacity <pid>`
	* `sudo -u tomcat jmap -histo <pid>`
	* `jmap -heap <pid>`
	* `jmap -dump:live,format=b,file=dump.bin <pid>` : live 되고 있는 객체만 dump 뜬다. file 위치는 홈디렉토리이다.
* Thread 별 CPU 도 확인 할수 있음: `htop` 혹은 `/proc/{pid}/task/000/stat` 정보로 확인 가능 

## GC Friendly 하게 작성하기

* new 를 남발해도 GC 오버헤드는 거의 없다. 중요한건 new 한 객체의 라이프사이클을 짧게 유지해야 한다. 
  * 특히 지역변수가 참조하는 객체들
* LinkedList 는 사용하지 않는게 GC 입장에서는 좋다.
* 중간데이터를 저장할 때 HashMap 등의 자료구조를 이용해서 데이터가 커지지 안헥 하는 습관이 좋다.
* 안쓰는 객체는 빨리 참조를 없애라.
* null out when it is done. if the data-structure is big.

## JIT Compiler Friendly 

* branch condition as constant
* Do not afraid of making smaller functions
  * 가독성이 더 중요하다. JIT Compiler 의 inlining 을 통한 최적화를 믿어라
* do not abuse native class
  * inlining is impossible

## 참고 링크

* [[JAVA] 가비지 컬렉터의 배경과 종류](https://okky.kr/article/379036)
* [JVM 메모리구조](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=2&ved=2ahUKEwi6lOjDq-fhAhXJS7wKHdCaBQUQFjABegQIBxAC&url=http%3A%2F%2Fjavaslave.tistory.com%2Fattachment%2Fcfile25.uf%402367C345566D35C5303FB9.pdf&usg=AOvVaw2yvS052I9N2riZ9fyqH1-I)
* [권남이 wiki - Java Memory Analysis](http://kwonnam.pe.kr/wiki/java/memory)
* [하나의 메모리 누수를 잡기까지 - Naver D2 Hello Wordl](https://d2.naver.com/helloworld/1326256)
* [스레드 덤프 분석학 - Naver D2](https://d2.naver.com/helloworld/10963)
* [Head dump 분석용 툴 - eclipse MAT](http://www.eclipse.org/mat/) 
* [[자바 객체의 크기] Shallow size와 Retained size](https://www.tuning-java.com/391)
* [Java GG 튜닝 - 기계인간](https://johngrib.github.io/wiki/java-gc-tuning/)
* [JVM 메모리 구조와 GC](https://johngrib.github.io/wiki/jvm-memory/)
 
