### GitHub Actions

GitHub Actions is a new platform hosted on GitHub for Continuous Integration and Continuous Deployments (CI/CD) purposes. Previously, RepoSense was using TravisCI for running the test suite, but due to [changes in their pricing model](https://blog.travis-ci.com/2020-11-02-travis-ci-new-billing), there was an urgent need to switch to GitHub Actions that provided unlimited minutes for open source projects.

The [workflow syntax](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions) is the main starting point in designing a new CI job in GitHub Actions (known as a workflow). There are [various types of events that trigger a workflow run](https://docs.github.com/en/actions/reference/events-that-trigger-workflows), although a `pull_request` event is the most common.

However, as code from a pull request is run based on the code submitted by the PR owner, [it can result in pwn requests](https://securitylab.github.com/research/github-actions-preventing-pwn-requests) and **cause a compromise in the secrets** stored in the repository. Hence, GitHub made a design decision of not sharing any repository secrets to workflow runs that are triggered by the `pull_request` event. If a workflow needs to run and access secrets, they will have to be triggered using the `pull_request_target` event.

RepoSense was vulnerable for a period of time due to this. Hence, [work was done to mitigate the issue](https://github.com/reposense/RepoSense/pull/1411) through the use of [workflow artifacts](https://docs.github.com/en/actions/guides/storing-workflow-data-as-artifacts).

Thus, a lesson learnt is to always write code with security in mind. The original design of the workflow was aimed at working around the limitations imposed by GitHub. However, it turns out **it is still possible to compromise security** with a carefully designed PR. Therefore, understanding the security implications of writing code is vital in ensuring that we write secure code.

### JUnit

[JUnit](https://junit.org/junit4/) is a framework for writing tests for Java code. It is also a framework used by RepoSense in writing test cases quickly and easily.

When conducting research [to reduce the testing time](https://github.com/reposense/RepoSense/issues/1349), I had to learn more about how the test suite was designed, especially with the annotations used. Thankfully, there was [a tutorial on JUnit annotations](https://www.softwaretestinghelp.com/junit-annotations-tutorial/) which explained the sequence of functions that are run based on the annotations provided. A simplified sequence is as follows:

<box>
@BeforeClass → @Before → @Test (1) → @After → @Before → @Test (2) → @After → @AfterClass
</box>

However, as many of the test functions are spread across multiple test classes, there was a need to run certain code after all test classes were run. That is, the code needs to run after every class's @AfterClass. A [StackOverflow question](https://stackoverflow.com/questions/9903341/cleanup-after-all-junit-tests) suggested to use JUnit's RunListener, which is a hook that can run before any test runs and after all tests have finished.

That felt like it was sufficient, but it turns out, **it is possible that the tests were prematurely terminated**, resulting in the code for RunListener not being able to run.

Instead, a [blog post suggested](http://stiemannkj1.gitlab.io/use-Runtime-addShutdownHook-instead-of-RunListener-testRunFinished/):

> Don’t Clean Up After Tests with `RunListener.testRunFinished()`, Add a Shutdown Hook Instead

Even so, it is still possible for the Java Virtual Machine (JVM) to be terminated externally, such that it is not even possible to run code from the shutdown hook. Nonetheless, it does show that despite your best efforts, **there is always a way for the user to cause havoc**, which is really a demonstration of Murphy's Law.

### CommonJS and ES6

When doing frontend development work, the use of external libraries and packages is inevitable. However, there are different ways in which these external libraries can be included into a project, two of which are CommonJS and ES6.

[CommonJS](https://medium.com/@cgcrutch18/commonjs-what-why-and-how-64ed9f31aa46) is a type of standard or specification that declares how libraries can be imported. Previously, global or shared variables will have to be shared using the `module.exports` object, which is inherently insecure. The other way is to load dependencies using the `<script>` tag, which is rather slow and prone to errors.

ES6 is another standard which indicates how JavaScript-like programming languages should be written in order to ensure web pages render the same way on different web browsers. While this is the official standard used, it is not able to support modules that were written before this standard is produced. Hence, CommonJS and other workarounds were used to allow for the same import and export features to be made available for earlier versions of JavaScript.

[A comment on Reddit](https://www.reddit.com/r/javascript/comments/668cvh/commonjs_vs_es6_importexport_which_is_the_standard/) explains the differences between CommonJS and ES6. It also explains when to use which, although for best practices, ES6 should be used moving forward. [Another article](https://redfin.engineering/node-modules-at-war-why-commonjs-and-es-modules-cant-get-along-9617135eeca1) lists the various differences and goes deeper into other topics such as asynchronous loading of modules, etc.

Going deeper into this topic is useful for understanding the JavaScript programming language better, and to understand the differences between server-side JavaScript and frontend JavaScript.

### GitHub API

The [GitHub REST API](https://docs.github.com/en/rest) provides many endpoints for interacting with various aspects of GitHub programmatically. It also allows using GitHub in a way that the web interface does not offer.

The API was first used in RepoSense as part of the migration from TravisCI to GitHub Actions earlier in the semester. At that time, we wanted to set up pull request statuses in a way that allows for marking the preview websites as deployments. This required interacting with the [Deployments API](https://docs.github.com/en/rest/reference/repos#deployments) that is part of repositories, so that a nice "View deployment" button can appear for pull request reviewers to click on.

It is important to note that the GitHub API documentation is very incomplete. The [repository](https://github.com/github/docs) is available for anyone to make a contribution to, but there are many features that are "hidden" and takes a while to find. For instance, as part of the deployment status, a special `Accept` header needs to be specified, known as preview notices. However, I encountered a situation where I needed to specify multiple preview notices, but this information was not provided in the GitHub documentation.

After some trial-and-error, I figured the way to indicate this, which is to separate them by a comma:

```
Accept: application/vnd.github.flash-preview+json,application/vnd.github.ant-man-preview+json
```

This allows for both `application/vnd.github.flash-preview+json` and `application/vnd.github.ant-man-preview+json` to be used, which allows for `in_progress` & `queued` statuses, as well as the `inactive` state to be used.
