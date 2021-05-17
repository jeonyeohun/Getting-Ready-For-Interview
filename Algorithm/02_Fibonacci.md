# 피보나치 수열 구현 



## 재귀

```c++
int fibonacci (int n){
 if (n < 2){
   return n;
 }
  
  return fibonacci(n - 1) + fibonacci(n - 2);
}
```



## 메모이제이션

```c++
int memo[100] = {0,};
int fibonacci (int n){ 
 if (n < 2){
   return n;
 }
 if (memo[n]) return memo[n];
 return memo[n] = fibonacci(n - 1) + fibonacci(n - 2);
}
```



## 다이나믹 프로그래밍 - Bottom Up

~~~c++
int fibonacci (int n){
  int dp[n];
  dp[1] = 1, dp[2] = 1;
  
  for (int i = 3 ; i <= n ; i++){
    dp[i] = dp[i - 1] + dp[i - 2];
  }
  
  return dp[n];
}
~~~



