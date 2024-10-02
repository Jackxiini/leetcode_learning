n, bagweight = map(int, input().split())

weight = list(map(int, input().split()))
value = list(map(int, input().split()))

dp = [[0]*(bagweight+1) for _ in range(n)]

for i in range(bagweight):
    if dp[0][i]>= weight[0]:
        dp[0][i] = value[0]

for i in range(n):
    for j in range(bagweight+1):
        if j >= weight[i]:
            dp[i][j] = max(dp[i-1][j], value[i]+dp[i-1][j-weight[i]])
        else:
            dp[i][j] = dp[i-1][j]

print(dp[-1][-1])
