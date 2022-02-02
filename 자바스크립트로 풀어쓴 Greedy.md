그리디 알고리즘은 선택의 순간마다 당장 눈 앞에 보이는 최적의 상황만을 쫓아 최종적인 해답에 도달하는 방법입니다. 
그리디 알고리즘을 해결하기 위해서는 최적이라 생각되는 해답을 찾고 이를 통해 최종 문제의 해답에 도달하는 방식으로 접근해야합니다. 

> locally optimal solution → globally optimal solution 

그러나 이런 방법은 항상 최적의 결과를 보장하지는 않습니다. 

따라서 두 가지의 조건을 만족하는 “특정한 상황”이 아니면 탐욕 알고리즘은 최적의 해를 보장하지 못합니다. 그래서 두 가지의 조건을 성립해야 합니다. 

> 1. 탐욕적 선택 속성(Greedy Choice Property) : 앞의 선택이 이후의 선택에 영향을 주지 않는다. 
2. 최적 부분 구조(Optimal Substructure) : 문제에 대한 최종 해결 방법은 부분 문제에 대한 최적 해결 방법으로 구성된다. 

탐욕 알고리즘은 항상 최적의 결과를 도출하는 것은 아니지만, 어느 정도 최적에 근사한 값을 빠르게 도출할 수 있다는 장점이 있습니다. 

## 짐나르기(backpack Problem)
> 
김코딩과 박해커는 사무실 이사를 위해 짐을 미리 싸 둔 뒤, 짐을 넣을 박스를 사왔다. 박스를 사오고 보니 각 이사짐의 무게는 들쭉날쭉한 반면, 박스는 너무 작아서 한번에 최대 2개의 짐 밖에 넣을 수 없었고 무게 제한도 있었다.
예를 들어, 짐의 무게가 [70kg, 50kg, 80kg, 50kg]이고 박스의 무게 제한이 100kg이라면 2번째 짐과 4번째 짐은 같이 넣을 수 있지만 1번째 짐과 3번째 짐의 무게의 합은 150kg이므로 박스의 무게 제한을 초과하여 같이 넣을 수 없다.
**박스를 최대한 적게 사용하여 모든 짐을 옮기려고 합니다.**
짐의 무게를 담은 배열 stuff와 박스의 무게 제한 limit가 매개변수로 주어질 때, 모든 짐을 옮기기 위해 필요한 박스 개수의 최소값을 return 하도록 movingStuff 함수를 작성하세요.

이 문제를 해결하기 위해서는 최대 2개의 짐을 넣을 수 있으니 2개를 같이 옮기는 것이 효율적일 것이다. 그래서 가장 무거운 것과 가장 가벼운 것을 박스에 넣는 시도를 위해 정렬을한다. 

그게 안된다면 가장 무거운 것 부터 나른다.

```jsx
function movingStuff(stuff, limit){
	let count = 0; //옮긴 횟수
	let sortedStuff = stuff.sort((a,b) => a - b);

	while (sortedStuff.length !== 0){
		if(sortedStuff[0] + sortedStuff[sortedStuff.length-1] <= limit){
			count++;
			sortedStuff.shitf(); //shift() 메서드는 배열에서 첫 번째 요소를 제거하고, 제거된 요소를 반환합니다. 이 메서드는 배열의 길이를 변하게 합니다.
			sortedStuff.pop();
		}else{
			count++;
			sortedStuff.pop(); //가장 무거운 걸 나른다.
		}
	}
	return count;
}
```

**.push()** : 배열의 맨 끝에 값을 추가한다.

**.unshift()** : 배열의 맨 앞에 값을 추가한다.

**.pop()** : 배열의 맨 끝에 값을 제거한다.

**.shift()** : 배열의 맨 앞에 값을 제거한다.

