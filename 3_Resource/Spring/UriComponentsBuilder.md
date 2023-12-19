#papercut 

여러개의 파라미터를 이용하여 URL을 작성하게끔 도와주는 라이브러리.

`fromHttpUrl` : 인자값으로 전달받은 pathUrl에 pathVariable과 queryParam을 추가시킬 수 있다.

`newInstance`: pathUrl부터 전부 정의

```java
public String getParamString (String url) {  
    return UriComponentsBuilder.fromHttpUrl(url)  
          .queryParam("dateFrom", this.dateFrom)  
          .queryParam("dateTo", this.dateTo)  
          .queryParam("vendorNameList", this.vendorNameList)  
          .queryParam("msgYn", this.msgYn)  
          .queryParam("deliveryYn", this.deliveryYn)  
          .queryParam("deliveryType", this.deliveryType)  
          .queryParam("correspondType", this.correspondType)  
          .queryParam("shippingIssueType", this.shippingIssueType)  
          .queryParam("orderCode", this.orderCode)  
          .queryParam("phone", this.phone)  
          .queryParam("msg", this.msg)  
          .queryParam("limit", this.limit)  
          .queryParam("offset", this.offset)  
          .build().toString();  
}
```