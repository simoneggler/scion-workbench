![SCION Workbench](/resources/site/logo/scion-workbench-banner.png)

[Overview][menu-overview] | [Workbench][menu-workbench] | [Workbench&nbsp;Application&nbsp;Platform][menu-workbench-application-platform] | [Contributing][menu-contributing] | [Changelog][menu-changelog] | [Sponsoring][menu-sponsoring] | [Links][menu-links]
|---|---|---|---|---|---|---|

## How to open activities and views of lazy loaded modules

There is nothing special in opening views or activities of lazy loaded modules. The magic is done by Angular router.

Follow the steps below to create a feature module and open its activity and views.

### 1. Create a feature module with routing capability using Angular command-line tool
   
The following commands create the feature module named 'feature', an activity component and two view components:

```
ng generate module feature --routing

ng generate component feature/activity
ng generate component feature/view1
ng generate component feature/view2
```

### 2. In `FeatureModule`, manifest a dependency to the workbench
```typescript
@NgModule({
  imports: [
    CommonModule,
    FeatureRoutingModule,
    WorkbenchModule.forChild(),
  ],
  declarations: [
    ActivityComponent, 
    View1Component,
    View2Component
  ]
})
export class FeatureModule {
}
```

### 3. In `FeatureRoutingModule`, add the routes to the activity component and the two view components

```typescript
const routes: Routes = [
  {
    path: '',
    component: ActivityComponent
  },      
  {
    path: 'view-1',
    component: View1Component
  },
  {
    path: 'view-2',
    component: View2Component
  },
];

@NgModule({
  imports: [RouterModule.forChild(routes)],
  exports: [RouterModule]
})
export class FeatureRoutingModule {
}
```
### 4. In `AppRoutingModule`, update the routes array to point to the feature module

```typescript
const routes: Routes = [
  {
    path: 'feature', // base path to the feature module
    loadChildren: './feature/feature.module#FeatureModule'
  },      
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule {
}
```
Notice that the lazy loading syntax uses loadChildren followed by a string that is the relative path to the module, a hash mark or #, and the module’s class name. It is important to pass a string instead of a symbol to not load the module eagerly.

### 5. Open activities or views of the feature module

In `app.component.html`, add an activity to open the activity of the feature module. Because the activity is registered on an empty path, the router link corresponds to the path to the feature module.

```html
<wb-workbench>
  <wb-activity title="Activity of feature module"
               itemText="star_outline"
               itemCssClass="material-icons"
               routerLink="feature">
  </wb-activity>
</wb-workbench>
```

To open a view from the app module, use router link as following:

```html
<a wbRouterLink="/feature/view-1">Open view 1 of feature module</a>
<a wbRouterLink="/feature/view-2">Open view 2 of feature module</a>
```

To open a view from within the activity of the feature module, use its relative path instead:

```html
<a wbRouterLink="view-1">Open view 1</a>
<a wbRouterLink="view-2">Open view 2</a>
```

To open another view from within a view of the feature module, go back one level first, and then use its relative path:

```html
<a wbRouterLink="../view-2">Open view 2</a>
```

[menu-overview]: /README.md
[menu-workbench]: /resources/site/workbench.md
[menu-workbench-application-platform]: /resources/site/workbench-application-platform.md
[menu-contributing]: /CONTRIBUTING.md
[menu-changelog]: /resources/site/changelog.md
[menu-sponsoring]: /resources/site/sponsors.md
[menu-links]: /resources/site/links.md