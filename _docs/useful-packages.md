---
title: Useful Packages
order: 4
---

Here's a list of useful npm packages that are compatible with **roblox-ts**.

## [rbx-services](https://www.npmjs.com/package/rbx-services)
Exports most commonly used Roblox services. This makes them really easy to use.
```TS
import { Players } from "rbx-services";
Players.PlayerAdded.Connect(player => print("Player was added!", player.Name));
```

## [rbx-event](https://www.npmjs.com/package/rbx-event)
A type safe implementation of BindableEvents written in TypeScript

```TS
import { Event } from "rbx-event";
const e = new Event<[number, string]>();
e.connect((a, b) => print(a, b));
let n = 0;
while (true) {
    wait(1);
    e.fire(++n, "test");
}
```

## [rbx-inspect](https://www.npmjs.com/package/rbx-inspect)
A copy of [kikito's inspect.lua library](https://github.com/kikito/inspect.lua) with type definitions.
```TS
import inspect = require("rbx-inspect");
print(inspect([1, 2, 3]));
```

## [rbx-datastore2](https://www.npmjs.com/package/rbx-datastore2)
A copy of [Kampfkarren's DataStore2 library](https://devforum.roblox.com/t/how-to-use-datastore2-data-store-caching-and-data-loss-prevention/136317) with type definitions.
```TS
import { ReplicatedStorage, Players } from "rbx-services";
import DataStore2 = require("rbx-datastore2");

Players.PlayerAdded.Connect(player => {
    const coinsStore = DataStore2<number>("coins", player);

    const cointAmountRemote = ReplicatedStorage.CoinAmount as RemoteEvent;
    function callRemote(value: number) {
        cointAmountRemote.FireClient(player, value);
    }

    // Fire a remote event to the player telling them how many coins they have.
    // If this is their first time playing the game, they'll start out with 100.
    callRemote(coinsStore.Get(100));

    // Everytime the coin store updates, we'll send the RemoteEvent again.
    coinsStore.OnUpdate(callRemote);
});
```

## [rbx-resources](https://www.npmjs.com/package/rbx-resources)
rbx-resources lets you easily access game assets and create named instances in your game.

```ts
const event = getResource(RemoteEvent, "SomeRemoteEvent");
const sword = getResource<Model>("Weapon", "Darkheart");
const bindable = getLocalResource(BindableEvent, "FooBarBaz");
```

## [rbx-roact](https://www.npmjs.com/package/rbx-roact)
TypeScript and JSX support for [Roact](https://github.com/Roblox/roact).
```tsx
import * as Roact from 'rbx-roact';

const LocalPlayer = game.GetService("Players").LocalPlayer as Player;
const PlayerGui = LocalPlayer.FindFirstChildOfClass("PlayerGui");

const tree = <screengui>
  <textlabel Key="Label" Text="Hello, World!" Size={new UDim2(1, 0, 1, 0)}/>
</screengui>;

Roact.mount(tree, PlayerGui, "HelloWorld");
```

## [rbx-cmdr](https://www.npmjs.com/package/rbx-cmdr)
Cmdr is a fully extensible and type safe command console for Roblox developers.

Great for admin commands, but does much more.
- Make commands that tie in specifically with your game systems.
- Intelligent autocompletion and instant validation.
- Run commands programmatically on behalf of the local user.
- Bind commands to user input.