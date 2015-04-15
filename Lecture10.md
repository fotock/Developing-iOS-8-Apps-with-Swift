
#Today

###UITextField

Bonus Topic!

###Table View

A UIView for displaying long lists or tables of data





#UITextField

###Like UILabel, but editable

Typing things in on an iPhone is secondary UI (keyboard is tiny).

More of a mainstream UI element on iPad.

Don’t be fooled by your UI in the simulator (because you can use physical keyboard!).

You can set attributed text, text color, alignment, font, etc., just like a UILabel.

###Keyboard appears when UITextField becomes “first responder”

It will do this automatically when the user taps on it.

Or you can make it the first responder by sending it the becomeFirstResponder message.

To make the keyboard go away, send resignFirstResponder to the UITextField.

###Delegate can get involved with Return key, etc.
```swift
func textFieldShouldReturn(sender: UITextField) // sent when “Return” key is pressed
```
Oftentimes, you will sender.resignFirstResponder in this method.

Returns whether to do normal processing when Return key is pressed (e.g. target/action). 

###Finding out when editing has ended

Another delegate method ...
```swift
func textFieldDidEndEditing(sender: UITextField)
```
Sent when the text field resigns being first responder.

###UITextField is a UIControl

So you can also set up target/action to notify you when things change.

Just like with a button, there are different UIControlEvents which can kick off an action.

Right-click on a UITextField in a storyboard to see the options available.





#Keyboard
###Controlling the appearance of the keyboard

Set the properties defined in the UITextInputTraits protocol (which UITextField implements).

```swift
var UITextAutocapitalizationType autocapitalizationType // words, sentences, etc.
var UITextAutocorrectionType autocorrectionType // yes or no
var UIReturnKeyType returnKeyType // Go, Search, Google, Done, etc.
var BOOL secureTextEntry // for passwords, for example
var UIKeyboardType keyboardType // ASCII, URL, PhonePad, etc.
```

###The keyboard comes up over other views!

So you may need to adjust your view positioning (especially to keep the text field itself visible).

You do this by reacting to the UIKeyboard{Will,Did}{Show,Hide}Notifications sent by UIWindow.!

We have not talked about NSNotifications yet, but it’s pretty simple.!

You register a method to get called when a named “event” occurs like this …

```swift
NSNotificationCenter.defaultCenter().addObserver(self,
selector: “theKeyboardAppeared:”,
name: UIKeyboardDidShowNotification
object: view.window)
```

The event here is UIKeyboardDidShowNotification.

The object is the one who is causing the even to happen (our MVC’s view’s window).!
```swift
func theKeyboardAppeared(notification: NSNotification) will get called when it happens.!
```
The notification.userInfo will have details about the appearance.!

UITableViewController listens for this & scrolls table automatically if a row has a UITextField.



#UITextField
###Other UITextField properties
```swift
var clearsOnBeginEditing: Bool
var adjustsFontSizeToFitWidth: Bool
var minimumFontSize: CGFloat // always set this if you set adjustsFontSizeToFitWidth
var placeholder: String // drawn in gray when text field is empty
var background/disabledBackground: UIImage
var defaultTextAttributes: Dictionary // applies to entire text
```
###Other UITextField functionality

UITextFields have a “left” and “right” overlays.

You can control in detail the layout of the text field (border, left/right view, clear button).

###Other Keyboard functionality
Keyboards can have accessory views that appear above the keyboard (custom toolbar, etc.).
```swift
var inputAccessoryView: UIView // UITextField method
```



#UITableView
###Very important class for displaying data in a table
One-dimensional table.

It’s a subclass of UIScrollView.

Table can be static or dynamic (i.e. a list of items).

Lots and lots of customization via a dataSource protocol and a delegate protocol.

Very efficient even with very large sets of data.

###Displaying multi-dimensional tables ...

Usually done via a UINavigationController with multiple MVC’s where View is UITableView

###Kinds of UITableViews

Plain or Grouped

Static or Dynamic

Divided into sections or not

Different formats for each row in the table (including completely customized) 

The class UITableViewController provides a convenient packaging of a UITableView in an MVC.

It’s mostly useful when the UITableView is going to fill all of self.view (in fact self.view in a UITableViewController is the UITableView).

You can add one to your storyboard simply by dragging it from here.

Controller: (subclass of) UITableViewController

Controller’s view property: the UITableView


#UITableView Protocols

###How to connect all this stuff up in code?

 - Connections to code are made using the UITableView‘s dataSource and delegate

      The delegate is used to control how the table is displayed (it’s look and feel)

      The dataSource provides the data that is displayed inside the cells

- UITableViewController automatically sets itself as the UITableView’s delegate & dataSource

  Your UITableViewController subclass will also have a property pointing to the UITableView …
```swift
  var tableView: UITableView // self.view in UITableViewController
```
###When do we need to implement the dataSource?

- Whenever the data in the table is dynamic (i.e. not static cells)

- There are three important methods in this protocol …

     How many sections in the table?

     How many rows in each section?

     Give me a view to use to draw each cell at a given row in a given section.

  Let’s cover the last one first (since the first two are very straightforward) ...




#Customizing Each Row
###Providing a UIView to draw each row …
It has to be a UITableViewCell (which is a subclass of UIView) or subclass thereof

Don’t worry, if you have 10,000 rows, only the visible ones will have a UITableViewCell

But this means that UITableViewCells are reused as rows appear and disappear

This has ramifications for multithreaded situations, so be careful in that scenario

The UITableView will ask its UITableViewDataSource for the UITableViewCell for a row …
```swift
func tableView(tv: UITableView, cellForRowAtIndexPath indexPath: NSIndexPath) -> UITableViewCell
{
    let data = myInternalDataStructure[indexPath.section][indexPath.row]
    
    let cell = . . . // create a UITableViewCell and load it up with data
    
    return cell
}
```


