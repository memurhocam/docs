# Available Hooks

The following is a list of all hooks present in NodeBB. This list is
intended to guide developers who are looking to write plugins for
NodeBB. For more information, please consult
Writing Plugins for NodeBB &lt;create&gt;.

There are four types of hooks, **filters**, **actions**, **static**, and **response** hooks.

* Filters take an input (provided as a single argument), parse it in some way, and return the changed value.
* Actions take multiple inputs, and execute actions based on the inputs received. Actions do not return anything.
* Static hooks are similar to action hooks, except NodeBB will wait for the hook to complete (by calling its passed-in callback) before continuing.
* Response hooks are similar to action hooks, except that listeners are called one at a time, and ends prematurely if a response is sent to the client.

Please consult the [**autogenerated list of hooks**](https://github.com/NodeBB/NodeBB/wiki/Hooks/). The aforementioned list is automatically generated from the NodeBB codebase and is not guaranteed to be complete at all times.

## Special Hooks

There are a number of special hooks that are included in the NodeBB codebase that are not explicitly called out in the autogenerated hooks page. They are included below as they are either a) useful, or b) difficult to understand given its context.

### Page Build Hooks

In addition to hooks executed on certain actions, a pair of hooks are also fired whenever a page is loaded (either via direct page access, or via a page transition):

* `filter:<template>.build` is fired on a particular page load, based on the template file being rendered. For example, `/recent` draws its view from `recent.tpl`, and thus a plugin can subscribe to `filter:recent.build` and react whenever the recent page is loaded.
    * Keep in mind that there is no guarantee that the hook is **only** fired on that particular page. A plugin may elect to render that same template, which would also cause the hook to be fired.
* `filter:router.page` is called on every single page load, prior to any processing (template generation, etc.)
* `filter:middleware.render` is called on every single page load, and is useful for situations where you want to append or change data for every page. It is called after the bulk of processing is done.

### Widget Render Hook `filter:widget.render:<widget>`

A widget defined by a plugin needs to let NodeBB know what's in the widget (e.g. html, and other assorted data). This is handled via a plugin hook fired whenever a widget is rendered. The hook name itself is defined by the widget name, so if a widget with the `widget` property set to `mywidget` is rendered, the hook `filter:widget.render:mywidget` will be fired.

For more information, please consult the [page on writing widgets](../widgets)