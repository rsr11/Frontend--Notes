# Frontend-Notes
Here is all the notes that i think is import for me regardinging react and frontend .

## Lets talk about react-optimization
Memoization
- memo
- UseMemo
- UseCallBack

 Derived State
- It's useless to make another state just to store the prop and to check if it changed or not as , our component will  rerender if that will happen.

Debounce/Throttle
- It's best to stop sending multiple api on a shorter time period, we should make a function for debouncing and throttling too (throttling help to prevent multiple dom changes in a shorter time period).

Lazy Loading
- it's mean we don't have to download everthing at starting because of that at starting the page load late, but if we download the basic file that we have to show at frontend then when ever we require that code aur api call, we aur assest we can download it at that time and between that time we will show loader for good user experience.

"After React 19v"

react-compilar
-now no need for useMemo,React.memo and useCallback 



