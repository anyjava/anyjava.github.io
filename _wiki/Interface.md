---
layout  : wiki
title   : java interface
summary : inteface 는 어떨때 만들어야 햐는가? 
date    : 2018-10-02 22:30:57 +0900
updated : 2018-10-02 22:34:56 +0900
tags    : java, oop, interface
toc     : true
public  : true
parent  : Java
latex   : false
---
* TOC
{:toc}

# Java Interface 는 무조건 만드는게 좋은가? 
	
	* spring 기준으로 설명
		* spring 4.0 이전 버젼에서 @Transactional 을 사용하려면 class일 경우에 CGLib을 이용하여 AOP를 구현 하였는데, 그때는 성능에 이슈가 있었다고 한다.
		* 하지만, 4.0 이후로 오면서 성능이슈가 해결되면서 각자 성향대로 사용하게되었다고
	* 결론으로 나는?
		* 나는 우선 Service Layer	에서는 inteface 를 만들지 않는다.
		* 변화의 가능성이 높은 부분에 대해서만 interface 를 만든다. (아래 링크 글 참고)

# 참고 글 
	
	* (안정된 의존관계 원칙과 안정된 추상화 원칙에 대하여)[http://woowabros.github.io/study/2018/03/05/sdp-sap.html]
