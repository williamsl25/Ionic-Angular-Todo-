### 1) install Cordova
##### - npm install -g cordova
### 2) Install Ionic
#### - npm install -g ionic
### 3) Create the project
#### - ionic start todo blank
#### - cd todo && ls
### 4) Configure Platforms
#### - ionic platform add ios
#### - ionic platform add android
### 5) Test it
#### - ionic build ios
#### - ionic emulate ios
### 6) create index
 <!DOCTYPE html>
    <html>
      <head>
        <meta charset="utf-8">
        <title>Todo</title>
        <meta name="viewport" content="initial-scale=1, maximum-scale=1, user-scalable=no, width=device-width">

        <link href="lib/ionic/css/ionic.css" rel="stylesheet">

        <script src="lib/ionic/js/ionic.bundle.js"></script>
         <!-- tell Angular to initialize app.js -->
        <script src="js/app.js"></script>
        <!-- Needed for Cordova/PhoneGap (will be a 404 during development) -->
        <script src="cordova.js"></script>
      </head>
      <body ng-app="todo">
        <ion-side-menus>

          <!-- Center content -->
          <ion-side-menu-content>
            <ion-header-bar class="bar-dark">
              <h1 class="title">Todo</h1>
            </ion-header-bar>
            <ion-content>
            </ion-content>
          </ion-side-menu-content>

          <!-- Left menu -->
          <ion-side-menu side="left">
            <ion-header-bar class="bar-dark">
              <h1 class="title">Projects</h1>
            </ion-header-bar>
          </ion-side-menu>

        </ion-side-menus>
      </body>
    </html>
### 7) add the ng-app attribute to the body tag:
  <body ng-app="todo">
### 8) Testing your app
#### - in the browser:
- ionic serve
#### - Simulator testing
- ionic build ios
- ionic emulate ios
#### 9) add ng-repeat
- add where list of items will go
#### 10) add Angular controller (TodoCtrl)
- <body ng-app="todo" ng-controller="TodoCtrl">
#### 11) define this controller in app.js
angular.module('todo', ['ionic']) //Initializing the app

.controller('TodoCtrl', function($scope) {
  $scope.tasks = [
    { title: 'Collect coins' },
    { title: 'Eat mushrooms' },
    { title: 'Get high enough to grab the flag' },
    { title: 'Find the Princess' }
  ];
});
#### 12) Creating tasks
