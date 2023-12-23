```javascript
import { useReducer } from "react";

function powerUpReducer(state, action) {
  if (action.type === "collectCoin") {
    return {
      coins: state.coins + 1,
    };
  }
  throw Error("Unknown action.");
}

export default function MarioAdventure() {
  const [state, dispatch] = useReducer(powerUpReducer, { coins: 42 });

  return (
    <>
      <button
        onClick={() => {
          dispatch({ type: "collectCoin" });
        }}
      >
        Collect Coin
      </button>
      <p>Mario, you have collected {state.coins} coins!</p>
    </>
  );
}
```

```javascript
import { useReducer } from "react";

function powerUpReducer(state, action) {
  // ... Reducer logic for handling 'collectCoin' action
}

export default function MarioAdventure() {
  const [state, dispatch] = useReducer(powerUpReducer, { coins: 42 });
  // ... Component rendering
}

```

```jsx
import { useReducer } from "react";
import "./styles.css";

function init(initialCoins) {
  return { coins: initialCoins };
}

function powerUpReducer(state, action) {
  if (action.type === "collectCoins") {
    return {
      coins: state.coins + 1,
    };
  }
  throw Error("Unknown action.");
}

export default function MarioAdventure() {
  const initialState = 42;
  const [state, dispatch] = useReducer(powerUpReducer, initialState, init);

  return (
    <div className="marioAdventure">
      <button tabindex="0" onClick={() => dispatch({ type: "collectCoins" })}>
        Collect Coin
      </button>
      <p>Mario, you have collected {state.coins} coins!</p>
    </div>
  );
}
```

```javascript
// Using state and dispatch from useReducer
const [state, dispatch] = useReducer(reducer, initialState);

// Example usage in a component
console.log(state); // Current state
dispatch({ type: "action_type" }); // Method to update state
```

```javascript
// Correct useReducer usage at the top level of a component
function MyComponent() {
  const [state, dispatch] = useReducer(reducer, initialState);
  // Component logic
}

// Incorrect usage: useReducer inside a function or conditional
function AnotherComponent() {
  if (condition) {
    const [state, dispatch] = useReducer(reducer, initialState);
    // This can lead to unpredictable behavior
  }
  // Rest of the component logic
}
```

```javascript
function marioFormReducer(state, action) {
  switch (action.type) {
    case "SET_CHARACTER":
      return { ...state, character: action.payload };
    case "SET_COINS":
      return { ...state, coinsCollected: action.payload };
    default:
      return state;
  }
}

function MarioFormComponent() {
  const [state, dispatch] = useReducer(marioFormReducer, {
    character: "",
    coinsCollected: 0,
  });

  return (
    <form>
      <label>
        Choose your character:
        <select
          value={state.character}
          onChange={(e) =>
            dispatch({ type: "SET_CHARACTER", payload: e.target.value })
          }
        >
          <option value="Mario">Mario</option>
          <option value="Luigi">Luigi</option>
          <option value="Peach">Peach</option>
          <option value="Bowser">Bowser</option>
        </select>
      </label>
      <label>
        Coins Collected:
        <input
          type="number"
          value={state.coinsCollected}
          onChange={(e) =>
            dispatch({ type: "SET_COINS", payload: e.target.value })
          }
        />
      </label>
    </form>
  );
}
```

```javascript
// Incorrect way to update state in reducer
function reducer(state, action) {
  switch (action.type) {
    case "INCREMENT":
      state.count += 1; // Wrong: Mutating the state directly
      return state;
    default:
      return state;
  }
}

// Correct way to update state in reducer
function reducer(state, action) {
  switch (action.type) {
    case "INCREMENT":
      return { ...state, count: state.count + 1 }; // Correct: returning new object
    default:
      return state;
  }
}
```

```javascript
const counterReducer = (state, action) => {
  // ...
};

function CombinedStateComponent() {
  const [title, setTitle] = useState("Welcome to the Counter App");
  const [state, dispatch] = useReducer(counterReducer, { count: 0 });

  return (
    <div>
      <h1>{title}</h1>
      <div>
        Count: {state.count}
        <button onClick={() => dispatch({ type: "increment" })}>+</button>
        <button onClick={() => dispatch({ type: "decrement" })}>-</button>
      </div>
      <button onClick={() => setTitle("Counter App Updated")}>
        Update Title
      </button>
    </div>
  );
}
```

```javascript
import React, { useReducer } from "react";
import "./styles.css";

const initialState = {
  items: [],
};

function cartReducer(state, action) {
  switch (action.type) {
    case "ADD_ITEM":
      const existingItem = state.items.find(
        (item) => item.id === action.payload.id
      );
      if (existingItem) {
        return {
          ...state,
          items: state.items.map((item) =>
            item.id === action.payload.id
              ? { ...item, quantity: item.quantity + 1 }
              : item
          ),
        };
      } else {
        return {
          ...state,
          items: [...state.items, { ...action.payload, quantity: 1 }],
        };
      }
    case "REMOVE_ITEM":
      return {
        ...state,
        items: state.items.filter((item) => item.id !== action.payload.id),
      };
    case "UPDATE_QUANTITY":
      return {
        ...state,
        items: state.items.map((item) =>
          item.id === action.payload.id
            ? { ...item, quantity: action.payload.quantity }
            : item
        ),
      };
    default:
      return state;
  }
}

function ShoppingCart() {
  const [state, dispatch] = useReducer(cartReducer, initialState);

  const products = [
    { id: 1, name: "Apple", price: 0.5 },
    { id: 2, name: "Banana", price: 0.3 },
    { id: 3, name: "Carrot", price: 0.2 },
  ];

  const addItem = (product) => {
    dispatch({ type: "ADD_ITEM", payload: product });
  };

  const removeItem = (itemId) => {
    dispatch({ type: "REMOVE_ITEM", payload: { id: itemId } });
  };

  const updateQuantity = (itemId, quantity) => {
    dispatch({ type: "UPDATE_QUANTITY", payload: { id: itemId, quantity } });
  };

  return (
    <div className="shopping-cart">
      <h2>Shopping Cart</h2>
      {state.items.map((item) => (
        <div key={item.id}>
          <p>
            {item.name} - Quantity: {item.quantity}
          </p>
          <button onClick={() => updateQuantity(item.id, item.quantity + 1)}>
            +
          </button>
          <button
            onClick={() =>
              updateQuantity(item.id, Math.max(1, item.quantity - 1))
            }
          >
            -
          </button>
          <button onClick={() => removeItem(item.id)}>Remove</button>
        </div>
      ))}
      <h3>Products</h3>
      {products.map((product) => (
        <button key={product.id} onClick={() => addItem(product)}>
          Add {product.name}
        </button>
      ))}
    </div>
  );
}

export default ShoppingCart;
```


