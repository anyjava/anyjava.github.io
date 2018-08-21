# Spring Boot Actuactor

## 2.0 Migration 

* Actuator 는 편리하지만 Jvm 의 대부분의 상태를 볼수 있으므로, 보안에 신경써야함.
* end point 들은 `/health` `/info` 를 제외하고는 모두 비활성화 되어 있다.
* 1.X 버젼에서 사용하더 management.security prorperty 는 Deprecated 됨. 자세한 이유는 다음 링크에
	*	https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-security.html#boot-features-security-actuator
	
	
	
## 참고자료

* https://github.com/spring-projects/spring-boot/wiki/Spring-Boot-2.0-Migration-Guide
