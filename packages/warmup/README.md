# Middy warmup middleware

<div align="center">
  <img alt="Middy logo" src="https://raw.githubusercontent.com/middyjs/middy/master/img/middy-logo.png"/>
</div>

<div align="center">
  <p><strong>Warmup middleware for the middy framework, the stylish Node.js middleware engine for AWS Lambda</strong></p>
</div>

<div align="center">
<p>
  <a href="http://badge.fury.io/js/%40middy%2Fwarmup">
    <img src="https://badge.fury.io/js/%40middy%2Fwarmup.svg" alt="npm version" style="max-width:100%;">
  </a>
  <a href="https://snyk.io/test/github/middyjs/middy">
    <img src="https://snyk.io/test/github/middyjs/middy/badge.svg" alt="Known Vulnerabilities" data-canonical-src="https://snyk.io/test/github/middyjs/middy" style="max-width:100%;">
  </a>
  <a href="https://standardjs.com/">
    <img src="https://img.shields.io/badge/code_style-standard-brightgreen.svg" alt="Standard Code Style"  style="max-width:100%;">
  </a>
  <a href="https://greenkeeper.io/">
    <img src="https://badges.greenkeeper.io/middyjs/middy.svg" alt="Greenkeeper badge"  style="max-width:100%;">
  </a>
  <a href="https://gitter.im/middyjs/Lobby">
    <img src="https://badges.gitter.im/gitterHQ/gitter.svg" alt="Chat on Gitter"  style="max-width:100%;">
  </a>
</p>
</div>

Warmup middleware that helps to reduce the [cold-start issue](https://serverless.com/blog/keep-your-lambdas-warm/). Compatible by default with [`serverless-plugin-warmup`](https://www.npmjs.com/package/serverless-plugin-warmup), but it can be configured to suit your implementation.

This middleware allows you to specify a schedule to keep Lambdas that always need to be very responsive warmed-up. It does this by regularly invoking the Lambda, but will terminate early to avoid the actual handler logic from being run.

If you use [`serverless-plugin-warmup`](https://www.npmjs.com/package/serverless-plugin-warmup) the scheduling part is done by the plugin and you just have to attach the middleware to your "middyfied" handler. If you don't want to use the plugin you have to create the schedule yourself and define the `isWarmingUp` function to define whether the current event is a warmup event or an actual business logic execution.

**Important:** AWS recently announced Lambda [Provisioned Concurrency](https://aws.amazon.com/about-aws/whats-new/2019/12/aws-lambda-announces-provisioned-concurrency/). If you have this enabled, you do not need this middleware.

To update your code to use Provisioned Concurrency see:
- [AWS Console](https://aws.amazon.com/blogs/compute/new-for-aws-lambda-predictable-start-up-times-with-provisioned-concurrency/)
- [Serverless](https://serverless.com/blog/aws-lambda-provisioned-concurrency/)
- [Terraform](https://www.terraform.io/docs/providers/aws/r/lambda_provisioned_concurrency_config.html)

## Install

To install this middleware you can use NPM:

```bash
npm install --save @middy/warmup
```


## Options

 - `isWarmingUp`: a function that accepts the `event` object as a parameter
   and returns `true` if the current event is a warmup event and `false` if it's a regular execution. The default function will check if the `event` object has a `source` property set to `serverless-plugin-warmup`.

## Sample usage

```javascript
const middy = require('@middy/core')
const warmup = require('@middy/warmup')

const isWarmingUp = (event) => event.isWarmingUp === true

const originalHandler = (event, context, cb) => {
  /* ... */
}

const handler = middy(originalHandler)
  .use(warmup({ isWarmingUp }))
```


## Middy documentation and examples

For more documentation and examples, refers to the main [Middy monorepo on GitHub](https://github.com/middyjs/middy) or [Middy official website](https://middy.js.org).


## Contributing

Everyone is very welcome to contribute to this repository. Feel free to [raise issues](https://github.com/middyjs/middy/issues) or to [submit Pull Requests](https://github.com/middyjs/middy/pulls).


## License

Licensed under [MIT License](LICENSE). Copyright (c) 2017-2021 Luciano Mammino, will Farrell, and the [Middy team](https://github.com/middyjs/middy/graphs/contributors).

<a href="https://app.fossa.io/projects/git%2Bgithub.com%2Fmiddyjs%2Fmiddy?ref=badge_large">
  <img src="https://app.fossa.io/api/projects/git%2Bgithub.com%2Fmiddyjs%2Fmiddy.svg?type=large" alt="FOSSA Status"  style="max-width:100%;">
</a>
