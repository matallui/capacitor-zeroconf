# capacitor-zeroconf-lt

Capacitor ZeroConf plugin (based on capacitor-zeroconf)

This plugin allows you to browse and publish ZeroConf/Bonjour/mDNS services from applications developed using Ionic's Capacitor.

This is not a background service. When the cordova view is destroyed/terminated, publish and watch operations are stopped.

Android, iOS and [Electron](https://github.com/capacitor-community/electron) platforms are supported.

The has been ported from [Cordova ZeroConf Plugin](https://github.com/becvert/cordova-plugin-zeroconf).

## Install

```bash
npm install capacitor-zeroconf-lt
npx cap sync
```

or

```bash
yarn add capacitor-zeroconf-lt
yarn cap sync
```

## API

<docgen-index>

* [`getHostname()`](#gethostname)
* [`register(...)`](#register)
* [`unregister(...)`](#unregister)
* [`stop()`](#stop)
* [`watch(...)`](#watch)
* [`unwatch(...)`](#unwatch)
* [`close()`](#close)
* [Interfaces](#interfaces)

</docgen-index>

<docgen-api>
<!--Update the source file JSDoc comments and rerun docgen to update the docs below-->

### getHostname()

```typescript
getHostname() => Promise<{ hostname: string; }>
```

**Returns:** <code>Promise&lt;{ hostname: string; }&gt;</code>

--------------------


### register(...)

```typescript
register(request: ZeroConfRegisterRequest) => Promise<void>
```

| Param         | Type                                                                        |
| ------------- | --------------------------------------------------------------------------- |
| **`request`** | <code><a href="#zeroconfregisterrequest">ZeroConfRegisterRequest</a></code> |

--------------------


### unregister(...)

```typescript
unregister(request: ZeroConfUnregisterRequest) => Promise<void>
```

| Param         | Type                                                                            |
| ------------- | ------------------------------------------------------------------------------- |
| **`request`** | <code><a href="#zeroconfunregisterrequest">ZeroConfUnregisterRequest</a></code> |

--------------------


### stop()

```typescript
stop() => Promise<void>
```

--------------------


### watch(...)

```typescript
watch(request: ZeroConfWatchRequest, callback?: ZeroConfWatchCallback | undefined) => Promise<CallbackID>
```

| Param          | Type                                                                  |
| -------------- | --------------------------------------------------------------------- |
| **`request`**  | <code><a href="#zeroconfwatchrequest">ZeroConfWatchRequest</a></code> |
| **`callback`** | <code>ZeroConfWatchCallback</code>                                    |

**Returns:** <code>Promise&lt;string&gt;</code>

--------------------


### unwatch(...)

```typescript
unwatch(request: ZeroConfUnwatchRequest) => Promise<void>
```

| Param         | Type                                                                  |
| ------------- | --------------------------------------------------------------------- |
| **`request`** | <code><a href="#zeroconfwatchrequest">ZeroConfWatchRequest</a></code> |

--------------------


### close()

```typescript
close() => Promise<void>
```

--------------------


### Interfaces


#### ZeroConfRegisterRequest

| Prop        | Type                                    |
| ----------- | --------------------------------------- |
| **`port`**  | <code>number</code>                     |
| **`props`** | <code>{ [key: string]: string; }</code> |


#### ZeroConfUnregisterRequest

| Prop       | Type                |
| ---------- | ------------------- |
| **`name`** | <code>string</code> |


#### ZeroConfWatchRequest

| Prop         | Type                |
| ------------ | ------------------- |
| **`type`**   | <code>string</code> |
| **`domain`** | <code>string</code> |

</docgen-api>

### Example

Below you can find an example of what a React component could look like:

```jsx
const MdnsService: React.FC<Props> = (props) => {
  const options = { type: '_castor-display._tcp.', domain: 'local.' };

  useEffect(() => {
    let listener: any;

    const onDiscover = (result: ZeroConfWatchResult) => {
      console.log('mDNS listener result:', result);
    };

    (ZeroConf as any)
      .addListener('discover', onDiscover)
      .then((res: any) => (listener = res));

    ZeroConf.watch(options)
      .then((res: any) => console.log('mDNS success:', res))
      .catch((err: any) => console.log('mDNS error:', err));

    return () => {
      if (listener) {
        console.log('removing listener', listener);
        if (listener.remove) {
          console.log('... using remove()');
          listener.remove();
        } else {
          (ZeroConf as any).removeListener(listener);
        }
      }
      ZeroConf.unwatch(options)
        .then(() => {
          console.log('unwatch success');
          // need to close for Android to rescan
          // TODO: try fixing Android implementation
          ZeroConf.close().then(() => console.log('close success'));
        })
        .catch((err: any) => console.log('unwatch error:', err));
    };
  }, []);

  return <></>;
};
```
