# j-dgraph

The easy-to-use package to query dgraph without GraphQL.  So far it has been tested on Angular, Angular Universal, and Sveltekit.  It should, however, work in almost any javascript framework both server side and client side. 

Let me know if you have problems in the issue section.

It uses GraphQL, Urql, and my [easy-dgraph](https://github.com/jdgamble555/easy-dgraph) package under-the-hood!

# Installation

```typescript
npm i j-dgraph
```

# Example 1

A basic query...

```typescript
import { dgraph } from 'j-dgraph';

const _dgraph = new dgraph({ url: 'https://your-endpoint/graphql' });

const r = await _dgraph.type('post').filter('0x1').query({ id: 1, name: 1 }).build();

console.log(r);
```

# Example 2

Pretty print errors, use headers, and a custom query...

```typescript
import { dgraph } from 'j-dgraph';

...

const dg = new dgraph({
    url: 'https://your-endpoint/graphql',
    headers: async () => ({ "X-Auth-Token": await this.getToken() }),
    isDevMode: isDevMode()
}).pretty();

const r = await dg.type('queryFeatureSortedByVotes')
      .customQuery({
        id: 1,
        name: 1,
        url: 1,
        author: { id: 1 },
        totalVotes: 1,
        description: 1,
        votes: {
          id: 1
        }
      })
      .build()

console.log(r);
```

# Example 3

Subscriptions work out-of-the-box!

```typescript
import { dgraph } from 'j-dgraph';

const _dgraph = new dgraph({ url: 'https://your-endpoint/graphql' });

const sub = _dgraph.type('post').filter('0x1').query({ id: 1, name: 1 })
  .buildSubscription().subscribe((r: any) => console.log(r));

...

onDestroy(() => {
    sub.unsubscribe();
});

```

# Constructor Options

```typescript
/**
 * @param _opts 
 *   url - api endpoint url
 *   type? - node name
 *   isDevMode? - boolean for Developer Mode
 *   fetch? - fetch function
 *   headers? - headers function, can be async
 */
```

**Note:** In development mode, all GraphQL queries and results are printed to the console.  I have simplified all messages in DGraph to be easily readable, including errors.

You can also import the EnumType from easy-dgraph like so:

`import { dgraph, EnumType } from 'j-dgraph';`

Every single thing you can do in Dgraph's GraphQL, you can do with no configuration with this package.

See easy-dgraph below for how to query.

J
________

For Documentation, see: [dev.to: easy-dgraph](https://dev.to/jdgamble555/easy-dgraph-create-dgraph-graphql-on-the-fly-10bm)

For Examples, see: [Test File](https://github.com/jdgamble555/easy-dgraph/blob/master/src/lib/easy-dgraph.test.ts)
