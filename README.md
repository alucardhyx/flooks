# 🍸 flooks

A state manager for React Hooks. Maybe the simplest. ^\_^

![npm](https://img.shields.io/npm/v/flooks?style=flat-square)
![Travis (.org)](https://img.shields.io/travis/nanxiaobei/flooks?style=flat-square)
![Codecov](https://img.shields.io/codecov/c/github/nanxiaobei/flooks?style=flat-square)
![npm type definitions](https://img.shields.io/npm/types/typescript?style=flat-square)
![GitHub](https://img.shields.io/github/license/nanxiaobei/flooks?style=flat-square)

🍰 Simple | 🍭 Auto loading | 🍕 Modules | 🥂 Flexible

---

## Install

```shell
yarn add flooks
```

or

```shell
npm install flooks
```

## Usage

```jsx harmony
import { setModel, useModel } from 'flooks';

const model = {
  state: {
    count: 0,
  },
  actions: ({ getModel, setState }) => ({
    increment() {
      const { count } = getModel();
      setState({ count: count + 1 });
    },
    decrement() {
      const { count } = getModel();
      setState({ count: count - 1 });
    },
    async incrementAsync() {
      const { increment } = getModel();
      await new Promise((resolve) => setTimeout(resolve, 1000));
      increment();
    },
  }),
};

setModel('counter', model);

function Counter() {
  const { count, increment, decrement, incrementAsync } = useModel('counter');
  return (
    <>
      Count: {count}
      <button onClick={increment}>+</button>
      <button onClick={decrement}>-</button>
      <button onClick={incrementAsync}>+ async{incrementAsync.loading && '...'}</button>
    </>
  );
}
```

## API

### setModel

```js
setModel(name, model);
```

Accepts a `name` string and an initial `model` object, creates a namespaced model.

Initial `model` contains a `state` object and an `actions` function.

### useModel

```js
const model = useModel(name);
```

A React Hook. Accepts a model's name, returns a model object with state and actions.

### getModel

```js
const model = getModel(name?);
```

Argument to `actions`. Like `useModel`, but when get own model, `name` can be omitted.

i.e. `getModel()` for own model, `getModel('other')` for any model else.

### setState

```js
setState(payload);
```

Argument to `actions`. Update own model's state with `payload` object, can't update other models'.

## FAQ

### Auto loading?

```js
actions: ({ getModel, setState }) => ({
  async someAsyncAction() {},
});
```

When an action is async, `someAsyncAction.loading` can be use.

### Code splitting?

Supported naturally. Call `setModel` in different components, then use libraries like [`loadable-components`](https://github.com/smooth-code/loadable-components).

### Create models together?

```js
import { setModel } from 'flooks';
import a from '...';
...

const models = { a, b, c, d };
Object.entries(models).forEach(([name, model]) => {
  setModel(name, model);
});
```

This is not recommended. Call `setModel` in components, which is more clear and flexible.

### Anything else?

1\. Our philosophy is decentralization, so we recommend to bind a model and a route entry component as one module, call `setModel` in the component to bind two.

2\. No need to add a file like `store.js` or `models.js`, because we don't need to distribute the store from top now. Without the centralized store, just models and components.

3\. A module has its own space, with `useModel` and `getModel`, all other models can be reached. So modules are independent, but also connected.

4\. Don't call `setModel` multiple times on a model, if you have a "common" model used in several place, recommend to initialize the "common" model in a skeleton component like `App.jsx`.

5\. That's all, enjoy it~

## License

[MIT License](https://github.com/nanxiaobei/flooks/blob/master/LICENSE) (c) [nanxiaobei](https://mrlee.me/)
