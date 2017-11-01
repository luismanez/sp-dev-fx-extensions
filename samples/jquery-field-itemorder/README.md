# SPFx Item Order

## Summary
Sample SharePoint Framework field customizer extension that enables reordering of list items through intuitive drag and drop directly in your list view. Demonstrates the use of jQuery and jQuery UI, custom property JSON, and PnP JS Core Batching.

![Reordering of List Items](./assets/spfxItemOrder-Animation.gif)

## Used SharePoint Framework Version 
![1.3.0](https://img.shields.io/badge/version-1.3.0-green.svg)

## Applies to

* [SharePoint Framework Extensions](https://dev.office.com/sharepoint/docs/spfx/extensions/overview-extensions)
* [jQuery](http://jquery.com/)
* [jQuery UI](http://jqueryui.com/)

## Solution

Solution|Author(s)
--------|---------
jquery-field-itemorder | Chris Kent ([thechriskent.com](https://thechriskent.com), [@thechriskent](https://twitter.com/thechriskent))

## Version history

Version|Date|Comments
-------|----|--------
1.0|September 5, 2017|Initial release
1.1|September 28, 2017|Updated for SPFx Extensions GA 1.3.0

## Disclaimer
**THIS CODE IS PROVIDED *AS IS* WITHOUT WARRANTY OF ANY KIND, EITHER EXPRESS OR IMPLIED, INCLUDING ANY IMPLIED WARRANTIES OF FITNESS FOR A PARTICULAR PURPOSE, MERCHANTABILITY, OR NON-INFRINGEMENT.**

---

## Minimal Path to Awesome

- Clone this repository
- Update the `pageUrl` properties in the **config/serve.json** file
   - The `pageUrl` should be a list view in your tenant
   - This property is only used during development in conjunction with the `gulp serve` command
- In the command line navigate to **samples/jquery-field-itemorder** and run:
  - `npm install`
  - `gulp serve`
    - if you want to use the custom field then use the customField serveConfiguration using this command: `gulp serve --config customField`
- In a web browser
  - Optionally, follow the steps below for a **Placeholder Field** (or choose some other field)
  - Ensure your list/view is orderable using the one of the options under **Making Your List Orderable**
  - Choose **Load Debug Scripts** when prompted
  - Reorder your items and feel the power flow through you

## Features
SPFx Item Order allows you to quickly reorder your list items through intuitive drag and drop directly in your list view.

This extension illustrates the following concepts:

- Loading **jQuery** and **jQuery UI** from a CDN
- Loading **PnP JS Core** from a CDN
- Using **jQuery UI** in a Field Customizer
- Updating multiple items in a single **Batch**
- Applying **CSS Animations** in SCSS
- Taking advantage of the under utilized `Order` field
- Configuration through properties
- Multiple **serveConfigurations**
- Conditionally altering behavior & display based on the **user's permission level**
- Theme syntax for applying official colors to custom CSS classes
- Optionally, **PnP PowerShell** for view adjustment _(see below)_

## Debug URLs for testing
Here's some example debug querystrings for testing this sample:

### Default

```
?loadSPFX=true&debugManifestsFile=https://localhost:4321/temp/manifests.js&fieldCustomizers={"Reorder":{"id":"e6bdc269-2080-47b8-b096-e7bf2a9263a9","properties":{}}}
```
* Overrides the **Reorder** field
* Utilizes the internal `Order` field

Your URL will look similar to the following (replace with your domain and site address):
```
https://yourtenant.sharepoint.com/sites/yoursite/lists/yourlist/AllItems.aspx?loadSPFX=true&debugManifestsFile=https://localhost:4321/temp/manifests.js&fieldCustomizers={"Reorder":{"id":"e6bdc269-2080-47b8-b096-e7bf2a9263a9","properties":{}}}
```

### Custom Order Field

```
?loadSPFX=true&debugManifestsFile=https://localhost:4321/temp/manifests.js&fieldCustomizers={"Reorder":{"id":"e6bdc269-2080-47b8-b096-e7bf2a9263a9","properties":{"OrderField":"CustomOrder"}}}
```
* Overrides the **Reorder** field
* Utilizes the **CustomOrder** field

Your URL will look similar to the following (replace with your domain and site address):
```
https://yourtenant.sharepoint.com/sites/yoursite/lists/yourlist/AllItems.aspx?loadSPFX=true&debugManifestsFile=https://localhost:4321/temp/manifests.js&fieldCustomizers={"Reorder":{"id":"e6bdc269-2080-47b8-b096-e7bf2a9263a9","properties":{"OrderField":"CustomOrder"}}}
```

## Making Your List Orderable

In order to enable reordering, you'll need one of the following to be true:
* Your list is already orderable
* The view has been adjusted to use the internal `Order` field
* You've provided your own custom ordering field.

### Using an Orderable List

Some lists are orderable by default. For instance, a **Links** list is marked as orderable and in the classic view a Reorder List Items button is displayed in the ribbon. By default, the views also use the _"Allow users to order items in this view"_ option as seen here on the view edit screen:

![Allow users to order items in this view option](./assets/OrderableView.PNG)

If your view has this option and it's set to _Yes_, then you're good to go! Otherwise, you'll need to pick one of the other 2 options below.

### Adjusting Your View to Use the Order Field

Every list has the internal `Order` field but it is hidden by default and so it isn't available in the view editor. To sort your view by this field you can adjust the Query XML of the view using SharePoint Designer. An even easier option, however, is to use the included PowerShell script [ConvertView.ps1](,/assets/ConvertView.ps1) located in the assets folder.

#### Prerequisites

You'll need the [SharePoint PnP PowerShell Cmdlets for SharePoint Online](https://github.com/SharePoint/PnP-PowerShell). It's a very quick install and if you don't have it already, go get it! You'll end up using it for far more than just this sample.

#### Running the PowerShell Script

Using a PowerShell console (you can even use the powershell terminal included in Visual Studio Code), navigate to the assets folder in this sample. Run the script like this:

```PowerShell
.\ConvertView.ps1 https://yourtenant.sharepoint.com/sites/yoursite -ListName "Your List" -ViewName "SomeView"
```

You'll be prompted for your credentials and then the original view will be removed and a duplicate will be added with the exception that it is now sorted by the internal `Order` field. Since this view can no longer be edited in the view editor (without running this script again), it's best to go ahead and get everything but the sort configured the way you want it.

#### Benefits of Using the OOTB Order Field

Using the OOTB `Order` field is the preferred option for the following reasons:
* It's already there! _(No additional fields required)_
* It auto increments as you add new items _(end users don't have to care)_
* This field customizer uses it by default _(this can be overridden - see below)_

### Custom Ordering Field

Finally, if you'd rather use a custom field for ordering you absolutely can. You'll just need to add an additional number column. Then in the **ClientSideComponentProperties** specify the `OrderField` property with the internal name of your column (see the Debug URLs above for an example).

> Note - This solution expects that your ordering column does not have duplicate values. It won't break anything, but the order won't be changed either.

## Placeholder Field

This field customizer will work with any existing field. However, it will take over the display with an icon and won't show the original values. So, your best bet is to add an empty field like the following:

1. Add a new field to the list (either by going to list settings or by hitting the **+** and choosing **More**)
2. Set the field to type **Calculated**
3. Set the formula to **=""** with a data type of **single line of text**

![Placeholder Field Settings](./assets/PlaceholderField.PNG)

This will ensure that you've got a field that won't show up on your new/edit forms and is readily available for your views.

## Reordering!

![Reordering a List View](./assets/spfxItemOrder-Preview.png)

## View with Inadequate Permissions

Edit list permissions are required to be able to reorder the list items. Here's what it looks like for those that don't have at least that permission level:

![Reordering Denied!](./assets/spfxItemOrder-NoPermissions.png)

<img src="https://telemetry.sharepointpnp.com/sp-dev-fx-extensions/samples/jquery-field-itemorder" />
