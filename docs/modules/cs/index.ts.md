---
title: cs/index.ts
nav_order: 2
parent: Modules
---

## index overview

Sets up a central system, that can communicate with charge points

---

<h2 class="text-delta">Table of contents</h2>

- [Central System](#central-system)
  - [CentralSystem (class)](#centralsystem-class)
    - [addConnectionListener (method)](#addconnectionlistener-method)
    - [close (method)](#close-method)
    - [sendRequest (method)](#sendrequest-method)
    - [setupSoapServer (method)](#setupsoapserver-method)
    - [setupWebsocketsServer (method)](#setupwebsocketsserver-method)
    - [getSoapHandler (method)](#getsoaphandler-method)
    - [handleConnection (method)](#handleconnection-method)
- [utils](#utils)
  - [CSSendRequestArgs (type alias)](#cssendrequestargs-type-alias)

---

# Central System

## CentralSystem (class)

Represents the central system, can communicate with charge points

**Signature**

```ts
export declare class CentralSystem {
  constructor(port: number, cpHandler: RequestHandler<ChargePointAction, RequestMetadata>, host: string = '0.0.0.0')
}
```

**Example**

```ts
import { CentralSystem } from '@voltbras/ts-ocpp'

// port and request handler as arguments
const centralSystem = new CentralSystem(3000, (req, { chargePointId }) => {
  switch (req.action) {
    case 'Heartbeat':
      // returns a successful response
      // (we pass the action and ocpp version so typescript knows which fields are needed)
      return {
        action: req.action,
        ocppVersion: req.ocppVersion,
        currentTime: new Date().toISOString(),
      }
  }
  throw new Error('message not supported')
})
```

### addConnectionListener (method)

**Signature**

```ts
public addConnectionListener(listener: ConnectionListener)
```

### close (method)

**Signature**

```ts
public close()
```

### sendRequest (method)

**Signature**

```ts
sendRequest<V extends OCPPVersion, T extends CentralSystemAction>(args: CSSendRequestArgs<T, V>): EitherAsync<OCPPRequestError, Response<T, V>>
```

### setupSoapServer (method)

**Signature**

```ts
private setupSoapServer()
```

### setupWebsocketsServer (method)

**Signature**

```ts
private setupWebsocketsServer(): WebSocket.Server
```

### getSoapHandler (method)

**Signature**

```ts
private getSoapHandler(action: ActionName): ISoapServiceMethod
```

### handleConnection (method)

**Signature**

```ts
private handleConnection(socket: WebSocket, request: IncomingMessage)
```

# utils

## CSSendRequestArgs (type alias)

**Signature**

```ts
export type CSSendRequestArgs<T extends CentralSystemAction<V>, V extends OCPPVersion> =
  | {
      ocppVersion: 'v1.6-json'
      chargePointId: string
      payload: Omit<Request<T, V>, 'action' | 'ocppVersion'>
      action: T
    }
  | {
      ocppVersion: 'v1.5-soap'
      chargePointUrl: string
      chargePointId: string
      payload: Omit<Request<T, V>, 'action' | 'ocppVersion'>
      action: T
    }
```