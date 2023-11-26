# 빅데이터 융합기술
## 6주차: pandas 활용 복습과제
pandas를 사용하기 위해서는?

    import pandas as pd

python 프로그램 맨 위에 pandas 사용 선언해준다.

- Series 생성

  
  인덱스 a, b, c,'d' 와 값 1, 2, 3, 4 의 값을 갖는 Series 'A' 생성

       A=pd.Series(range(1,5), index=['a','b','c','d'])

  A :
  
  a    1
  
  b    2
  
  c    3
  
  d    4
  
  dtype: int64
  
  Series() 함수에 list 입력

  index 지정하지 않을 시 기본 인덱스 0부터 부여됨

- DataFrame 생성

  |과목명|수강학년|시간수|
  |---:|---:|---:|
  |글로벌시장과경제의이해|2|3|
  |PLC응용|3|4|
  |제어공학|3|3|
  |로봇명사와의만남|1|2|
  
  위 DataFrame 'subject' 생성
  
  - list 사용

    ```
    col=['과목명','수강학년','시간수']
    list1=list([['글로벌시장과경제의이해',2, 3],
                ['PLC응용',3, 4],
                ['제어공학',3, 3],
                ['로봇명사와의만남',1, 2],])
    subject=pd.DataFrame(list1, columns=col)
    ```
    
    DataFrame() 함수에 이차원 list 입력
    
    columns 지정하지 않을 시 기본 인덱스 0부터 부여됨

  - dict 사용
  
    ```
    dict1={'과목명':{0:'글로벌시장과경제의이해',1:'PLC응용',2:'제어공학',3:'로봇명사와의만남'},
         '수강학년':{0:2,1:3,2:3,3:1},
         '시간수':{0:3,1:4,2:3,3:2}}
    subject=pd.DataFrame(dict1)
    ```
  
    
    DataFrame() 함수에 dict 입력
    
    {col:{index:value, index:value}} 순서로 입력
  
- csv 파일 저장 및 읽기

  - subject 데이터프레임을 'filename.csv' 로 저장

    ```
    subject.to_csv('./filename.csv', header=true. index=false, encoding='utf-8')
    ```
  
    subject 데이터프레임을 현재 디렉토리에 filename.csv로 저장, 헤더 및 인덱스 True/False 설정으로 저장 여부 설정 가능

  - 현재 디렉토리에 저장되어 있는 filename.csv 파일을 'df' 로 읽기
  
    ```
    df=pd.read_csv('./filename.csv')
    ```
- 열 이름 조회

  ```
  df.columns
  ```

  실행 결과: Index(['과목명', '수강학년', '시간수'], dtype='object')

- 데이터프레임 값 통계 출력

  ```
  df.describe()
  ```
  
    | |수강학년|시간수|
    |---|---|---|
    |count|4.000000|4.000000|
    |mean|2.250000|3.000000|
    |std|0.957427|0.816497|
    |min|1.000000|2.000000|
    |25%|1.750000|2.750000|
    |50%|2.500000|3.000000|
    |75%|3.000000|3.250000|
    |max|3.000000|4.000000|


- 데이터프레임 정렬 상위/하위 n개 열람

  ```
  df.head(n)
  df.tail(n)
  ```

  각각 데이터프레임의 처음/마지막 n개 행 반환

- 특정 열 기준으로 행 오름차순/내림차순 정렬

  ```
  df.sort_values(by=['수강학년'], ascending=0)
  ```
  
  |  |과목명|수강학년|시간수|
  |---:|---:|---:|---:|
  |1|PLC응용|3|4|
  |2|제어공학|3|3|
  |0|글로벌시장과경제의이해|2|3|
  |3|로봇명사와의만남|1|2|

  
  수강학년 기준으로 정렬, ascending=True/False 설정으로 오름차순/내림차순 정렬

- 열 또는 조건식을 기준으로 데이터 조회

  ```
  df[['과목명','시간수']]
  ```
  
  |  |과목명|시간수|
  |---:|---:|---:|
  |0|글로벌시장과경제의이해|3|
  |1|PLC응용|4|
  |2|제어공학|3|
  |3|로봇명사와의만남|2|

- 인덱스로 데이터 조회

  ```
  df.iloc[1:3,0:2]
  ```

  | |과목명|수강학년|
  |---:|---:|---:|
  |1|PLC응용|3|
  |2|제어공학|3|

  1:3 인덱스, 0:2 열 조회

- 조건을 만족하는 데이터 조회

  ```
  df[df['시간수']>2]
  ```
  
  ||과목명|수강학년|시간수|
  |---:|---:|---:|---:|
  |0|글로벌시장과경제의이해|2|3|
  |1|PLC응용|3|4|
  |2|제어공학|3|3|

  시간수 열의 값이 2를 넘는 행만 출력

  연산자 또는 함수를 연계하여 복잡한 조건 설정 가능
  
- 데이터 수정

  ```
  df.loc[0,'과목명] ='인간과현대사회'
  ```

  인덱스 0, 열 '과목명' 의 값 '글로벌시장과경제의이해'를 '인간과현대사회'로 수정

- 중복 데이터가 없는 열을 인덱스로 지정

  ```
  df.set_index('과목명',inplace=True)
  ```
    
  ||수강학년|시간수|
  |---:|---:|---:|
  |과목명|||
  |인간과현대사회|2|3|
  |PLC응용|3|4|
  |제어공학|3|3|
  |로봇명사와의만남|1|2|

  inplace=True 지정하여 원본에 수정사항 적용 가능

  ```
  df.reset_index(inplace=True)
  ```
  
  reset_index() 함수 사용하여 기본 인덱스로 복구

  
