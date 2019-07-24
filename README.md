# react-naitive google-auth tutorial for test project


To get access to googleali wit react-native  follow the instructions

1) Install package
```
npm install react-native-google-signin --save
```
2) Update android/build.gradle with
```
dependencies{
    ...
        classpath 'com.google.gms:google-services:4.2.0'
        // NOTE: Do not place your application dependencies here;
        ...
}
```
3) Update android/settings.gradle with
```include ':react-native-google-signin'
   project(':react-native-google-signin').projectDir = new File(rootProject.projectDir, '../node_modules/react-native-     google-signin/android')
```
4) Update android/app/build.gradle with
```a)

dependencies{
    implementation fileTree(dir: "libs", include: ["*.jar"])
        implementation(project(":react-native-google-signin"))
        implementation "com.facebook.react:react-native:+"  // From node_modules
        implementation "com.android.support:appcompat-v7:23.0.1"
        implementation(project(":react-native-google-signin"))
        implementation 'com.google.firebase:firebase-core:17.0.0'
        ...
}
```
```b) in the end of file

apply plugin: 'com.google.gms.google-services'
```
5)  Go to https://console.firebase.google.com/u/0/

6) Create new project or select already exists

7) Select creacting android/ios and add name from ~/android/app/src/main/AndroidManifest.xml

8) create certificate by run command
```
keytool -list -v -keystore /android/app/debug.keystore
```

9) download google.services and place it to ~/android/app

10) run command to sync project with frebase DB
```
react-native run-android

react-native start
```
11) add SHA-256 certificate to your firebase app

12) after it open firebase console and open  Develop/Authentification and allow auth with google

13) go to https://console.developers.google.com and create new project with  similar name

14) go to https://developers.google.com/identity/sign-in/android/start and press "CONFIGURE A PROJECT"

15) select your created project in console.developer.google and configure it by adding SHA-1


16) add to your App
```
    *This is th Example of google Sign in*/
    import React from 'react';
    import { StyleSheet, Text, View, Alert, Button } from 'react-native';
    import {
        GoogleSignin,
            GoogleSigninButton,
            statusCodes,
    } from 'react-native-google-signin';
    export default class App extends React.Component {
        constructor(props) {
            super(props);
            this.state = {
    userInfo: '',
            };
        }
        componentDidMount() {
            GoogleSignin.configure({
                    //It is mandatory to call this method before attempting to call signIn()
    scopes: ['https://www.googleapis.com/auth/drive.readonly'],
    // Repleace with your webClientId generated from Firebase console
    webClientId:
    'Replace Your Web Client Id here',
    });
    }
    _signIn = async () => {
        //Prompts a modal to let the user sign in into your application.
        try {
            await GoogleSignin.hasPlayServices({
                    //Check if device has Google Play Services installed.
                    //Always resolves to true on iOS.
    showPlayServicesUpdateDialog: true,
    })
    const userInfo = await GoogleSignin.signIn();
    console.log('User Info --> ', userInfo);
    this.setState({ userInfo: userInfo });
    } catch (error) {
        console.log('Message', error.message);
        if (error.code === statusCodes.SIGN_IN_CANCELLED) {
            console.log('User Cancelled the Login Flow');
        } else if (error.code === statusCodes.IN_PROGRESS) {
            console.log('Signing In');
        } else if (error.code === statusCodes.PLAY_SERVICES_NOT_AVAILABLE) {
            console.log('Play Services Not Available or Outdated');
        } else {
            console.log('Some Other Error Happened');
        }
    }
    };
    _getCurrentUser = async () => {
        //May be called eg. in the componentDidMount of your main component.
        //This method returns the current user
        //if they already signed in and null otherwise.
        try {
            const userInfo = await GoogleSignin.signInSilently();
            this.setState({ userInfo });
        } catch (error) {
            console.error(error);
        }
    };
    _signOut = async () => {
        //Remove user session from the device.
        try {
            await GoogleSignin.revokeAccess();
            await GoogleSignin.signOut();
            this.setState({ user: null }); // Remove the user from your app's state as well
        } catch (error) {
            console.error(error);
        }
    };
    _revokeAccess = async () => {
        //Remove your application from the user authorized applications.
        try {
            await GoogleSignin.revokeAccess();
            console.log('deleted');
        } catch (error) {
            console.error(error);
        }
    };
    render() {
            return (
                    <View style={styles.container}>
                        <GoogleSigninButton
                        style={{ width: 312, height: 48 }}
                        size={GoogleSigninButton.Size.Wide}
                        color={GoogleSigninButton.Color.Light}
                        onPress={this._signIn}
                        />
                        <Button  style={{ width: 312, height: 48 }} onPress={this._signOut}/>
                    </View>
            );
    }
    }
    const styles = StyleSheet.create({
        container: {
            flex: 1,
            backgroundColor: '#fff',
            alignItems: 'center',
            justifyContent: 'center',
        },
    });
```
17) update your webclientid with webclient from https://console.developers.google.com/apis/credentials

18) run command
```
    react-native run-android

    react-native start
```
19) Well done, Project must work)

