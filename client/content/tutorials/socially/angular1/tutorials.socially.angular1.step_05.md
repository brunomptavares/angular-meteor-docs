{{#template name="tutorials.socially.angular1.step_05.md"}}
{{> downloadPreviousStep stepName="step_04"}}

In this step, you will learn how to create a layout template and how to build an app that has multiple views by adding routing, using an Angular 1 module called `ui-router`.

The goals for this step:

* When you navigate to `index.html`, you will be redirected to `/parties` and the party list should appear in the browser.
* When you click on a party link the URL should change to one specific to that party and the stub of a party detail page is displayed.

# Dependencies

The routing functionality added by this step is provided by the [ui-router](https://github.com/angular-ui/ui-router) module, which is distributed separately from the core Angular 1 framework.

Type in the command line:

    meteor npm install --save angular-ui-router

{{> DiffBox tutorialName="meteor-angular1-socially" step="5.1"}}

Then add the ui-router as a dependency to our Socially app in `socially.js`:

{{> DiffBox tutorialName="meteor-angular1-socially" step="5.2"}}

# Multiple Views, Routing and Layout Template

Our app is slowly growing and becoming more complex.
Until now, the app provided our users with a single view (the list of all parties), and all of the template code was located in the `main.html` file.

The next step in building the app is to add a view that will show detailed information about each of the parties on our list.

To add the detailed view, we could expand the `main.html` file to contain template code for both views, but that would get messy very quickly.

Instead, we are going to turn the `index.html` template into what we call a "layout template". This is a template that is common for all views in our application.
Other "partial templates" are then included into this layout template depending on the current "route" — the view that is currently displayed to the user.

Application routes in Angular 1 are declared via the [$stateProvider](https://github.com/angular-ui/ui-router/wiki), which is the provider of the $state service.
This service makes it easy to wire together controllers, view templates, and the current URL location in the browser.
Using this feature we can implement deep linking, which lets us utilize the browser's history (back and forward navigation) and bookmarks.


# Template

The `$state` service is normally used in conjunction with the uiView directive.
The role of the `ui-view` directive is to include the view template for the current route into the layout template.
This makes it a perfect fit for our `main.html` file.

Now let's go back to `index.html` to add base tag to our main html file:

{{> DiffBox tutorialName="meteor-angular1-socially" step="5.3"}}

We still need to add uiView directive, Socially is the best place for it:

{{> DiffBox tutorialName="meteor-angular1-socially" step="5.4"}}

Let's define a default route and use html5 mode to make urls look a lot fancier:

{{> DiffBox tutorialName="meteor-angular1-socially" step="5.6"}}

It would be nice to have a navigation. Create one! Let's call our new component `Navigation`:

{{> DiffBox tutorialName="meteor-angular1-socially" step="5.7"}}

{{> DiffBox tutorialName="meteor-angular1-socially" step="5.8"}}

And implement it in `Socially`:

{{> DiffBox tutorialName="meteor-angular1-socially" step="5.9"}}

{{> DiffBox tutorialName="meteor-angular1-socially" step="5.10"}}

Notice we did 3 things:

1. Replaced all the content inside Socially component with ui-view (this will be responsible for including the right content according to the current URL).
2. Defined default route.
3. Created navigation.
4. We also added a `base` tag in the head (required when using HTML5 location mode - would be explained a bit further).

> Note that you can remove `main.html` now, because it's no longer in use!

# Routes definition

Now let's configure our routes.
There are no states at this stage, so let's add `parties` inside `PartiesList` component:

{{> DiffBox tutorialName="meteor-angular1-socially" step="5.5"}}

And we will also add a state for a new page that will display the party details.

Our application routes are defined as follows:

* **('/parties')**: The parties list view will be shown when the URL hash fragment is /parties. To construct this view, Angular 1 will use the parties-list Component.
* **('/parties/:partyId')**: The party details view will be shown when the URL hash fragment matches '/parties/:partyId', where `:partyId` is a variable part of the URL. To construct the party details view, Angular will use the party-details Component.
* **$urlRouterProvider.otherwise('/parties')**: Triggers a redirection to /parties when the browser address doesn't match either of our routes.
* **$locationProvider.html5Mode(true)**: Sets the URL to look like a regular one. More about it [here](https://docs.angularjs.org/guide/$location#hashbang-and-html5-modes).
* Each template is just a regular usage of our components.

Note the use of the `:partyId` parameter in the second route declaration.
The $state service uses the route declaration — `/parties/:partyId` — as a template that is matched against the current URL.
All variables defined with the : notation are passed into the Component through the `$stateParams` object.

# Components

I mentioned a state with party details. We have to define our `partyDetails` component.

Let's create the view for this Component in a new file:

{{> DiffBox tutorialName="meteor-angular1-socially" step="5.11"}}

Now we can create actual component:

{{> DiffBox tutorialName="meteor-angular1-socially" step="5.12"}}

And also define new route which was mentioned before as `/parties/:partyId`:

{{> DiffBox tutorialName="meteor-angular1-socially" step="5.13"}}

Now let's add a link from each party in the parties list to it's details page:

{{> DiffBox tutorialName="meteor-angular1-socially" step="5.14"}}

{{> DiffBox tutorialName="meteor-angular1-socially" step="5.15"}}

Now all is in place.  Run the app and you'll notice a few things:

* Click on the link in the name of a party - notice that you moved into a different view and that the party's id appears in both the browser's url and in the template.
* Click back - you are back to the main list, this is because of ui-router's integration with the browser's history.
* Try to put arbitrary text in the URL - something like [http://localhost:3000/strange-url](http://localhost:3000/strange-url).  You should to be automatically redirected to the main parties list.

# Summary

With the routing set up and the parties list view implemented, we're ready to go to the next step and implement the party details view.

{{/template}}
