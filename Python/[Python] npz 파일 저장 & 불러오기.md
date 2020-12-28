# npz 파일 저장 & 불러오기

## 🤿준비물

- 배열
- Numpy



## npz 파일이 뭔데?

Numpy 라이브러리에서 지원하는 파일 포맷 중 하나. 여러 개의 배열을 저장할 때 사용한다.

한 개의 배열만 저장할 땐 `.npy` 포맷을 사용한다.



## 저장하기

### (1) np.savez()

여러 개의 배열을 압축하지 않은 1 개의 `.npz`파일로 저장한다.

```python
numpy.savez('파일명', <저장할 배열>)
```



**$ 예시**

```python
import numpy as np

x = np.array([[0,1,2,3,4,5,6], [2,3,4,5,6,7,8], [9,10,11,12,13,14,16]])
y = np.array([0,1,3,4,5,6,7])

np.savez('text.npz', X=x, Y=y) # X=x : 배열마다 이름을 부여한다.
```



### (2) np.savez_compressed()

여러 개의 배열을 압축된 1 개의 `.npz` 파일로 저장한다.

```python
numpy.savez_compressed()
```



**$ 예시**

```python
import numpy as np

np.savez_compressed('test_comp.npz', X=x, Y=y)
```



## 불러오기

저장했던 `.npy` 혹은 `.npz` 파일을 불러와 조회한다.

### np.load()

```python
numpy.load('파일명')
```



**$ 예시**

```python
import numpy as np

b = np.load('./test_comp.npz')

## 객체 타입
print(type(b)) 
# <class 'numpy.lib.npyio.NpzFile'>

## 저장된 배열
print(b.files)
# ['X', 'Y']

## 배열의 길이
print(len(b['X'])) 
# 3
print(len(b['Y']))
# 7

## 배열 원소
print(b['X'])
# [[ 0  1  2  3  4  5  6]
#  [ 2  3  4  5  6  7  8]
#  [ 9 10 11 12 13 14 16]]
print(b['Y'])
# [0 1 3 4 5 6 7]

## 원소의 총 개수
print(b['X'].size)
# 21
print(b['Y'].size)
# 7

## 배열 구조
print(b['X'].shape)
# (3, 7)
print(b['Y'].shape)
# (7,)
```



## 📜참고

- [numpy의 array를 저장하고 읽어보자[Website].(2020.12.11)](https://jangjy.tistory.com/330)

