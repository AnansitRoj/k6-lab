# K6 lab results

## Part 0
- Step 0.1, http_req_duration: 377.2 ms (avg = min = med = max = p(90) = p(95), because the run sent only 1 request)
- Step 0.2, requests, rate and p(95), and why the rate stays below 5: 58 requests, 2.42 requests per second, p(95) = 2.23 s. One iteration is one request plus sleep(1). The iteration_duration average was 1.85 s (the request took 0.73 s on average, plus the 1 s sleep). So each virtual user finishes only about 0.54 iterations per second, and 5 virtual users give about 2.4 requests per second, not 5. The rate is also averaged over the whole 24 s of the run, which includes the last iterations finishing after the 20 s duration.
- Step 0.6, exit code of the normal run and of the run with P95=300: normal run exit code = 0 (p(95) = 537.9 ms, below 1500 ms, and all three thresholds passed). Run with P95=300 exit code = 99. The rule p(95)<300 showed a cross because the measured p(95) was 1.75 s, so K6 printed that the thresholds on http_req_duration have been crossed. The p(95) differed a lot between the two runs (537.9 ms and 1.75 s) because QuickPizza is a shared public site.
- Step 0.7, rules that passed, p(95), p(99), exit code after your change: Requirement: 3 VUs, 30 s, p(95)<1000 ms, p(99)<1500 ms, failed rate <1%. In my run only the failed-rate rule passed (0.00%). The p(95) rule failed with p(95) = 1.52 s and the p(99) rule failed with p(99) = 1.78 s, even though the median was only 485 ms, because a few slow requests (max 1.99 s) pulled the high percentiles up on the shared public site. So the API did not meet the requirement in this run. After I changed p(95)<1000 to p(95)<100, the test failed and the exit code was [99].
- Step 0.8, what was wrong, and how the checks showed it:

## Part A results
| Step | Test type | Users | Req/s | p(95) | Max | Failed | Checks |
| ---- | ------------ | ----- | ----- | ----- | --- | ------ | ------ |
| A1 | Smoke | 1 | | | | | |
| A2 | Average load | 3 | | | | | |
| A3 | Stress | 5 | | | | | |
| A4 | Spike | 5 | | | | | |
| A5 | Soak | 2 | | | | | |

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