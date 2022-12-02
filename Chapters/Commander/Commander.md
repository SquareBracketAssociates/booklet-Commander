## Commander: a Powerful and Simple Command Framework
	instanceVariableNames: ''
	classVariableNames: ''
	package: 'EgContactBook'
	^ self context
	^ self contactBookPresenter contactBook
	^ self contactBookPresenter selectedContact
	^ self contactBookPresenter isContactSelected
	instanceVariableNames: ''
	classVariableNames: ''
	package: 'EgContactBook'
	super initialize.
	self
		basicName: 'New contact'; 
		basicDescription: 'Creates a new contact and add it to the contact book.'
	| contact |
	contact := self contactBookPresenter newContact.
	self hasSelectedContact
		ifTrue: [ self contactBook 
					addContact: contact 
					after: self selectedContact ]
		ifFalse: [ self contactBook addContact: contact ].
	self contactBookPresenter updateView
	table items: contactBook contacts
presenter := EgContactBookPresenter on: EgContactBook coworkers.
cmd := EgAddContactCommand new context: presenter.
cmd execute
	instanceVariableNames: ''
	classVariableNames: ''
	package: 'EgContactBook'
	super initialize.
	self
		name: 'Remove'; 
		description: 'Removes the selected contact from the contact book.'
	^ self context isContactSelected
	self contactBook removeContact: self selectedContact.
	self contactBookPresenter updateView
	self assert: presenter contactBook size equals: 3.
	presenter table selectIndex: 1.
	(EgRemoveContactCommand new context: presenter) execute.
	self assert: presenter contactBook size equals: 2
	buildCommandsGroupWith: presenter 
	forRoot: rootCommandGroup
	
	rootCommandGroup 
		register: (EgAddContactCommand forSpec context: presenter);
		register: (EgRemoveContactCommand forSpec context: presenter)
	table := self newTable.
	table 
		addColumn: (SpStringTableColumn title: 'Name' evaluated: #name);
		addColumn: (SpStringTableColumn title: 'Phone' evaluated: #phone).
	table contextMenu: [ self rootCommandsGroup beRoot asMenuPresenter ].
	table items: contactBook contacts.
	^ (CmCommandGroup named: 'Adding') asSpecGroup
			description: 'Commands related to contact addition.';
			register: (EgAddContactCommand forSpec context: presenter);
			beDisplayedAsGroup;
			yourself
	^ (CmCommandGroup named: 'Removing') asSpecGroup
			description: 'Commands related to contact removal.';
			register: (EgRemoveContactCommand forSpec context: presenter);
			beDisplayedAsGroup;
			yourself
	^ (CmCommandGroup named: 'Context Menu') asSpecGroup
			register: (self buildAddingGroupWith: presenter);
			register: (self buildRemovingGroupWith: presenter);
			yourself
	buildCommandsGroupWith: presenter 
	forRoot: rootCommandGroup
	
	rootCommandGroup
		register: (self buildContextualMenuGroupWith: presenter)

	table := self newTable.
	table 
		addColumn: (SpStringTableColumn title: 'Name' evaluated: #name);
		addColumn: (SpStringTableColumn title: 'Phone' evaluated: #phone).
	
	table contextMenu: [ (self rootCommandsGroup / 'Context Menu') beRoot asMenuPresenter ].
	table items: contactBook contacts
	instanceVariableNames: 'newPhone'
	classVariableNames: ''
	package: 'EgContactBook-Extensions'
	newPhone := anObject
	^ newPhone 
	super initialize.
	self
		name: 'Change phone';
		description: 'Change the phone number of the contact.'
	self selectedContact phone: self contactBookPresenter newPhone.
	self contactBookPresenter updateView
	| phone |
	phone := self 
		request: 'New phone for the contact'
		initialAnswer: self selectedContact phone 
		title: 'Set new phone for contact'.
	(phone matchesRegex: '\d\d\d\s\d\d\d')
		ifFalse: [ 
			SpInvalidUserInput signal: 'The phone number is not well formated. 
Should match "\d\d\d\s\d\d\d"' ].
	^ phone
	changePhoneCommandWith: presenter 
	forRootGroup: aRootCommandsGroup
	
	<extensionCommands>
	
	(aRootCommandsGroup / 'Context Menu')
		register: (EgChangePhoneCommand forSpec context: presenter)
	^ super asSpecCommand
			iconName: #removeIcon;
			shortcutKey: $x meta;
			yourself
	^ super asSpecCommand
			shortcutKey: $n meta;
			iconName: #changeAdd;
			yourself
	
	super initializeWindow: aWindowPresenter.
	self rootCommandsGroup installShortcutsIn: aWindowPresenter
	instanceVariableNames: ''
	classVariableNames: ''
	package: 'EgContactBook-Extensions'
	super initialize.
	self
		name: 'Inspect';
		description: 'Inspect the context of this command.'
	self context inspect
	extraCommandsWith: presenter 
	forRootGroup: aRootCommandsGroup
	
	<extensionCommands>
	
	aRootCommandsGroup / 'Context Menu'
		register:
			((CmCommandGroup named: 'Extra') asSpecGroup
				description: 'Extra commands to help during development.';
				register:
					((EgInspectCommand forSpec context: [ presenter selectedContact ])
						name: 'Inspect contact';
						description: 'Open an inspector on the selected contact.';
						iconName: #smallFind;
						yourself);
				register:
					((EgInspectCommand forSpec context: [ presenter contactBook ])
						name: 'Inspect contact book';
						description: 'Open an inspector on the contact book.';
						yourself);
				yourself)
	instanceVariableNames: ''
	classVariableNames: ''
	package: 'EgContactBook'
	super initialize.
	self
		name: 'Print';
		description: 'Print the contact book in Transcript.'

	Transcript open.
	self contactBook contacts do: [ :contact | self traceCr: contact name , ' - ' , contact name ]
	^ (CmCommandGroup named: 'MenuBar') asSpecGroup
		register: (EgPrintContactCommand forSpec context: presenter);
		yourself
	buildCommandsGroupWith: presenter 
	forRoot: rootCommandGroup
	
	rootCommandGroup
		register: (self buildMenuBarGroupWith: presenter);
		register: (self buildContextualMenuGroupWith: presenter)
	table := self newTable.
	table 
		addColumn: (SpStringTableColumn title: 'Name' evaluated: #name);
		addColumn: (SpStringTableColumn title: 'Phone' evaluated: #phone).
	table contextMenu: [ (self rootCommandsGroup / 'Context Menu') beRoot asMenuPresenter ].
	table items: contactBook contents.
	menuBar := (self rootCommandsGroup / 'MenuBar') asMenuBarPresenter.

	^ SpBoxLayout newTopToBottom
			add: #menuBar
			withConstraints: [ :constraints | constraints height: self toolbarHeight ];
			add: #table; 
			yourself

	^ (super iconNamed: aName) asGrayScaleWithAlpha
	extraCommandsWith: presenter 
	forRootGroup: aRootCommandsGroup
	
	<extensionCommands>
	
	aRootCommandsGroup / 'Context Menu'
		register:
			((CmCommandGroup named: 'Extra') asSpecGroup
				description: 'Extra commands to help during development.';
				register:
					((EgInspectCommand forSpec context: [ presenter selectedContact ])
						name: 'Inspect contact';
						description: 'Open an inspector on the selected contact.';
						iconName: #smallFind;
						yourself);
				register:
					((EgInspectCommand forSpec context: [ presenter contactBook ])
						name: 'Inspect contact book';
						description: 'Open an inspector on the contact book.';
						yourself);
				yourself)
	table := self newTable.
	table 
		addColumn: (SpStringTableColumn title: 'Name' evaluated: #name);
		addColumn: (SpStringTableColumn title: 'Phone' evaluated: #phone).
	table contextMenu: [ (self rootCommandsGroup / 'Context Menu') beRoot asMenuPresenter ].
	table items: contactBook contents.
	menuBar := (self rootCommandsGroup / 'MenuBar') asToolbarPresenter.

	^ SpBoxLayout newTopToBottom
			add: #menuBar
			withConstraints: [ :constraints | constraints height: self toolbarHeight ];
			add: #table; 
			yourself

	^ self toolbarActions 
		asToolbarPresenterWith: [ :presenter | 
			presenter 
				displayMode: self application toolbarDisplayMode;
				addStyle: 'stToolbar' ]

	^ CmCommandGroup forSpec
			register: (CmCommandGroup forSpec
				register: (StPlaygroundDoItCommand forSpecContext: self);
				register: (StPlaygroundPublishCommand forSpecContext: self);
				register: (StPlaygroundBindingsCommand forSpecContext: self);
				register: (StPlaygroundPagesCommand forSpecContext: self);
				yourself);
		yourself