# Express.js

## 배포

배포 후 환경변수를 production으로 변경해야 함: `NODE_ENV=production node app.js`

앱 내부에서 `process.env.NODE_ENV` 값에 할당되어 express 배포시 최적화 처리됨

## ErrorHandling

Express.js 4버전에서는 Router에서 Promise처리를 지원하지 않음

따라서 Router에서 Async Function을 사용하려면 두 방법 중 하나를 선택해야 함

### `express-promise-router` 사용

Express 기본 라우터 대신 사용한다.

```js
// app.ts
import expressPromiseRouter from 'express-promise-router';

const router = expressPromiseRouter();

app.use((error: Error, request: Request, response: Response, next: NextFunction) => {
  response.status(500).json({ message: error.message });
  next();
});
```

### Wrapping Route Functions

```ts
export const errorHandler = (error: Error, request: Request, response: Response, next: NextFunction) => {
  response.status(500).json({ message: error.message });
  next();
};

type AsyncFunc = (req: Request, resp: Response, next: NextFunction) => Promise<any>;

export const asyncHandler: (func: AsyncFunc) => AsyncFunc = func => {
  return (request, response, next) =>
    Promise.resolve(func(request, response, next)).catch((error: Error) => errorHandler(error, request, response, next));
};
```

다음처럼 사용한다

```ts
// app.ts
app.use(errorHandler);

// foo.controller.ts
this.routes.get('/hello', asyncHandler((req, resp) => this.foobar(req, resp)));
```

## merging interfaces

- <https://github.com/DefinitelyTyped/DefinitelyTyped/blob/master/types/express-serve-static-core/index.d.ts>
- <https://github.com/DefinitelyTyped/DefinitelyTyped/blob/master/types/express/index.d.ts>

`types/express.d.ts`

```ts
import { User } from '../src/domain/user';

declare global {
  namespace Express {
    export interface Request {
      user: User;
    }
  }
}
```

`tsconfig.json`

```json
{
  "files": ["types/express.d.ts"]
}
// OR
{
  "include": ["src/**/*", "types/*"]
}
```

> ts-node에서 타입인식이 되지 않는 경우: <https://github.com/TypeStrong/ts-node#help-my-types-are-missing>

`tsconfig.json`

```json
{
  "compilerOptions": {
    "typeRoots" : ["./node_modules/@types", "./types"]
  }
}
```

경로는 다음과 같음

```txt
<project_root>/
-- tsconfig.json
-- typings/
  -- <module_name>/
    -- index.d.ts
```