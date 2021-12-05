## **FrontEnd - 동작이 이상한 부분**

1. **Exercise** → db 테이블 내 exercise가 있는데도 선택이 안됨,
2. **Diet →** 음식 선택 후에도 테이블에 변화가 없음. → 새로고침 처리

---

# **Exercise** → db 테이블 내 exercise가 있는데도 선택이 안됨

front에서는 현재 axios를 통해서 컨트롤러에 접근하여 REST API통신을 하고 있습니다. 이때 axios는 브라우저, Node.js를 위해서 만들어진 **Promise(ES6) API를 활용하는 HTTP 비동기 통신 라이브러리 입니다.** 

이때 Promise란 코드의 실행 흐름서 비동기처리를 유연하게 처리하기 위한 API입니다. 아래는 실제 코드에서 사용중인 코드 입니다.

사용자 인증을 거치고 사용자의 데이터를 가져오기 위해 front에서는 REST API 통신을 하고 있습니다. REST API 설계시 첫번째로 URI는 정보의 자원을 표현해야 합니다. 그리고 두번째로 자원에 대한 행위는 HTTP Method(GET, POST, PUT, DELETE)로 표현합니다. 

```jsx
export const getWithAuth = (url: string) => {
    return axios.get(url, {
        headers: {
            authorization: `Bearer ${sessionStorage.getItem(TOKEN_KEY)}`
        }
    })
};

export const postWithAuth = (url: string, data: any) => {
    return axios.post(url, data, {
        headers: {
            authorization: `Bearer ${sessionStorage.getItem(TOKEN_KEY)}`
        }
    })
};
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/72ede145-143c-46af-8802-c8ea25aea685/Untitled.png)

이 때 사용자를 인증하기 위하여 OAuth 인증방식을 이용하고 있습니다.현재 헤더에 담긴 인증방식이 조금 생소하실 수도 있습니다. 

토큰에는 암호화 방식과 타입 등을 나타내는 **헤더**, 전송할 데이터가 담긴 **페이로드**, 토큰 검증을 위한 **서명**을 각각 인코딩(해싱)한 값이 포함되어 있습니다.

그리고 Bearer 는 토큰의 타입을 의미하고 JWT 혹은 OAuth에 대한 토큰을 사용한다는 의미입니다. 이를 활용하면 인터페이스 간 통신시 내용의 변경없이 실시간으로 통신이 가능합니다. 

```jsx
const handleClickAddExercise = () => {
        const today = new Date()
        const data = {
            'exerciseId': selectedExercise!!.id,
            'set': exerciseInputState.sets,
            'rep': exerciseInputState.reps,
            'weight': exerciseInputState.weight,
            'date': getFormatDate(today)
        };

        postWithAuth(`${process.env.REACT_APP_API_ENDPOINT}/exercise/histories`, data)
            .then(response => {
                alert('added exercise successfully')
		        **window.location.reload();**
            })
            .catch(e => {
                alert('add exercise fail')
                console.log(e)
            })
    };
```

그리고 전달받은 사용자 운동 정보 데이터는 리듀서를 통하여 새로운 데이터를 생성합니다. 리듀서를 한국어로 번역해보면 변화를 일으키는 것을 말합니다. 리듀서는 현재 상태와 액션 객체를 받아, 필요하다면 새로운 상태를 리턴하는 함수입니다. 액션 유형을 기반으로 이벤트를 처리하는 이벤트 리스너라고 생각하면 됩니다. 

이렇게 컴포넌트의 상태관리를 하게 된다면 행동의 종류가 늘어나더라도 그에 따라 카운트 값이 어떻게 변하는지를 reducer 함수 안에 일목요연하게 정리할 수 있습니다

```jsx
const [exerciseInputState, dispatchExerciseInput] = useReducer(exerciseInputReducer, exerciseInputInitialState);
```

```jsx
export interface ExerciseInputState {
    weight: number
    reps: number
    sets: number
}

enum ExerciseInputEnum {
    WEIGHT = 'weight',
    REPS = 'reps',
    SETS = 'sets',
}

export interface ExerciseInputAction {
    type: ExerciseInputEnum
    value: string | number
}

export const exerciseInputReducer = (state: ExerciseInputState, action: ExerciseInputAction) => {
    return {
        ...state,
        [action.type] : action.value
    }
};

