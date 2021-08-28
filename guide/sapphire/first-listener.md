# Your first listener

Create a `listeners` folder in the same directory as your `main` file. Normally only one listener is defined per file, but you can also define groups of them. Create a `ready.js` file in your `listeners` folder, which will send a message and then edit it with the elapsed time. Arguments and other features are covered in other pages.

## Creating a listener class

To create a listener in Sapphire, the `Listener` class needs to be imported from `@sapphire/framework`. A listener can be exported through default or named exports.

:::: code-group
::: code-group-item CommonJS
```js
const { Listener } = require('@sapphire/framework');

module.exports = class ReadyListener extends Listener {};
// exports.default = class ReadyListener extends Listener {};
// exports.myListener = class ReadyListener extends Listener {};
```
:::
::: code-group-item ESM
```js
import { Listener } from '@sapphire/framework';

export default class ReadyListener extends Listener {}
// export class ReadyListener extends Listener {}
```
:::
::::


`module.exports` will be used for these pages. Let's begin populating the Listener class:

```js
module.exports = class ReadyListener extends Listener {
	constructor(context) {
		super(context, {
			once: true
		});
	}
};
```

An overview of what's defined in the constructor:

- `context`: an object that contains file metadata required by the `Piece` class (which `Listener` extends) in order to function.
- `name`: by default, the name of the file without the extension, i.e. `ready.js` becomes `ready`, so there's no need to define it.
- `once`: whether the event should be called only once or not
There are many other properties available, explained in upcoming sections.

## Creating the `run` method

listeners have a `run` method, which executes the listener logic. Define this below the listener's constructor:

<!-- eslint-disable constructor-super -->

```js {7-9}
module.exports = class ReadyListener extends Listener {
	constructor(context) {
		// ...
	}

	async run(message) {
		this.container.logger.info(`${this.container.client.user.tag} is ready!`)
	}
};
```
:::tip
Every piece (listeners, commands etc) in sapphire has a `container` which can be accessed via `this.container`. It is this container that contains the logger, the client and other properties. Here we access the logger via `this.container.logger` and call its info method to print a nicely formatted message in the console.
:::
<!-- eslint-enable constructor-super -->

Any discord.js code can be executed here, since Sapphire Framework is an extension of it. The listener will be triggered when the bot logs in

## Resulting code

<ResultingCode />
