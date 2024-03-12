# K6 Grafana: Load testing tool

See [documentation](https://grafana.com/docs/k6/latest/)

Docker image: [grafana/k6](https://hub.docker.com/r/grafana/k6)

## Installation

See [docs](https://grafana.com/docs/k6/latest/get-started/installation/) 

### Fedora/CentOS

```bash
sudo dnf install https://dl.k6.io/rpm/repo.rpm
sudo dnf install k6
```

## Usage

- Create and init a new script: `k6 new`. File `script.js` will be created.
- Run test: `k6 run script.js`

## Examples

### GET request

```js
import http from 'k6/http';
import { check, sleep } from 'k6';

export const options = {
  vus: 10,
  duration: '5s',
};

export default function () {
  const res = http.get('http://test.k6.io');
  check(res, { 'status was 200': (r) => r.status == 200 });
  sleep(1);
}
```

### POST request

```js
import http from 'k6/http';

export const options = {
    vus: 10,
    duration: '5s',
};

export default function () {
    const url = 'http://test.k6.io/login';
    const payload = JSON.stringify({
        email: 'aaa',
        password: 'bbb',
    });

    const params = {
        headers: {
            'Content-Type': 'application/json',
        },
    };

    http.post(url, payload, params);
}
```
