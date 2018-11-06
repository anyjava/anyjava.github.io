---
layout  : wiki
title   : Bean Validation
summary : Spring Bean Validation 에 관한 기록
date    : 2018-11-07 08:16:58 +0900
updated : 2018-11-07 08:24:29 +0900
tags    : spring, bean, validation
toc     : true
public  : true
parent  : Spring
latex   : false
---
* TOC
{:toc}

# `@Valid` annotation 을 이용하여 Http Request 파라미터 검증하기

* 왜 사용해야 하나?
	* Request Commnad 객체의 모든 필드에 대해서 유효성 검증을 할 수 있다.
	 
## Controller 에서 처리하는 방법
```
  @PostMapping("/api/members")
	public ResponseEntity<Object> newMember(
			@RequestBody @Valid RegisterRequest regReq,
			Errors errors) { // Errors 객체를 파라미터로 받지 않으면 validation 실패시 exception 발생
		if (errors.hasErrors()) {
			String errorCodes = errors.getAllErrors().stream()
					.map(error -> error.getCodes()[0])
					.collect(Collectors.joining(","));
			return ResponseEntity.status(HttpStatus.BAD_REQUEST)
					.body(new ErrorResponse("errorCodes = " + errorCodes));
		}
	}
```

## ControllerAdvice 를 이용하는 방법
```
@RestControllerAdvice("controller")
public class ApiExceptionAdvice {
	@ExceptionHandler(MethodArgumentNotValidException.class)
	public ResponseEntity<ErrorResponse> handleBindException(MethodArgumentNotValidException ex) {
		String errorCodes = errors.getAllErrors().stream()
				.map(error -> error.getCodes()[0])
				.collect(Collectors.joining(","));
		return ResponseEntity
				.status(HttpStatus.BAD_REQUEST)
				.body(new ErrorResponse("errorCodes = " + errorCodes));
	}
}
```