#UITableViewDataSource
###How does a dynamic table know how many rows there are?
And how many sections, too, of course?

Via these UITableViewDataSource protocol methods …
```swift
func numberOfSectionsInTableView(sender: UITableView) -> Int
func tableView(sender: UITableView, numberOfRowsInSection: Int) -> Int
```
###Number of sections is 1 by default
In other words, if you don’t implement numberOfSectionsInTableView, it will be 1
###No default for numberOfRowsInSection
This is a required method in this protocol (as is cellForRowAtIndexPath)
###What about a static table?
Do not implement these dataSource methods for a static table

UITableViewController will take care of that for you

You edit the data directly in the storyboard


###Summary
Loading your table view with data is simple …

1. set the table view’s dataSource to your Controller (automatic with UITableViewController)

2.implement numberOfSectionsInTableView and numberOfRowsInSection

3. implement cellForRowAtIndexPath to return loaded-up UITableViewCells

###Section titles are also considered part of the table’s “data”
So you return this information via UITableViewDataSource methods …
```swift
func tableView(UITableView, titleFor{Header,Footer}InSection: Int) -> String
```
If a String is not sufficient, the UITableView’s delegate can provide a UIView
###There are a number of other methods in this protocol

But we’re not going to cover them in lecture

They are mostly about dealing with editing the table by deleting/moving/inserting rows

That’s because when rows are deleted, inserted or moved, it would likely modify the Model
(and we’re talking about the UITableViewDataSource protocol here) 


#Table View Segues
###Preparing to segue from a row in a table view!
The sender argument to prepareForSegue is the UITableViewCell of that row …
```swift
func prepareForSegue(segue: UIStoryboardSegue, sender: AnyObject) {
     if let identifier = segue.identifier {
          switch identifier {
          case “XyzSegue”: // handle XyzSegue here
          case “AbcSegue”:
              let cell = sender as MyTableViewCell
              if let indexPath = tableView.indexPathForCell(cell) {
                   let seguedToMVC = segue.destinationViewController as MyVC
                   seguedToMVC.publicAPI = data[indexPath.section][indexPath.row]
              }
          default: break
          }
     }
 }
```
- You can see now why sender is AnyObject!

  Sometimes it’s a UIButton, sometimes it’s a UITableViewCell
  
- So you will need to cast sender with as or as? to turn it into a UITableViewCell

  If you have a custom UITableViewCell subclass, you can cast it to that if it matters
  
- Usually we will need the NSIndexPath of the UITableViewCell!
  
  Because we use that to index into our internal data structures

- Now we just get our destination MVC as the proper class as usual …

- and then get data from our internal data structure using the NSIndexPath’s section and row!

  and use that information to prepare the segued-to API using its public API


#UITableViewDelegate
###So far we’ve only talked about the UITableView’s dataSource
But UITableView has another protocol-driven delegate called its delegate
###The delegate controls how the UITableView is displayed
Not the data it displays (that’s the dataSource’s job), how it is displayed
###Common for dataSource and delegate to be the same object
Usually the Controller of the MVC containing the UITableView

Again, this is set up automatically for you if you use UITableViewController
###The delegate also lets you observe what the table view is doing
Especially responding to when the user selects a row

Usually you will just segue when this happens, but if you want to track it directly …

#UITableView “Target/Action”
###UITableViewDelegate method sent when row is selected
This is sort of like “table view target/action” (only needed if you’re not segueing, of course)

Example: if the master in a split view wants to update the detail without segueing to a new one
```swift
 func tableView(sender: UITableView, didSelectRowAtIndexPath indexPath: NSIndexPath) {
    // go do something based on information about my Model
    // corresponding to indexPath.row in indexPath.section
    // maybe directly update the Detail if I’m the Master in a split view?
 }
 ```
###Delegate method sent when Detail Disclosure button is touched
```swift
func tableView(UITableView, accessoryButtonTappedForRowWithIndexPath: NSIndexPath)
```
Again, you can just segue from that Detail Disclosure button if you prefer


#UITableViewDelegate
###Lots and lots of other delegate methods

will/did methods for both selecting and deselecting rows

Providing UIView objects to draw section headers and footers

Handling editing rows (moving them around with touch gestures)

willBegin/didEnd notifications for editing

Copying/pasting rows


#UITableView
###What if your Model changes?
```swift
func reloadData()
```
Causes the UITableView to call numberOfSectionsInTableView and numberOfRowsInSection
all over again and then cellForRowAtIndexPath on each visible row

Relatively heavyweight, but if your entire data structure changes, that’s what you need

If only part of your Model changes, there are lighter-weight reloaders, for example ...
```swift
func reloadRowsAtIndexPaths(indexPaths: [NSIndexPath],
                      withRowAnimation: UITableViewRowAnimation)
```

###Controlling the height of rows

Row height can be fixed (UITableView’s var rowHeight: CGFloat)

Or it can be determined using autolayout (rowHeight = UITableViewAutomaticDimension)

If you do automatic, help the table view out by setting estimatedRowHeight to something

The UITableView’s delegate can also control row heights …
```swift
func tableView(UITableView, {estimated}heightForRowAtIndexPath: NSIndexPath) -> CGFloat
```
Beware: the non-estimated version of this could get called A LOT if you have a big table

 
###There are dozens of other methods in UITableView itself

- Setting headers and footers for the entire table.

- Controlling the look (separator style and color, default row height, etc.).

- Getting cell information (cell for index path, index path for cell, visible cells, etc.).

- Scrolling to a row.

- Selection management (allows multiple selection, getting the selected row, etc.).

- Moving, inserting and deleting rows, etc.

- As always, part of learning the material in this course is studying the documentation
