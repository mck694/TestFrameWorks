# TestFrameWorks
Written Description, on how to create SDK/Framework for Swift


How to create Framework/SDK/pod

Xcode -> New Project -> Framework
Name it as MaccoValidator

In that Create a New Swift File, Validator.swift


import Foundation

public struct Validator {

    public static func validEmail(_ email: String) -> Bool {

        let emailRegEx = "[A-Z0-9a-z._%+-]+@[A-Za-z0-9.-]+\\.[A-Za-z]{2,64}"
        let emailPred = NSPredicate(format:"SELF MATCHES %@", emailRegEx)
        return emailPred.evaluate(with: email)
    }
    
    public static func sayHello() {

        print("Hello pretty lady. How are you doing????")
    }
}

// We are making it public so that it can be accessed outside the module

Now to test this, let's create new project

Xcode -> New project -> App 
Name it as TestApp

So to test our Framework, how do we do it

In Framework Click Cmd + b -> Build

Then In Framework
In Folder Structer
	- Expand Products

Click and Drag MaccoValidator.framework In your Project, Add it

So now in your Project
Click Project -> General -> Frameworks, Libraries, and Embedded Content
Click your framework and Select  Embed & Sign

Then in your project 
import MaccoValidator


import UIKit
import MaccoValidator

class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        
        // Do any additional setup after loading the view.
        
        Validator.sayHello()
        print(Validator.validEmail("test@haha.com"))
    }


}

Now let's deploy/publish our SDK

So, Let's push to Github

Github -> Create new Repository

-> Name: MaccoValidator

Add README.MD, .gitignore, license -> MIT

Create Repository

Code -> Copy URL

Open Terminal

cd ..

git clone "https://github.com"
If it gives error, already exists or its not empty directory
Change your folder name,
Add that project folder in your Git Created Folder -> To have example project

then terminal 
git cd MaccoValidator
git add . 
git commit -m "Add Framework"
git push origin master  // now it's online

let's create a tag

git tag 1.0.0

git push origin --tags

Now let's create podspec

pod spec create MaccoValidator

 - let's open podspec

open MaccoValidator.podspec -a xcode

Add like this 
Pod::Spec.new do |spec|

  spec.name         = "MaccoValidator"
  spec.version      = "1.0.0" // ----> This is first version
  spec.summary      = "This is the best framework"
  spec.description  = "I have no idea what to write as a description"

  spec.homepage     = "https://github.com/EMacco/MaccoValidator"
  spec.license      = "MIT"
  spec.author             = { "Emmanuel Okwara" => "emma4real37@gmail.com" }
  spec.platform     = :ios, "13.4"  // To work for older devices
  spec.source       = { :git => "https://github.com/EMacco/MaccoValidator.git", :tag => spec.version.to_s }
  spec.source_files  = "MaccoValidator/**/*.{swift}"
  spec.swift_versions = "5.0"
end

After editing this run

pod spec lint

pod spec lint --allowwarnings // if you didnt have license

SO Now Create New Project to test

pod init
open podfile -a xcode

pod 'MaccoValidator', :path => '../MaccoValidator'

to work move
MaccoValidator.podspec to MaccoValidator Folder

now run 
pod install

it's working

let's publish online in cocoapods

So let's create pod session

pod trunk me
to work this, we need to register
pod trunk register

then push to cocoapods
now go to where the MaccoValidator folder is

pod trunk push MaccoValidator.podspec

If everything is fine, now our pod is published 

So now push to git

git add .
git commit -m "add podspec"
git push origin master

Now check our pod in Cocoapods site

So now in new project

add

pod 'MaccoValidator'

pod repo update
pod install 

Now check it, It works fine

