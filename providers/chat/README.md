# XTRF Chat

Publishes messages about some of the events happening in chat.

## Generating docs

Documentation can be easily generated using [AsyncAPI Generator](https://github.com/asyncapi/generator)

```
npm install -g @asyncapi/generator
ag asyncapi.json @asyncapi/html-template -o ./dist
```

There is also a [GitHub Action](https://github.com/marketplace/actions/generator-for-asyncapi-documents) ready made for this purpose.

## TODO

- [ ] How to declare pubsub attributes?
- [ ] Consider flattening the evenlope and payload
