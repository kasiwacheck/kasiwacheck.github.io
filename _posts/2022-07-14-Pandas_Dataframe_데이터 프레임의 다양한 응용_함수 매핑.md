---
title:  "Pandas_Dataframe_데이터프레임 응용_함수매핑"
date: 2022-07-14 14:31:00
categories: pandas
---
Pandas: chapter 6 데이터프레임의 다양한 응용
==========
1.함수매핑
----------
### 1-1 개별원소에 함수맵핑
#### - 시리즈 원소에 함수 맵핑
```
import seaborn as sns

titanic = sns.load_dataset('titanic')
df = titanic.loc[:,['age','fare']] # titanic에서 age, fare열을 선택하여 df 만들기 row자리에 :, column자리에 ['age','fare']를 놓음
df['ten']=10
print(df.head())

def add_10(n):
    return n + 10

def add_two_obj(a,b):
    return a+b

print(add_10(10))
print(add_two_obj(10, 10))
#시리즈 객체에 적용
#apply() 메소드를 사용하여 각 열에 함수를 매핑할 수 있다.
sr1 = df['age'].apply(add_10)
print(sr1.head())
print('\n')
#시리즈 객체와 숫자에 적용(시리즈+숫자)
sr2 = df['age'].apply(add_two_obj, b=10)
print(sr2.head())
print('\n')
#lambda 함수 사용: 시리즈 객체에 적용
sr3 = df['age'].apply(lambda x: add_10(x))
print(sr3.head())
```
#### - 데이터 프레임 원소에 함수 매핑
```
import seaborn as sns
#df정의
titanic = sns.load_dataset('titanic')
df = titanic.loc[:,['age','fare']]

print(df.head())
#함수정의
def add_10(n):
    return n+10
#데이터프레임에 applymap()을 이용하여 add_10()함수를 매핑함
df_map=df.applymap(add_10)
print(df_map.head())
```
### 1-2 시리즈 객체에 함수 매핑
#### - 데이터 프레임 각열에 함수 매핑 axis=0
```
import seaborn as sns

titanic = sns.load_dataset('titanic')
df = titanic.loc[:, ['age', 'fare']]

print(df.head())
print('\n')

def missing_value(series):
    return series.isnull()

result = df.apply(missing_value, axis=0)

print(result.head())
print('\n')
print(type(result))


#하나의 값을 매핑하는 함수는 시리즈로 반환함. 그래서 axis=0을 설정하지 않아도 됨
import seaborn as sns

titanic = sns.load_dataset('titanic')
df = titanic.loc[:, ['age', 'fare']]

print(df.head())
print('\n')

def min_max(x):
    return x.max()-x.min()

result = df.apply(min_max)
print(result.head())
print('\n')
print(type(result))
```
#### -데이터 프레임 각행에 함수 매핑 axis=1
```
import seaborn as sns

titanic = sns.load_dataset('titanic')
df = titanic.loc[:,['age', 'fare']]
df['ten']= 10
print(df.head())
print('\n')

def add_two_obj(a, b):
    return a+b

df['add'] = df.apply(lambda x: add_two_obj(x['age'], x['ten']), axis = 1)

print(df.head())
```
### 1-3 데이터프레임 객체에 함수 매핑 pipe()로 함수매핑
```
import seaborn as sns

titanic = sns.load_dataset('titanic')
df = titanic.loc[:, ['age','fare']]

def missing_value(x):#각 열의 NAN찾기-데이터프레임 전달하면 데이터프레임 반환
    return x.isnull()
def missing_count(x):#각 열의 NAN 개수 반환-데이터프레임 전달하면 시리즈 반환
    return missing_value(x).sum()
def total_number_missing(x):#데이터프레임의 총 NAN 개수-데이터프레임 전달하면 값 반환
    return missing_count(x).sum()

result_df = df.pipe(missing_value)
print(result_df)
print('\n')
result_series = df.pipe(missing_count)
print(result_series)
print('\n')
result_value = df.pipe(total_number_missing)
print(result_value)
```