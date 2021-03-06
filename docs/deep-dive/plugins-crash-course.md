---
id: plugins-crash-course
title: Plugins Crash Course
sidebar_label: Plugins Crash Course
---

Everything in Webiny is built using plugins. To build a React app or an API you will only need a little bit of bootstrap code, and the rest is all done via plugins.

Plugins are the smallest building blocks. They are used to add routes, menus, page builder elements, settings, handle API security, add data models, GraphQL schemas, etc. The list is just too long to name all the usages here.

:::info
Check out our knowledge-sharing session on `Webiny Plugins`. 
In this session, we'll look at three main groups of plugins:
- `CLI plugins`
- `API plugins`
- `App plugins`

We'll first learn how `@webiny/plugins` package works outside of Webiny, using [codesandbox.io](https://www.codesandbox.io). Then we'll see how this same package is utilized within the CLI, within Apollo Server (resolvers) and within React apps.
:::

<iframe width="805" height="390" src="https://youtube.com/embed/4qcDLzu8kVM" frameborder="0" allowfullscreen></iframe>

:::info
Now that we understand the basics of Webiny Plugins, we will continue on practical examples. 

We will demonstrate how plugins are actually used in your everyday development: in the Admin app, where we’ll show how we can use plugins to add new routes, menus, and even how we can replace the logo in the top header bar. 

Also on the API side, we’ll learn what are context plugins and what they are used for, and finally, how we can use plugins to expand our GraphQL schema with additional GraphQL types and resolvers.
:::

<iframe width="805" height="390" src="https://youtube.com/embed/NGoZ3TDTYus" frameborder="0" allowfullscreen></iframe>

## Anatomy of a plugin

A plugin is nothing more than a simple object with 2 mandatory keys:

```js
{
    name: "your-unique-plugin-name",
    type: "plugin-type"
}
```

`name` is a plugin identifier and must be unique in the entire app. If a plugin is defined using a `name` that is already registered, it will replace the existing plugin.

`type` is a string that allows you to group plugins with similar functionality. You can then get plugins of a certain `type` and do whatever you need with them.

We will add a section with the full catalog of plugins as we progress with the docs.

## Creating a plugin

> NOTE: Don't try to create a plugin in your code just yet. Simply go through this section to get an idea of how plugins work. There are more articles on concrete plugins in the docs that will be easy to follow if you understand the concept.

Let's (virtually) create a simple plugin:

```js
// firstPlugin.js
export default () => ({
  name: "first-plugin",
  type: "test-plugin",
  sayHi() {
    console.log("Well, hello there!");
  }
});
```

> NOTE: We strongly recommend you create your plugins as factory functions. That way it is easy to add configuration that affects how plugin behaves. For API plugins, factory is a requirement.

Now let's register your new plugin. To register React app plugins you need to use a `@webiny/plugins` package, which is always installed with the default project setup:

```js
import { registerPlugins } from "@webiny/plugins";
import firstPlugin from "./firstPlugin";

registerPlugins(firstPlugin);
```

`registerPlugins` accepts a single plugin, arrays of plugins, multiple
plugins passed as separate arguments, you name it.

> Plugin registration can be performed anywhere in your app. Placing the registration at the entry point of your app is usually the best approach. But you can also register plugins at runtime, after some button was clicked, for example.

> We recommend to always use a namespace when naming your plugin: `pb-element-...` or `my-app-....`. That way you make sure you never accidentally overwrite another plugin.

If you have already created a Webiny project, you can find examples of plugin registration in the following files:

- `apps/admin/src/App.tsx`
- `apps/site/src/App.tsx`

## Using your plugin

You can use either `name` or `type` to retrieve your plugin.

### Using plugins by type

```js
import { getPlugins } from "@webiny/plugins";

// Get all plugins by type
const testPlugins = getPlugins("test-plugin");
testPlugins.forEach(pl => {
    pl.sayHi();
});
```

If no plugins were found, an empty array is returned, so you're safe to loop through the result of `getPlugins` function.

### Using a plugin by name

```js
import { getPlugin } from "@webiny/plugins";

// Get all plugins by name
const firstPlugin = getPlugin("first-plugin");

if (!firstPlugin) {
    throw Error(`"first-plugin" plugin was not found!`);
}

firstPlugin.sayHi();
```

> NOTE: We recommend to always add a check if a plugin was returned, and throw an error if the plugin is mandatory.

## Removing a plugin

If there is a plugin you'd like to remove, do it like this:

```js
import { unregisterPlugin } from "@webiny/plugins";

unregisterPlugin("plugin-name-to-remove");
```

If you just want to replace the existing plugin, simply register a new plugin with the same name as the plugin you want to replace.

## Conclusion

Now you're familiar with how the entire Webiny is built. It's just that - registration of plugins and creation of plugins. Lots of them 🚀. In `API Development` and `App Development` sections of the docs you'll see that there are minor differences in how we register plugins but the concept remains the same.
