# Ntfy Client

Send notifications to your devices using Ntfy.

This is a singular package fork of [ffflorian/node-packages](https://github.com/ffflorian/node-packages/tree/main/packages/ntfy), and includes a fix for
using Token authorization, a bug with v1.14.0 of the [ntfy](https://www.npmjs.com/package/ntfy) npmjs package.

[![npm (scoped)](https://img.shields.io/npm/v/@jc21/ntfy-client.svg?style=for-the-badge)](https://www.npmjs.com/package/@jc21/ntfy-client)
[![npm (types)](https://img.shields.io/npm/types/@jc21/ntfy-client.svg?style=for-the-badge)](https://www.npmjs.com/package/@jc21/ntfy-client)
[![npm (licence)](https://img.shields.io/npm/l/@jc21/ntfy-client.svg?style=for-the-badge)](https://www.npmjs.com/package/@jc21/ntfy-client)


### Install

```bash
yarn add @jc21/ntfy-client
```

## Usage

### Simple example

```ts
import {publish} from '@jc21/ntfy-client';

await publish({
  message: 'This is an example message.',
  topic: 'mytopic',
});
```

### Advanced example

```ts
import {publish, MessagePriority} from '@jc21/ntfy-client';

await publish({
  actions: [
    {
      clear: true,
      extras: {
        cmd: 'pic',
        camera: 'front',
      },
      intent: 'io.heckel.ntfy.USER_ACTION',
      label: 'Take picture',
      type: 'broadcast',
    },
    {
      body: '{"action": "close"}',
      clear: false,
      headers: {
        Authorization: 'Bearer zAzsx1sk..',
      },
      label: 'Close door',
      method: 'PUT',
      type: 'http',
      url: 'https://api.mygarage.lan',
    },
    {
      clear: true,
      label: 'Open portal',
      type: 'view',
      url: 'https://api.nest.com/',
    },
  ],
  authorization: {
    password: 'my-password',
    username: 'my-username',
  },
  // Or:
  // authorization: 'my-token,
  clickURL: 'https://github.com/ffflorian/',
  delay: '1m',
  disableCache: true,
  disableFirebase: true,
  emailAddress: 'name@example.com',
  iconURL: 'https://avatars.githubusercontent.com/ffflorian',
  message: 'Remote access to device detected. Act right away.',
  priority: MessagePriority.MAX,
  server: 'https://ntfy.custom',
  tags: ['warning', 'skull'],
  title: 'Unauthorized access detected',
  topic: 'mytopic',
});
```

### Use a custom server without specifying it every time

```ts
import {NtfyClient} from '@jc21/ntfy-client';
const ntfyClient = new NtfyClient('https://ntfy.custom');

await ntfyClient.publish({ ... });
await ntfyClient.publish({ ... });
```


### Compiling Source

```bash
yarn install
yarn build
yarn test
```
