# Vue Postcss-initial example

This is a Proof of Concept of preventing external css from overwriting styles of our app. The real world case is a widget sitting on a host page. In such a setup we don't have much control over widget environment, thus css creep is possible.

## TL;DR / Conclusion
Adding `all: initial;` to every rule (or some rules) is cumbersome, inefficient and can't protect from creep on elements you don't style (ex. just tags). Quite succesful though, was [reseting all tags within your app namespace](https://github.com/kubenstein/vue-postcss-initial-example/blob/master/src/app/Widget.vue#L14-L30) upfront.

## PoC
The tool used in this PoC is [`postcss-initial`](https://github.com/maximkoretskiy/postcss-initial) plugin, that resets 83 css properties to their initial values.

- [Part 1](https://github.com/kubenstein/vue-postcss-initial-example/commit/9b23df95108e3f200551f1a15774eee68a1bb7a8#diff-3e67d83f61248273832b17963519f89eR13) of the experiment was to add `all: initial;` to all css rules the are defined by the widget. The problem of this solution is, external styles still can alter widget layout by styling tags. For example: `body div a {}` rule may break layout because `a` was left unstyled.

- [Part 2](https://github.com/kubenstein/vue-postcss-initial-example/commit/114a64b79a9acde819aec6e09667dd2f0f6a341a) of the experiment was to reset all possible tags WITHIN widget namespace. By doing this we prevented some generic styles alteration comming from external css. Unfortunately external css still can define even more specific rules, bypassing reseting guard.


## Other improvements
To mitigate css collision even more, it is is recommended to use css modules as production class names are most likely unique. Vue scoped css method is more vulnerable because common class names like `.title` or `.wrapper` are preserved during production bundling increasing possibility of collision.


## Alternative solutions
To fully isolate a widget from its host, the widget can sit within embedded iFrame. IFrames can have transparent backgrounds or can be resized to fullscreen for modals but unfortunately comes with their own challenges.
