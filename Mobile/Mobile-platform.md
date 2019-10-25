# Next-generation mobile platform

[7 FrontEnd JavaScript Trends and Tools You Should Know for 2020](https://hackernoon.com/7-frontend-javascript-trends-and-tools-you-should-know-for-2020-fb1476e41083)

## Framework comparision

1. :white_check_mark: React/React Native
2. Angular.js
3. Vue.js
4. [Flutter](https://www.thedroidsonroids.com/blog/flutter-vs-react-native-what-to-choose-in-2019)
5. [Web Components](https://css-tricks.com/an-introduction-to-web-components/), [tools for web components](https://blog.bitsrc.io/7-tools-for-developing-web-components-in-2019-1d5b7360654d)
6. integrate RN into native app

resources:

1. [comparision](https://medium.com/@TechMagic/reactjs-vs-angular5-vs-vue-js-what-to-choose-in-2018-b91e028fa91d)
2. [benchmark](https://blog.bitsrc.io/benchmarking-angular-react-and-vue-for-small-web-applications-e3cbd62d6565)
3. [npm trends](https://www.npmtrends.com/react-vs-@angular/core-vs-vue)
4. [dislike RN](https://github.com/react-native-community/discussions-and-proposals/issues/104)

## Expo VS React Native

Good for simple mobile apps which requires little hardware supports

1. managed workflow
2. bare workflow

[Expo VS React Native](https://apiko.com/blog/expo-vs-vanilla-react-native/)

> **Expo cannot using native modules** The biggest advantage of React Native without Expo is that you can link the packages that use native modules and connect your native modules written in native languages.
> **Expo cannot update React Native version by self** You can choose that React Native version to use, update to the newer versions, and get new opportunities. As Expo decides what React Native version to use, it often takes a lot of time to issue the updates.

## :mag: React Native state management

### Redux

### MobX

### Hooks

## Android, iOS compatbility

## Typescript

## GraphQL

## Microservices front-end

### web application

#### When to use

1. Big application
2. can divide your app into small pieces by domain accessory
3. release small features

#### Architecture

#### Benefits

1. Maintainability. You can easily divide your resources into different teams which will grow their knowledge in particular domain related to some part of the application.
2. Technologies freedom. If you like to try VueJS or another new technology, go ahead! Not much risk, you just start one single microservice instead of rewriting everything.
3. Independent Deployment. It gives so much freedom when you can release small pieces of the application. Fixes and releases go more smoothly.

#### Costs

1. Maintaining consistency. You need to invest time to make your apps work together. I found more bugs once I’ve rewritten application to microservices model, which were invisible while the application was a monolith.
2. Operational Complexity. All magic with regular deploys must work like a charm, otherwise, you will get more pains than benefits.
3. Duplicate download dependencies, large payload size.

#### Possible solutions

1. Web Components
2. iFrame
3. Javascript using a `<script>` tag

> [example code1](https://github.com/andrewdacenko/web-components-angular-react)
[example code2](https://github.com/neuland/micro-frontends)
[example code3](https://martinfowler.com/articles/micro-frontends.html#TheExampleInDetail)

#### Resources

1. https://hackernoon.com/front-end-microservices-with-web-components-597759313393
2. https://martinfowler.com/articles/micro-frontends.html
3. https://micro-frontends.org/
4. Chinese https://www.infoq.cn/article/yuWiaiui6C18Od5uZiuF
5. https://medium.com/@lucamezzalira

### mobile application

flutter and RN?
1. MiniApp and [Electrode Native Platform](https://native.electrode.io/)

## from designers to UI components

how designers help with UI development

## Wallet-based mobile app

### Backgrounds

1. [dApp](https://www.coindesk.com/information/what-is-a-decentralized-application-dapp)
2. [wallet](https://medium.com/@stellabelle/cold-wallet-vs-hot-wallet-whats-the-difference-a00d872aa6b1)
3. [ERC token](https://medium.com/axionable-ai-and-blockchain/token-ercs-in-ethereum-erc-20-erc-223-erc-777-and-erc-721-8176c0f11c18)

### Core features of wallet-based mobile app

1. de-centralised authentication: store private key on mobile device.
2. communicate with smart contract on blockchain: send request to blockchain network instead of API server
3. support token operations: transfer tokens, get balance etc...

### Open source implementations

1. [Ethereum] trust wallet: Ethereum Wallet, Support ERC20 & ERC223
2. [Ethereum] [alphawallet](https://alphawallet.com/), on Ethereum, tokenscript

### Architecture support

1. decentralised auth
2. DApp client, communicate with smart contract
3. token system
