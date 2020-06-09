# JMeter

[JMeter](https://jmeter.apache.org/) is Java application designed to load test functional behavior and measure performance.

We create test and save it as `.jmx` file. After that we can run it in console.

### Run script in console

```bash
jmeter.bat -n -t my_tests/sales-local.jmx -l my_tests/output/out.csv
```

Report will be saved in CSV format at the file `out.csv`.
