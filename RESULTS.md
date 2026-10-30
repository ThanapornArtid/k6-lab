# K6 lab results

## Part 0

- Step 0.1, http_req_duration: 275.23ms
- Step 0.2, requests, rate and p(95), and why the rate stays below 5: 75 requests, 3.69 req/s, p(95) = 341.7 ms. The rate is below 5 because each user does one request and then sleeps for 1 s so one iteration takes about 1.35 s. One user can only send about 1 / 1.35 = 0.74 requests per second.
- Step 0.6, exit code of the normal run and of the run with P95=300: 0 and 99
- Step 0.7, rules that passed, p(95), p(99), exit code after your change:
'p(95)<1000' p(95)=489.86ms'
'p(99)<1500' p(99)=535.29ms'
change 'p(95)<1000' to 'p(95)<100' get exit code 99.
- Step 0.8, what was wrong, and how the checks showed it:
    ✗ no pepperoni
      ↳  89% — ✓ 67 / ✗ 8
    ✗ no knife needed
      ↳  72% — ✓ 54 / ✗ 21


## Part A results

| Step | Test type    | Users | Req/s | p(95) | Max | Failed | Checks |
| ---- | ------------ | ----- | ----- | ----- | --- | ------ | ------ |
| A1   | Smoke        | 1     |       |       |     |        |        |
| A2   | Average load | 3     |       |       |     |        |        |
| A3   | Stress       | 5     |       |       |     |        |        |
| A4   | Spike        | 5     |       |       |     |        |        |
| A5   | Soak         | 2     |       |       |     |        |        |

## Part B

1.
2.
3.
4.
5.
6.
7.
8.
9.