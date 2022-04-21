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

### Backend

#### Servlets and backend routing
My experience with the backend was limited to Express (a Node.JS framework), and did not even know that Java it was possible to have a backend in Java. TEAMMATES uses Jetty, a Java framework to route requests from users. The entry point to the backend is in `web.xml` a file that is read from top to bottom and can be thought of as a sieve which directs requests in that order.

```xml
<filter>
    <filter-name>WebSecurityHeaderFilter</filter-name>
    <filter-class>teammates.ui.servlets.WebSecurityHeaderFilter</filter-class>
</filter>
<filter-mapping>
    <filter-name>WebSecurityHeaderFilter</filter-name>
    <url-pattern>/web/*</url-pattern>
    <url-pattern>/index.html</url-pattern>
    <url-pattern>/</url-pattern>
</filter-mapping>
<servlet>
    <description>Servlet that handles the single web page application</description>
    <servlet-name>WebPageServlet</servlet-name>
    <servlet-class>teammates.ui.servlets.WebPageServlet</servlet-class>
    <load-on-startup>0</load-on-startup>
</servlet>
<servlet-mapping>
    <servlet-name>WebPageServlet</servlet-name>
    <url-pattern>/web/*</url-pattern>
</servlet-mapping>
<servlet>
    <description>REST API Servlet</description>
    <servlet-name>WebApiServlet</servlet-name>
    <servlet-class>teammates.ui.servlets.WebApiServlet</servlet-class>
    <load-on-startup>0</load-on-startup>
</servlet>
<servlet-mapping>
    <servlet-name>WebApiServlet</servlet-name>
    <url-pattern>/webapi/*</url-pattern>
    <url-pattern>/auto/*</url-pattern>
    <url-pattern>/worker/*</url-pattern>
</servlet-mapping>
```
Above is a snippet from the web.xml file in TEAMMATES. The main 2 constructs used in routing are `filters` and `servlets`. Filters can be thought of as sieves, where a request that matches the pattern found in `<filter-mapping>` would execute the filter defined in `filter-class` before continuing execution in the web.xml file. Servlets follow the same pattern but start with the prefix `servlet` for example `<servlet-mapping>`. Unlike filters, servlets dead-end a request, the request gets passed on to the servlet that it matches and does not get passed down any further.

https://cloud.google.com/appengine/docs/flexible/java/configuring-the-web-xml-deployment-descriptor#Servlets_and_URL_Paths

### Gradle

#### Tasks and Gradle configuration
Tasks are snippets of code that can be executed by gradle. There are a few tasks that are pre-configured to perform a task by configuring some variables. I used the `javaexec` task which executes a java script when run. One problem faced was that Gradle configures every task on build evaluating an expression and assigning it to the variable. This led to a null pointer exceptions even if the task was not run e.g user wants to use gradle to lint the project. This was because I had directly assigned the environment variable as the java class to be run. To get around this an if-statement which checks if the environment variable is present before assigning it to the variable. I had not done this previously as I had thought variables could only be assigned once and was not optional.

https://docs.gradle.org/current/dsl/org.gradle.api.tasks.JavaExec.html
https://stackoverflow.com/questions/31065193/passing-properties-to-a-gradle-build
https://www.youtube.com/watch?v=g56O_HeefBE
https://docs.gradle.org/current/userguide/build_lifecycle.html
http://www.joantolos.com/blog/gradletask/

### Git

#### Fetching
During PR reviews, I learnt that it is sometimes better to see the changes a person has made directly in my browser. Previously, I was only familiar with `git pull` which under the hood combined the fetch and the merge into a single step, but required the branch to track a remote which was not feasible for PR reviewing since this remote would be used once or at most a few times. I then found that git fetch allowed you to specify the repository URL (the link used to clone a repo) and the name of the branch to fetch, assigning it to a reference called FETCH_HEAD, which can be checkout-ed. This allowed me to quickly fetch the branch and switch into it and make some testing, usually I would also create a temporary branch so that I can check on the changes at another time. 

https://www.atlassian.com/git/tutorials/syncing/git-fetch
https://git-scm.com/docs/git-fetch

#### Rebasing
When my team started to work on the notification banner feature, we realised that no matter how much planning is done, it was sometimes inevitable for our code to be dependent on someone elses. Instead of more conventional methods such as copying pasting code, which does not have the benefit of resolving conflicts, merging branches into my working branch would also be messy since their branches are actively updated, sometimes even force pushed. Instead, I learnt how to rebase from my teammates, which lets you put a series of commits on your branch on top another branch. This can be done several times, if a branch X is in turn dependent on branch Y, and you require Y. One downside is that you would have to fetch and update each branch and perform a rebase ontop of this new branch, but this is a small cost instead of rushing out a PR or idling, when the code you require is completed but just not merged into master.