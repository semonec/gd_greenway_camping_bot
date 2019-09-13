# 강동 그린웨이 가족 캠핑장 봇

캠핑을 가야하지만 예약을 놓치는 바보를 위한 봇...

## API 콜 분석

1. Jsessionid 받아오기

 `request`
GET /web/main?shopEncode=5f9422e223671b122a7f2c94f4e15c6f71cd1a49141314cf19adccb98162b5b0 HTTP/1.1
Host: camp.xticket.kr
Connection: keep-alive
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_2) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/76.0.3809.100 Safari/537.36
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3
Sec-Fetch-Site: same-origin
Referer: https://camp.xticket.kr/web/main?shopEncode=5f9422e223671b122a7f2c94f4e15c6f71cd1a49141314cf19adccb98162b5b0
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9,ko;q=0.8,ny;q=0.7

`response`

AWSELB
JSESSIONID

2. 로그인

`request header`
POST /Web/Member/MemberLogin.json HTTP/1.1
Host: camp.xticket.kr
Connection: keep-alive
Content-Length: 69
Accept: */*
X-Requested-With: XMLHttpRequest
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_2) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/76.0.3809.100 Safari/537.36
Sec-Fetch-Mode: cors
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
Origin: https://camp.xticket.kr
Sec-Fetch-Site: same-origin
Referer: https://camp.xticket.kr/web/main?shopEncode=5f9422e223671b122a7f2c94f4e15c6f71cd1a49141314cf19adccb98162b5b0
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9,ko;q=0.8,ny;q=0.7
Cookie:  JSESSIONID=***; AWSELB=***;

`form data`
member_id: ***
member_password: ***
shopCode: 212820734901

3. get list

`request`
POST /Web/Book/GetBookProduct010001.json HTTP/1.1
Host: camp.xticket.kr
Connection: keep-alive
Content-Length: 111
Accept: */*
X-Requested-With: XMLHttpRequest
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_2) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/76.0.3809.100 Safari/537.36
Sec-Fetch-Mode: cors
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
Origin: https://camp.xticket.kr
Sec-Fetch-Site: same-origin
Referer: https://camp.xticket.kr/web/main?shopEncode=5f9422e223671b122a7f2c94f4e15c6f71cd1a49141314cf19adccb98162b5b0
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9,ko;q=0.8,ny;q=0.7
Cookie: JSESSIONID=***; AWSELB=***; 

`response json`
{
"data":{
"bookProductList":[
{
"status_code":"0",
"width":20,
"product_name":"매화01",
"product_discount_fee":27000,
"product_premium_fee":27000,
"product_member_fee":0,
"x_coordinate":109,
"select_yn":"1",
"height":20,
"product_eng_name":"tree1",
"product_code":"00030001",
"season_code":"0",
"y_coordinate":152,
"sale_product_fee":20000,
"product_fee":20000
},
.....
]
}
}

4. list 분석

let list = res.data.bookProductList;
let filtered = list.filter(item => {
  item.select_yn === "1" && item.product_name.includes("매화");
});

5. 예약 걸기...
캡챠가 붙어있다 ㅠㅠ
captcha solver를 만들어야 하는데..파이선에 머신러닝 공부까지 해야하나...

5 alter. 빈 리스트가 있다면 내 단말기에 푸시를 쏴주게해보까
firebase 연동을 시켜서 푸시를 쏘게 한다.
