## For framework designers
	^ self decorateWith: SpCommand
	^ (CmCommandGroup named: 'Adding') asSpecGroup
			description: 'Commands related to contact addition.';
			register: (EgAddContactCommand forSpec context: presenter);
			beDisplayedAsGroup;
			yourself
	^ self new asSpecCommand
	^ super asSpecCommand
		shortcutKey: $n meta;
		iconName: #changeAdd;
		yourself
	^ self decorateWith: SpCommandGroup
	^ SpMenuPresenterBuilder new
		visit: self;
		menuPresenter
	^ SpToolBarPresenterBuilder new
		visit: self;
		toolbarPresenter
	aCmCommandEntry positionStrategy
		addButton: (SpToolBarButton new
						label: aCmCommandEntry name;
						help: aCmCommandEntry description;
						icon: aCmCommandEntry icon;
						action: [ aCmCommandEntry execute ];
						yourself)
		toToolbar: self toolbarPresenter