# @player-tools/devtools-client

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](./LICENSE)

The `@player-tools/devtools-client` exposes the Panel with the ReactPlayer, which is responsible for running content sent by Player devtool plugins on the inspected Player UI instance.

You can check how to use it in the [browser-extension](https://github.com/player-ui/browser-devtools) and [Flipper desktop App client](https://github.com/player-ui/tools/tree/main/devtools/plugins/mobile/flipper-desktop-client).

## Overview

The Devtools client is a part of the Player UI Devtools architecture. It allows you to create custom devtools panels that can be used to debug and inspect your Player UI experiences, using the same plugin system used by other Player UI plugins.

The Devtools client conveniently receives its content from the devtools plugins running into the Player UI in use by the inspected page. This feature allows you to extend the dev tools with custom panels, without the need to create a new extension. You can create your own devtools plugins and use them in the Player UI Devtools Browser Extension.

For a more comprehensive understanding of the architecture of the Devtools client, you can always refer to the detailed information provided in the Devtools Browser Extension README.

## Installation

The Devtools client is available as an npm package. You can install it using npm or yarn:

```bash
npm install @player-tools/devtools-client
```

```bash
yarn add @player-tools/devtools-client
```

## Usage

The Devtools client is a React component that receives content from devtools plugins running in the Player UI used by the inspected page. It can be used in your React application like any other React component.

```jsx
import { Panel } from "@player-tools/devtools-client";
import type { MessengerOptions } from "@player-tools/devtools-messenger";
import browser from "webextension-polyfill";

const port = browser.runtime.connect();

const communicationLayer: Pick<
  MessengerOptions,
  "sendMessage" | "addListener" | "removeListener"
> = {
  sendMessage: async (message) =>
    port.postMessage({
      tabId: browser.devtools.inspectedWindow.tabId,
      body: message,
    }),
  addListener: (callback) => {
    port.onMessage.addListener(({ body }) => callback(body));
  },
  removeListener: (callback) => {
    port.onMessage.removeListener(callback);
  },
};

root.render(<Panel communicationLayer={communicationLayer} />);
```

## Contributing

We welcome contributions to the Player UI Devtools Browser Extension.

## Local Development
If you want to make any changes/contributions to the devtools extension addon. 

To get started with the local browser devtools extension:

```bash
cd devtools/plugins/desktop/test-env
pnpm run dev:chrome
```

This will create a `browser-devtools` on your desktop. Inside of this; you will find a `build` folder that contains `chrome-mv3-prod`. You load it as an extension with Chrome. (Manage extensions -> Load unpacked). You may need to turn on any developer mode on the top right.

You can then run your application containing Player and use the BasicWebDevPlugin so that your application can communicate with the extension.
