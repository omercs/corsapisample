# corsapisample
cors related sample

## How to Use ##

You can create a service that talks to this endpoint.

```JS
'use strict';
app.factory('contactService', ['$http', function ($http) {
    var serviceFactory = {};

    var _getItems = function () {
        $http.defaults.useXDomain = true;
        delete $http.defaults.headers.common['X-Requested-With'];
        return $http.get('http://yoursite.azurewebsites.net/api/contacts');
    };

    serviceFactory.getItems = _getItems;

    return serviceFactory;
}]);
```

Sample app config:
```JS
'use strict';
var app = angular.module('adalDemo', ['ngRoute', 'AdalAngular']);

app.config(['$httpProvider', '$routeProvider', 'adalAuthenticationServiceProvider', function ($httpProvider, $routeProvider, adalAuthenticationServiceProvider) {

    $routeProvider.when('/home', {
        controller: 'homeController',
        templateUrl: '/App/Views/landingPage.html',
    }).
    when('/login', {
        controller: 'homeController',
        templateUrl: '/App/Views/login.html'
    }).
    when('/todoList', {
        controller: 'todoListController',
        templateUrl: '/App/Views/todoList.html',
        requireADLogin: true
    }).
    when('/todoList/detail/:abc', {
        controller: 'todoDetailController',
        templateUrl: '/App/Views/todoDetail.html',
        requireADLogin: true
    }).
    when('/todoList/new', {
        controller: 'todoDetailController',
        templateUrl: '/App/Views/todoNew.html',
        requireADLogin: true
    }).
        when('/contactList', {
            controller: 'contactController',
            templateUrl: '/App/Views/contact.html'
        }).
        
    otherwise({ redirectTo: '/home' });

    // endpoint to resource mapping(optional)
    var endpoints = {
        'http://yoursite.azurewebsites.net/api/': 'be9ce842-47f2-456e-96c1-5a180c765a28',
    };

    adalAuthenticationServiceProvider.init(
        {
            // Config to specify endpoints and similar for your app
            tenant: '0fd157fc-29ea-4fb5-bdbc-a195bd16ff80',
            clientId: 'cb68f72f-2b04-42e1-bcf6-db25ddd48a5c',
            //instance: 'https://login.windows-ppe.net/',
            //redirectUri : 'your site', optional
            endpoints: endpoints,  // optional
           // cacheLocation: 'localStorage' // optional cache location default is sessionStorage
        },
        $httpProvider   // pass http provider to inject request interceptor to attach tokens
        );
}]);
```

External endpoint is defined to talk to the CorsAPI. Token will be added to the header.
