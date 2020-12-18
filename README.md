# @databyss-org/reach-router

A fork of [@reach/router](https://github.com/reach/router) that permits usage without the focus control.

## The problem

By default, Reach Router is designed to control the focus on route changes to comply with certain A11Y expectations. For example, when a user navigates to a new route, a screen reader would need the focus to transition to the newly shown route. If it didn't, the screen reader would not be able to correctly report and navigate the new content. To facilitate this, Reach Router wraps every `<Router>` and child route component in a `<div>`. This can obviously cause problems with some layouts, especially flex layouts.

## The solution, part 1

As outlined in [this issue](https://github.com/reach/router/issues/63#issuecomment-457840121), Reach allows you to override the wrapping component described above. If you supply a React Fragment as the `component` property and set the `primary` property on the `<Router>` component, the wrapper is not rendered around the route children components. However, the wrapper is still rendered around the `<Router>` component.

## The solution, part 2

This fork incorporates [PR #104](https://github.com/reach/router/pull/104/files), which normalizes the wrapping behavior between the `<Router>` component and its children.

## Suggested usage

In Databyss, we don't need Reach to manage the focus transitions during route changes. Where necessary, we update the focus. So we want to disable the focus management and component wrapping everywhere the `<Router>` component is used. We have one library file, `NavigationProvider.js` that composes the Reach `<Router>` component with the props described above set to disable focus management. Then we import that component in our app when we want to use a router.

```js
// NavigationProvider.js
import { Router as ReachRouter } from "@databyss-org/reach-router";
const RouteWrapper = ({ children }) => <>{children}</>;
export const Router = ({ children, ...others }) => (
  <ReachRouter primary={false} component={RouteWrapper} {...others}>
    {children}
  </ReachRouter>
);
```
