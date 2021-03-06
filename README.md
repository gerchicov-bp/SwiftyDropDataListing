# SwiftyDropDataListing

SwiftyDropDataListing provides a simple and effective way to browse, view, and get files(download) using the SwiftyDropbox. But you need to first integrate SwiftyDropbox. To integrate SwiftyDropbox You must follow this link [SwiftyDropbox](https://github.com/dropbox/SwiftyDropbox) to implement SwiftyDropbox first.


# Setup

Follow this link [SwiftyDropbox](https://github.com/dropbox/SwiftyDropbox) to implement SwiftyDropbox first.

After successfully integration, handle the redirection back into the Swift SDK once the authentication flow is completed and after that add the following code in your application's delegate and  fire a notification. And add Observer to recieve the notification in YourViewController and push to the DBListingViewController From Your controller.


# AppDelegate class  

    func application(_ application: UIApplication, open url: URL, sourceApplication: String?, annotation: Any) -> Bool {  
    
    if let authResult = DropboxClientsManager.handleRedirectURL(url) {
      switch authResult {
      case .success:
        
        NotificationCenter.default.post(name: Notification.Name(rawValue: "Dropboxlistrefresh"), object: nil)
        
        print("Success! User logged in.")
      case .error( _, let description):
        print("Error: \(description)")
      case .cancel:
        print("cancel")
      }
    }
    
     return false
    }

# YourViewController

    import UIKit
    import SwiftyDropbox

    class ViewController: UIViewController, SelectedDropboxData {
  
    override func viewDidLoad() {
     super.viewDidLoad()
    
     NotificationCenter.default.addObserver(self, selector: #selector(self.refreshDropboxList), name: NSNotification.Name(rawValue: "Dropboxlistrefresh"), object: nil)
    
    }
  
    override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
     if segue.identifier == "TableViewController"{
       let tableViewController = segue.destination  as! DBListingViewController
       tableViewController.delegate = self
       tableViewController.showDropboxData()
    }
    }
  
    //MARK: SelectedDropboxData method
    func getDropboxSelectedData(_ dataArr: [String]){
    print(dataArr)
    }
  
    // Handle Notification
    // and segue to TableViewController. DBListingViewController Must be connected through the storyboard. and it should be push through the navigation controller.
    func refreshDropboxList() {
     self.performSegue(withIdentifier: "TableViewController", sender: nil)
    }
  
    @IBAction func linkToDropboxClicked(_ sender: AnyObject) {
    
    if (DropboxClientsManager.authorizedClient == nil) {
      //authorize a user
      DropboxClientsManager.authorizeFromController(UIApplication.shared,
                                                    controller: self,
                                                    openURL: { (url: URL) -> Void in
                                                      UIApplication.shared.openURL(url)
      })
    } else {
      print("User is already authorized!")
      self.refreshDropboxList()
     }
     }
    }


# Features
Key Features of SwiftDropDataListing are listed below. It has a great interface for ios 9 ,solid file handling features and file search capability.

# User Interface 

SwiftyDropDataListing has a beautiful and simple interface similar to that of the actual Dropbox App. The interface is built for iOS 9 or greator. You can get all dropbox data with in your app simply by after autenticate in dropbox.

<img src = "https://cloud.githubusercontent.com/assets/7422405/25396939/162d3ff2-2a04-11e7-977e-ad977533111d.jpg" /> 

# Files

In SwiftyDropDataListing we are currently showing all files. When a user tap on file it get downloaded and it calls a Delegate method getDropboxSelectedData.

 To get selected/ downloaded file path you must implement SelectedDropboxData Delegate method in your class.

     func getDropboxSelectedData(_ dataArr: [String])
     
# File Search

Users can quickly get to the files they need by using the built-in search features. Just scroll to the top and start typing for instant search results. Download files directly from search results.

# Requirements
Requires minimum Xcode 9.0 for use in any iOS Project. Requires a minimum of iOS 9.0 as the deployment target.
Minimum Swift 3.

# Installation

# Cocoapods
CocoaPods is a dependency manager for Cocoa projects. You can install it with the following command:

$ gem install cocoapods To integrate `SwiftyDropDataListing` into your Xcode project using CocoaPods, specify it in your Podfile:

```platform :ios, '9.0'
use_frameworks!

target '<Your Target Name>' do
  pod 'SwiftyDropDataListing','~> 0.0.1'
end
Then, run the following command:
```

$ pod install

# Contributions

Any contribution is more than welcome! You can contribute through pull requests and issues on GitHub.

 



