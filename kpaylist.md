# kpaylist 요청 명세 (header)

  ## 참고
  ### 필수여부
  #### O : 필수 , X : 필수아님 , OX : 조건에 따라 필수여부 달라짐
  ----
  ### 포멧 
  #### A : 영문 ,   N : 숫자 ,   H : 한글 ,   (N) : 자리수
  #### YYYY : 네자리수 년도 ,   MM : 두자리수 월 ,   DD : 두자리수 일  
--- 


| Parameter   | Type   | 필수여부 | 포멧(Byte)   | 설명                                                   |
|-------------|--------|:--------:|:------------:|--------------------------------------------------------|
| orgcode     | String | O        | N(10)        | 귀사의 이용기관 식별번호                               |
| uuid        | String | O        | AN(64)       | 귀사의 KEY                                             |
| stdt        | String | O        | YYYYMMDD(8)  | 납부내역 조회 시작일 / 오늘을 기준으로 직전년도 01월01일부터 조회가능 |
| enddt       | String | O        | YYYYMMDD(8)  | 납부내역 조회 종료일                                   |
| custcd      | String | OX       | AN(20)       | 조회대상 납부자번호 / value_type=1 필수                |
| value1      | String | OX       | N(11)        | 조회대상 키값1 (현재 휴대폰번호만 제공됩니다.) / value_type=2 필수 |
| value_type  | String | O        | N(1)         | 조회대상 구분자 / 0 : 전체, 1 : 납부자번호, 2 : 키값1(value1) |
| sum_type    | String | O        | N(1)         | 조회종류 / 0 : 개별납부내역, 1 : 합산납부내역          |
| seltype     | String | O        | N(1)         | 조회기준일 / 0 : 실출금일, 1 : 납부월차                 |



----


## 예제 1 - 납부자번호로 개별 납부내역을 조회하는 경우

### php 샘플

  ```php
  <?php
    $curl = curl_init();
    
    curl_setopt_array($curl, array(
      CURLOPT_URL => "https://주소/api/kpaylist/",
      CURLOPT_RETURNTRANSFER => true,
      CURLOPT_ENCODING => "",
      CURLOPT_MAXREDIRS => 10,
      CURLOPT_TIMEOUT => 0,
      CURLOPT_FOLLOWLOCATION => false,
      CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
      CURLOPT_CUSTOMREQUEST => "GET",
      CURLOPT_HTTPHEADER => array(
        "orgcode: 귀사의 이용기관 식별변호", 
        "uuid: 귀사의 KEY",
        "stdt: 20240101" ,
        "enddt: 20241231" ,
        "custcd: 납부자번호" ,
        "value1: " ,
        "value_type: 1" ,    // 납부자번호로   
        "sum_type: 0" ,      // 개별납부내역을 
        "seltype: 0"         // 실출금일 기준으로
      ),
    ));
    $response = curl_exec($curl);
    echo $response;
  ?>
  ```

---
  
### 응답 명세 (json)

##### 현재 미납내역은 제공하지 않으므로 (★) PaymentAmt 혹은 (★) PaidAmt 둘 중 하나만 참고하세요
      {
          "OrgCode": "귀사의 이용기관 식별번호",
          "CustCd": "납부자번호",
          "PaymentDt": "납부년월일(납부월차)",
          "PaymentSeq": "납부회차",
          "PaymentAmt": 청구금액 (★) ,
          "PaidDt": "실출금일",
          "PaidAmt": 실출금액 (★) ,
          "Status": "0",
          "remark": "비고",
          "Bnm": "은행명",
          "Acc": "계좌번호(비식별조치)"
      }
---     

  ```json
    [
      {
          "OrgCode": "1111111111",
          "CustCd": "1020304050",
          "PaymentDt": "20240105",
          "PaymentSeq": "1",
          "PaymentAmt": 10000,
          "PaidDt": "20240105",
          "PaidAmt": 10000,
          "Status": "0",
          "remark": "None",
          "Bnm": "은행명",
          "Acc": "*********1234"
      },
      {
          "OrgCode": "1111111111",
          "CustCd": "1020304050",
          "PaymentDt": "20240205",
          "PaymentSeq": "2",
          "PaymentAmt": 10000,
          "PaidDt": "20240210",
          "PaidAmt": 10000,
          "Status": "0",
          "remark": "None",
          "Bnm": "은행명",
          "Acc": "*********1234"
      },
      {
          "OrgCode": "1111111111",
          "CustCd": "1020304050",
          "PaymentDt": "20240305",
          "PaymentSeq": "3",
          "PaymentAmt": 10000,
          "PaidDt": "20240306",
          "PaidAmt": 10000,
          "Status": "0",
          "remark": "None",
          "Bnm": "은행명",
          "Acc": "*********1234"
      }      
    ]
  ```
  
  ----


## 예제 2 - 키값1(value1)로 합산 납부내역을 조회하는 경우 
   ####  참고 : 키값1 은 휴대폰번호 / 휴대폰 번호가 동일한 동일 납부자가 2명인 경우로 가정

### php 샘플

  ```php
  <?php
    $curl = curl_init();
    
    curl_setopt_array($curl, array(
      CURLOPT_URL => "https://주소/api/kpaylist/",
      CURLOPT_RETURNTRANSFER => true,
      CURLOPT_ENCODING => "",
      CURLOPT_MAXREDIRS => 10,
      CURLOPT_TIMEOUT => 0,
      CURLOPT_FOLLOWLOCATION => false,
      CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
      CURLOPT_CUSTOMREQUEST => "GET",
      CURLOPT_HTTPHEADER => array(
        "orgcode: 귀사의 이용기관 식별변호",
        "uuid: 귀사의 KEY",
        "stdt: 20240101" ,
        "enddt: 20241231" ,
        "custcd: " ,
        "value1: 휴대폰번호" ,
        "value_type: 2" ,      // 키값1(value1)로 
        "sum_type: 1" ,        // 합산납부내역을
        "seltype: 0"           // 실출금일 기준으로
      ),
    ));
    $response = curl_exec($curl);
    echo $response;
  ?>
  ```

---
  
### 응답 명세 (json)

##### 현재 미납내역은 제공하지 않으므로 (★) paymentamt_sum 혹은 (★) paidamt_sum 둘 중 하나만 참고하세요
      {
          "orgcode": "귀사의 이용기관 식별번호",
          "custcd": "납부자번호",
          "custnm": "납부자명",
          "paymentamt_sum": "조회기간 총 청구금액 (★) ",  
          "paidamt_sum": "조회기간 총 출금금액 (★) ",   
          "data_count": "총 납부회차"
      }
---     

  ```json
[
    {
        "orgcode": "1111111111",
        "custcd": "A000000001",
        "custnm": "홍길동",
        "paymentamt_sum": "60000",
        "paidamt_sum": "60000",
        "data_count": "3"
    },
    {
        "orgcode": "1111111111",
        "custcd": "B000000001",
        "custnm": "홍길동",
        "paymentamt_sum": "100000",
        "paidamt_sum": "100000",
        "data_count": "2"
    }
]
  ```


