# redux-persist

1、npm install redux-persist

2、

```
import{persistStore,persistReducer} from 'redux-persist'
import storage from 'redux-persist/lib/storage'   //defaults to localStorage for web and AsyncStorage for react-native
import { persistStore, persistReducer } from 'redux-persist'
import { PersistGate } from 'redux-persist/integration/react'
```

3、

```
const persistConfig = {
  key: 'root',
  storage,
}
const persistedReducer = persistReducer(persistConfig, reducers)
const store = createStore(persistedReducer, undefined,
  compose(
    applyMiddleware(thunk),
    /* autoRehydrate() */
  ))
const persistor = persistStore(store)
```

4、

```
render() {
    return (
      <Provider store={store}>
        <PersistGate persistor={persistor} loading={null}>          
            <App />         
        </PersistGate>
      </Provider>
    )
  }
```



