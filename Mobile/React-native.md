# Mobile-React-native

React-native base project

## Basics

### 1. JSX

The other unusual thing in this code example is `<View><Text>Hello world!</Text></View>`. This is JSX - a syntax for embedding XML within JavaScript. It looks like HTML on the web, except instead of web things like `<div>` or `<span>`, you use React components.

### 2. Components

A component can be pretty simple - the only thing that's required is a render function which returns some JSX to render.

### 3. Props

Most components can be customized when they are created, with different parameters. These creation parameters are called props, short for properties.

### 4. State

There are two types of data that control a component: `props` and `state`. `props` are set by the parent and they are fixed throughout the lifetime of a component. For data that is going to change, we have to use `state`. In general, you should initialize `state` in the constructor, and then call `setState` when you want to change it. In a real application, You might set state when you have new data from the server, or from user input. In that case you would use `Redux` or `Mobx` to modify your state rather than calling setState directly.

### 5. Style

As a component grows in complexity, it is often cleaner to use `StyleSheet.create` to define several styles in one place.

### 6. Flex Dimension

Normally you will use flex: 1, which tells a component to fill all available space, shared evenly amongst other components with the same parent. The larger the flex given, the higher the ratio of space a component will take compared to its siblings. A component can only expand to fill available space if its parent has dimensions greater than 0. If a parent does not have either a fixed width and height or flex, the parent will have dimensions of 0 and the flex children will not be visible.

### 7. Flexbox

[flexbox](https://facebook.github.io/react-native/docs/flexbox)

### 8. relative and absolute layout

[layout](https://facebook.github.io/react-native/docs/flexbox#absolute-relative-layout)

### 9. Built-in components

[component](https://facebook.github.io/react-native/docs/components-and-apis)

### 10. redux reducer, redux debug

### 11. state and lifecycle

### 12. navigation

- planning app navigation
- prevent Android back button:

```Javascript
   componentDidMount() {
        BackHandler.addEventListener('hardwareBackPress', this.handleBackButton);
    }

    componentWillUnmount() {
        BackHandler.removeEventListener('hardwareBackPress', this.handleBackButton);
    }

    handleBackButton() {
        ToastAndroid.show('Back button is pressed', ToastAndroid.SHORT);
        return true;
    }
```

- [react-navigation API refernce](https://reactnavigation.org/docs/en/1.x/navigation-prop.html)
- [reset nested stack](https://github.com/react-navigation/react-navigation/issues/1127#issuecomment-368657601)

```Javascript
   const resetAction = NavigationActions.reset({
      index: 0,
      actions: [NavigationActions.navigate({ routeName: 'HomeNavigator' })],
      key: null
    });
    const navigateAct = NavigationActions.navigate({ routeName: 'ScanHistoriesNavigator' })
    this.props.navigation.dispatch(resetAction);
    this.props.navigation.dispatch(navigateAct);
```

### 13. debug

## build

### ios

### android

debugger apk

release apk

android sdk

android build tool

## Issues

### How to change theme in `native-base`

[customise theme](https://stackoverflow.com/questions/46150451/how-to-change-theme-in-native-base)

[theme font](https://docs.nativebase.io/Customize.html#theme-color-headref)
[change font family](https://medium.com/react-native-training/react-native-custom-fonts-ccc9aacf9e5e)

[disable uppercase button](https://github.com/GeekyAnts/NativeBase/issues/1033)

## resources

1. awesome react native: http://www.awesome-react-native.com/
2. layout playgounrd: [yoga](https://facebook.github.io/react-native/docs/components-and-apis)
3. UI framework: [native-base](https://docs.nativebase.io/), [native-base-KitchenSink](https://github.com/GeekyAnts/NativeBase-KitchenSink)
4. UI framework recommendation: https://blog.bitsrc.io/11-react-native-component-libraries-you-should-know-in-2018-71d2a8e33312


### refactor activities

1. redux and reducer
2. error code and error handler
3. styling and layout