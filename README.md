# React Sucks (2023 Update)

Struggle with React for years...

## Throw a promise to suspense, seriously?

React favors function components instead of class components. This makes life cycle management harder. You can find that React has to use dark magic for function components to achieve something very simple for class components. Look how they make suspense function component, it is so dirty (and buggy!):

```jsx
export default function Albums({ artistId }) {
  const albums = use(fetchData(`/${artistId}/albums`));
  return (
    <ul>
      {albums.map(album => (
        <li key={album.id}>
          {album.title} ({album.year})
        </li>
      ))}
    </ul>
  );
}

// This is a workaround for a bug to get the demo running.
// TODO: replace with real implementation when the bug is fixed.
function use(promise) {
  if (promise.status === 'fulfilled') {
    return promise.value;
  } else if (promise.status === 'rejected') {
    throw promise.reason;
  } else if (promise.status === 'pending') {
    throw promise;
  } else {
    promise.status = 'pending';
    promise.then(
      result => {
        promise.status = 'fulfilled';
        promise.value = result;
      },
      reason => {
        promise.status = 'rejected';
        promise.reason = reason;
      },      
    );
    throw promise;
  }
}
```

## No ES module, no tree shaking

While front-end and Node.js eco-system are migrating to ES modules, React is still a CommonJS module, which doesn't support tree-shaking.

## No built-in TypeScript support

Facebook is still promoting its Flow.
