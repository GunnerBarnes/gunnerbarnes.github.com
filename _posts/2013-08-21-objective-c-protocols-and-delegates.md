---
layout: post
title: "Objective-C Protocols and Delegates"
description: ""
category: 
tags: [iOS, protocols, delegates, core data]
---

For the second time in this project, I'd been struggling with a core piece of functionality for the iPhone app I'm attempting to build. Last month, I spent three weeks trying to wrap my head around Core Data. This month, I've already spent an equivalent amount of time trying to figure out protocols and delegates, an important aspect of navigating between view controllers. Luckily, I found another great tutorial at [www.raywenderlich.com](http://www.raywenderlich.com/tutorials) that helped guide me through the concept. 

###Protocols 
A protocol is a set of methods that another class is expected to implement.
By defining a protocol, we are saying that any class that conforms to this protocol must implement all methods declared in the protocol, unless the method is specified to be optional. By default, however, all methods are required. The protocol is defined in the delegating class. 

###Delegates
The class that is implementing the methods declared in the protocol (therefore, conforming to the protocol) is the delegate for the defining class. The delegate will receive information from the delegating class, and perform the protocol using that information.

###Using Protocols and Delegates to Add New Contacts
In my currently-in-progress contact management app, I have two views that are responsible for displaying and creating a list of contacts. My MasterViewController is just a UITableViewController that lists all of my contacts, and my AddContactViewController is where new contacts are created. There is a button on the MasterViewController to add a new contact, and there are two buttons on the AddContactViewController, to cancel creating the new contact, and to save the new contact we're creating. My AddContactViewController knows nothing about the data that is displayed in my MasterViewController, nor should it need to. This is why I'm going to use delegation to handle the creation of new contacts.

###AddContactViewController
Here is my **AddContactViewController.h** file, where we define the protocol:

	#import "ContactInfo.h"
	
	@class AddContactViewController;
	
	@protocol AddContactViewControllerDelegate <NSObject>
	
	- (void)addContactViewControllerDidCancel:
		(AddContactViewController *)controller;
	- (void)addContactViewController:(AddContactViewController *) controller
		 didAddContact:(ContactInfo *)newContact;
	
	@end
	
	@interface AddContactViewController : 
		UITableViewController <UITableViewDelegate>
	@property (weak, nonatomic) IBOutlet UITextField *nameTextField;
	@property (weak, nonatomic) IBOutlet UITextField *phoneTextField;
	
	@property (weak, nonatomic) id 
		<AddContactViewControllerDelegate> delegate;
	
	- (IBAction)cancel:(id)sender;
	- (IBAction)done:(id)sender;
	
	@end

As you can see, I'm defining two methods, DidCancel and didAddContact, as my protocol. My ContactInfo class is just a simple definition of a contact with a name and phone number. We have two text fields for adding the name and phone number of our new contact, and our two buttons for either canceling or saving. Now let's take a look at the implementation file, **AddContactViewController.m**, where I'm defining what happens when we push either button.

	#import "AddContactViewController.h"
	#import "ContactInfo.h"
	#import "AppDelegate.h"
	
	@interface AddContactViewController ()

	@end

	@implementation AddContactViewController

	@synthesize delegate;
	
	- (IBAction)cancel:(id)sender
	{
    	[self.delegate addContactViewControllerDidCancel:self];
	}

	- (IBAction)done:(id)sender
	{
    	AppDelegate *appDelegate = 
    		[[UIApplication sharedApplication] delegate];
    	NSManagedObjectContext *context = appDelegate.managedObjectContext;
    	NSEntityDescription *entity = [NSEntityDescription
    		entityForName:@"ContactInfo" inManagedObjectContext:context];
	    	ContactInfo *info = [[ContactInfo alloc] 
	    	initWithEntity:entity insertIntoManagedObjectContext:context];
    	info.name = self.nameTextField.text;
    	info.phone = self.phoneTextField.text;
    	
        	[self.delegate addContactViewController:self didAddContact:info];
	}
	
All we're doing here is defining what happens when each button is pushed. When the cancel button is touched, the line:
	[self.delegate addContactViewControllerDidCancel:self];
tells my delegated class to call it's addContactViewControllerDidCancel method.

In my case, I'm using Core Data and my AppDelegate class is handling my NSManagedObjectContext. So when the **done** button is touched, I'm creating a new NSManagedObjectContext here for my new contact, giving my new object the name and phone number values we've entered, and then passing that info to the didAddContact method of my delegated class.

As you can see, the AddContactViewController class only worries about creating a new contact based on the values entered in the AddContactView. The delegated class, **MasterViewController**, will implement the methods that I defined in my protocol to act on the new contact info.

###MasterViewController
The **MasterViewController.h** file only requires two small changes:

	#import "AddContactViewController.h"
	
	@interface MasterViewController : UITableViewController
		<AddContactViewControllerDelegate>
All we're doing here is importing the header file for the AddContactViewController class, and declaring this class to be a delegate for AddContactViewController. Now all we have left is the implementation of the two methods we defined in our protocol.

	#pragma mark AddContactViewControllerDelegate

	- (void)addContactViewControllerDidCancel:
		(AddContactViewController *)controller
	{
    	[self dismissViewControllerAnimated:YES completion:nil];
	}

	- (void)addContactViewController:
		(AddContactViewController *)controller 
		didAddContact:(ContactInfo *)newContact
	{
    	NSError *error;
    	if (![self.managedObjectContext save:&error])
    	{
        		NSLog(@"Couldn't save newly created contact. Error: %@", error);
        		abort();
    	}
    	[self dismissViewControllerAnimated:YES completion:nil];
	}

Now, if the **cancel** button is touched, the AddContactViewController is just dismissed. If the **done** button is touched, we save our new contact and dismiss the AddContactViewController. 

###Takeaway
It took me quite a while to fully grasp the concepts of the delegate design pattern and protocols. Just remember: the protocol is defined in the delegating class, and 
the implementation of the protocol methods occurs in the delegate class.

If you're interested in creating iOS apps, [www.raywenderlich.com](http://www.raywenderlich.com/tutorials) is a fantastic reference/guide. Each time I've gotten hung up on a concept, I've found the answers I needed there.
