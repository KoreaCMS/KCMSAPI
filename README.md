# KCMSAPI

프로젝트에 대한 간단한 소개

KCMS 네오, 필터, 로컬 씨엠에스의  출금 결과를 볼 수 있는 기능

## 설치 및 시작하기

설치 및 시작하는 방법에 대한 지침

## 사용법

- 등록된 IP로만 접속이 가능

- 토큰 방식, 키 방식으로 존재

- 토큰 발급 후 출금결과를 받는 방식

- 키 방식은 키 받은 후 출금 결과 진행


## 예제

api 사용 예제

- 기간변 납부내역 조회
   1) 시작일과 종료일로 해당기간 납부내역 조회(밑줄 : 필수입력)
       - 검색조건(seltype): 0(실출금일), 1(청구일)
       - 예) 15일이 일요일인 경우 16일 월요일에 출금 => 청구일 15일, 실출금일 16일

    2) 접근 토큰 방식(Access TOKEN)
       - GET /api/tpaylist/
       - HTTP Headers 형식 
         ※ authorization: Bearer {Access TOKEN}, seltype: {검색조건}, stdt: {시작일}, enddt: {종료일}
       - 처리결과는 키 방식과 동일
   
     3) 키 방식(API KEY)
       - GET /api/kpaylist/
       - HTTP Headers 형식 
         ※ orgcode:{식별번호}, 
         uuid: {API KEY}, 
         seltype: {검색조건}, 
         stdt: {시작일}, 
         enddt: {종료일}


  처리 결과
  
    {
       "OrgCode": "귀사의이용기관식별번호",
  
       "CustCd": "납부자번호",
  
       "PaymentDt": "20240105",
  
       "PaymentSeq": "1",
  
       "PaymentAmt": 10000,
  
       "PaidDt": "20240105",
  
       "PaidAmt": 10000,
  
       "RegDt": "2024-01-19 10:19:42"
  
     }

----------------------------------------------------------------------------------------------
  ### 예시

  #토근 발급

    PHP Curl
    
   ```
   <?php

      $curl = curl_init();
      
      curl_setopt_array($curl, array(
        CURLOPT_URL => 'http://주소/auth/token',
        CURLOPT_RETURNTRANSFER => true,
        CURLOPT_ENCODING => '',
        CURLOPT_MAXREDIRS => 10,
        CURLOPT_TIMEOUT => 0,
        CURLOPT_FOLLOWLOCATION => true,
        CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
        CURLOPT_CUSTOMREQUEST => 'POST',
        CURLOPT_POSTFIELDS => 'orgcode=orgcode입력&uuid=uuid입력',
        CURLOPT_HTTPHEADER => array(
          'Content-Type: application/x-www-form-urlencoded'
        ),
      ));
      
      $response = curl_exec($curl);
      
      curl_close($curl);
      echo $response;

   ?>
   ```



  #납부내역 조회 / KEY 방식 예시

```PHP Curl 
   <?php

      $curl = curl_init();
      
      curl_setopt_array($curl, array(
        CURLOPT_URL => 'https://주소/api/kpaylist/',
        CURLOPT_RETURNTRANSFER => true,
        CURLOPT_ENCODING => '',
        CURLOPT_MAXREDIRS => 10,
        CURLOPT_TIMEOUT => 0,
        CURLOPT_FOLLOWLOCATION => true,
        CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
        CURLOPT_CUSTOMREQUEST => 'GET',
        CURLOPT_HTTPHEADER => array(
		      'orgcode: 귀사의 이용기관 식별번호',
		      'uuid: 귀사의 KEY',
		      'stdt: 조회시작일' ,
		      'enddt: 조회종료일' ,
		      'custcd: 조회대상 납부자번호' ,
		      'value1: 조회대상 키값 1' ,
		      'value_type: 조회대상 구분자',
		      'sum_type: 조회종류',
		      'seltype: 조회기준일'
        ),
      ));
      
      $response = curl_exec($curl);
      
      curl_close($curl);
      echo $response;

   ?>


   ```


## 1. [납부내역조회 KEY 방식 요청 명세 및 샘플](https://github.com/KoreaCMS/KCMSAPI/blob/main/kpaylist.md)


--- 


## 2. [납부자등록 KEY 방식 요청 명세 및 샘플](https://github.com/KoreaCMS/KCMSAPI/blob/main/kcustint.md)

  
