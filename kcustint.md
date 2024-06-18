# kcustint 요청 명세 (header / body)

  ## 참고
  ### 필수여부
  #### O : 필수 , X : 필수아님 , OX : 조건에 따라 필수여부 달라짐
----
  ### 포멧 
  #### A : 영문 ,   N : 숫자 ,   H : 한글 ,   (N) : 자리수
  #### YYYY : 네자리수 년도 ,   MM : 두자리수 월 ,   DD : 두자리수 일  
--- 



  ## HEADER

| Parameter     | Type   | 필수여부 | 포멧(Byte)                | 설명                     |
|---------------|--------|:--------:|:-------------------------:|--------------------------|
| orgcode       | String | O        | N(10)                     | 귀사의 이용기관 식별번호 |
| uuid          | String | O        | AN(64)                    | 귀사의 KEY               |
| Content-Type  | String | O        |                           |application/x-www-form-urlencoded |

----

 ## BODY / (POST)

| Parameter   | Type   | 필수여부 | 포멧(Byte) | 설명 |
|-------------|:------:|:--------:|:----------:|------|
| orgcode     | String |    O      |N(10)      | 귀사의 이용기관 식별번호     |
| custcd      | String |    X      |AN(20)     | 납부자번호, 자동채번 이용기관의 경우 공란     |
| custnm      | String |    X      |AHN(40)    | 납부자명      |
| telno1      | String |    X      |AN(15)     | 전화번호     |
| hphoneno    | String |    X      |AN(15)     | 휴대번호     |
| zipcode     | String |    X      |AN(7)      | 우편번호     |
| address1    | String |    X      |AHN(80)    | 주소     |
| address2    | String |    X      |AHN(80)    | 상세주소     |
| mailaddr    | String |    X      |AN(40)     | 이메일     |
| remark      | String |    X      |AHN(300)   | 비고/메모     |
| bankcd      | String |    X      |N(3)       | 은행코드 / 3자리       |
| accountno   | String |    X      |AN(300)    | 계좌번호     |
| depositer   | String |    X      |AHN(30)    | 예금주명     |
| depjuminno  | String |    X      |AN(20)     | 생년월일/사업자등록번호     |
| payday      | String |    X      |N(2)       | 출금일 / 2자리 ex) 5일인 경우 05     |
| paymoney    | String |    X      |N(11)      | 출금액     |
| paystartdt  | String |    X      |YYYYMMDD(8)|출금시작일    |


## 예제  

### php 샘플

  ```php
  <?php
    $curl = curl_init();

    curl_setopt_array($curl, array(
      CURLOPT_URL => "https://주소/api/kcustins/",
      CURLOPT_RETURNTRANSFER => true,
      CURLOPT_ENCODING => "",
      CURLOPT_MAXREDIRS => 10,
      CURLOPT_TIMEOUT => 0,
      CURLOPT_FOLLOWLOCATION => true,
      CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
      CURLOPT_CUSTOMREQUEST => "POST",
      CURLOPT_HTTPHEADER => array(
        "orgcode: 귀사의 이용기관 식별번호",
        "uuid: 귀사의 KEY",
        "Content-Type: application/x-www-form-urlencoded"  // 고정값 
      ),
      CURLOPT_POSTFIELDS => http_build_query(array(
    		"orgcode" => "귀사의 이용기관 식별번호" , 
    		"custcd" => "" ,                      // 자동채번 이용기관의 경우 공란으로 두세요. 대부분 공란입니다.
    		"custnm" => "홍길동" ,
    		"telno1" => "02-1111-1111" ,
    		"hphoneno" => "010-1234-1234" ,
    		"zipcode" => "" ,
    		"address1" => "대한민국" ,
    		"address2" => "좋은집 1층" ,
    		"mailaddr" => "email@email.com" ,
    		"remark" => "비고사항" ,
    		"bankcd" => "001" ,
    		"accountno" => "1-1111-1111-1" ,
    		"depositer" => "홍길동" ,
    		"depjuminno" => "111111" ,
    		"payday" => "05" ,
    		"paymoney" => "10000" ,
    		"paystartdt" => "20240101"
        )),
    ));
    
    $response = curl_exec($curl);
    echo $response;
    echo curl_getinfo($curl, CURLINFO_HTTP_CODE); // 코드 200을 확인하세요. 

  ?>
  ```

---
  
### 응답 명세 (json)

##### 등록 요청한 정보를 반환합니다. 
      {
        "orgcode": "귀사의 이용기관 식별번호",
        "custcd": "납부자번호",  
        "custnm": "납부자명",
        "telno1": "전화번호",
        "hphoneno": "휴대번호",
        "zipcode": "우편번호,
        "address1": "주소",
        "address2": "상세주소",
        "mailaddr": "이메일",
        "remark": "비고/메모",
        "bankcd": "은행코드",
        "accountno": "계좌번호",
        "depositer": "예금주명",
        "depjuminno": "생년월일/사업자등록번호",
        "payday": "출금일",
        "paymoney": "출금액",
        "paystartdt": "출금시작일"
      }
---     

  ```json
    [
        [
            {
                "orgcode": "1111111111",
                "custcd": null,
                "custnm": "홍길동",
                "telno1": "02-1111-1111",
                "hphoneno": "010-1234-1234",
                "zipcode": null,
                "address1": "대한민국",
                "address2": "좋은집 1층",
                "mailaddr": "email@email.com",
                "remark": "비고사항",
                "bankcd": "001",
                "accountno": "1-1111-1111-1",
                "depositer": "홍길동",
                "depjuminno": "111111",
                "payday": "05",
                "paymoney": "10000",
                "paystartdt": "20240101"
            }
        ],
    ]
  ```
  
  ----
