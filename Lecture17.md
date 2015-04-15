#Today 

###Internationalization and Localization 
Making your app marketable around the world 
###Settings
Adding UI to the General Settings application 


#Internationalization 

###Two steps to making international versions of your application

Internationalization (i18n)

Localization (l10n)

###Internationalization

This is a process of making strings externally editable (from storyboard or code).

It also involves using certain “formatting” classes for things like dates, numbers, etc.

You (the developer) get to do this work.
 
###Localization 

A process of translating those externalized strings for a given language.

You usually hire a localization company to do this work. 
 
# Internationalization
###Storyboards are localized by changing the strings inside only

And we rely on Autolayout to make it all look nice. 

###First step: Registering Localizable Languages

Go to the Project pane in Xcode (top in Navigator), then Info tab to add Localizations.
 

You must inspect the project itself here, not the Target you build. 

Click this + to add more languages that you intend to support. 



### Storyboards in the Navigator will now support “localizations”

If you select a storyboard in the navigator, you’ll see the localizations there …

… and in the File Inspector (leftmost tab in Utilities area)

That’s all there is to it. Your storyboards are now localizable (you’ve internationalized them). 



###What about strings not in storyboards?

i.e., literal strings “string”

We replace them with calls to NSLocalizedString()… 
```swift
func NSLocalizedString(
    key:String,// usually this is the literal string (in the developer’s language) 
    tableName:String,bundle:NSBundle,// can be used to organize strings (not required) 
    value:String,// value to use if key is not found in a localization (defaults to key) 
    comment:String// instructions for the person translating this string 
)->String
```
###Most of the parameters have good defaults

Therefore most of the time, we just use … 
```swift
let localString = NSLocalizedString(“Foo”,comment:“This string is shown when…”)
```
The bundle defaults to NSBundle.mainBundle()and tableName to “Localizable”

#Formatted Strings 

###Strings created from a format (println-like) string

Any string you create for the UI with a format string, should use this method … 
```swift
let string = String.localizedStringWithFormat(
     NSLocalizedString(“the result is %g”,comment:”gives numeric result to the user”),
     theNumber
)
```
###Localizers can specify the order of arguments like this …
“result number %d is %d”=“%2$d is the result at position %1$d”


#Formatters 

###Anything else? Yes … formatting for locale

A locale is a region of the world

It is separate from language

Your application might be localized to Spanish, but the user might be in Spain or in Mexico

In different regions some things might display differently: numbers, dates, currency, etc.
 
###iOS will format these things for you if you let it

But you must use the appropriate formatter class … 
```swift
NSNumberFormatter(including currency) 
NSDateFormatter
NSDateComponentsFormatter
NSByteCountFormatter
MKDistanceFormatter
```
Health Kit (see HKUnit) formatters 

Formatters will even let you set their locale to work in if you deal with multiple locales at once 


#NSNumberFormatter

###Lots going on here. Check out the documentation. 
But we’ll look at two simple cases ... 
###Displaying numbers
```swift
let formatter=NSNumberFormatter()
```
… configure it … 
```swift
var numberStyle:NSNumberFormatterStyle// .DecimalStyle, .CurrencyStyle, etc. 
```
Can also control number of digits before/after decimal, rounding, currency, etc., then … 
```swift
let stringToDisplay = formatter.stringFromNumber(theNumber)
```
###Parsing numbers
```swift
formatter.numberStyle=.DecimalStyle
let parsedNumber = formatter.numberFromString(userInputtedString){ … }
```
Note that this will return nil if a number of the proper format is not found. 


#NSDateFormatter

###Dates are rather complicated to display properly

Hopefully one of the built-in styles will work for you

The style of date can be set separately for time and date 
```swift
var timeStyle:NSDateFormatterStyle
var dataStyle:NSDateFormatterStyle
.ShortStyle or .MediumStyle or .LongStyle or .FullStyle or .NoStyle(exclude)
```
If you need more control over presentation than this, you’ll need to study up! 


#UIImage

###How to localize images?

There are a number of ways to approach this.

The image files themselves (if not in Images.xcassets) can be made localizable (File Inspector) 

Or you can keep them in Images.xcassets and use NSLocalizedString for the image name 

#Localizing

###Congratulations! Your application is Internationalized!

Now what? 

Simply generate an .xliff file for each language (an industry-standard “for localizers” file) 

The .xliff will contain all of your strings and comments 

When the .xliff comes back from the localizers, just import it

You can then export/import/export/import repeatedly without losing any translations 


#Previewing a Localization 

###When you get a localization back from your localizer

 - You can preview what the localization will look like

 - You can do this in Xcode (if you just want to see the storyboard localized) 

   Note this “pseudo-language”.

   It’s a quick way to check your autolayout against “long stringed languages” (e.g. German). 


 - Or you can create a “Scheme” to Run your application in that language 
 
   Then change the language and region here. 

   Duplicate an existing scheme. 
   
   Set its name appropriately. 

 - You can also turn on localization debugging.
 
   Unlocalized strings will now be brought to your attention.



#Localization 

###That’s all it takes 

Localization in iOS 8 has gotten substantially more streamlined, so there’s no excuse! 

Get that autolayout right!

Get those NSLocalizedString calls in there! 

Use those formatters! 


#Settings 

###A little bit of UI for your application in the Settings application

You should use this sparingly (if at all).

It’s appropriate only for very rarely used settings or default behavior.

You don’t want to make your users ever have to go here for normal use of your application.

The settings appear in your application via NSUserDefaults. 

You specify the UI and the associated defaults in a property list file. 

###Creating a Settings Bundle

Choose New File from the menus and then … 

###A “sample” settings bundle is created

Check the documentation for all the possible things you can createThe sample is a good place to start when trying to understand how things work 

###Localization of Settings

Localization does not use the normal mechanism in Settings

Instead, you create strings files inside the .lproj directories in your Settings bundle

Evidently you’ll have to send them to your translators separately from your .xliffs

