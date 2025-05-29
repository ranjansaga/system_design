# React Router 

## Key Concepts

- **BrowserRouter**  
  A Router implementation that uses the HTML5 history API (`pushState`, `popstate`) to keep UI in sync with the URL.  
  Wrap your app with `<BrowserRouter>` to enable client-side routing without full page reloads.

- **Router vs BrowserRouter**  
  - `Router` is a low-level interface that requires you to pass a `history` object.  
  - `BrowserRouter` is a high-level wrapper that creates and manages its own history internally.

- **Routes and Route**  
  - `<Routes>` replaces `<Switch>` in React Router v6, used to group `<Route>` components.  
  - `<Route>` defines path and corresponding element/component to render.

- **Route syntax**  
  - v6: Use `element` prop with a React element:  
    ```jsx
    <Route path="/hello" element={<HelloComponent />} />
    ```  
  - Do NOT pass component class/function directly like `component={HelloComponent}` or `Component={HelloComponent}`.

- **React.lazy with Routes**  
  - Lazy load remote or big components:  
    ```jsx
    const LazyComp = React.lazy(() => import('./MyComp'));
    <Suspense fallback={<Loading />}>
      <Routes>
        <Route path="/lazy" element={<LazyComp />} />
      </Routes>
    </Suspense>
    ```

## Common Issues & Fixes

- **404 on route refresh**  
  - Happens when dev server does not fallback to `index.html`.  
  - Fix: Use `historyApiFallback: true` in devServer config (Webpack Dev Server).

- **Page reload on route change**  
  - Caused by using normal `<a href="...">` instead of React Router's `<Link>`.  
  - Fix: Use `<Link to="...">` for client-side navigation, prevents full page reload and bundle refetch.

- **Router errors**  
  - `Router` component requires a `history` object. Prefer using `BrowserRouter` unless you provide custom history.

- **Route element undefined error**  
  - Ensure `element` receives a React element (`<Component />`), not the component reference.

## DevServer Config for SPA Routing

```js
devServer: {
  static: "./dist",
  hot: true,
  port: 3000,
  open: true,
  historyApiFallback: true, // Redirect 404s to index.html for SPA routing
}
