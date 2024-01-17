# Area Developement Guide

A brief description of your project.

## Getting Started

### Prerequisites

- Node.js
- Yarn
- Expo CLI

### Installation

1. Clone the repository.
2. Navigate to the project directory.
3. Run `yarn install` to install the dependencies.

### Running the App

- Run `yarn expo start` to start the development server.

## Folder Structure

Many folders and files are at your disposal, let's take a closer look at what they are used for.

• The main folder, [tabs][tab], in this folder you can access all pages of the mobile application. We will go into more detail about the codes later.

• In the [Components][compo] folder, you can find components like the dropdown menu. This folder allows you to create large components that are easily usable in your page.

• This [assets][asset] folder brings together all the images from your mobile application so that you can later find and use them

## For contribuat at this project

Refer to the [Getting Started](#getting-started) part to clone the project.

After cloning the project, here is the [App.js][appjs], the initial file of the application. In this file you can change the `initialRouteName` to the page you want to display first. Just below, you will see the other navigation pages of your mobile application

If you want to add APIs for connections or Action / Reaction, you can refer to the [env.js][envjs] file, which will allow you to enter all the information useful for connecting the API for your application

Now, let's go into more detail in the tabs folder.
All pages have approximately the same format. We coded them in React native, a simple and intuitive language with great potential with our application.

Look at the [login page][loginP]. Firstly at the top of the page, we need to do our imports to be able to use the libraries or even simply move from page to page.

```jsx
import React, { useState } from 'react';
import { StyleSheet, TextInput, Text, TouchableOpacity, View, Image, ScrollView, } from 'react-native';
import { useNavigation } from '@react-navigation/native';
import env from '../env';
import axios from 'axios';
import AsyncStorage from '@react-native-async-storage/async-storage';
import CheckboxComponent from '../components/CheckBoxComponent'; // Update the path based on your file structure
```

Next, we have the functions. They allow you to take actions. For example, when you click on a button, you can use these functions to navigate to another page.

An example of a function that does this:

```jsx
const navigateSignUp = async () => {
  console.log('Sign-up Pressed');
  navigation.navigate('RegisterPage');
};
```

We then have, the first visual part, an equivalent of html in react native. We will use tags like <view></view> to frame our sub tags like <Text></Text> tags.

Here is an example which makes the header of our application:

```jsx
<View style={styles.container}>

  <View style={styles.header}>
    <Image source={require('../assets/png/AREA_logo.png')} style={styles.AREAlogo} />
    <TouchableOpacity style={styles.signInButton} onPress={navigateSignUp}>
      <Text style={styles.signInButtonText}>Sign Up</Text>
    </TouchableOpacity>
  </View>

  <View style={styles.separator} />
</View>
```

Finally, here is the second visual part, an equivalent of CSS in react native, you will be able to modify everything in terms of the style of the text page, the beauty of the pages will mainly be done here.

We can make precise location references or we want to do this with the typography `style={styles.header}` for example.

I have given you an example below which goes with the views from the previous example.

```jsx
container: {
    flex: 1,
    backgroundColor: 'white',
    padding: 20,
  },
  header: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    alignItems: 'center',
    marginBottom: 20,
  },
  AREAlogo: {
    width: 45,
    height: 45,
    marginLeft: 20,
    marginTop: 35,
  },
  signInButton: {
    backgroundColor: '#9300FF',
    borderRadius: 10,
    paddingVertical: 8,
    paddingHorizontal: 16,
    borderWidth: 1,
    borderColor: '#9300FF',
    marginLeft: 20,
    marginTop: 35,
  },
  signInButtonText: {
    color: 'white',
    fontSize: 16,
  },
  separator: {
    borderBottomColor: 'black',
    borderBottomWidth: 1,
    width: '125%',
    marginLeft: -25,
  },
```

[tab]: https://github.com/EpitechPromo2026/B-DEV-500-MAR-5-2-area-thibault.avon/tree/web/AREA-MOBILE/tabs
[compo]: https://github.com/EpitechPromo2026/B-DEV-500-MAR-5-2-area-thibault.avon/tree/web/AREA-MOBILE/components
[asset]: https://github.com/EpitechPromo2026/B-DEV-500-MAR-5-2-area-thibault.avon/tree/web/AREA-MOBILE/assets
[appjs]: https://github.com/EpitechPromo2026/B-DEV-500-MAR-5-2-area-thibault.avon/blob/web/AREA-MOBILE/App.js
[envjs]: https://github.com/EpitechPromo2026/B-DEV-500-MAR-5-2-area-thibault.avon/blob/web/AREA-MOBILE/env.js
[loginP]: https://github.com/EpitechPromo2026/B-DEV-500-MAR-5-2-area-thibault.avon/blob/web/AREA-MOBILE/tabs/login_page.js
