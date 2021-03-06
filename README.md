# Step Template

## Introduction 
This repository contains the base template for building a RainCatcher workflow step. 
Template contains form that collects first name last name. Knowledge of Angular.js is required to use this template.

## Step structure

Steps are divided into two different categories:

### Module

Modules can contain one or more angular directives for rendering steps.
RainCatcher steps use Angularjs directives to build feature rich UI as part of the workflow.
Each step is divided into two types for directives:

- Edit section

This section is displayed when workflow is executed and allows users to edit information, 
Section always consist from the html template and directive.

For example: `base-form.tpl.html`

- Preview section

Displayed as part of summary when workflow is finished
Section always consist from the html template and directive.
For example: `base.tpl.html`

### Definition

Definition can contain step definition.
It may be array of definitions when npm module provides more than one step.

For example:

```javascript 
module.exports = {
  ngModule: require('./angular/base'),
  // Definition for this step that is being used in portal
  definition: require('./definition.json')
};
```

## Definitions

Definitions are required for the raincatcher-angularjs portal application to define a workflow step. Definitions are defined the `definition.json` file which is directly linked to the angular directives names through the templates field. The name and description fields will be the step name and decription in the portal·

```json
[
    {
        "code": "base-form",
        "name": "Base Form",
        "description": "base form used to for steps",
        "templates": {
            "form": "<base-form></base-form>",
            "view": "<base></base>"
        }
    }
]
```

> **Note:** File can contain array or object depending if your npm module provides one or more definitions. 

## Naming conventions

What naming conventions 
- Angular directive uses camel case e.g. `baseForm` 
- File name and `definition.json` use hypens e.g. 

file: 
    
    base-form.tpl.html

definition.json:
```json
"templates": {
    "form": "<base-form></base-form>",
    "view": "<base></base>"
}
```


## Limitations

Having a separate the scope inside a directive from the scope outside,and then map the outer scope to a directive's inner scope is not supported. Workflows require the scope to be global so using Angular scoped directives will cause issues when adding multiple steps within a workflow.

## User interface

If your user interface requires additional styles or libraries they need to be included in top level application.
Standard bootstrap and angular material directives are available in top level mobile and portal applications.

## Example use case 

In following example we will edit base template to build 'User Reciept form'.

To customize the base step modify the html templates for 

**Edit section - Form**
    
    step-base/lib/angular/template/base-form.tpl.html

Add a email field to the ng-form div

```html
<md-input-container class="md-block" flex-gt-sm>
  <label>Email </label>
  <input type="text" id="title" name="title" ng-model="ctrl.model.email" required>
</md-input-container>
```    

**Preview section - Results**  
    
    step-base/lib/angular/template/base.tpl.html

Add a email field to `<md-list>` to the base template

```html
<md-list-item class="md-2-line">
  <div class="md-list-item-text">
    <h3>{{model.email}} </h3>
    <p>Email</p>
  </div>
  <md-divider></md-divider>
</md-list-item>
```
Notice how output `{{model.email}}` is directly linked to `ng-model=ctrl.model.email` input on the form.

HTML templates are written with [angular.js](https://angularjs.org/).

Once the html templates are edited you need to use grunt to build using [wfmTemplate](https://www.npmjs.com/package/fh-wfm-template-build) with the following command. 

    grunt wfmTemplate:build

alternatively you can run `npm run watch` to rebuild the step automatically on all changes.

Package name can be customised via the package.json

---

# Adding a step to raincatcher-angularjs 

## Add step to mobile application
Add step to mobiles package.json as a package `demo/mobile/package.json`
```javascript 
    "@raincatcher/step-base": "0.0.1",
```
Add step to mobiles `demo/mobile/src/app/app.js`
```javascript
    // apply a variable name to the module
    var variableName = require(‘@raincatcher/step-base’);

    // add ngModule() for the module to angular.module array;
    variableName.ngModule();
```
Import the step scss for step-base into `demo/mobile/src/sass/app.scss`
```javascript    
    @import 'node_modules/@raincatcher/step-base/lib/step-base.scss';
```
## Add step to portal application

Add step package to `demo/portal/package.json`  
```javascript
    "@raincatcher/step-base": "0.0.1",                
```
Add step to portal in angular.modules in `demo/portal/src/app/main.js`
```javascript
    // apply a variable name to the module
    var variableName = require(‘@raincatcher/step-base’);
```
Add step definition to angular.modules stepDefinitions array
```javascript
    stepDefinitions: [
                        variableName.definition 
                     ]
```

Also add step ngModule() in angular.modules
```javascript
    variableName.ngModule()
```

Import the step scss `demo/portal/src/sass/portal.scss`
```javascript
    @import 'node_modules/@raincatcher/step-base/lib/step-base.scss';
```