**`sort()`** 메서드는 배열의 요소를 적절한 위치에 정렬한 후 그 배열을 반환합니다. 정렬은 [stable sort](https://en.wikipedia.org/wiki/Sorting_algorithm#Stability)가 아닐 수 있습니다. 기본 정렬 순서는 문자열의 유니코드 코드 포인트를 따릅니다.

`arr.sort([compareFunction])`

`compareFunction`이 제공되지 않으면 요소를 문자열로 변환하고 유니 코드 코드 포인트 순서로 문자열을 비교하여 정렬됩니다. 예를 들어 "바나나"는 "체리"앞에옵니다. 숫자 정렬에서는 9가 80보다 앞에 오지만 숫자는 문자열로 변환되기 때문에 "80"은 유니 코드 순서에서 "9"앞에옵니다.

따라서 compare 함수의 형식은 다음과 같습니다.

```jsx
function compare(a, b) {
  if (a is less than b by some ordering criterion) {
    return -1;
  }
  if (a is greater than b by the ordering criterion) {
    return 1;
  }
  // a must be equal to b
  return 0;
}
```

```jsx
var numbers = [4, 2, 5, 1, 3];
numbers.sort(function(a, b) {
  return a - b;
});
console.log(numbers);

// [1, 2, 3, 4, 5]

//es5문법부터 화살표 함수를 제공하고 있기 때문에 더 간결하게 표현이 가능하다. 
let numbers = [4, 2, 5, 1, 3];
numbers.sort((a, b) => a - b);
console.log(numbers);

// [1, 2, 3, 4, 5]
```

## 편의점 알바(Coin Change Problem)

### 문제

> 편의점에서 아르바이트를 하고 있는 중에, 하필이면 피크 시간대에 손님에게 거스름돈으로 줄 동전이 부족하다는 것을 알게 되었습니다.현재 가지고 있는 동전은 1원, 5원, 10원, 50원, 100원, 500원으로 오름차순으로 정렬되어 있고, 각 동전들은 서로 배수 관계에 있습니다.
동전 개수를 최소화하여 거스름돈 K를 만들어야 합니다. **이때, 필요한 동전 개수의 최솟값을 구하는 함수를 작성해 주세요.**

이때 동전의 관계를 보면 모두 배수관계를 가지고 있습니다. 그렇기 때문에 탐욕선택 속성과 최적 부분 구조에 적용이 쉽게 될거라 생각됩니다. 필요한 동전의 최솟값을 구하는 문제이기 때문에 가장 큰 동전 부터(Most Significant Digit) 주면 해결될 것 같습니다.

```jsx
function partTimeJob(k){
	let count = 0; 
	const arr = [500,100,50,10,5,1];
	for(let item of arr){
		count = count + Math.floor(k/item); //동전의 갯수
		k = k - item * Math.floor(k/item); //남은 돈 계산
	}
	return count; 
}
```
**`sort()`** 메서드는 배열의 요소를 적절한 위치에 정렬한 후 그 배열을 반환합니다. 정렬은 [stable sort](https://en.wikipedia.org/wiki/Sorting_algorithm#Stability)가 아닐 수 있습니다. 기본 정렬 순서는 문자열의 유니코드 코드 포인트를 따릅니다.

`arr.sort([compareFunction])`

`compareFunction`이 제공되지 않으면 요소를 문자열로 변환하고 유니 코드 코드 포인트 순서로 문자열을 비교하여 정렬됩니다. 예를 들어 "바나나"는 "체리"앞에옵니다. 숫자 정렬에서는 9가 80보다 앞에 오지만 숫자는 문자열로 변환되기 때문에 "80"은 유니 코드 순서에서 "9"앞에옵니다.

따라서 compare 함수의 형식은 다음과 같습니다.

```jsx
const array1 = ['a', 'b', 'c'];

for (const element of array1) {
  console.log(element);
}

// expected output: "a"
// expected output: "b"
// expected output: "c"
```

**`Math.floor()`** 함수는 주어진 숫자와 같거나 작은 정수 중에서 가장 큰 수를 반환합니다. 반대로 Math.ceil() 함수는 주어진 숫자보다 크거나 같은 숫자 중 가장 작은 숫자를 integer 로 반환합니다.

```jsx
console.log(Math.floor(5.95));
// expected output: 5

console.log(Math.floor(5.05));
// expected output: 5

console.log(Math.floor(5));
// expected output: 5

console.log(Math.floor(-5.05));
// expected output: -6

Math.ceil(.95);    // 1
Math.ceil(4);      // 4
Math.ceil(7.004);  // 8
Math.ceil(-0.95);  // -0
```
