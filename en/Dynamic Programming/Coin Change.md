# Coin Change

#### Problem Statement
Given a value `N`, if we want to make change for `N` cents, and we have infinite supply of each of `S = {S1, S2, .. , Sm}` valued coins, how many ways can we make the change? The order of coins doesn’t matter.

#### Approach
Let the `dp[i]` be the length of the coin change of prefix `N[1..i]`. Our answer is `dp[N]`.
We fill `dp[0]` as 1 because there is only one way to get 0 coins (We pick no coins).

Now let's try calculate `dp[i]` for any `i`. `dp[0..i]` will store each sub problems from `0` to `N`. That's why the answer will be `dp[N]`. First, we need to iterate each coin types to pick them one-by-one. Then we iterate the sub problems from current coin that we pick before  to `N` cents. That's why we must make dp table with `N` columns.

This is the codes for the Coin Change algorithm:
```
    for coin_val in S:
        for i in range(coin_val, n + 1):
            dp[i] += dp[i - coin_val]
```

In the second iteration, for every cent that can be exchanged, we take it by subtracting the i-th column by the value of the coin we take and adding it into the current column. So `dp[i]` will store the current sub problem.

#### Time Complexity
`O(N * S)` in any case

#### Space Complexity
`O(N)` - simple implementation. We only need 1D array to store the answer.

#### Example
Let's say we have 3 coin types `[1,2,3]` and we want to change for `7` cents. So we will define our table like this.
```
[1, 0, 0, 0, 0, 0, 0, 0]
```
0th column will store 1 since there is only one way to get 0 cents.

* For the first iteration we take a coin that has a value of 1. Then for all sub problems, there is only one way to make change. For 7 cents, the only way is `{1,1,1,1,1,1,1}`. On the final iteration, our table be like:
```
[1, 1, 1, 1, 1, 1, 1, 1]
```

* For the second iteration, we take a coin that has a value of 2. From here, all sub problems that can be divided by 2 will store another new way to make change. So, when the iteration stopped at 2nd column it will be like `dp[2] += dp[0]`. We know that `dp[0]` stored a value of 1. Thus, dp[2] will store the value of `1 + 1 = 2`. From here we know that for 2 cents, there are 2 ways `{1,1}` and `{2}`. And this operation will continue. Now our table be like:
```
[1, 1, 2, 2, 3, 3, 4, 4]
```
4 ways to make 7 cents using value of 1 and 2. `{{1,1,1,1,1,1,1}, {1,1,1,1,1,2}, {1,1,1,2,2}, {1,2,2,2}}`

* For the final iteration (3rd iteration), we take a coin that has a value of 3. Like before, now all the columns that can be divided by 3 will store another new way. And the final result will be like:
```
[1, 1, 2, 3, 4, 5, 7, 8]
```
So the final answer is **8**. 8 ways to make change of 7 cents using all coin types. `{{1,1,1,1,1,1,1}, {1,1,1,1,1,2}, {1,1,1,2,2}, {1,2,2,2}, {1,1,1,1,3}, {1,3,3}, {2,2,3}, {1,1,2,3}}`

#### Code Implementation Link
[Python](https://github.com/TheAlgorithms/Python/blob/master/dynamic_programming/coin_change.py)

#### Video Explanation
[Total Unique Ways To Make Change by Back To Back SWE](https://www.youtube.com/watch?v=DJ4a7cmjZY0)
