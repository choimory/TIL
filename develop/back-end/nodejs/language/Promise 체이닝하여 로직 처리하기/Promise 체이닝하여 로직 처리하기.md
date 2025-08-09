# TODO

- 프로미스.then(logic1).then(logic2)...catch(); 로 처리하면 로직을 순차적으로 처리하면서 진행 가능
- resolve(), reject()를 사용하여 then으로 보낼지 catch로 보낼지 결정가능
- 다음 then() 혹은 catch()는 resolve(), reject()시 건낸 인자를 파라미터로 받음
- https://joshua1988.github.io/web-development/javascript/promise-for-beginners/

---

# Promise란

# Promise.then()

- 기본적으로 수행할 내용들. 여러번의 체이닝이 가능하며, 마지막 then()까지 순차적으로 모두 성공적으로 수행했을시 마지막 then()의 return값이 반환된다.

# Promise.catch()

- then에서의 수행되는 일련의 과정들에서 예외가 발생하거나, resolve됐을시 수행되는곳 

# 결과

```typescript

```