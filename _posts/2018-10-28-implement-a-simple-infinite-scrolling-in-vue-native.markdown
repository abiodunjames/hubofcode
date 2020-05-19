---
title: Vue-native infinite scroll
date: 2018-10-28 15:07:12 Z
layout: post
author: Samuel
comments: true
---

This post is aimed to show you how to implement infinite scrolling in [Vue-native](https://vue-native.io/). Without doubts, infinite scrolling is your best bet when it comes to paginating large data set, especially in mobile apps.

It's exciting to know that infinite scrolling can be implemented in a few easy steps with [Vue-native](https://vue-native.io/)

## What we will use

* [Reqres api](https://reqres.in/)
* [Vue-native](https://vue-native.io)
* [Axios](https://reqres.in/)


### Install Vue-native-cli

Install [vue-native cli](https://github.com/GeekyAnts/vue-native-cli) if it's not installed.

```
npm install -g vue-native-cli
```

### Start a Vue-native project

```
vue-native init infinteScroll
```

Select `blank` from the options as shown below: 

![Blank](https://res.cloudinary.com/samueljames/image/upload/c_scale,h_786/v1540740190/Screen_Shot_2018-10-28_at_16.09.51.png)

At this point, everything you need to start a [vue-native](https://vue-native.io) application should have been created. Sweet! right? Let’s go ahead and install a few more dependencies.

Navigate to the project root and run:

```
npm install -s axios native-base
```

### Making a 'UserList' Component

Create directory `component` and add a file `UserList.vue` in the directory.

```javascript
/** filename: component/UserList.vue **/

<template>
    <nb-container>
        <nb-header>
                <nb-title>Infinite Scrolling</nb-title>
        </nb-header>
        <scroll-view :on-scroll="(event) => {loadMore(event)}" :scroll-event-throttle="400">
            <nb-content>
                <nb-list-item avatar v-for="user in users">
                    <nb-left>
                        <nb-thumbnail small :source="{uri: user.avatar}"/>
                    </nb-left>
                    <nb-body>
                        <nb-text>{{user.first_name}} {{user.last_name }}</nb-text>
                    </nb-body>
                </nb-list-item>
            </nb-content>
        </scroll-view>
        <view :style="{flex: 1, justifyContent: 'center'}" v-if="loading">
            <activity-indicator size="large" color="gray"/>
        </view>
    </nb-container>

</template>

<script>
    import axios from "axios"

    export default {
        data: function () {
            return {
                loading: false,
                users: [],
                per_page: 15
            }
        },
        mounted: function () {
            this.getData();
        },

        methods: {
            getData: function () {
                let uri = 'https://reqres.in/api/users';
                this.loading = true;
                axios.get(uri, {
                    params: {
                        per_page: this.per_page
                    }
                }).then((result) => {
                    let response = result.data.data;
                    console.log(response);
                    for (let i in response) {
                        this.users.push(response[i]);
                    }
                    this.loading = false;
                }).catch((error) => {
                    console.log(error)
                })
            },
            loadMore: function (event) {
                let paddingToBottom = 0;
                paddingToBottom += event.nativeEvent.layoutMeasurement.height;
                if (event.nativeEvent.contentOffset.y >= event.nativeEvent.contentSize.height - paddingToBottom) {
                    this.getData()
                }
            }
        }
    }
</script>

```


### What the heck is happening in UserList.vue?


We listen to `scroll event` which is fired once per frame, we then call `loadMore` which receives `event` as argument. Basically, it detects the end of scrolling and calls `getData`  if a user has scrolled to the bottom.

We control how often this event is fired by setting `scroll-event-throttle` to `400`. 
 
When users scroll down, we also want to give them a sense that more data are being loaded, so, we added `activity-indicator` that becomes visible when `loading` is true.

### Wrapping it off

If you take a look at the root folder, you will  see `App.vue` - a file created when we ran `vue-native init infinteScroll` to generate the boilerplate.

We update `App.vue` with:

```javascript
//App.vue

<template>
    <view class="container">
        <nb-container>
            <root>
                <user-list v-if="isReady"></user-list>
            </root>
        </nb-container>
    </view>
</template>

<script>
    import UserList from "./component/UserList"
    import {Root, VueNativeBase} from "native-base";
    import Vue from "vue-native-core";

    Vue.use(VueNativeBase);
    export default {
        data: function () {
            return {
                isReady: false
            }
        },
        created: function () {
            this.loadAppFonts();
        },
        methods: {
            loadAppFonts: async function () {
                try {
                    this.isReady = false;
                    await Expo.Font.loadAsync({
                        Roboto: require("native-base/Fonts/Roboto.ttf"),
                        Roboto_medium: require("native-base/Fonts/Roboto_medium.ttf"),
                        Ionicons: require("@expo/vector-icons/fonts/Ionicons.ttf")
                    });
                    this.isReady = true;
                } catch (error) {
                    console.log("Can't load fonts", error);
                    this.isReady = true;
                }
            },
        },
        components: {
            Root, UserList
        }

    }
</script>
<style>
    .container {
        justify-content: center;
        flex: 1;
    }
</style>

```

Above, we imported `UserList` component so we can reuse it in `App.vue` (in the root instance) and define a method: `loadAppFonts`  loads custom fonts required by native base asynchronously.


### Previewing the app 

If you are on iOS and you have iOS emulator installed, you can preview the app by running `npm run ios`. On Android, run `npm run android` (Android build tools must be installed)

![Infinite Scrolling](https://res.cloudinary.com/samueljames/image/upload/v1540737925/infinite-scrolling.gif)

You can also find the project [here](https://github.com/abiodunjames/Vue-native-Infinite-scroll) 
