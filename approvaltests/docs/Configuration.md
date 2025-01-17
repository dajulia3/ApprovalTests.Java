<!--
GENERATED FILE - DO NOT EDIT
This file was generated by [MarkdownSnippets](https://github.com/SimonCropp/MarkdownSnippets).
Source File: /approvaltests/docs/mdsource/Configuration.source.md
To change this file edit the source file and then run MarkdownSnippets.
-->
<a id="top"></a>

# Configuration



<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Contents**

- [What is configurable?](#what-is-configurable)
- [Using Subdirectories for Approval Output Files](#using-subdirectories-for-approval-output-files)
- [Alternative Base Directory for Output File](#alternative-base-directory-for-output-files)
- [PackageLevelSetting](#packagelevelsettings)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->


## What is configurable?
Configuration of ApprovalTests mainly occurs via @Annotations and PackageSettings. 
(the API is also extendible) 
Currently you can configure:

 * [Reporters](Reporters.md#class-and-method-level) (package/class/method)
 * FrontLoadedReporters ( package/class/method)
 * ApprovalSubdirectories (package)

## Using Subdirectories for Approval Output Files

Approved and received files can be stored in a preferred location. To do so, write a class as follows: 
<!-- snippet: package_settings_approval_subdirectory -->
```java
public class PackageSettings
{
  public static String UseApprovalSubdirectory = "approvals";
}
```
<sup>[snippet source](/approvaltests/src/test/java/org/approvaltests/packagesettings/subdirectory/PackageSettings.java#L3-L8)</sup>
<!-- endsnippet -->

The approved & received files will now be created in the subdirectory `/approvals/` for any test in the same package as this file, or any test in any subpackage under this.  

## Alternative Base Directory for Output Files  

Approved and received files can be stored in a different branch of the code base (for example, under the `/resources/` folder).

To do so, write a class as follows:    

<!-- snippet: package_settings_approval_base_directory -->
```java
public class PackageSettings
{
  public static String ApprovalBaseDirectory = "../resources";
}
```
<sup>[snippet source](/approvaltests/src/test/java/org/approvaltests/packagesettings/basedirectory/PackageSettings.java#L3-L8)</sup>
<!-- endsnippet -->

The approved and received files will now be created in the directory `/source/test/resources/` for any test in the same package as this file, or any test in any under this.  

## PackageLevelSettings  

Package Level Settings allows for programmatic setting of configuration at the package level. It follows the principle of least surprise.   

Your Package Leveling configuration must be in class called PackageSettings. The fields can be private, public and or static. They will be picked up regardless. All methods will be ignored.

For example if you had a class:

<!-- snippet: /approvaltests/src/test/java/org/packagesettings/PackageSettings.java -->
```java
package org.packagesettings;

public class PackageSettings
{
  public String        name     = "Llewellyn";
  private int          rating   = 10;
  public static String lastName = "Falco";
}

```
<sup>[snippet source](/approvaltests/src/test/java/org/packagesettings/PackageSettings.java#L1-L9)</sup>
<!-- endsnippet -->

If you where to call at the org.packagesettings level.

<!-- snippet: package_level_settings_get -->
```java
Map<String, Settings> settings = PackageLevelSettings.get();
```
<sup>[snippet source](/approvaltests/src/test/java/org/packagesettings/PackageSettingsTest.java#L13-L15)</sup>
<!-- endsnippet -->

Then you would get the following settings

<!-- snippet: /approvaltests/src/test/java/org/packagesettings/PackageSettingsTest.testRetriveValue.approved.txt -->
```txt
lastName : Falco [from org.packagesettings.PackageSettings] 
name : Llewellyn [from org.packagesettings.PackageSettings] 
rating : 10 [from org.packagesettings.PackageSettings] 

```
<sup>[snippet source](/approvaltests/src/test/java/org/packagesettings/PackageSettingsTest.testRetriveValue.approved.txt#L1-L4)</sup>
<!-- endsnippet -->

However, if you also had

<!-- snippet: /approvaltests/src/test/java/org/packagesettings/subpackage/PackageSettings.java -->
```java
package org.packagesettings.subpackage;

public class PackageSettings
{
  public String   name        = "Test Name";
  private boolean rating      = true;
  public String   ratingScale = "logarithmic";
}

```
<sup>[snippet source](/approvaltests/src/test/java/org/packagesettings/subpackage/PackageSettings.java#L1-L9)</sup>
<!-- endsnippet -->

and you ran the same code but from the org.packagesettings.subpackage  
then you would get a blended view of the two classes where anything in the sub-package would override the parents.

<!-- snippet: /approvaltests/src/test/java/org/packagesettings/subpackage/PackageSettingsTest.testRetriveValueWithOverRide.approved.txt -->
```txt
lastName : Falco [from org.packagesettings.PackageSettings] 
name : Test Name [from org.packagesettings.subpackage.PackageSettings] 
rating : true [from org.packagesettings.subpackage.PackageSettings] 
ratingScale : logarithmic [from org.packagesettings.subpackage.PackageSettings] 

```
<sup>[snippet source](/approvaltests/src/test/java/org/packagesettings/subpackage/PackageSettingsTest.testRetriveValueWithOverRide.approved.txt#L1-L5)</sup>
<!-- endsnippet -->

---

[Back to User Guide](README.md#top)
