<p align="center"><a href="https://koa-vue-notes-web.innermonkdesign.com/" target="_blank"><img width="200" src="./static/koa-vue-notes-icon.png"></a></p>

<p align="center">
  <a href="http://opensource.org/licenses/MIT"><img src="https://img.shields.io/badge/license-MIT-blue.svg" alt="License"></a>
  <a href="https://twitter.com/intent/tweet?url=https%3A%2F%2Fgithub.com%2Fjohndatserakis%2Fkoa-vue-notes-web&text=Check%20out%20koa-vue-notes-web%20on%20GitHub&via=innermonkdesign">
  <img src="https://img.shields.io/twitter/url/https/github.com/johndatserakis/koa-vue-notes-web.svg?style=social" alt="Tweet"></a>
</p>

# Koa-Vue-Notes-Web

This is a simple SPA built using [Koa](http://koajs.com/) (2.3) as the backend and [Vue](https://vuejs.org/) (2.4) as the frontend. Click [here](https://github.com/johndatserakis/koa-vue-notes-api) to see the backend Koa code. Click [here](https://koa-vue-notes-web.innermonkdesign.com/) to view the app live.

## Features
- Vue 2.4 (Initialized by Vue-CLI with Webpack)
- Vue-Router
- Vuex
- Fully written using async/await
- Bootstrap 4 Beta
- SASS
- Vuelidate validation library
- Vue-Toasted toast messages
- JWT for authentication
- Axios
- Font-Awesome
- Vue-Progressbar
- Vue-Js-Modal
- Jest and Vue-Test-Utils for testing
- And more...

## Installing / Getting started

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run watch

# build for production with minification
npm run build

# run tests
npm run test

# run linter
npm run lint

# run linter with auto-fix
npm run lint-fix
```

## General Information

This frontend is part of a pair of projects that serve a simple notes app. I chose a notes app because it gives you a good look at the different techniques used in both the frontend and backend world. What's really cool is these projects feature a fully fleshed-out user login/signup/forgot/reset authentication system using JWT.

For the base of the project make sure to check out the [Vue-CLI](https://github.com/vuejs/vue-cli) docs if you haven't already. The base of this project is laid out in the *Vue-CLI* way. I chose this path because Evan did a really great job thinking through the different aspects of laying out an application. `NODE_ENV`, `API_URL`, and `APP_URL` are some `.env` variables I've added in `config` and use throughout the code - so keep an eye out for those and replace them with your relevant information.

I've liberally commented the code and tried to balance the project in a way that it's complex enough to learn from but not so complex that it's impossible to follow. It can be tough to learn from a boilerplate that has too much or too little.

Having used mainly PHP for the backend in the past - I am very glad I checked out Koa as I think it is absolutely awesome in the way it handles the server code. Same thing with Vue - I've used mainly jQuery in the past - albeit with the really structured Revealing-Module-Pattern - and using Vue was such a pleasure. You can really tell right away what kind of power a well-structured library can give you.

The `src` folder is laid out in the following fashion:

### assets

Here you'll find the program's SASS files. There's a bunch of component files as you drill down. I do use some of the .vue style concepts on certain components - but there is most definitely a case to be made for having all your base style code in one place. We're also importing Font-Awesome icons in `src/App.js`.

Honestly, I really like Bootstrap, and v4 is very nice - but I'm not a huge fan of using its components because they still use jQuery. Also, I really wish it was written using the BEM syntax - something I use for my own components. With that being said - this project only makes use of Bootstrap's grid, buttons, form-groups, and navbar - although you may want to go in a different direction with that.

### common

For utlity functions.

### components

Here are the .vue components that make up the app. This folder is further broken down into a few subfolders to keep the views organized. Pretty straight-forward. Because this app used Vuex the components only have local data when needed - like form elements. Otherwise I'm mapping out Vuex store variables in the `computed` section of each component.

### router

This is the vue-router code. Here you'll find the creation and connection of each view in the app. One thing you're going to want to take a look at is the `beforeEach` method where we deal with the user authentication taking place in the app.

On each router action we grab the `accessToken` and `refreshToken` from our `localStorage`. If the `accessToken` is present we set the user in our Vuex store and continue on our way.

### User Authentication Process

As mentioned in the backend code, the user authentication process is this:

- User create an account
- User logs in
- The server sends and `accessToken` and a `refreshToken` back
- We take the `accessToken` and decoded it using `jwt-decode`. This gets us the logged in user's information. We stick this in the Vuex variable `user`. Then we store the `refreshToken`.
- Each protected endpoint will be expecting you to attach the `accessToken` you have to the call (using Authentication: Bearer). After a short amount of time, the server will respond with `401 TOKEN EXPIRED`. When you see this - that means you need to send your `refreshToken` and `user.email` to the endpoint that deals with `accessToken` refreshing. Once you do that, you'll received a brand new `accessToken` and `refreshToken`.
- Repeat the process as needed.

I've utilized the great Axios `axios.interceptors.response` utility to capture the case of an expired `accessToken` and refresh it - all without the user being made aware of the process. The key is to keep the promise-chain alive - this is so the component caller can update it's local state - things like page count, sort - stuff that's important but really doesn't belong in our Vuex store because it's only relevant to the calling component. Take a look at the user.js store - that's where the interceptor is set up. If it recognizes this is a refresh situation it calls two Vuex actions and then resolves with the resent request.

### store

The store folder is where all the Vuex files are. We are using the modules feature of Vuex which allows us to have different stores for each module. In this app there are two modules - `user` and `note`. Vuex turned out to be really cool. One of the main things I'll point out is that each action should return a promise. If you follow this methodology you'll find it makes it really easy to keep in sync with a API call a component might make.

### App.vue file

This is our main app component. Things like the navbar, footer, v-dialog (modals), and vue-progress-bar are placed here.

### main.js file

Our main entrance to our JavaScript code - all the main modules like our Vuex store and router are loaded here. This is also where our main Vue instance is implemented.

### Todo

- ~~Use async/await on entire frontend~~
- Continue to add tests

## Testing

Testing is done using the newly official [vue-test-utils](https://github.com/vuejs/vue-test-utils) library - it's very good - but in the early stages of development. Due to some of the issues that I'm currently experiencing with the library - there are only a few simple tests that make sure the Home and User Action components are loaded, can add data to inputs, and click their respective submit buttons. I'll be adding more involved tests in the future.

## Hit Me Up

Go ahead and fork the project! Message me here if you have questions or submit an issue if needed. I'll be making touch-ups as time goes on. Have fun with this!

## License

Copywrite 2017 John Datserakis

[MIT](http://opensource.org/licenses/MIT)