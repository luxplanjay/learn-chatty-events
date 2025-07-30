# Chatty Events

In frontend development, we often need to handle events like window resizing (`resize`), page scrolling (`scroll`), mouse movement (`mousemove`), or user input (`input`, `keydown`). These actions improve the user experience (UX), enabling dynamic interfaces, animations, and real-time feedback — but they also come with a cost.

> **Chatty events** are events that fire too frequently, putting unnecessary strain on the browser and JavaScript execution thread.

For example, a `scroll` handler can fire **30-60 times per second** during mouse scrolling. On mobile, slow swiping can trigger up to **100 events per second**.

Now imagine your event handler is doing any of the following:

- DOM manipulations
- Position calculations
- Filtering or sorting large arrays

In this case, performance issues are almost guaranteed — especially on low-end devices or older browsers.

Common consequences of chatty events:

- **FPS drops** and lack of smoothness in UI
- **Laggy animations** and scroll behavior
- **High CPU usage**
- **Unresponsive interface**
- Dropped frames, especially during `scroll` and `resize`

**👇 Click the image to open the interactive StackBlitz example**
[![Open example in StackBlitz](/assets/intro-embed-thumb.jpg)](https://stackblitz.com/edit/vitejs-vite-aeffy5sv?embed=1&file=src%2Fmain.js&hideNavigation=1)

## Reducing event handler frequency

To avoid performance issues caused by chatty events, we can use **throttle** and **debounce** — two techniques that control how often a function is executed.

- **Throttle**: calls the function **at most once** every N milliseconds
- **Debounce**: calls the function **only after N milliseconds of inactivity**

> In modern projects, we typically use implementations from [**Lodash**](https://lodash.com/) — a reliable utility library.

Let's install Lodash and look at practical examples.

## Installing Lodash

If you're using a modern development environment (like Vite), install Lodash with npm:

```bash
npm install lodash
```

Import only the functions you need:

```jsx
import throttle from "lodash/throttle";
import debounce from "lodash/debounce";
```

Quick check:

```jsx
import add from "lodash/add";

const result = add(2, 3);
console.log(result); // 5
```

You're now ready to use Lodash utilities in your project.

## Throttle

`Throttle` limits how often a function can be called — once every N milliseconds, no matter how many times the event fires.

> It provides **regular, rate-limited execution** of your function.

![Throttle illustration](/assets/throttle.png)

```jsx
import throttle from "lodash/throttle";

document.addEventListener(
  "scroll",
  throttle(() => {
    console.log("Scroll handler call every 300ms");
  }, 300)
);
```

Key points:

- First argument: the function to throttle
- Second argument: interval in milliseconds
- Returns a **new throttled function**

**👇 Click the image to open the interactive StackBlitz example**
[![Open example in StackBlitz](/assets/throttle-embed-thumb.jpg)](https://stackblitz.com/edit/vitejs-vite-ol3eu4lo?embed=1&file=src%2Fmain.js&hideNavigation=1)

When to use throttle:

- On `scroll`: update element positions, lazy-load images, change header style
- On `mousemove`: track cursor position
- On `resize`: recalculate layout

## Debounce

`Debounce` delays function execution until a pause in events — it fires **only after N milliseconds of inactivity**.

> Debounce waits for the event stream to end before executing the function.

![Debounce illustration](/assets/debounce.png)

```jsx
import debounce from "lodash/debounce";

document.addEventListener(
  "scroll",
  debounce(() => {
    console.log("Scroll handler call after 300ms pause");
  }, 300)
);
```

Key points:

- First argument: the function to delay
- Second argument: pause duration in ms
- Returns a **new debounced function**

**👇 Click the image to open the interactive StackBlitz example**
[![Open example in StackBlitz](/assets/debounce-embed-thumb.jpg)](https://stackblitz.com/edit/vitejs-vite-17rjjtxl?embed=1&file=src%2Fmain.js&hideNavigation=1)

When to use debounce:

- On `input`: avoid triggering API calls on every keystroke
- On `scroll`: run logic **after** scrolling ends
- On `resize`: recalculate layout **after** resizing ends

## Debounce modes: trailing vs leading

By default, `debounce` uses **trailing mode** — it fires the function **after** the event stream ends (i.e., after a pause of N milliseconds).

Sometimes, you may want the function to fire **at the beginning** of the event stream and ignore the rest until the pause — this is called **leading mode**.

Summary:

- **Trailing (`trailing: true`)**: runs at the **end** (default)
- **Leading (`leading: true`)**: runs at the **start**

![Leading debounce illustration](/assets/debounce-leading.png)

Lodash's `debounce` accepts a third parameter — an options object:

```jsx
import debounce from "lodash/debounce";

document.addEventListener(
  "scroll",
  debounce(
    () => {
      console.log("Scroll handler call on every event stream start");
    },
    300,
    {
      leading: true,
      trailing: false,
    }
  )
);
```

In this case:

- The function runs **at the start of scrolling**
- It won't be called again until 300ms have passed without events

**👇 Click the image to open the interactive StackBlitz example**
[![Open example in StackBlitz](/assets/leading-embed-thumb.jpg)](https://stackblitz.com/edit/vitejs-vite-htfv12r6?embed=1&file=src%2Fmain.js&hideNavigation=1)

When to use leading mode:

- Send an API request on the **first button click**
- Trigger animation immediately when interaction begins
- Detect the start of scrolling, swiping, or key pressing

## ✅ Summary

- **Chatty events** fire very frequently and can hurt performance
- Use **`throttle`** to run code at regular intervals (no more than once per N ms)
- Use **`debounce`** to run code after a pause in events
- Both methods are available via **Lodash**
- `debounce` supports both `leading` and `trailing` modes for fine control

> Use these tools to create smoother, more performant interfaces with better user experience.
