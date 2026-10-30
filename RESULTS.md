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
The request used lowercase names, 'pepperoni' and 'knife', but explore.js showed that the API uses "Pepperoni" and "Knife".
    ✗ no pepperoni
      ↳  89% — ✓ 67 / ✗ 8
    ✗ no knife needed
      ↳  72% — ✓ 54 / ✗ 21


## Part A results

| Step | Test type    | Users | Req/s | p(95) |  Max  | Failed | Checks |
| ---- | ------------ | ----- | ----- | ----- |  ---  | ------ | ------ |
| A1   | Smoke        | 1     | 0.46  |448.52 | 479.76| 0.00%  |100.00% |
| A2   | Average load | 3     | 1.26  |434.34 | 820.35| 0.00%  |100.00% |
| A3   | Stress       | 5     | 2.13  |420.34 | 611.14| 0.00%  |100.00% |
| A4   | Spike        | 5     | 1.02  |481.07 | 655.68| 0.00%  |100.00% |
| A5   | Soak         | 2     | 0.96  |458.29 | 772.74| 0.00%  |100.00% |

## Part B

1. fill already
2. Each iteration took about 1.35 s, so one user sent about 0.74 requests per second and 5 users sent about 3.7 requests per second.
3. No, p(95) went from 434.34 (3 users) to 420.3 (5 users), because 5 users is still inside QuickPizza's operational envelope so the time we measured is mostly network time, not server load.
4. p(95) up from about 400 ms to about 620 ms during the spike and returned to normal within about 10 seconds after the users dropped back to 1.
5. No. p(95) was about 350-600 ms with some peaks near 700 ms in the first minutes, and about 350-450 ms in the last minutes, and no requests failed in 633 requests over 11 minutes. A soak test of several hours could reveal a memory leak, where the server slowly uses more memory until it slows down or crashes.
6. The average hides the slowest requests, while p(95) shows the time that 1 in 20 users still waits longer than, which is closer to what users actually experience.
7. Our requests travel over the internet from Thailand, so network delay is part of every number, and QuickPizza is shared with other people around the world, so their traffic changes the load at the same time.
8. Hundreds of users can overload a site that other people depend on, which is the same as a denial-of-service attack and can be illegal without written permission.
9. Vague: "LINE should send messages fast." Testable: "With 100 users sending messages at the same time for 10 minutes, 95% of messages are sent within 500 ms, 99% within 1 s, and fewer than 0.5% fail."
    thresholds: {
      http_req_duration: ['p(95)<500', 'p(99)<1000'],
      http_req_failed: ['rate<0.005'],
    },