export const exerciseInputInitialState: ExerciseInputState = {
    weight: 0,
    reps: 0,
    sets: 0
};
```

**자 그렇다면 왜 우리는 DB에 저장된 내역이 출력되지 않았을까요?** 문제는 간단합니다. 중요한 점은 front에서의 object를 확인해보면 대문자로 적혀있는 걸 확인 할 수 있습니다. 하지만 안타깝게도 DB table 생성시 PART에 소문자를 넣어놔서 문제가 됐던 것이지요. 

보이시나요? Redux 패턴이 적용된 코드에서 pros에 대한 값 변화를 체크하기 위해 useState를 이용하고 있습니다. 그런데 front에서 저장한 ExercisePart와 다른 값이 DB에서 요청된다면 값이 바뀌지 않을 것이고 렌더링도 일어나지 않을 것입니다. 

```jsx
const ExerciseListContainer = (props: Props) => {
    const [exercises, setExercises] = useState<Array<Exercise>>([]);
    const [selectedExercise, setSelectedExercise] = useState<Exercise>();
    const [exerciseInputState, dispatchExerciseInput] = useReducer(exerciseInputReducer, exerciseInputInitialState);

    useEffect(() => {
        getWithAuth(`${process.env.REACT_APP_API_ENDPOINT}/exercise?part=${props.selectedPart}`)
            .then(response => response.data)
            .then(data => {
                setExercises(data)
            })
    }, []);
```

```jsx
interface Props {
    selectedPart: ExercisePart
    handleClickBackButton: () => void
}
```

```jsx
export enum ExercisePart {
    CHEST = "CHEST",
    BACK = "BACK",
    LEG = "LEG",
    SHOULDER = "SHOULDER",
    BICEPS = "BICEPS",
    TRICEPS = "TRICEPS",
    ABDOMEN = "ABDOMEN"
}
```

# **Diet →** 음식 선택 후에도 테이블에 변화가 없음. → 새로고침 처리

리액트의 구조에 대해 이해해볼 필요가 있습니다. 프론트엔드에서 리액트는 CSR(Client Side Rendering)을 도와줄 수 있도록 하는 웹 라이브러리입니다. 기존의 코드는 Server에 저장되어 있는 파일의 주소로 가서 화면에 불러왔지만 CSR은  **자바스크립트 파일을 브라우저에서 해석해 렌더링하는 방식입니다.** 클라이언트가 자바스크립트 파일을 브라우저에서 해석한 후 가상 DOM에 HTML을 렌더링합니다. 

자 그렇다면 만약 서버에서 요청한 데이터가 변경되었다면 CSR은 이를 어떻게 처리할까요? CSR에서는 새로운 데이터가 반환되어야만 이를 인식하고 가상 DOM에 렌더링하게 됩니다. 컴포넌트에서 보여줘야 하는 내용이 사용자 인터랙션에 따라 바뀌어야 할 때 어떻게 구현하기 위해서 리액트 16.8 에서 Hooks 라는 기능이 도입되면서 함수형 컴포넌트에서도 상태를 관리할 수 있게 되었습니다. **useState** 라는 함수를 이용하면 됩니다! 

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/202c0442-dad0-4ee4-aced-df2bd2065577/Untitled.png)

위의 코드에서도 확인했지만 아래의 코드를 보면 useState hook fucntion을 사용하여 동적으로 데이터의 상태를 확인하고 있습니다. 

```jsx
const DietDetailContainer = () => {
    **const [foods, setFoods] = useState<Array<Food>>();
    const [dailyDiet, setDailyDiet] = useState<Array<DietDetail>>();
    const [selectedFoodId, setSelectedFoodId] = useState<number>();**

    useEffect(() => {
        getWithAuth(`${process.env.REACT_APP_API_ENDPOINT}/diet?q=`)
            .then(response => response.data)
            .then(data => {
                setFoods(data)
            })

        getWithAuth(`${process.env.REACT_APP_API_ENDPOINT}/diet/today`)
            .then(response => response.data)
            .then(data => {
                setDailyDiet(data)
            })
    }, []);
```

그런데 한가지 문제점이 존재해요. 우리의 코드는 상태가 변하는건 동적으로 확인하였지만 컴포넌트가 마운트 됐을 때 화면에 동적으로 출력될 수 있는 함수를 생성하지 않았죠. 이렇게 해주지 않으면 CSR의 특성상 DOM에 렌더링 되지 않아요. 

**그래서 가장 간단한 방법을 선택했어요. 만약 새로운 데이터를 지정하는 이벤트가 발생했다면 화면을 새로 랜더링 해주면 됩니다! 물론 각 컴포넌트의 렌더링을 해주면 좋지만 현재로선 가장 빠르고 정확한 해결책입니다.** 

```jsx
const handleClickAddFood = () => {
        const today = new Date();
        const data = {
            foodId: selectedFoodId,
            date: getFormatDate(today)
        };

        postWithAuth(`${process.env.REACT_APP_API_ENDPOINT}/diet/histories`, data)
            .then(response => {
                alert('added diet successfully');
						**window.location.reload();**
            })
            .catch(e => {
                alert('add diet fail')
                console.log(e)
            })
    };
```
