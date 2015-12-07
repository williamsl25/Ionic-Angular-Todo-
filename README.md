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
####  in the browser:
- ionic serve
####  Simulator testing
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
- add a simple Modal window that slides up, letting us put in a new task.
- Place
the following script tag after the closing </ion-side-menu> tag in the <body> of the HTML file:

    <script id="new-task.html" type="text/ng-template">

- define the template as an angular template:


<!-- set a header with a button to close the modal, and then set up our content area. For the form, we are calling createTask(task) when the form is submitted. The task that is passed to createTask is the object corresponding to the entered form data. Since our text input has ng-model="task.title", that text input will set the title property of the task object. -->
  <div class="modal">

    <!-- Modal header bar -->
    <ion-header-bar class="bar-secondary">
      <h1 class="title">New Task</h1>
      <button class="button button-clear button-positive" ng-click="closeNewTask()">Cancel</button>
    </ion-header-bar>

    <!-- Modal content area -->
    <ion-content>

      <form ng-submit="createTask(task)">
        <div class="list">
          <label class="item item-input">
            <input type="text" placeholder="What do you need to do?" ng-model="task.title">
          </label>
        </div>
        <div class="padding">
          <button type="submit" class="button button-block button-positive">Create Task</button>
        </div>
      </form>

    </ion-content>

  </div>

</script>

 - In order to trigger the Modal to open, we need a button in the main header bar and some code to open the modal, the center content then becomes:
<html>
<!-- Center content -->
  <ion-side-menu-content>
    <ion-header-bar class="bar-dark">
      <h1 class="title">Todo</h1>
      <!-- New Task button-->
      <button class="button button-icon" ng-click="newTask()">
        <i class="icon ion-compose"></i>
      </button>
    </ion-header-bar>
    <ion-content>
      <!-- our list and list items -->
      <ion-list>
        <ion-item ng-repeat="task in tasks">
          {{task.title}}
        </ion-item>
      </ion-list>
    </ion-content>
  </ion-side-menu-content>
</html>
-  in our controller code:
angular.module('todo', ['ionic'])

.controller('TodoCtrl', function($scope, $ionicModal) {
  <!-- // No need for testing data anymore -->
  $scope.tasks = [];

  <!-- // Create and load the Modal -->
  $ionicModal.fromTemplateUrl('new-task.html', function(modal) {
    $scope.taskModal = modal;
  }, {
    scope: $scope,
    animation: 'slide-in-up'
  });

  <!-- // Called when the form is submitted -->
  $scope.createTask = function(task) {
    $scope.tasks.push({
      title: task.title
    });
    $scope.taskModal.hide();
    task.title = "";
  };

  <!-- // Open our new task modal -->
  $scope.newTask = function() {
    $scope.taskModal.show();
  };

  <!-- // Close the new task modal -->
  $scope.closeNewTask = function() {
    $scope.taskModal.hide();
  };
});
- new content area markup:
<!-- Center content -->
<ion-side-menu-content>
  <ion-header-bar class="bar-dark">
    <button class="button button-icon" ng-click="toggleProjects()">
      <i class="icon ion-navicon"></i>
    </button>
    <h1 class="title">{{activeProject.title}}</h1>
    <!-- New Task button-->
    <button class="button button-icon" ng-click="newTask()">
      <i class="icon ion-compose"></i>
    </button>
  </ion-header-bar>
  <ion-content scroll="false">
    <ion-list>
      <ion-item ng-repeat="task in activeProject.tasks">
        {{task.title}}
      </ion-item>
    </ion-list>
  </ion-content>
</ion-side-menu-content>
#### - new side menu markup:
<!-- This adds a side menu of projects, letting us click on each project and also add a new one with a small plus icon button in the header bar. The ng-class directive in the <ion-item> makes sure to add the active class to the currently active project. -->
<!-- Left menu -->
 <ion-side-menu side="left">
   <ion-header-bar class="bar-dark">
     <h1 class="title">Projects</h1>
     <button class="button button-icon ion-plus" ng-click="newProject()">
     </button>
   </ion-header-bar>
   <ion-content scroll="false">
     <ion-list>
       <ion-item ng-repeat="project in projects" ng-click="selectProject(project, $index)" ng-class="{active: activeProject == project}">
         {{project.title}}
       </ion-item>
     </ion-list>
   </ion-content>
 </ion-side-menu>
#### -To enable adding, saving, and loading projects in app.js:
angular.module('todo', ['ionic'])
<!--
 * The Projects factory handles saving and loading projects
 * from local storage, and also lets us save and load the
 * last active project index.
-->
.factory('Projects', function() {
  return {
    all: function() {
      var projectString = window.localStorage['projects'];
      if(projectString) {
        return angular.fromJson(projectString);
      }
      return [];
    },
    save: function(projects) {
      window.localStorage['projects'] = angular.toJson(projects);
    },
    newProject: function(projectTitle) {
      // Add a new project
      return {
        title: projectTitle,
        tasks: []
      };
    },
    getLastActiveIndex: function() {
      return parseInt(window.localStorage['lastActiveProject']) || 0;
    },
    setLastActiveIndex: function(index) {
      window.localStorage['lastActiveProject'] = index;
    }
  }
})

.controller('TodoCtrl', function($scope, $timeout, $ionicModal, Projects, $ionicSideMenuDelegate) {

  // A utility function for creating a new project
  // with the given projectTitle
  var createProject = function(projectTitle) {
    var newProject = Projects.newProject(projectTitle);
    $scope.projects.push(newProject);
    Projects.save($scope.projects);
    $scope.selectProject(newProject, $scope.projects.length-1);
  }


  // Load or initialize projects
  $scope.projects = Projects.all();

  // Grab the last active, or the first project
  $scope.activeProject = $scope.projects[Projects.getLastActiveIndex()];

  // Called to create a new project
  $scope.newProject = function() {
    var projectTitle = prompt('Project name');
    if(projectTitle) {
      createProject(projectTitle);
    }
  };

  // Called to select the given project
  $scope.selectProject = function(project, index) {
    $scope.activeProject = project;
    Projects.setLastActiveIndex(index);
    $ionicSideMenuDelegate.toggleLeft(false);
  };

  // Create our modal
  $ionicModal.fromTemplateUrl('new-task.html', function(modal) {
    $scope.taskModal = modal;
  }, {
    scope: $scope
  });

  $scope.createTask = function(task) {
    if(!$scope.activeProject || !task) {
      return;
    }
    $scope.activeProject.tasks.push({
      title: task.title
    });
    $scope.taskModal.hide();

    // Inefficient, but save all the projects
    Projects.save($scope.projects);

    task.title = "";
  };

  $scope.newTask = function() {
    $scope.taskModal.show();
  };

  $scope.closeNewTask = function() {
    $scope.taskModal.hide();
  }

  $scope.toggleProjects = function() {
    $ionicSideMenuDelegate.toggleLeft();
  };


  // Try to create the first project, make sure to defer
  // this by using $timeout so everything is initialized
  // properly
  $timeout(function() {
    if($scope.projects.length == 0) {
      while(true) {
        var projectTitle = prompt('Your first project title:');
        if(projectTitle) {
          createProject(projectTitle);
          break;
        }
      }
    }
  });

});
