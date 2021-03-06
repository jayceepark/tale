---

title: 자바 개발자가 이해하는 R 언어(1) - R 언어 기초/데이터 유형
author: jaycee
category: R language
published : true
tag: R language, R 기초

---

## 변수의 종류 (논리적인 정의)
1. 범주형 변수 (categorical variables)
  - 질적인 변수(qualitative)
  - 표(table) 등 계수(counting)에 기반한 요약
  - 자료가 가질 수 있는 값이 몇 개의 범주로 국한
  - 막대그래프, 파이차트로 시각화 가능
    + 명목형 자료: 성별 직업 등 순서가 없는 범주
    + 순서형자료: 만족도, 직급 등 고유한 순서를 갖는 자료 ex)매우만족,만족,보통,불만족,매우불만족 등의 숫자가 아닌 순서형
    
2. 수치형 변수 (numerical variables)
  - 양적인 변수(quantitative)
  - 정렬이나 합계에 기반한 요약
  - 수치적인 의미가 있어서 연산이 가능
  - 히스토그램, 상자그림, 선 도표로 시각화 가능
    + 연속형 자료: 키, 연봉 등 구간 내에 모든 값을 가질 수 있음
    + 이산형 자료: 횟수, 건수 등 몇개의 다른 값 만을 가지는 경우 (비연속성)
    
## 데이터 유형
  - 기본 데이터 유형
    + 수치형(numeric): 정수형 또는 실수형
    + 논리형(logical): 참/거짓, 참(T/TRUE) or 거짓(F/FALSE)
    + 문자형(character): 문자, 숫자 또는 이들의 조합의 문자열
    + 복소수형(complex): 실수와 허수로 구성
    + NULL: 비어있는 값

  - 특수한 데이터 유형
    + NA:결측치. 값이 비어있어 표현이 불가능
    + NaN: 수학적으로 정의가 불가능
    + Inf, -Inf: 무한대
 
## R 객체
  - 벡터(Vector)
    + R에서 사용되는 대부분의 데이터 객체를 벡터를 통해 생성
    + 하나의 변수와 같이 취급되므로 하나의 벡터에 입력되는 데이터의 속성은 모두 동일
> 자바의 배열과 유사. 같은 데이터형끼리 묶일 수 있음. R에도 배열이라는 객체가 존자하나 R의 배열은 3차원부터의 벡터를 표현한다.
  
  - 요인(Factor)
    + 데이터가 범주형
> 자바 배열에 컬럼 정보를 갖고 있는 코드 값을 매핑시킨 형태라고 보면 되겠다.
> 자바에서 key:M > text:남자, key:F > text:여자라는 코드값을 ArrayList<String>에서 for 문으로 반복시켜 매핑시키는 작업을
> R에서 factor(c("M","F","F"), levels("M","F"), labels=c("남자","여자"))로 표현. c("M","F","F")부분은 매핑시킬 벡터를 기술.

  - 순서(ordered)
    + 데이터가 순서형
> 자바와 딱히 비교할만한 객체가 없음.
> factor에서 특정 문자나 숫자에 대한 매핑이 된다고하면 ordered는 숫자,문자들을 오름차순으로 정렬 뒤 순서대로 라벨을 매핑. 문자도 abc순서대로 정렬되긴 함.
```R
AgeG <- c(4,2,3,2,1,3,2,4,0)
age <- ordered(AgeG,labels=c("10대","20대","30대","40대","50대"))
#결과 값: 50대      30대      40대      30대      20대  40대      30대      50대      10대
```
  
  - 행렬(Matrix)
    + 행렬은 행과 열의 원소를 나열한 것으로 벡터들의 모임
    + 행렬로 입력된 데이터를 이용해 행렬 연산을 수행
> 다른 변수형을 써도 컴파일이나 실행 할 때 오류는 나지 않으나, 자동으로 형변환이 되어 같은 자료형으로 캐스팅된다.
  
  - 배열(Array)
    + 행렬과 유사하나 2차원 이상의 행렬을 표현. 실무에서는 대부분 2차원 행렬을 이용함
> 이건 필요할 때 다시 공부하자
  
  - 데이터 프레임(Data Frame)
    + 행렬과 같은 구조로 이루어져 있으나, 열에 따라 데이터의 속성을 다르게 입력 가능
    + 통계분석에서 일반적으로 사용되는 데이터 저장소(데이터 셋)
> 메트릭스와 보기에는 비슷해 보이지만, 열 단위로 서로 다른 객체를 가질 수 있으므로, 범주형 자료분석에 사용
  
  - 리스트(List)
    + 벡터, 행렬, 데이터프레임 등의 데이터 개체를 원소로 갖는 객체
    + 여러 데이터 개체들을 한곳에 저장
> 여러 형태의 체를 원소로 가질 수 있다는 면에서 자바의 Map과 유사함.
