- [[노드(Node.js)]]에서 미리 정의해놓은 [[모듈(Module)]]로 [[require()]]를 이용하여 가져올 수 있다.
- 내장 모듈이기 때문에 경로 대신 이름만 적어주어도 된다.

- os 모듈은 애플리케이션에서 많이 사용되는 모듈은 아니지만 운영체제와 시스템의 정보를 가져올 수 있는 모듈이다.

- [[노드(Node.js)]]는 싱글스레드이기 때문에 확인을 위해 os.cpus()를 많이 사용한다.

## networkInterfaces

- 네트워크 인터페이스 정보를 담은 배열이다.

```jsx
const os = require('os');

const nets = os.networkInterfaces();

> console.log(nets);

{
  'Wi-Fi': [
    {
      address: 'fe80::ec5:9127:215a:e9a4',
      netmask: 'ffff:ffff:ffff:ffff::',
      family: 6,
      mac: 'b0:a4:60:96:17:57',
      internal: false,
      cidr: 'fe80::ec5:9127:215a:e9a4/64',
      scopeid: 10
    },
    {
      address: '172.30.1.9',
      netmask: '255.255.255.0',
      family: 4,
      mac: 'b0:a4:60:96:17:57',
      internal: false,
      cidr: '172.30.1.9/24'
    }
  ],
  'Loopback Pseudo-Interface 1': [
    {
      address: '::1',
      netmask: 'ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff',
      family: 6,
      mac: '00:00:00:00:00:00',
      internal: true,
      cidr: '::1/128',
      scopeid: 0
    },
    {
      address: '127.0.0.1',
      netmask: '255.0.0.0',
      family: 4,
      mac: '00:00:00:00:00:00',
      internal: true,
      cidr: '127.0.0.1/8'
    }
  ]
}
```

```jsx
for (const attr in nets) {
  console.group('Network장치 이름: %s', attr);

  const item = nets[attr];

  item.forEach((v, i) => {
    console.debug('주소형식: %s', v.family);
    console.debug('IP주소: %s', v.address);
    console.debug('맥주소: %s', v.mac);
    console.debug('넷마스크: %s', v.netmask);
    console.debug();
  });

  console.groupEnd();
}

Network장치 이름: Wi-Fi
  주소형식: 6
  IP주소: fe80::ec5:9127:215a:e9a4
  맥주소: b0:a4:60:96:17:57
  넷마스크: ffff:ffff:ffff:ffff::
  
  주소형식: 4
  IP주소: 172.30.1.9
  맥주소: b0:a4:60:96:17:57
  넷마스크: 255.255.255.0
  
Network장치 이름: Loopback Pseudo-Interface 1
  주소형식: 6
  IP주소: ::1
  맥주소: 00:00:00:00:00:00
  넷마스크: ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff
  
  주소형식: 4
  IP주소: 127.0.0.1
  맥주소: 00:00:00:00:00:00
  넷마스크: 255.0.0.0
```

## 나머지 [[메서드(Method)]]

```jsx
console.log(os.tmpdir());       // 임시 저장 폴더의 위치
console.log(os.endianness());   // CPU의 endianness(BE 또는 LE)
console.log(os.hostname());     // 호스트 이름(컴퓨터 이름)
console.log(os.type());         // 운영체제 이름
console.log(os.platform());     // 운영체제 플랫폼
console.log(os.arch());         // 운영체제 아키텍처
console.log(os.release());      // 운영체제 버전
console.log(os.uptime());       // 운영체제가 실행된 시간
console.log(os.loadavg());      // 로드 에버리지 정보를 담은 배열
console.log(os.totalmem());     // 시스템의 총 메모리
console.log(os.freemem());      // 시스템의 가용 메모리
console.log(os.cpus());         // CPU의 정보를 담은 객체
console.log(os.networkInterfaces()); // 네트워크 인터페이스 정보를 담은 배열
console.log(os.EOL);            // 운영체제의 개행 문자(\n 이나 \r\n 같은 문자)
```

## urlFormat 만들기

`UtilHelper.js`

```jsx
class UtilHelper {
  static #current = null;

  static getInstance() {
    if (UtilHelper.#current === null) {
      UtilHelper.#current = new UtilHelper();
    }

    return UtilHelper.#current;
  }

  urlFormat(urlObject) {
    return String(Object.assign(new URL("http://a.com"), urlObject));
  }
}

module.exports = UtilHelper.getInstance();
```

`urlFormat.js`

```jsx
const { urlFormat } = require('../helper/UtilHelper');

const url1 = urlFormat({
  protocol: 'https',
  hostname: 'example.com',
  pathname: 'somepath'
});

> console.log(`url1: ${url1}`);
url1: https://example.com/somepath

const url2 = urlFormat({
  protocol: 'https:',
  hostname: 'example.com',
  pathname: '/somepath'
});

> console.log(`url2: ${url2}`);
url2: https://example.com/somepath

const url3 = urlFormat({
  protocol: 'https:',
  host: 'example.com:8080',
  pathname: '/somepath'
});

> console.log(`url3: ${url3}`);
url3: https://example.com:8080/somepath

const url4 = urlFormat({
  protocol: 'http:',
  hostname: 'example.com',
  port: 8080,
  pathname: '/somepath'
});

> console.log(`url4: ${url4}`);
url4: http://example.com:8080/somepath

const url5 = urlFormat({
  protocol: 'https:',
  hostname: 'example.com',
  port: 8080,
  pathname: '/somepath',
  username: 'john',
  password: 'abc',
  search: 'item=bike'
});

> console.log(`url5: ${url5}`);
url5: https://john:abc@example.com:8080/somepath?item=bike
```

## 내 아이피 주소 찾기

`UtilHelper.js`

```jsx
const { networkInterfaces } = require('os');

class UtilHelper {
  static #current = null;

  static getInstance() {
    if (UtilHelper.#current === null) {
      UtilHelper.#current = new UtilHelper();
    }

    return UtilHelper.#current;
  }

  myip() {
    const ipAddress = [];
    const nets = networkInterfaces();

    for (const attr in nets) {
      const item = nets[attr];

      item.map((v, i) => {
        if ((v.family == 'IPv4' || v.family == 4) && v.address != '127.0.0.1') {
          ipAddress.push(v.address);
        }
      });
    }

    return ipAddress;
  };
}
module.exports = UtilHelper.getInstance();
```

`myip.js`

```jsx
const utilHelper = require('../helper/UtilHelper');

const ip = utilHelper.myip();

> console.debug(ip);
[ '172.30.1.9' ]
```