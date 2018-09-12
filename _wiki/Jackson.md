---
layout  : wiki
title   : Jackson
summary : json, jackson
date    : 2018-09-13 08:37:20 +0900
updated : 2018-09-13 08:38:06 +0900
tags    : json, jackson
toc     : true
public  : true
parent  : Java
latex   : false
---
* TOC
{:toc}

# Json Filter

```
import com.fasterxml.jackson.annotation.JsonFilter;
import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.databind.ser.FilterProvider;
import com.fasterxml.jackson.databind.ser.impl.SimpleBeanPropertyFilter;
import com.fasterxml.jackson.databind.ser.impl.SimpleFilterProvider;
import lombok.Getter;
import lombok.ToString;
import lombok.experimental.FieldNameConstants;
import org.junit.Before;
import org.junit.Test;

import static org.assertj.core.api.Java6Assertions.assertThat;

/**
 * JsonFilter 를 사용해서 json 을 generate 하는 learning Test 작성
 * Ad-center 내부에서 사용하는 API 이고 entity 를 json 으로 변환해서 내려야 한다면, JsonFilter 를 이용해 보는것도 좋을듯.
 * @see <a href="http://kwonnam.pe.kr/wiki/java/jackson">권남님 블로그: Java Jackson Library </a>
 * @see <a href="https://github.com/Baeldung/spring-hypermedia-api/blob/master/src/main/java/com/baeldung/web/controller/NewBookController.java">Spring MVC에서 사용한 에제코드 </a>
 * @see <a href="https://projectlombok.org/features/experimental/FieldNameConstants">Lombok 을 이용한 필드면 사용 방법 - 매직스트링 제거 </a>
 */
public class JsonFilterTest {

    public static final String TEST_JSON_FILTER = "testJsonFilter";
    private ObjectMapper om;

    @Before
    public void setup() {
        om = new ObjectMapper();
    }

    @Test
    public void jsonFilterShouldReturnJsonWithCompositionObject() throws JsonProcessingException {
        String expectedJson = "{\"fooField\":\"fooValue\",\"fooInt\":111,\"bar\":{\"barField\":\"barValue\"}}";

        FilterProvider filters = new SimpleFilterProvider()
                .addFilter(TEST_JSON_FILTER, SimpleBeanPropertyFilter.filterOutAllExcept(Foo.FIELD_FOO_FIELD, Foo.FIELD_FOO_INT, Foo.FIELD_BAR, Bar.FIELD_BAR_FIELD));

        om.setFilterProvider(filters);
        String actualJson = om.writeValueAsString(new Foo());

        assertThat(actualJson).isEqualTo(expectedJson);
    }

}

@JsonFilter(JsonFilterTest.TEST_JSON_FILTER)
@FieldNameConstants
@Getter
@ToString
class Foo {
    private String fooField = "fooValue";
    private int fooInt = 111;
    private String fooDummy = "fooDummy";

    private Bar bar = new Bar();
}

@JsonFilter(JsonFilterTest.TEST_JSON_FILTER)
@FieldNameConstants
@Getter
class Bar {
    private String barField = "barValue";
    private String barDummy = "barDummy";
}

```
