# BELLEROPHON #

![Bellerophon fighting Chimera](http://www1.artflakes.com/artwork/products/246494/poster/246494.jpg)

Bellerophon is a hero of Greek mythology. He was *"the greatest hero and slayer of monsters, alongside Cadmus and Perseus, before the days of Heracles"*, and his greatest feat was killing the **Chimera**, a monster that Homer depicted with a lion's head, a goat's body, and a serpent's tail.

Sometimes in a development phase, it happens for different reasons that the app available in the store has to be killed. It could be because it contains a major issue (crash, security breach...) or for a business decision (killing the app before a Sale starts, so users are not buying before the others for example).

### What is this repository for? ###

Bellerophon is a Swift implementation of a protocol and convenient methods that allow you to either kill your app or force the users to update using the App Store. By implementing Bellerphon, you will be able to easily add the logic to present a kill state view in your app, or force users to update.

#### Version ####
Bellerophon is at version **1.0**


### How do I get set up? ###

#### Installation ####

TODO

#### Configuration ####

In order to use Bellerophon, you'll have to follow these steps:

1 - In your AppDelegate implementation file, import the project and register to the `BellerophonManagerProtocol`.

```swift
class AppDelegate: UIResponder, UIApplicationDelegate, BellerophonManagerProtocol {
	...
}
```

2 - Implement and configure the Bellerophon shared manager by setting the delegate property and the kill view.

```swift
BellerophonManager.sharedInstance.delegate = self
BellerophonManager.sharedInstance.killSwitchView = <YOUR_VIEW>
```

3 - Start the check the app status

```swift
BellerophonManager.sharedInstance.checkAppStatus()
```

Here is for the Bellerophon basic implementation. Now you will need to create your own model that conforms to the Bellerophon status protocol.

4 - Creates your model and make it conforms to `BellerophonStatusProtocol`.

```swift
class BellerophonModel: BellerophonStatusProtocol { }
```

5 - Implements the `BellerophonStatusProtocol` methods.

```swift
@objc func apiInactive() -> Bool {
	...
}

@objc func forceUpdate() -> Bool {
	...
}

@objc func retryInterval() -> NSTimeInterval {
	...
}

@objc func userMessage() -> String {
	...
}
```

6 - Now that you have your model, you are ready to implement the `BellerophonManagerProtocol` methods in your App Delegate.

```swift
func bellerophonStatus(manager: BellerophonManager, completion: (status: BellerophonStatusProtocol?, error: NSError?) -> ()) {
	// MAKE API CALL
	APIManager.GET() { (status, error)
		// Send the API data to the Bellerophon manager.
		completion(status: status, error: error)
		}
	}
}

func checkVersion(manager: BellerophonManager) {
	// CHECK APP VERSION
}
```

Your app is not ready to work with Bellerophon. If the status tells Bellerophon to kill the app, it will display a full screen view on top of everything and will keep it like that until the status API indicates that the app should work again. A timer is also starting based on the retry interval provided by the status object.

Your API and model should be able to indicate at least these informations :

* Is the API active or inactive?
* Should the app be forced to update?
* What is the retry interval?
* What is the user message to display on the kill view?

The specifications that we are using at Prolific Interactive are this one:

```
{
    "apiInactive": false,
    "forceUpdate": false,
    "retryInterval": null,
    "userMessage": null
}
```


#### How to test? ####

Currently there is not an easy way to test Bellerophon. The best way is either to force the response model from the API to return an API inactive state so you can see the kill switch, or use a web debugging proxy like [Charles](http://www.charlesproxy.com) to set a breakpoint on the endpoint and change the API response to deactivate the API.

### LICENSE ###

Bellerophon is Copyright (c) 2015 Prolific Interactive. It may be redistributed under the terms specified in the [LICENSE](LICENSE.md) file.

### Who do I talk to? ###

* Repo owner or admin
	* Thibault Klein - thibault@prolificinteractive.com
* Other community or team contact
	* Max Mamis - max@prolificinteractive.com
	* Paul Miard - p@prolificinteractive.com 