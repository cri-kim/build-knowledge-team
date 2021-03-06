# 배열

## 두 수의 합(two-sum)

https://leetcode.com/problems/two-sum/

- 풀이 1

  Brute force

## 빗물 트래핑(trapping-rain-water)

https://leetcode.com/problems/trapping-rain-water/

- 풀이 1

  Brute force

## 세 수의 합(3sum)

https://leetcode.com/problems/3sum/

- 풀이 1

  Brute force

## 배열 파티션 I(array-partition-i)

https://leetcode.com/problems/array-partition-i/

1. Array.prototype.sort() 만 사용할 경우 원하는 정렬 값을 얻을 수 없다. 왜냐하면 배열의 요소 값을 문자열로 변환하여 유니코드 값을 비교하기 때문이다. 따라서 함수를 따로 만들어줘야 한다.

```JavaScript
// 오름차순
const compareDesc = (a, b) => {
  return nums.sort((a, b) => a - b);
};

// 내림차순
const compareAsc = (nums) => {
  return nums.sort((a, b) => b - a);
};
```

## 자신을 제외한 배열의 곱(product-of-array-except-self)

https://leetcode.com/problems/product-of-array-except-self/

- 풀이 1

  Brute force [Wrong Answer > 시간 초과]

1. reduce 는 배열의 각 요소에 대해 callback 을 실행하여 단 1개의 값을 출력한다.

```JavaScript
Array.prototype.reduce(previousValue, currentValue) => previousValue * currentValue
```

2. 배열의 startIndex부터 endIndex(필수값 아님)에 대한 얕은 복사본을 만들어 새로운 배열 객체로 반환한다. 원본 배열은 바뀌지 않는다.

```JavaScript
Array.prototype.slice(startIndex, endIndex)
```

- 풀이 2

| array  | nums[0]           | nums[1]        | nums[2]       | nums[3]           |
| ------ | ----------------- | -------------- | ------------- | ----------------- |
| value  | 1                 | 2              | 3             | 4                 |
| left   | 1                 | 1              | 1\*2          | 1\*2\*3           |
| right  | 2\*3\*4           | 3\*4           | 4             | 1                 |
| answer | (1)\*(2\*3\*4)=24 | (1)\*(3\*4)=12 | (1\*2)\*(4)=8 | (1\*2\*3)\*(1) =6 |

## 주식을 사고팔기 가장 좋은 시점(best-time-to-buy-and-sell-stock)

https://leetcode.com/problems/best-time-to-buy-and-sell-stock/

#### [Wrong Answer]

- 풀이 1

  Brute force [Wrong Answer > 시간 초과]

1. 인자로 받은 0개 이상의 숫자 중 가장 큰 숫자를 반환한다.

```JavaScript
Math.max(1, 3, 2) // 3
```

- 풀이 2

1. 가장 큰 상수를 반환한다.

```JavaScript
Number.MAX_SAFE_INTEGER; // 9007199254740991
```

2. 인자로 받은 0개 이상의 숫자 중 가장 작은 숫자를 반환한다.

```JavaScript
Math.max(1, 3, 2) // 1
```