- 열 추가

  ```
  df['수강인원']=df['시간수']*10
  ```
  
  '시간수'*10의 값을 갖는 '수강인원' 열 생성
  
  ||과목명|수강학년|시간수|수강인원|
  |---:|---:|---:|---:|---:|
  |0|인간과현대사회|2|3|30|
  |1|PLC응용|3|4|40|
  |2|제어공학|3|3|30|
  |3|로봇명사와의만남|1|2|20|

- 열 삭제

  ```
  sj.drop('수강인원',axis=1,inplace=True)
  ```
  
  추가한 '수강인원' 열을 삭제

- 데이터 치환

  ```
  rep_cond = {'수강학년':{1:'A', 2:'B',3:'C',4:'D'}}
  df.replace(rep_cond)
  ```
    
  |과목명|수강학년|시간수|
  |---:|---:|---:|
  |인간과현대사회|B|3|
  |PLC응용|C|4|
  |제어공학|C|3|
  |로봇명사와의만남|A|2|

  '수강학년' 열의 1, 2, 3, 4 를 각각 A, B, C, D 로 치환

- 특정 열 기준 조회

  ```
  df.groupby(by=['수강학년'],as_index=False)['시간수'].mean()
  ```
  
  ||수강학년|시간수|
  |---:|---:|---:|
  |0|1|2.0|
  |1|2|3.0|
  |2|3|3.5|

  '수강학년'의 값으로 그룹화, 각 '시간수' 값의 평균 계산

  평균: mean(), 표준편차: std(), 합: sum() 등 함수 사용 가능

## 7주차 복습

- .csv 파일을 'dataset' 데이터프레임으로 불러오기

    ```
    import pandas as pd
    
    dataset=pd.read_csv('DATA.csv')
    ```

- 행렬 정보 확인

    ```
    dataset.shape
    ```
    
- 열 이름 확인

    ```
    dataset.columns
    ```
    
- 값 문자형 확인

    ```
    dataset.info()
    ```
    
- 기술 통계

    ```
    dataset.describe()
    ```

- 상위/하위 10개 항목

    ```
    dataset.head(10)
    dataset.tail(10)
    ```

- '고객명', '나이' '성별' 선택한 열만 출력

    ```
    dataset[['고객명','나이','성별']].head(10)
    ```

- '고객명', '나이' 순서대로 절렬, '고객명', '나이' '성별' 선택한 열만 출력

    ```
    dataset.sort_values(by=['고객명','나이'], ascending=[1,0])[['고객명','나이','성별']].head(10)
    ```

- '키'가 165 이상, '몸무게'가 60 초과인 행만 출력

    ```
    dataset[(dataset['키']>=165)&(dataset['몸무게']>60)]
    ```
    
- '김' 문자를 포함하는 행만 출력

    ```
    dataset[dataset['고객명'].str.contains('김')]
    ```

- ('나이'//10)*10의 값을 갖는 '연령대' 행 생성

    ```
    dataset['연령대']=(dataset['나이']//10)*10
    ```

- '연령대', '주사용OTT' 행 개수별로 정렬,

    ```
    dataset.groupby(by=['연령대', '주사용OTT'],as_index=False).size().sort_values(by=['size'])
    ```


## 12주차: SQL 기초

MySQL Workbench 프로그램 실행

![제목 없음](https://github.com/pengonee0/bigdata/assets/147499525/b5891a83-1b32-441c-80db-60d787410f1d)


- Connection 생성

  
    ![제목 없음](https://github.com/pengonee0/bigdata/assets/147499525/b21cfffd-736d-4bf6-90b9-6d501c5301f6)

    -항목명- 안에 올바른 정보들을 입력하여 DB에 접근할 수 있다.

    ![image](https://github.com/pengonee0/bigdata/assets/147499525/37f1e682-b34b-4dd4-8c36-5e7351c3968b)

    connection이 생성되면 위와 같은 항목이 생긴다. 클릭하여 DB에 접근한다.

    연결 시 Password를 요구할 경우, 올바른 Password를 입력해준다.


- DB 열람

    ![image](https://github.com/pengonee0/bigdata/assets/147499525/b147262a-c734-4785-98af-f5de0cfa0f74)

    다음과 같은 창으로 접속되었다. Navigator - Schemas 를 클릭하여 항목들을 확인한다.

    ![image](https://github.com/pengonee0/bigdata/assets/147499525/27877a9d-1b52-4efe-87f1-08e6e52b040d)

    위 db 테이블 중 cust_sales 를 열람

    ```
    SELECT * FROM bigdb.cust_sales;
    ```

    ![image](https://github.com/pengonee0/bigdata/assets/147499525/0be39482-d3d2-4924-96f3-05dff39ae33c)

    Result Grid에서 결과를 확인할 수 있다.

    조건을 설정하여 DB 열람

    ```
    select cust_id, cust_nm, sales_dt, goods_nm, age, sex from cust_sales where cust_id='3806'
    ```

    ![image](https://github.com/pengonee0/bigdata/assets/147499525/7ead4c84-70e9-4739-9ef6-e92e6367bebf)

    select (1) from (2) where (3)
  
    : (2)에서, (3)을, (1)에 설정한 열만 출력
