## A Simple Contact Book
	instanceVariableNames: 'name phone'
	classVariableNames: ''
	package: 'EgContactBook'

		super printOn: aStream.
		aStream nextPut: $(.
		aStream nextPutAll: name.
		aStream nextPut: $).
	^ name
	^ phone
	name := aString
	phone := anObject
	^ name includesSubstring: aString caseSensitive: false
	^ self new
		name: aNameString;
		phone: aPhoneString;
		yourself
	instanceVariableNames: 'contacts'
	classVariableNames: ''
	package: 'EgContactBook'
		super initialize.
		contacts := OrderedCollection new
	^ contacts
	contacts := aColl
	contacts add: aContact
	contacts remove: aContact
	contacts add: newContact after: contactAfter
	^ contacts includes: aContact
	| contact |
	contact := EgContact new name: contactName; phone: phone.
	self addContact: contact.
	^ contact
	^ contacts select: [ :e | e hasMatchingText: aText ]
	^ contacts size
	^family ifNil: [
		family := self new
			add: 'John' phone: '342 345';
			add: 'Bill' phone: '123 678';
			add: 'Marry' phone: '789 567';
			yourself]
	^coworkers ifNil: [
		coworkers := self new
			add: 'Stef' phone: '112 378';
			add: 'Pavel' phone: '898 678';
			add: 'Marcus' phone: '444 888';
			yourself]
	<script>
	coworkers := nil.
	family := nil
	instanceVariableNames: 'table contactBook'
	classVariableNames: ''
	package: 'EgContactBook'
	^ contactBook
	table := anObject
	^ table
	super setModelBeforeInitialization: aContactBook.
	contactBook := aContactBook
	^ SpBoxLayout newTopToBottom add: #table; yourself
	table := self newTable.
	table
		addColumn: (StringTableColumn title: 'Name' evaluated: #name);
		addColumn: (StringTableColumn title: 'Phone' evaluated: #phone).
	table items: contactBook contents.
	<example>
	^ (self on: EgContactBook coworkers) openWithSpec
	| rawData splitted |
	rawData := self
		request: 'Enter new contact name and phone (split by comma)'
		initialAnswer: ''
		title: 'Create new contact'.
	splitted := rawData splitOn: $,.
	(splitted size = 2 and: [ splitted allSatisfy: #isNotEmpty ])
		ifFalse: [ SpInvalidUserInput signal: 'Please enter contact name and phone (split by comma)'  ].

	^ EgContact new
		name: splitted first;
		phone: splitted second;
		yourself
	openWithSpec presenter inspect
	^ self table selectedItems isNotEmpty
	^ table selection selectedItem