# Space complexity

1. Wgat is it? 
   where as time complexity measures the time to run program, space complexity measures memory usage of a specific program

2. What makes sapce complexity increase? 
   1. Assigning variables
   2. Creating new data structures
   3. Function calling and allocation

```javascript
//Space complexity looks at the memory usage of a function relative to size of the input

function test(arr){
    for(let i = 0; i <arr.length; i++){ //O(n) O(1)
        console.log(arr[i]);
    }
}

function test2(arr){ //O(n) O(n)
    let count = 0; 

    for (let i = 0; i < arr.length; i++){
        result[i] = arr[i];
        count++;
    }

    console.log(count); // 5
    return result;
}

const arr = [1,2,3,4,5];
console.log(test2(arr)); //[1,2,3,4,5];

```
