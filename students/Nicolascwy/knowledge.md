### Angular

#### File Structure

Unlike React which bundles structure and functionality into a single file, Angular divides this task for a few files such as:
- Component file: Defines the functionality of the component using Typescript (x.component.ts)
- Template file: Define structure and what the component looks like in HTML (x.component.html)
- Specification file: Used to perform unit/ integration on the component (x.spec.ts)

#### Directives

After defining the component it is still possible to use provide it with extra behaviour using directives. Directives are classes that can be added to the component when it is used on a page.
The directives I have encountered are
1. (NgIf)[https://angular.io/api/common/NgIf]: Allows conditional rendering for a component based on a boolean condition
2. (NgbDropdown)[https://ng-bootstrap.github.io/#/components/dropdown/api]: Helps create dropdown menus - added from Angular Bootstrap

#### [Event Emitters](https://angular.io/api/core/EventEmitter)

Communication between components from the child to the parent are done through event emitters. These are helpful when a child component on a page has been clicked, and the parent has to have knowledge of which child has been selected before fetching data for the child to be displayed to the user. After defining an event emitter, the `emit()` method can be called to return a value to the parent.

This snippet uses the @Output decorator to informs the parent which row has been clicked when the function is called.
```
// Component file - reminder.component.ts
@Output()
sendRemindersToAllNonSubmittersEvent: EventEmitter<number> = new EventEmitter();

sendRemindersToAllNonSubmitters(rowIndex: number): void {
    this.sendRemindersToAllNonSubmittersEvent.emit(rowIndex);
  }

// child template file - child.component.html
<reminder (click)="sendRemindersToAllNonSubmitters(idx)">

// parent template file - parent calls sendEmail function open receiving event from child.
<table (sendRemindersToAllNonSubmittersEvent)="sendEmail($Event)">
``` 

#### Flow of inputs and outputs
There are 3 ways data can flow between components.
1. [disabled]="canViewSessionInSections": [] bracket indicate data flows from parent's property canViewSessionInSections to the child's property disabled.
2. (click)="sendRemindersToAllNonSubmitters(idx): () like the previous example, triggers the event emitter to inform the parent to send all reminders for the course on the xth row.
3. [(size)]="fontSizePx": A combination of 1 and 2 and is called 2 way binding, where data can flow between parent and child.

References: [Event Emitters inputs and outputs](https://angular.io/guide/inputs-outputs)


### Tool/Technology 2

...