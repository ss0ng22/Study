### N111 EDA (1)

### 오늘의 키워드
- EDA
- Pandas 메서드 활용
- Feature Engineering
- Business Insight

### 개념 정리
**EDA는 무엇인가?** </br>
EDA는 본격적으로 분석에 들어가기에 앞서 데이터에 대해 뜯어보는 과정 -> 시각화, 통계치 활용할 수 있음

**EDA가 왜 필요하지?** </br>
EDA없이 바로 분석에 들어가는 것은 재료에 대해 살펴보지 않고 요리를 하는 것 </br>
우리가 분석을 하는 이유는 **데이터 안에서 유의미한 Insight를 얻기 위함**인데, 데이터에 대한 이해 없이 이러한 목적을 달성할 수 없음 </br>
+) 문제 정의 과정이나 데이터 수집 과정에서 발견하지 못했던 **문제들을 발견**할 수 있음 (GIGO랑 연결되는듯?)

**Feature Engineering** </br>
주어진 column들을 재조합하여 의미있는 feature를 만들어내는 과정

### Lecture Note에서 나온 코드 정리
- `.shape`: 데이터프레임의 행과 열 개수 파악
- `.info()`: 데이터의 개괄적인 특징 파악 (column 이름, 결측치 여부, 데이터 타입)
- `.groupby()`: 특정 column을 기준으로 정렬해주는 함수
  - [관련 레퍼런스](https://rfriend.tistory.com/383)
- `.append()`: 데이터프레임 합치기 (리스트에 넣을 때에도 쓸 수 있음)
- `.query()`: 데이터프레임에서 필요한 부분 필터링 할 때 사용

### 좀 더 찾아봐야 하는 내용
- [ ] `.info()`랑 `.describe()`랑 어떤 차이가 있는지
- [ ] `seaborn`, `matplotlib` 라이브러리 활용한 시각화 구현 방법
- [ ] `.query()` 잘 사용하는 방법 -> 너무 어려움...ㅠㅠ

## N113

## 💡 **Data wrandling**
![image](https://user-images.githubusercontent.com/115196489/195279181-98625e75-882d-4516-801d-56adf3710924.png)

정제되지 않은 Raw Data 로는, 데이터 분석을 통한 정보를 추출하고 insight를 도출하기 어려울 수 있다.
따라서 Raw Data를 보다 쉽게 접근/분석할 수 있도록 데이터를 정리하고 통합하는 과정인 Data Wrangling을 시행해야 한다.

---
## 💡 **결측치 처리 방식**

### 1️⃣ 결측치 제거

1. 특정 변수에 결측치가 있을 경우 그 행 제거
 - filter(!is.na(변수명))

2. 행에 결측치가 하나라도 있으면 그 행 제거
 - na.omit(df)
 - complete.cases(df)

3. 기타
 - 결측치 행 단위로 제거: df.dropna()
 - 결측치 열 단위로 제거: df.dropna(axis=1)
 
### 2️⃣ 결측치 대체

1. 특정 변수의 결측값을 모두 동일한 값으로 대체
 - 데이터명$변수명[is.na(데이터명$변수명)]<-"대체할 문자"

2. 그룹별 결측치 제외 평균값 확인
 - tapply(데이터명$결측치 있는 변수명,데이터명$변수명(그룹),na.rm = TRUE,mean)

3. 그룹별 결측치를 그룹별 통계량으로 대체
 - 데이터명$결측변수명[is.na(데이터명$그룹변수명 == "대체할 문자"&데이터명$결측변수명)]<-연속형데이터

4. 기타
 - 특정값으로 채우기 : df.fillna(0)
 - 앞에 값으로 채우기 : df.fillna(method=‘pad’)
 - 뒤에 값으로 채우기 : df.fillna(method=‘bfill’)
 - replace함수 : df.replace(to_replace=np.nan, value=-50)
 - interpolate함수: df.interpolate(method=‘linear’, limit_direction= ‘forward’)


### 3️⃣ Conclusion

1. 컬럼에 결측치가 많을 경우 결측치를 제거하는 것이 유리.
2. 컬럼에 random한 null값이 있거나, 계산하거나 통계값을 내야하는경우 결측치를 대체하는 것이 유리.

---
## 💡 **object와 category의 차이점**

### 1️⃣ Object
판다스에서는 문자열을 object라는 자료형으로 나타낸다. 파이썬에서는 문자열을 string이라고 하지만, 판다스는 object라고 합니다

### 2️⃣ Category
category는 문자열을 직접 저장하지 않고, 출현한 문자열의 유일 값으로 Index - 문자열을 만들어 놓고 실제 저장소에  index만을 저장하는 형태이다.
따라서 유일 값이 적을 때는 아주 많은 메모리를 절약할 수 있다.

<예시>
- 사이즈 (X-Small, Small, Medium, Large, X-Large)
- 색깔 (빨강, 검정, 흰색)
- 스타일 (반팔, 긴팔)

pandas에서는 category 데이터를 어떻게 표현할 수 있는걸까?
category data type은 hybrid data type이다.
보기에는 string처럼 보이나 내부적으로는 integer의 배열로 표현이 되어있다.
이를 통해 사용자가 원하는대로 순서를 정해 저장할 수도 있고, 효율적으로 데이터 저장이 가능해진다.

### 3️⃣ Conclusion
object를 category로 변경하였을 때 4배~7배 메모리를 줄였다는 레퍼런스를 확인하였다.
category는 엑셀 필터 거는 것과 유사하다고 생각함.
