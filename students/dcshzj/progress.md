## Summary

Since the conclusion of CS3281 in AY2020/21 Semester 2, I have assumed the role of a committer for the RepoSense project since June 2021. My primary focus was to improve the existing codebase to increase its stability and reliability. Most of the work done were on reviewing the pull requests submitted from CP3108A/B and CS3281 students, as well as guiding them to work on higher priority issues and resolving any problems that they face in contributing to the project.

Beyond RepoSense, I have also been working on expanding my knowledge in GitHub Actions. This is evident through the development of a minor project that involved smoke testing Java GUI applications on Windows, Mac and Ubuntu via GitHub Actions, and the repository is available at [dcshzj/smoke-java](https://github.com/dcshzj/smoke-java). Unfortunately, the project was only partially successful in running on Ubuntu, and further work is required to get it working on the other platforms.

In addition, I have made some minor contributions to other open source projects beyond NUS. The projects were [carbon-app/carbon](https://github.com/carbon-app/carbon) and [codemirror/CodeMirror](https://github.com/codemirror/CodeMirror). The details on the contributions are elaborated below.

## [EP] Contributions to External Project

The contributions to [carbon-app/carbon](https://github.com/carbon-app/carbon) and [codemirror/CodeMirror](https://github.com/codemirror/CodeMirror) are detailed in the following table.

| Issue | Description | Remarks |
| ----- | ----------- | ------- |
| [carbon-app/carbon#1308](https://github.com/carbon-app/carbon/issues/1308) | Investigate syntax highlighting issue for Fortran | [Pull request #1319](https://github.com/carbon-app/carbon/pull/1319) was submitted, but the repository owner had already made a [separate commit](https://github.com/carbon-app/carbon/commit/671b1ac52e16b718a9e3e8f175719b20fd822a97) |
| [codemirror/CodeMirror#6869](https://github.com/codemirror/CodeMirror/issues/6869) | Fortran mode: Incorrect highlighting for certain operators | This issue was filed as part of the investigation done for [carbon-app/carbon#1308](https://github.com/carbon-app/carbon/issues/1308). A pull request [was originally created](https://github.com/codemirror/CodeMirror/pull/6865) to resolve this issue, but the repository owner eventually made their own [separate commit](https://github.com/codemirror/CodeMirror/commit/191ae470666f877d6598dce020e8bff2823a216f) to resolve the issue. |

In addition, some contributions were made to [dcshzj/smoke-java](https://github.com/dcshzj/smoke-java) in an attempt to use GitHub Actions to perform smoke testing of Java GUI applications on Windows, Mac and Ubuntu. The project was partially successful, in which it works for Ubuntu, but not for Windows and Mac.

## Project Management

As a committer and stage 1 reviewer for the RepoSense project, my role was to ensure that the pull requests submitted are up-to-standard. I primarily reviewed backend and DevOps-related pull requests, although there were some instances where I gave my inputs to frontend-related pull requests. I have also cleaned up the issue tracker, closing issues that were no longer relevant and highlighting issues for the CS3281 students to work on.

Beyond reviewing pull requests, I have also contributed to the project in terms of maintenance, such as updating the GitHub Actions workflows and the documentation.

### Issues opened

| Date        | Issue |
| ----------- | ----- |
| 12 Jul 2021 | [Allow forcing of a fresh repository close for some test cases (#1563)](https://github.com/reposense/RepoSense/issues/1563) |
| 15 Jan 2022 | [GitHub Environments should be cleaned up after a pull request is merged (#1615)](https://github.com/reposense/RepoSense/issues/1615) |
| 19 Jan 2022 | [Upgrade highlight.js to version 11 (#1622)](https://github.com/reposense/RepoSense/issues/1622) |
| 7 Feb 2022  | [Report does not display the value of reportTitle in &lt;title&gt; (#1648)](https://github.com/reposense/RepoSense/issues/1648) |
| 26 Mar 2022 | [Update how Codecov is invoked in pull requests (#1734)](https://github.com/reposense/RepoSense/issues/1734) |

### Pull requests merged

| Date merged | Pull request |
| ----------- | ------------ |
| 7 May 2021  | [[#1435] Expand continuous integration testing to Ubuntu 18.04 and 20.04 (#1503)](https://github.com/reposense/RepoSense/pull/1503) *(leftover from CS3281)* |
| 21 May 2021 | [[#1354] Allow disowning of code by using empty author tags (#1520)](https://github.com/reposense/RepoSense/pull/1520) *(leftover from CS3281)* |
| 3 Jun 2021  | [[#1349] Clone test repositories only once when testing](https://github.com/reposense/RepoSense/pull/1478) *(leftover from CS3281)* |
| 6 Jul 2021  | [[#1493] DevOps Guide: move from Wiki to DG](https://github.com/reposense/RepoSense/pull/1548) |
| 12 Dec 2021 | [[#1606] GitHub Actions: update list of runners](https://github.com/reposense/RepoSense/pull/1608) |
| 15 Jan 2022 | [README.md: update status badges](https://github.com/reposense/RepoSense/pull/1614) |
| 28 Feb 2022 | [[#1166] Add pull request template](https://github.com/reposense/RepoSense/pull/1649) |
| 30 Mar 2022 | [[#1736] Temporary adjust cypress test codeView_switchAuthorship](https://github.com/reposense/RepoSense/pull/1737) |
| 20 Apr 2022 | [Docs: update list of project team members and commitment periods](https://github.com/reposense/RepoSense/pull/1760) |

### Pull requests reviewed

The table below has 2 dates. The first is the date of the first review made, and the second is the date when the pull request was merged. This is because for almost all the pull requests, I have made multiple reviews until the pull request was merged. All the pull requests were merged by me.

| Date of first review | Date merged | Pull request |
| -------------------- | ----------- | ------------ |
| 30 Jun 2021          | 9 Jul 2021  | [[#1212] RepoLocation: Remove redundant backslashes in regex](https://github.com/reposense/RepoSense/pull/1550) |
| 30 Jun 2021          | 9 Jul 2021  | [[#435] TestUtil: fix issue with whitespace in paths](https://github.com/reposense/RepoSense/pull/1556) |
| 5 Jul 2021           | 15 Jul 2021 | [[#954] RepoConfiguration: remove redundant usage of streams](https://github.com/reposense/RepoSense/pull/1558) |
| 9 Jul 2021           | 24 Aug 2021 | [[#1557] Migrate docs to use MarkBind V3](https://github.com/reposense/RepoSense/pull/1559) |
| 12 Jul 2021          | 29 Aug 2021 | [ConfigParser: fix Ignore Commits List header typo in CSV files](https://github.com/reposense/RepoSense/pull/1561) |
| 17 Jul 2021          | 5 Aug 2021  | [Test: remove redundant throws Exception clauses](https://github.com/reposense/RepoSense/pull/1567) |
| 17 Jul 2021          | 6 Aug 2021  | [[#937] Make all tests throw generic Exception](https://github.com/reposense/RepoSense/pull/1566) |
| 28 Jul 2021          | 6 Aug 2021  | [[#1575] Fix misidentification of deleted files as edited files](https://github.com/reposense/RepoSense/pull/1574) |
| 28 Jul 2021          | 22 Aug 2021 | [[#1546] RepoCloner: fix local repos relative pathing and spaces](https://github.com/reposense/RepoSense/pull/1562) |
| 28 Jul 2021          | 14 Sep 2021 | [[#1563, #1579] Tests: allow a fresh clone for some test cases](https://github.com/reposense/RepoSense/pull/1570) |
| 12 Aug 2021          | 22 Aug 2021 | [[#1463] Fix case insensitive bug in @@author feature](https://github.com/reposense/RepoSense/pull/1577) |
| 12 Aug 2021          | 23 Aug 2021 | [[#1547] Style: change if statements to use ternary operator](https://github.com/reposense/RepoSense/pull/1578) |
| 22 Aug 2021          | 24 Aug 2021 | [[#1571] Fix incorrect invalidation of file path with spaces](https://github.com/reposense/RepoSense/pull/1576) |
| 23 Aug 2021          | 23 Aug 2021 | [[#1536] Frontend: show short commit hash in commits view](https://github.com/reposense/RepoSense/pull/1581) |
| 23 Aug 2021          | 23 Aug 2021 | [[#1510] Frontend: implement resetting of file type checkboxes](https://github.com/reposense/RepoSense/pull/1582) |
| 23 Aug 2021          | 5 Sep 2021  | [Docs: update links in developer guide to frontend source files](https://github.com/reposense/RepoSense/pull/1583) |
| 29 Aug 2021          | 10 Sep 2021 | [Improve @@author annotation tag detection](https://github.com/reposense/RepoSense/pull/1586) |
| 5 Sep 2021           | 5 Sep 2021  | [Test: add unchecking and checking breakdown by file type test](https://github.com/reposense/RepoSense/pull/1587) |
| 5 Sep 2021           | 10 Sep 2021 | [[#569] Allow finding previous authors for ignored commits](https://github.com/reposense/RepoSense/pull/1565) |
| 15 Sep 2021          | 15 Sep 2021 | [Docs: add GitCatFile and GitShow wrapper classes to the DG](https://github.com/reposense/RepoSense/pull/1590) |
| 12 Dec 2021          | 12 Dec 2021 | [[#1595] Add release branch usage and hot patching instructions](https://github.com/reposense/RepoSense/pull/1596) |
| 13 Dec 2021          | 17 Dec 2021 | [[#1564] Lint script: add files under cypress folder](https://github.com/reposense/RepoSense/pull/1604) |
| 15 Jan 2022          | 7 Feb 2022  | [[#1603] Terminate if a CSV config file has an unknown column](https://github.com/reposense/RepoSense/pull/1609) |
| 24 Jan 2022          | 26 Jan 2022 | [[#1597] Remove duplicate error messages in error summary](https://github.com/reposense/RepoSense/pull/1605) |
| 24 Jan 2022          | 26 Jan 2022 | [[#1621] Docs: case 3 report and settings links do not match](https://github.com/reposense/RepoSense/pull/1623) |
| 24 Jan 2022          | 21 Feb 2022 | [[#1601] Convert datetime functionalities to java.time package](https://github.com/reposense/RepoSense/pull/1625) |
| 24 Jan 2022          | 13 Feb 2022 | [[#1616] Fix issue generating repos with special chars on Windows](https://github.com/reposense/RepoSense/pull/1628) |
| 27 Jan 2022          | *closed, not merged* | [[#1622] Upgrade highlight.js to version 11](https://github.com/reposense/RepoSense/pull/1629) |
| 3 Feb 2022           | 7 Feb 2022  | [Add wildcard type parameter](https://github.com/reposense/RepoSense/pull/1642) |
| 4 Feb 2022           | 28 Mar 2022 | [[#1572] Accept different types of URLs besides https github](https://github.com/reposense/RepoSense/pull/1644) |
| 13 Feb 2022          | 16 Feb 2022 | [[#1615] Clear deployments for closed pull requests](https://github.com/reposense/RepoSense/pull/1654) |
| 13 Feb 2022          | 16 Feb 2022 | [Docs: update documentation for frontend architecture](https://github.com/reposense/RepoSense/pull/1660) |
| 13 Feb 2022          | 24 Feb 2022 | [[#1619] Remove unused GitLsTree class and methods](https://github.com/reposense/RepoSense/pull/1653) |
| 13 Feb 2022          | 4 Mar 2022  | [Frontend: remove deprecated slot syntax](https://github.com/reposense/RepoSense/pull/1658) |
| 13 Feb 2022          | 26 Mar 2022 | [[#1202] Missing javadoc for functions](https://github.com/reposense/RepoSense/pull/1650) |
| 14 Feb 2022          | 24 Feb 2022 | [[#1657] Escape special chars for bash commands](https://github.com/reposense/RepoSense/pull/1662) |
| 14 Feb 2022          | 4 Mar 2022  | [[#1636] Fix ArrayOutOfBoundsException for binary files](https://github.com/reposense/RepoSense/pull/1663) |
| 16 Feb 2022          | 16 Feb 2022 | [Actions: use pull_request_target for delete deploy workflow](https://github.com/reposense/RepoSense/pull/1675) |
| 19 Feb 2022          | 21 Feb 2022 | [Remove unused variables in Java files](https://github.com/reposense/RepoSense/pull/1673) |
| 19 Feb 2022          | 21 Feb 2022 | [Actions: pull deployment info specific to the PR for deletion](https://github.com/reposense/RepoSense/pull/1677) |
| 19 Feb 2022          | 4 Mar 2022  | [Docs: fix table format in configFiles.md](https://github.com/reposense/RepoSense/pull/1666) |
| 21 Feb 2022          | 24 Feb 2022 | [[#1681] Fix log output for cloneFromBareAndUpdateBranch failures](https://github.com/reposense/RepoSense/pull/1683) |
| 28 Feb 2022          | 7 Mar 2022  | [[#1655] Remove duplicated method in GitRevList](https://github.com/reposense/RepoSense/pull/1690) |
| 5 Mar 2022           | 7 Mar 2022  | [Frontend: add deep watchers to arrays](https://github.com/reposense/RepoSense/pull/1670) |
| 5 Mar 2022           | 9 Mar 2022  | [Docs: update ternary operator in the style guide](https://github.com/reposense/RepoSense/pull/1688) |
| 5 Mar 2022           | 25 Mar 2022 | [[#1651] Enhance Checkstyle](https://github.com/reposense/RepoSense/pull/1652) |
| 23 Mar 2022          | 25 Mar 2022 | [[#1682] Fix space in local repo causing cloning exception](https://github.com/reposense/RepoSense/pull/1685) |
| 23 Mar 2022          | 26 Mar 2022 | [[#1667] Refactor AnnotatorAnalyzer class](https://github.com/reposense/RepoSense/pull/1687) |
| 23 Mar 2022          | 26 Mar 2022 | [Remove unused test resource for ConfigSystemTest](https://github.com/reposense/RepoSense/pull/1730) |
| 23 Mar 2022          | 27 Mar 2022 | [[#1704] Upgrade to JUnit 5](https://github.com/reposense/RepoSense/pull/1705) |
| 23 Mar 2022          | 27 Mar 2022 | [[#1703] Upgrade Node.js to 14](https://github.com/reposense/RepoSense/pull/1714) |
| 25 Mar 2022          | 25 Mar 2022 | [Bump minimist from 1.2.5 to 1.2.6 in /frontend/cypress](https://github.com/reposense/RepoSense/pull/1733) |
| 26 Mar 2022          | 28 Mar 2022 | [[#1728] GitVersion: Fix insufficient Git version build failure](https://github.com/reposense/RepoSense/pull/1729) |
| 26 Mar 2022          | 30 Mar 2022 | [[#1723] Run Windows CI on GitHub Actions instead of AppVeyor](https://github.com/reposense/RepoSense/pull/1731) |
| 9 Apr 2022           | 11 Apr 2022 | [[#1630] Rearrange Gradle tasks and their dependencies](https://github.com/reposense/RepoSense/pull/1631) |
| 9 Apr 2022           | 11 Apr 2022 | [[#1641] Fix tilde pathing in repo config not working](https://github.com/reposense/RepoSense/pull/1727) |
| 9 Apr 2022           | 11 Apr 2022 | [[#1734] DevOps: update uploading of coverage reports to Codecov](https://github.com/reposense/RepoSense/pull/1735) |
| 9 Apr 2022           | 14 Apr 2022 | [[#231] Modify local repository URL to the correct remote repository](https://github.com/reposense/RepoSense/pull/1679) |
| 9 Apr 2022           | 15 Apr 2022 | [Refactor Json comparison methods and resources in systemtest](https://github.com/reposense/RepoSense/pull/1745) |
| 9 Apr 2022           | 15 Apr 2022 | [Refactor GitClone::clone method to increase flexibility in usage](https://github.com/reposense/RepoSense/pull/1749) |
| 10 Apr 2022          | 12 Apr 2022 | [Release Logger resources once RepoSense finishes processing](https://github.com/reposense/RepoSense/pull/1747) |
| 14 Apr 2022          | 16 Apr 2022 | [[#1680] Delete PR deployments with >100 deployments](https://github.com/reposense/RepoSense/pull/1732) |
| 14 Apr 2022          | 16 Apr 2022 | [Checkstyle: implement new checks and update throwsIndent value](https://github.com/reposense/RepoSense/pull/1757) |
