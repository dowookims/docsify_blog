## 1. 정의
bcrypt는 애초부터 패스워드 저장을 목적으로 설계되었다. Niels Provos와 David Mazières가 1999년 발표했고, 현재까지 사용되는 가장 강력한 해시 메커니즘 중 하나이다.

bcrypt는 보안에 집착하기로 유명한 OpenBSD에서 기본 암호 인증 메커니즘으로 사용되고 있고 미래에 PBKDF2보다 더 경쟁력이 있다고 여겨진다. bcrypt에서 “work factor” 인자는 하나의 해시 다이제스트를 생성하는 데 얼마만큼의 처리 과정을 수행할지 결정한다.
> 출처: [naver D2](https://d2.naver.com/helloworld/318732)

## 2. 설치
기존에 사용하던 `bcrypt-nodejs`의 경우 ***deprecated*** 되었으므로 *bcrypt* 또는 *bcryptjs* 를 사용합니다.

[bcrypt와 bcryptjs의 차이점](https://github.com/kelektiv/node.bcrypt.js/issues/705)은
* bcrypt는 native c++ moudle이여서 훨신 더 빠르고
* bcryptjs는 순수 js module 입니다.

저는 bcryptjs 로 사용해보겠습니다.

```npm install bcryptjs```

## 3. 사용

### 1) 동기적 사용(Usage - Sync)
비밀번호를 Hash 하기 위해
```js
var bcrypt = require('bcryptjs')
var salt = bcrypt.genSaltSync(10);
var hash = bcrypt.hashSync("B4c0/\/", salt);
```
이렇게 나온 hash 결과가 암호화된 비밀번호 이고, 이를 db에 저장하면 됩니다.

저장한 이후, db에 저장된 비밀번호와 사용자가 입력한 비밀번호를 비교하려면
```js
bcrypt.compareSync("B4c0/\/", hash); // true
bcrypt.compareSync("not_bacon", hash);  // false
```
를 하시면 됩니다.
저 위에 두 단계를 합치면
```js
var hash = bcrypt.hashSync('bacon', 8)
```
이렇게 사용 할 수 있습니다.

### 2) 비동기적 사용(Usage - Async)
비밀번호를 Hash화 하는 법은
```js
var bcrypt = require('bcryptjs');
bcrypt.genSalt(10, function(err, salt) {
    bcrypt.hash("B4c0/\/", salt, function(err, hash) {
        // Store hash in your password DB.
    });
});
```
이며, 비밀번호를 체크하기 위해서는
```js
bcrypt.compare("B4c0/\/", hash, function(err, res) {
  // res = true
});
bcrypt.compare("not_bacon", hash, function(err, res) {
  // res = false
});
 
// bcryptjs 2.4.0,부터 promise를 이용하여 결과를 처리 할 수 있습니다.:
bcrypt.compare("B4c0/\/", hash).then((res) => {
    // res === true
});
```
salt와 hash를 한번에 하는 방법으로
```js
bcrypt.hash('bacon', 8, function(err, hash) {

});
```
이 있습니다.

이와 더불어 bcryptjs 의 내부 메서드를 확인하고 싶으면

https://www.npmjs.com/package/bcryptjs 에서 확인 하실 수 있습니다.