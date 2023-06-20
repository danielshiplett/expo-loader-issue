# Overview

This project demonstrates the 'additional loader' issue with React Native/Expo & AWS Amplify.  This problem only occurs when you build the application for the web.  Android is fine.

# Recreating

If you want to recreate this problem from scratch, you can apply the following steps.  The `App.tsx` file from this project is already modified as needed.

```
# Create a new Expo application (in Typescript)
npx create-expo-app@latest -t expo-template-blank-typescript

# Change to your new project directory.

# Add development and application dependencies
npx expo install @expo/webpack-config -- --save-dev
npx expo install react-native-web react-dom
npx expo install @aws-amplify/ui-react-native react-native-get-random-values react-native-url-polyfill

# Set up Auth in Amplify.  Note: defaults are fine for this.
amplify init
amplify add auth
amplify push
```

If you just want to use this project as is and test it, all you need to do is perform the AWS Amplify steps.

# Web Testing

First, just try to build the production web bundle.

```
npx expo export:web
```

You should receive the following error:

```
Failed to compile
CommandError: Module parse failed: Unexpected token (40:26)
File was processed with these loaders:
 * ./node_modules/source-map-loader/dist/cjs.js
You may need an additional loader to handle the result of these loaders.
|     const typedFields = getRouteTypedFields({ fields, route });
|     if (isAuthenticatedRoute(route)) {
>         return children ? <>{children}</> : null;
|     }
|     return (<SafeAreaProvider>
```

You can also just run the web build in development mode.

```
npm run web
```

This will launch the build and start a new browser tab.  Once the transpilation is complete and the page loads in the browser, you will see the following in your terminal:

```
ERROR in ./node_modules/@aws-amplify/ui-react-native/dist/Authenticator/Authenticator.js 40:26
Module parse failed: Unexpected token (40:26)
File was processed with these loaders:
 * ./node_modules/source-map-loader/dist/cjs.js
You may need an additional loader to handle the result of these loaders.
|     const typedFields = getRouteTypedFields({ fields, route });
|     if (isAuthenticatedRoute(route)) {
>         return children ? <>{children}</> : null;
|     }
|     return (<SafeAreaProvider>

ERROR in ./node_modules/@aws-amplify/ui-react-native/dist/Authenticator/withAuthenticator.js 5:16
Module parse failed: Unexpected token (5:16)
File was processed with these loaders:
 * ./node_modules/source-map-loader/dist/cjs.js
You may need an additional loader to handle the result of these loaders.
| export default function withAuthenticator(Component, options = {}) {
|     return function WrappedWithAuthenticator(props) {
>         return (<Authenticator.Provider>
|         <Authenticator {...options}>
|           <Component {...props}/>

ERROR in ./node_modules/@aws-amplify/ui-react-native/dist/InAppMessaging/components/InAppMessageDisplay/InAppMessageDisplay.js 23:11
Module parse failed: Unexpected token (23:11)
File was processed with these loaders:
 * ./node_modules/source-map-loader/dist/cjs.js
You may need an additional loader to handle the result of these loaders.
|         onMessageAction,
|     });
>     return <Component {...props}/>;
| }
| InAppMessageDisplay.BannerMessage = BannerMessage;

ERROR in ./node_modules/@aws-amplify/ui-react-native/dist/InAppMessaging/components/withInAppMessaging/withInAppMessaging.js 6:16
Module parse failed: Unexpected token (6:16)
File was processed with these loaders:
 * ./node_modules/source-map-loader/dist/cjs.js
You may need an additional loader to handle the result of these loaders.
| export default function withInAppMessaging(Component, options) {
|     return function WrappedWithInAppMessaging(props) {
>         return (<InAppMessagingProvider>
|         <InAppMessageDisplay {...options}/>
|         <Component {...props}/>

ERROR in ./node_modules/@aws-amplify/ui-react-native/dist/theme/ThemeProvider.js 6:12
Module parse failed: Unexpected token (6:12)
File was processed with these loaders:
 * ./node_modules/source-map-loader/dist/cjs.js
You may need an additional loader to handle the result of these loaders.
| export const ThemeProvider = ({ children, theme, colorMode, }) => {
|     const value = React.useMemo(() => ({ theme: createTheme(theme, colorMode) }), [theme, colorMode]);
>     return (<ThemeContext.Provider value={value}>{children}</ThemeContext.Provider>);
| };
| 

web compiled with 5 errors

```

# Android Testing

Launch the application in an Android emulator.

```
npm run android
```

Once the application loads, you should see the `Sign In` screen.