### Error messages and Hint labels in Angular forms 

Forms have a `FormGroup`, where each part of the form is controled by a `FormControl` component. 

In the ts file, Validators check and ensure the form is valid for submission. If it is not, the submit button is greyed out. 

In the html file, the individual form input boxes are built, and shown with `*ngIf` statements. `<input>` also has additional parameters to specify whether the input is required and the max length of the input. The form error messages are programmed here in the html file, for example: 
```
<mat-error *ngIf="title.errors && title.errors['required'] && (title.touched || title.dirty)">
    Title required.
</mat-error>
```

Hint labels can be used to show the remaining characters in a input box with limited characters when approaching that limit. 

While a string with validators could be used to instantiate a `FormGroup`, a `FormControl` element ensured that validators are processed such that the form error messages are shown in components that are children to other Angular components. (PR [#861](https://github.com/CATcher-org/CATcher/pull/861))

Resources:
* [Show Validation Error Messages for Reactive Forms in Angular 9](https://www.kiltandcode.com/2020/08/13/show-validation-error-messages-for-reactive-forms-in-angular-9/)


### Lifecycle Hooks in Angular

After a component is instantiated, the ts file has lifecycle hooks in the form of methods that initialize or modify the component content or state. These methods are prefixed with `ng`. 

The sequence in which these lifecycle hooks are called:
* OnChanges
* OnInit
* DoCheck  - repeated
* AfterContentInit
* AfterContentChecked  - repeated
* AfterViewInit
* AfterViewChecked   - repeated
* OnDestroy

Most notably used is `ngOnInit`, which used to instantiate the component variables. In CATcher, `ngAfterViewInit` is also used to load issues after the component has been initialized.

Resources:
* [Lifecycle Hooks](https://angular.io/guide/lifecycle-hooks)
* [Angular lifecycle hooks explained](https://blog.logrocket.com/angular-lifecycle-hooks/)
* [What's the difference between ngOnInit and ngAfterViewInit of Angular2?](https://stackoverflow.com/questions/40817336/whats-the-difference-between-ngoninit-and-ngafterviewinit-of-angular2#:~:text=ngOnInit()%20is%20called%20right,its%20children's%20views%2C%20are%20created)
* [Video: Angular - Zero to Hero - Life Cycle Hooks](https://www.youtube.com/watch?v=kKtrHrciIVs&ab_channel=WebTechTalk)

### ViewChild in Angular

While html files can add custom child components using custom defined decorators such as `<my-custom-component>`, the parent component may need references to these children components to add change content or add interactability. ViewChild, ContentChild are queries to access child components from the parent component.

For example:
```
@ViewChild(ViewIssueComponent, { static: false }) viewIssue: ViewIssueComponent;
```

#### Static vs Dynamic queries

Static queries are queries on child components that do not change during runtime, as such result of the reference to the child component can be made available in `ngOnInit`. 

Dynamic queries are queries on child components that change during runtime, so reference to child component can only be made available in `ngAfterViewInit`.

Resources:
* [Angular @ViewChild](https://blog.angular-university.io/angular-viewchild/)
* [Static query migration guide](https://angular.io/guide/static-query-migration#what-does-this-flag-mean-and-why-is-it-necessary)
* [Video: Better concepts, less code in Angular 2 - Victor Savkin and Tobias Bosch](https://www.youtube.com/watch?v=4YmnbGoh49U&ab_channel=AngularConnect)
* [What's the difference between @ViewChild and @ContentChild?](https://stackoverflow.com/questions/34326745/whats-the-difference-between-viewchild-and-contentchild)


### Property Binding

Square brackets in html tags in angular indicate that the right hand side is a dynamic expression. For example:

```
<span
        (click)="$event.stopPropagation()"
        *ngIf="issue.testerDisagree"
        [ngStyle]="this.labelService.setLabelStyle(this.labelService.getColorOfLabel('Rejected'))" >

```

The dynamic expression can be evaluated in the context of the corresponding .ts file of the html file.

### Event binding

Parenthesis within html tags, for example `(click)` are used to call the component's corresponding method on click. In the example above, `$event.stopPropagation()` is a Javascript call that prevents the label `Disagree` within the issue bar from being clickable because its parent is clickable. The click event from parent is stopped from propagating to this particular child. 


Resource:
* [Angular Doc Property Binding](https://angular.io/guide/property-binding)
* [Angular Doc Event Binding](https://angular.io/guide/event-binding)
* [StackOverflow simple summary](https://stackoverflow.com/a/35944965)
* [StackOverflow Stop Propagation](https://stackoverflow.com/questions/32315647/javascript-how-does-event-stoppropagation-work)