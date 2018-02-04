# Package Manager
A package is a container that holds anything from an entire module of Unity (such as Physics or Animation) to any combination of Assets, Shaders, Textures, plug-ins, icons, and scripts that enhance various parts of your project. Unity packages are newer, more tightly integrated versions of Asset Store packages, able to deliver a wide range of enhancements to Unity. 

Use the Package Manager window to view which packages are available for installation or already installed in your project. In addition, you can use this window to [install](#PackManInstall), [remove](#PackManRemove), [disable](#PackManDisable), or [update](#PackManUpdate) packages for each project.

![Package Manger window](Images/PackageManagerUI-Main.png)

The Package Manager window displays a [list view](#PackManLists) on the left and a [detail view](#PackManDetails) on the right.

<a name="PackManLists"></a>
## Finding packages
The Package Manager window displays two types of Unity packages:
 - Read-only **Packages**, which Package Manager downloads from the [Unity package registry](#PackManRegistry) as needed. They are not bundled with the project source and they may have dependencies on other packages in external projects. This type is typical of most Unity packages.
 - **Built In Packages** or *Modules*, which represent some of the core Unity features. You can use these packages to [turn Unity modules on and off](#PackManDisable).<br>
 **Note:** You can find out more about what each module implements in the [Unity Scripting API](https://docs.unity3d.com/ScriptReference/). Each module assembly page lists which APIs the module implements.

By default, the Package Manager window displays the list of packages in the **In Project** mode, so that only the packages and modules already installed in your project appear in the list. 

To expand the list to include all available packages and modules, click the **All** button. The list now displays all packages and modules registered in the [package registry](#PackManRegistry), regardless of whether they are already installed in the project.

![In Project and All modes](Images/PackageManagerUI-Modes.png)


<a name="PackManDetails"></a>
## Viewing package details
The pane on the right side of the Package Manager window displays details about the selected package.

![Details pane](Images/PackageManagerUI-DetailsPane.png)

These details include the following information:
 - (A) The display name 
 - (B) The version number
 - \(C\) View the documentation for this package
 - (D) The official package name from the registry starting with `com.unity.`
 - (E) The installation or update status
 - (F) A brief description
 - (G) Buttons to install, remove, disable, or update the package

### Version tags
Some packages display special tags next to the version number. These tags convey special information about that version of the package. 

![Tagged version information](Images/PackageManagerUI-Tags.png)

Package Manager uses the following values:

| **Tag** | **Meaning** |
|--|--|
| `recommended` | This package is officially tested and approved by Unity. |
| `alpha` or `beta` | This package is at an early stage of the release cycle and may not have been documented and validated by either the development team or Unity's Quality Assurance team. |
| `experimental` | This package is in development. |

<a name="PackManOpen"></a>
## Accessing the Package Manager window
You can perform a variety of tasks through the Package Manager window:

 - [Install a new package](#PackManInstall)
 - [Remove an installed package](#PackManRemove)
 - [Disable a Unity module](#PackManDisable)
 - [Update an installed package](#PackManUpdate)

 To open the Package Manager window, select **Window > Package Manager** from the main menu. 
 
![Window > Package Manager](Images/PackageManagerUI-Access.png)

<a name="PackManInstall"></a>
### Installing a new package
![Install button](Images/PackageManagerUI-InstallButton.png)

To install a new package:
 1. Open the Project Manager window and click the **All** button. 
 2. Select the package you want to install from the **Packages** list. The package information appears in the Details pane.
 3. Click the **Install X.X.X** button. When the progress bar finishes, the new package is ready to use.

<a name="PackManRemove"></a>
### Removing an installed package
![Remove button](Images/PackageManagerUI-RemoveButton.png)

To remove an installed package:
 1. Open the Project Manager window. 
 2. Click the **In Project** button if you are in **All** mode. 
 3. Select the package you want to remove from the **Packages** list. The package information appears in the Details pane.
 4. Click the **Remove X.X.X** button. When the progress bar finishes, the package disappears from the list.

**Notes:** 
 - You can only remove packages which are not required by another package. 
 - When you remove a package, any editor or runtime functionality which it implemented is no longer available.

<a name="PackManDisable"></a>
### Disabling a Unity module
![Disable button](Images/PackageManagerUI-DisableButton.png)

To disable a module:
 1. Open the Project Manager window.
 2. Click the **In Project** button if you are in **All** mode. 
 3. Expand the **Built In Packages** list in the left pane.
 4. Select the module you want to remove from the **Built In Packages** list. The module information appears in the Details pane.
 5. Click the **Disable** button. When the progress bar finishes, the module disappears from the list.
 
**Note:** When you disable a module, the corresponding Unity functionality is no longer available:
- If you use a Scripting API implemented by a disabled module, you get compiler errors.
- Components implemented by that module are disabled too. You cannot add them to any GameObjects. If you have a GameObject that already has one of these components, Unity ignores them in Play mode. You can see them in the Inspector window but they are greyed out to indicate that they are not available.
- When building a game, Unity strips all disabled components. For build targets which support engine code stripping (like WebGL, iOS, and Android), Unity doesn't add any code from a disabled module.

<a name="PackManUpdate"></a>
### Updating an installed package
![Update button](Images/PackageManagerUI-UpdateButton.png)

You can update a package while in either the **In Project** or **All** mode:
 1. Open the Project Manager window. Any packages that have updates available appear with an arrow indicator.
 2. Select the package you want to update from the **Packages** list. The package information appears in the Details pane. 
 3. Click the **Update to X.X.X** button. When the progress bar finishes, the new package version information appears in the Details pane and any new functionality is available. 

## Advanced package topics
This section provides more advanced information about the package registry and the manifest files but you don't need to know anything about these topics to install, remove, disable, and update packages.

A Unity package contains a [package manifest file](#PackManManifestsPackage) in addition to the contents (Assets, Shaders, Textures, plug-ins, icons and scripts). The package manifest tells Unity how to display its information page in the Package Manager window and how to install the package in the project.

![Folder structure of a package](Images/PackageManagerUI-PackageStructure.png)

In addition, there are several files that help manage the package deployment, including the tests, the samples, the license, the changelog, and the documentation.

<a name="PackManManifests"></a>
### Manifests
There are two types of manifest files: [project](#PackManManifestsProject) manifests (`manifest.json`), and [package](#PackManManifestsPackage) manifests (`package.json`). Both files use JSON (JavaScript Object Notation) syntax to communicate with Package Manager by describing which packages are available for each project and what each package contains. 

<a name="PackManManifestsProject"></a>
#### Project manifests
Project manifests (`manifest.json`) tell Package Manager which packages and versions are available to the project. In addition, the manifest also optionally specifies the location of the package registry and which packages to test.

The following values are supported:

| Key | JSON Type | Description |
|--|--|--|
| `dependencies` | Object |List of packages for Package Manager to load or remove. These are usually packages officially registered with Unity. For example, the Package Manager window is available in Unity when the project manifest contains this dependencies entry: `"com.unity.package-manager-ui": "1.3.0"` <br><br>After the user removes a package from the project, the value that appears in the project manifest is "exclude" (instead of the version number). For example: `"com.unity.purchasing": "exclude"` |
| `registry` | String |The location of the registry where the packages are stored (optional). Package Manager uses this location to download information and updates for packages. By default, the official Unity registry (`packages.unity.com`) is assumed. |
| `testables` | Array of Strings |List of packages to be tested (optional). |

Example of a `manifest.json` file:

	{
		"dependencies": {
			"com.unity.package-1": "1.0.0",
			"com.unity.package-2": "2.0.0",
			"com.unity.package-3": "3.0.0",
			"com.unity.package-4": "exclude"
		},
		"registry": "https://packages.unity.com",
		"testables": [
			"com.unity.package-1",
			"com.unity.package-2",
			"com.unity.package-3"
		]
	}

Unity stores each project manifest in the `[your_project_root_folder]/Packages/manifest.json` file.

<a name="PackManManifestsPackage"></a>
#### Package manifests
Package Manager uses package manifests (`package.json`) to determine which version of the package to load and what information to display in the Package Manager window.

The following values are supported:

| Key | JSON Type | Description |
|--|--|--|
| `name` | String |Officially registered package name, following this naming convention: `"com.unity.[your package name]"`. For example, `"com.unity.resourcemanager"` |
| `displayName` | String |Package name as it appears in the Package Manager window. For example, `Resource Manager"` |
| `version` | String |Package version `"MAJOR.MINOR.PATCH"`. Unity packages follow the [Semantic Versioning](https://semver.org) system. For example, `"1.3.0"`. |
| `unity` | String |The Unity version that supports this package. For example, `"2018.1"` indicates compatibility starting with Unity version 2018.1. |
| `description` | String |Brief description of the package. This is the text that appears on the Details pane of the Package Manager window. Some special formatting character codes are supported, such as line breaks (`\n`) and bullets (`\u25AA`). |
| `keywords` | Array of Strings |Keywords used for searching in the Package Manager window specified as a JSON array of strings. For example, `["Physics", "RigidBody", "Component"]`. |
| `category` | String |Category of the package. For example, `"Forces"`. |
| `dependencies` | Object |List of packages this package depends on, expressed as a JSON dictionary where the key is the package name and the value is the version number. Unity downloads all dependencies and loads them in the project with the package. |

Example of a `package.json` file: 

	{
		"name": "com.unity.package-4",
		"displayName": "Package Number 4",
		"version": "2.5.1",
		"unity": "2018.1",
		"description": "This package provides X, Y, and Z. \n\nTo find out more, click the \"View documentation\" button.",
		"keywords": ["key X", "key Y", "key Z"],
		"category": "Controllers",
		"dependencies": {
			"com.unity.package-1": "1.0.0",
			"com.unity.package-2": "2.0.0",
			"com.unity.package-3": "3.0.0"
		}
	}

Unity stores each package manifest in the `[your_package_root_folder]/package.json` file.

<a name="PackManRegistry"></a>
### The Package Registry
Unity maintains a central registry of official packages that are available for distribution. When Unity loads, Package Manager communicates with the registry, checks the project manifest file, and displays the status of each available package in the Package Manager window.

When you remove a package from the project, Package Manager updates the project manifest to exclude that package from the list in **In Project** mode but it is still available in **All** mode because it is still on the registry.

When you install or update a package, Package Manager downloads the package from the registry.


# Technical details

## Requirements

This version of Package Manager is compatible with the following versions of the Unity Editor:

* 2018.1 and later (recommended)

## Known limitations

Package Manager includes the following known limitations:

* If you manually edit the `manifest.json` file, the Package Manager window doesn't refresh the list of packages. You need to either [re-open the window](#PackManOpen) or [toggle between In Project and All modes](#PackManLists) to force an update.
* You cannot install or remove built-in modules.

## Package contents

The following table provides an alphabetical list of the important files and folders included in this package.

|Folder or Filename|Description|
|---|---|
|`Documentation`|Folder containing the source files used to build the documentation.|
|`Editor`|Folder containing the source code for the Package Manager window.|
|`Tests`|Folder containing the Package Manager tests.|

## Documentation revision history
|Date|Reason|
|---|---|
|Jan 31, 2018|Documentation updated (developmental review)|
|Jan 29, 2018|Document updated. Matches package version 1.6.0.|
|Jan 18, 2018|Document updated. Matches package version 1.5.1.|
|Jan 17, 2018|Document updated. Matches package version 1.5.0.|
|Jan 12, 2018|Document updated. Matches package version 1.4.0.|
|Nov 7, 2017|Document created. Matches package version 1.0.0.|