![Eureka: Elegant form builder in Swift](Eureka.png)

<p align="center">
<a href="https://travis-ci.org/xmartlabs/Eureka"><img src="https://travis-ci.org/xmartlabs/Eureka.svg?branch=master" alt="Build status" /></a>
<img src="https://img.shields.io/badge/platform-iOS-blue.svg?style=flat" alt="Platform iOS" />
<a href="https://developer.apple.com/swift"><img src="https://img.shields.io/badge/swift2-compatible-4BC51D.svg?style=flat" alt="Swift 2 compatible" /></a>
<a href="https://github.com/Carthage/Carthage"><img src="https://img.shields.io/badge/Carthage-compatible-4BC51D.svg?style=flat" alt="Carthage compatible" /></a>
<a href="https://cocoapods.org/pods/Eureka"><img src="https://img.shields.io/badge/pod-1.1.0-blue.svg" alt="CocoaPods compatible" /></a>
<a href="https://raw.githubusercontent.com/xmartlabs/Eureka/master/LICENSE"><img src="http://img.shields.io/badge/license-MIT-blue.svg?style=flat" alt="License: MIT" /></a>
</p>

By [XMARTLABS](http://xmartlabs.com).
This is the re-creation of [XLForm] in Swift 2. If you have been using it then many terms will result familiar to you.

* [Introduction]
* [Requirements]
* [Examples]
* [Usage]
  + [How to create a Form]
  + [Operators]
  + [Rows]
  + [Customization]
  + [Section Header and Footer]
  + [How to dynamically hide and show rows (or sections)]
* [Extensibility]
  + [How to create custom rows and cells]
  + [How to create custom inline rows]
  + [Custom rows catalog]
* [Installation]
* [FAQ]

## Introduction

**Eureka!** is a library to create dynamic table-view forms from a [DSL] specification in Swift. This DSL basically consists of *Rows*, *Sections* and *Forms*. A *Form* is a collection of *Sections* and a *Section* is a collection of *Rows*.

Both `Form` and `Section` classes conform to `MutableCollectionType` and `RangeReplaceableCollectionType` protocols. This makes a whole bunch of functions available to be executed on them.

**For more information look at [our blog post] that introduces *Eureka!*.**

## Requirements

* iOS 8.0+
* Xcode 7.0+


## Getting involved
* If you **want to contribute** please feel free to **submit pull requests**.
* If you **have a feature request** please **open an issue**.
* If you **found a bug** or **need help** please **check older issues or threads on [StackOverflow] (tag 'eureka') before submitting an issue**.

If you use **Eureka** in your app We would love to hear about it! Drop us a line on [twitter].

## Examples

Follow these 3 steps to run Example project: Clone Eureka repository, open Eureka workspace and run the *Example* project.

You can also experiment and learn with the *Eureka Playground* which is contained in *Eureka.workspace*.

<img src="Example/Media/EurekaNavigation.gif" width="300"/>
<img src="Example/Media/EurekaRows.gif" width="300"/>

## Usage

### How to create a form
It is quite simple to create a form, just like this:

```swift
import Eureka

class CustomCellsController : FormViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        form +++ Section("Custom cells")
	            	<<< WeekDayRow(){
	            		$0.value = [.Monday, .Wednesday, .Friday]
	            	}
	            	<<< FloatLabelRow() {
	            		$0.title = "Float Label Row, type something to see.."
	            	}
    }
}
```

In this example we create a [CustomCellsController] and then simply add a section with two custom rows to the `form` variable inherited from [FormViewController]. And this is the product:

<img src="Example/Media/EurekaCustomCells.gif" width="300" alt="Screenshot of Custom Cells"/>

### Operators

We declared a series of custom operators to make the creation and modification of forms more readable and appealing.
#### +++=
This operator is used to append a row or section to a form. In case you are appending a row, then a section will be created that contains that row and that section will be appended to the form.

```swift
// Will append a section at the end of the 'form' Form.
form +++= Section()

// Will internally create a Section containing a PhoneRow and append it to the 'form' variable:
form +++= PhoneRow()
```

#### +++
Lets you add a Section to a Form or create a Form by adding two sections. For example:

```swift
// Will add two sections at the end of the 'form' Form.
form +++ Section("Custom cells") +++ Section("Another section")

// Will create and assign a Form to the 'form' variable (containing the two sections):
form = Section("Custom cells") +++ Section("Another section")
```

#### <<<
This one is used to append rows to sections or to create a Section by appending two rows. For example:


```swift
// Will append a Check row to the last section of the 'form' Form.
form.last! <<< CheckRow()

// Will implicitly create a Section and add it to the 'form' (containing the two rows):
form +++ PhoneRow("Phone"){ $0.title = "Phone"}
         <<< IntRow("IntRow"){ $0.title = "Int"; $0.value = 5 }
```


#### +=
This operator just invokes the corresponding `appendContentsOf` method which means that it can be used to append arrays of elements to either a Form or a Section like this:

```swift
// Will append three sections to form.
form += [Section("A"), Section("B"), Section("C")]
```

###### To learn more about these operators try them out in Eureka Playground.


### Rows

This is a list of the rows that are provided by default:

* **Field Rows**
	This rows have a textfield on the right side of the cell. The difference between each one of them consists in a different capitalization, autocorrection and keyboard type configuration.
	+ **TextRow**
	+ **NameRow**
	+ **UrlRow**
	+ **IntRow**
	+ **PhoneRow**
	+ **PasswordRow**
	+ **EmailRow**
	+ **DecimalRow**
	+ **TwitterRow**
	+ **AccountRow**
	+ ZipCodeRow


* **Date Rows**
  Date Rows hold a NSDate and allow us to set up a new value through UIDatePicker control. The mode of the UIDatePicker and the way how the date picker view is shown is what changes between them.

  + **DateRow**
  + **DateInlineRow**
  + **TimeRow**
  + **TimeInlineRow**
  + **DateTimeRow**
  + **DateTimeInlineRow**
  + **CountDownRow**
  + **CountDownInlineRow**
  + **DatePickerRow**
  + **TimePickerRow**
  + **DateTimePickerRow**
  + **CountDownPickerRow**


* **Options Selector Rows** These are rows with a list of options associated from which the user must choose. You can see them in the examples above.
	+ **AlertRow**

    <img src="Example/Media/EurekaAlertRow.gif" width="300" alt="Screenshot of Custom Cells"/>

	+ **ActionSheetRow**

		The ActionSheetRow will show an action sheet with options to choose from.

	+ **SegmentedRow**

    <img src="Example/Media/EurekaSegmentedRow.gif" width="300" alt="Screenshot of Segment Row"/>

	+ **PushRow**

		This row will push to a new controller from where to choose options listed using Check rows.

	+ **ImageRow**

		Will let the user pick a photo

	+ **MultipleSelectorRow**

		This row allows the selection of multiple options

  + **LocationRow** (Included as custom row in the the example project)

    <img src="Example/Media/EurekaLocationRow.gif" width="300" alt="Screenshot of Location Row"/>

* **Other Rows**
	These are other rows that might be useful
	+ **ButtonRow**
	+ **CheckRow**
	+ **LabelRow**
	+ **SwitchRow**
	+ **TextAreaRow**

There are also some custom rows in the examples project.

### Customization

A *row* holds the basic information that will be displayed on a *cell* like title, value, options (if present), etc. All the stuff that has to do with appearance like colors, fonts, text alignments, etc. normally go in the cell. Both, the row and the cell hold a reference to each other.

You will often want to customize how a row behaves when it is tapped on or when its value changes and you might be interested in changing its appearance as well.
There are many callbacks to change the default appearance and behaviour of a row.

* **onChange()**

	This will be called when the value of a row changes. You might be interested in adjusting some parameters here or even make some other rows appear or disappear.
* **onCellSelection()**

	This one will be called each time the user taps on the row and it gets selected.
* **cellSetup()**

	The cellSetup will be called once when the cell is first configured. Here you should set up your cell with its permanent settings.
* **cellUpdate()**

	The cellUpdate will be called each time the cell appears on screen. Here you can change how the title and value of your row is set or change the appearance (colors, fonts, etc) depending on variables that might not be present at cell creation time.

* **onCellHighlight()**

  The onCellHighlight will be invoked whenever the cell or any subview become the first responder.

* **onCellUnHighlight()**

  The onCellUnHighlight will be invoked whenever the cell or any subview resign the first responder.

* **onExpandInlineRow()**

  The onExpandInlineRow will be invoked before expand the inline row. This does only apply to the rows conforming to the `InlineRowType` protocol.

* **onCollapseInlineRow()**

  The onCollapseInlineRow will be invoked before collapse the inline row. This does only apply to the rows conforming to the `InlineRowType` protocol.

* **onPresent()**

	This method will be called by a row just before presenting another view controller. This does only apply to the rows conforming to the `PresenterRowType` protocol. You can use this to set up the presented controller.

Each row also has an initializer where you should set the basic attributes of the row.

Here is an example:

```swift
let row  = CheckRow("set_disabled") { // initializer
              $0.title = "Stop at disabled row"
              $0.value = self.navigationOptions?.contains(.StopDisabledRow)
           }.onChange { [weak self] row in
              if row.value ?? false {
                  self?.navigationOptions = self?.navigationOptions?.union(.StopDisabledRow)
              }
              else{
                  self?.navigationOptions = self?.navigationOptions?.subtract(.StopDisabledRow)
              }
           }.cellSetup { cell, row in
              cell.backgroundColor = .lightGrayColor()
           }.cellUpdate { cell, row in
              cell.textLabel?.font = .italicSystemFontOfSize(18.0)
           }
```

Now it would look like this:

<img src="Example/Media/EurekaCustomCellDisabled.gif" width="300" alt="Screenshot of Disabled Row"/>

### Section Header and Footer

*work in progress...*

### How to dynamically hide and show rows (or sections)  <a name="hide-show-rows"></a>

Many forms have conditional rows, I mean rows that might appear on screen depending on the values of other rows. You might want to have a SegmentedRow at the top of your screen and depending on the chosen value ask different questions:

<figure>
  <img src="Example/Media/EurekaSwitchSections.gif" width="300" alt="Screenshot of Hidden Rows" />
	<figcaption style="text-align:left;">In this case we are hiding and showing whole sections</figcaption>
</figure>

To accomplish this each row has an `hidden` variable that is of optional type `Condition`.
```swift
public enum Condition {
    case Predicate(NSPredicate)
    case Function([String], (Form?)->Bool)
}
```
The `hidden` variable might be set with a function that takes the form the row belongs to and returns a Bool that indicates whether the row should be hidden or not. This is the most powerful way of setting up this variable as there are no explicit limitations as to what can be done in the function.

Along with the function you have to pass an array with all the tags of the rows this row depends on. This is important so that Eureka can reevaluate the function each time a value changes that may change the result of this function. In fact that will be the only time the function is reevaluated.
You can define it like this:
```swift
<<< AlertRow<Emoji>("tag1") {
        $0.title = "AlertRow"
        $0.optionTitle = "Who is there?"
        $0.options = [💁, 🍐, 👦, 🐗, 🐼, 🐻]
        $0.value = 👦
    }
   // ...
<<< PushRow<Emoji>("tag2") {
        $0.title = "PushRow"
        $0.options = [💁, 🍐, 👦, 🐗, 🐼, 🐻]
        $0.value = 👦
        $0.hidden = Condition.Function(["tag1"]) { form in
                        if let r1 : AlertRow<Emoji> = form?.rowByTag("tag1") {
                            return r1.value == 👦
                        }
                        return false
                    }
      }
```

The `hidden` variable can also be set with a NSPredicate. In the predicate string you can reference values of other rows by their tags to determine if a row should be hidden or visible.
This will only work if the values of the rows the predicate has to check are NSObjects (String and Int will work as they are bridged to their ObjC counterparts, but enums won't work).
Why could it then be useful to use predicates when they are more limited? Well, they can be much simpler, shorter and readable than functions. Look at this example:
```swift
$0.hidden = Condition.Predicate(NSPredicate(format: "$switchTag == false"))
```
And we can write it even shorter since `Condition` conforms to StringLiteralConvertible:
```swift
$0.hidden = "$switchTag == false"
```

*Note: we will substitute the value of the row whose tag is 'switchTag' instead of '$switchTag'*

For all of this to work, **all of the implicated rows must have a tag** as the tag will identify them.

We can also hide a row by doing:
```swift
$0.hidden = true
```
as `Condition` conforms to BooleanLiteralConvertible.

Not setting the `hidden` variable will leave the row always visible.

##### Sections
For sections this works just the same. That means we can set up section `hidden` property to show/hide it dynamically.

##### Disabling rows
To disable rows, each row has an `disabled` variable which is also an optional Condition type property . This variable also works the same as the `hidden` variable so that it requires the rows to have a tag.

Note that if you want to disable a row permanently you can also set `disabled` variable to `true`.

## Extensibility

### How to create custom rows and cells  <a name="custom-rows"></a>

To create a custom row you will have to create a new class subclassing from `Row<ValueType, CellType>` and conforming to `RowType` protocol.
Take for example the SwitchRow:

```swift
public final class SwitchRow: Row<Bool, SwitchCell>, RowType {

    required public init(tag: String?) {
        super.init(tag: tag)
        displayValueFor = nil
    }
}
```

Most times you will want to create a custom cell as well as most of the specific logic is here. What you have to do is subclassing Cell<ValueType>:

```swift
public class SwitchCell : Cell<Bool> {

    required public init(style: UITableViewCellStyle, reuseIdentifier: String?) {
        super.init(style: style, reuseIdentifier: reuseIdentifier)
    }

    public var switchControl: UISwitch? {
        return accessoryView as? UISwitch
    }

    public override func setup() {
        super.setup()
        selectionStyle = .None
        accessoryView = UISwitch()
        editingAccessoryView = accessoryView
        switchControl?.addTarget(self, action: "valueChanged", forControlEvents: .ValueChanged)
    }

    public override func update() {
        super.update()
        switchControl?.on = formRow.value ?? false
        switchControl?.enabled = !formRow.isDisabled
    }

    func valueChanged() {
        formRow.value = switchControl?.on.boolValue ?? false
    }
}
```

The setup and update methods are similar to the cellSetup and cellUpdate callbacks and that is where the cell should be customized.

Note: ValueType and CellType are illustrative. You have to replace them with the type your value will have and the type of your cell (like Bool and SwitchCell in this example)

## How to create custom inline rows

A inline row is a specific type of row that shows dynamically a row below it, normally an inline row changes between a expand and collapse mode whenever the row is tapped.

So to create a inline row we need 2 rows, the row that are "always" visible and the row that will expand/collapse.

Another requirement is that the the value type of these 2 rows must be the same.

Once we have these 2 rows, we should make the top row type conforms to `InlineRowType` which will add some methods to the top row class type such as:

```swift
func expandInlineRow()
func hideInlineRow()
func toggleInlineRow()
```

Finally we must invoke `toggleInlineRow()` when the row is selected, for example overriding the customDidSelect() row method.

```swift
public override func customDidSelect() {
    toggleInlineRow()
}
```

### Custom rows catalog

Have you created a custom row, theme, etc?
Let us know about it, we would be glad to mention it here..

## Installation

#### CocoaPods

[CocoaPods](https://cocoapods.org/) is a dependency manager for Cocoa projects.

Specify Eureka into your project's `Podfile`:

```ruby
source 'https://github.com/CocoaPods/Specs.git'
platform :ios, '8.0'
use_frameworks!

pod 'Eureka', '~> 1.0'
```

Then run the following command:

```bash
$ pod install
```

#### Carthage

[Carthage](https://github.com/Carthage/Carthage) is a simple, decentralized dependency manager for Cocoa.

Specify Eureka into your project's `Cartfile`:

```ogdl
github "xmartlabs/Eureka" ~> 1.0
```

#### Manually as Embedded Framework

* Clone Eureka as a git [submodule](http://git-scm.com/docs/git-submodule) by running the following command from your project root git folder.

```bash
$ git submodule add https://github.com/xmartlabs/Eureka.git
```

* Open Eureka folder that was created by the previous git submodule command and drag the Eureka.xcodeproj into the Project Navigator of your application's Xcode project.

* Select the Eureka.xcodeproj in the Project Navigator and verify the deployment target matches with your application deployment target.

* Select your project in the Xcode Navigation and then select your application target from the sidebar. Next select the "General" tab and click on the + button under the "Embedded Binaries" section.

* Select `Eureka.framework` and we are done!

## Authors

* [Martin Barreto](https://github.com/mtnBarreto) ([@mtnBarreto](https://twitter.com/mtnBarreto))
* [Mathias Claassen](https://github.com/mats-claassen)

## FAQ

#### How to change the bottom navigation accessory view?

To change the behaviour of this you should set the navigation options of your controller. The `FormViewController` has a `navigationOptions` variable which is an enum and can have one or more of the following values:

- **Disabled**: no view at all
- **Enabled**: enable view at the bottom
- **StopDisabledRow**: if the navigation should stop when the next row is disabled
- **SkipCanNotBecomeFirstResponderRow**: if the navigation should skip the rows that return false to `canBecomeFirstResponder()`

The default value is `Enabled & SkipCanNotBecomeFirstResponderRow`

If you want to change the whole view of the bottom you will have to override the `navigationAccessoryView` variable in your subclass of `FormViewController`.

#### How to get a Row using its tag value

We can get a particular row by invoking any of the following functions exposed by the `Form` class:

```swift
public func rowByTag<T: Equatable>(tag: String) -> RowOf<T>?
public func rowByTag<Row: RowType>(tag: String) -> Row?
public func rowByTag(tag: String) -> BaseRow?
```

For instance:

```swift
let dateRow : DateRow? = form.rowByTag("dateRowTag")
let labelRow: LabelRow? = form.rowByTag("labelRowTag")

let dateRow2: Row<NSDate>? = form.rowByTag("dateRowTag")

let labelRow2: BaseRow? = form.rowByTag("labelRowTag")
```

#### How to get a Section using its tag value

```swift
let section: Section?  = form.sectionByTag("sectionTag")
```

#### How to get the form values

We can get all form values by invoking the following `Form` function:

```swift
public func values(includeHidden includeHidden: Bool = false) -> [String: Any?]
```

Passing `true` as `includeHidden` parameter value will also include the hidden rows values in the dictionary.

As you may have noticed the result dictionary key is the row tag value and the value is the row value. Only rows with a tag value will be added to the dictionary.

#### How to set the form values using a dictionary

Invoking `setValues(values: [String: Any?])` which is exposed by `Form` class.

For example:

```swift
form.setValues(["IntRowTag": 8, "TextRowTag": "Hello world!", "PushRowTag": Company(name:"Xmartlabs")])
```

Where `"IntRowTag"`, `"TextRowTag"`, `"PushRowTag"` are row tags (each one uniquely identifies a row) and `8`, `"Hello world!"`, `Company(name:"Xmartlabs")` are the corresponding row value to assign.

The value type of a row must match with the value type of the corresponding dictionary value otherwise nil will be assigned.

If the form was already displayed we have to reload the visible rows either by reloading the table view `tableView.reloadData()` or invoking `updateCell()` to each visible row.

<!--- In file -->
[Introduction]: #introduction
[Requirements]: #requirements

[How to create a Form]: #how-to-create-a-form
[Examples]: #examples
[Usage]: #usage
[Operators]: #operators
[Rows]: #rows
[Customization]: #customization
[Section Header and Footer]: #section-header-and-footer
[How to create custom rows and cells]: #custom-rows
[How to create custom inline rows]: #how-to-create-custom-inline-rows
[Custom rows catalog]: #custom-rows-catalog
[How to dynamically hide and show rows (or sections)]: #hide-show-rows
[Extensibility]: #extensibility
[Installation]: #installation
[FAQ]: #faq


<!--- In Project -->
[CustomCellsController]: Example/Example/ViewController.swift
[FormViewController]: Example/Source/Controllers.swift

<!--- External -->
[XLForm]: http://github.com/xmartlabs/XLForm
[DSL]: https://en.wikipedia.org/wiki/Domain-specific_language
[StackOverflow]: http://stackoverflow.com/questions/tagged/eureka
[our blog post]: http://blog.xmartlabs.com/2015/09/29/Introducing-Eureka-iOS-form-library-written-in-pure-Swift/
[twitter]: https://twitter.com/xmartlabs
