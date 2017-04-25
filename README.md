# Wiry

[![Build Status](https://travis-ci.org/AdrianSchneider/wiry.svg?branch=master)](https://travis-ci.org/AdrianSchneider/wiry)
[![Coverage Status](https://coveralls.io/repos/github/AdrianSchneider/wiry/badge.svg)](https://coveralls.io/github/AdrianSchneider/wiry)

Wiry is an unopinionated dependency injection container for your node or typescript appllication.

## Design Goals

**Your code should not be aware of Wiry**.

You should have one file, or one layer that deals with configuring the container, but every other class or function should have no knowledge of a container. Classes will not have magic annotations that get processed, or assumptions of how the class will be used.

## Features

- Automatic, async dependency resolution
- Get array of services by pattern (ie `server.controllers.*` as `controllers: Controller[]`)
- Middleware/behaviours to wrap services (ie cache, etc.)

## Usage

At its core, wiry allows you to register and retrieve services, while resolving their dependency trees for you. Services are defined within functions, so they can be fetched lazily and asynchronously. They can be functions, class instances, or any values that can be returned.

If the service has dependencies, pass it as the 3rd argument with an array of names, and they will be fed into the function for that service.

```typescript
// class instance, depends on 'db' and 'cache'
container.registerService(
  'user-management',
  (db: Database, cache: Cache) => new UserManagement(db, cache),
  ['db', 'cache']
);

// register config
container.registerService('config' => () => config);

// register database which depends on config
container.registerService(
  'db',
  new Database(config.DB_CONNECTION_STRING),
  ['config']
);

// register cache which depends on config
container.registerService(
  'cache',
  new Cache(new RedisCacheBackend(config.CACHE_REDIS_HOST, config.CACHE_REDIS_PORT)),
  ['config']
);

// use the service
const service = container.get('user-management');
service.createUser(/* ... */);
```

This keeps all of the wiring or dependency resolution out of your classes or code, and isolated to one place. For a fully fledged example, check out [qissues](https://github.com/AdrianSchneider/qissues/tree/typescript/src/app/bootstrap).
