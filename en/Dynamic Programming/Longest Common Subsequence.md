# Longest Common Subsequence

#### Problem Statement

Given two strings `S` and `T`, find the length of the longest common subsequence (<b>LCS</b>).

#### Approach

Let the `dp[i][j]` be the length of the longest common subsequence of prefixes `S[1..i]` and `T[1..j]`. Our answer (the length of LCS) is `dp[|S|][|T|]` since the prefix of the length of string is the string itself.

Both `dp[0][i]` and `dp[i][0]` are `0` for any `i` since the LCS of empty prefix and anything else is an empty string.     

Now let's try to calculate `dp[i][j]` for any `i`, `j`. Let's say `S[1..i] = *A` and `T[1..j] = *B` where `*` stands for any sequence of letters (could be different for `S` and `T`), `A` stands for any letter and `B` stands for any letter different from `A`. Since `A != B`, our LCS doesn't include `A` or `B` as a last character. So we could try to throw away `A` or `B` character. If we throw `A`, our LCS length will be `dp[i - 1][j]` (since we have prefixes `S[1..i - 1]` and `T[1..j]`). If we try to throw `B` character, we will have prefixes `S[1..i]` and `T[1..j - 1]` so the length of LCS will be `dp[i][j - 1]`. As we are looking for the <b>Longest</b> common subsequence, we will pick <b>the maximum value</b> from `dp[i][j - 1]` and `dp[i - 1][j]`.

But what if `S[1..i] = *A` and `T[1..j] = *A`? We could say that the LCS of our prefixes is LCS of prefixes `S[1..i - 1]` and `T[1..j - 1]` <b>plus</b> the letter `A`. So `dp[i][j] = dp[i - 1][j - 1] + 1` if `S[i] = T[j]`.

We could see that we can fill our `dp` table row by row, column by column. So our algorithm will be like:

- Let's say that we have strings `S` of the length N and `T` of the length M (numbered from 1). Let's create the table `dp` of size `(N + 1) x (M + 1)` numbered from 0.
- Let's fill the 0th row and the 0th column of `dp` with 0.
- Then, we follow the algorithm:
```
for i in range(1..N):
    for j in range(1..M):
        if(S[i] == T[j])
            dp[i][j] = dp[i - 1][j - 1] + 1
        else
            dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])
```


#### Time Complexity

`O(N * M)` In any case     

#### Space Complexity

`O(N * M)` - simple implementation
`O(min {N, M})` - two-layers implementation (as `dp[i][j]` depends on only i-th and i-th layers, we coudld store only two layers).

#### Example

Let's say we have strings `ABCB` and `BBCB`. We will build such a table:
```
# # A B C B
# 0 0 0 0 0
B 0 ? ? ? ?
B 0 ? ? ? ?
C 0 ? ? ? ?
B 0 ? ? ? ?
```
Now we will start to fill our table from 1st row. Since `S[1] = A` and `T[1] = B`, the `dp[1][1]` will be the maximal value of `dp[0][1] = 0` and `dp[1][0] = 0`. So `dp[1][1] = 0`. But now `S[2] = B = T[1]`, so `dp[1][2] = dp[0][1] + 1 = 1`. `dp[1][3]` is `1` since `A != C` and we pick `max{dp[1][2], dp[0][3]}`. And `dp[1][4] = dp[0][3] + 1 = 1`.
```
# # A B C B
# 0 0 0 0 0
B 0 0 1 1 1
B 0 ? ? ? ?
C 0 ? ? ? ?
B 0 ? ? ? ?
```
Now let's fill the other part of the table:
```
# # A B C B
# 0 0 0 0 0
B 0 0 1 1 1
B 0 0 1 1 2
C 0 0 1 2 2
B 0 0 1 2 3
```
So the length of LCS is `dp[4][4] = 3`.

#### Code Implementation Links

- [Java](https://github.com/TheAlgorithms/Java/blob/master/Dynamic%20Programming/LongestCommonSubsequence.java)
- [Python](https://github.com/TheAlgorithms/Python/blob/master/dynamic_programming/longest_common_subsequence.py)
- [C++](https://github.com/TheAlgorithms/C-Plus-Plus/blob/master/Dynamic%20Programming/Longest%20Common%20Subsequence.cpp)

#### Video Explanation

[Video explanation by Tushar Roy](https://youtu.be/NnD96abizww)
