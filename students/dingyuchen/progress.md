# Summary

In summary, this semester for CS3281, the main contributions were improvements to the UI and UX, as well as the refactoring of the parsing components.
I suggested making use of a parser combinator library, `arcsecond`, in order to build reusable components and make the code more readable.

I gave my review for a majority of PRs, with a heavier emphasis on the parser refactor,
since I have experience with parsers after completing the module `CS4212` on compilers.

For [IP], I made the following PRs, one to resolve an issue with dependency verions and another to update the behaviour of selecting duplicate issues.

- [#696](https://github.com/CATcher-org/CATcher/pull/696)
- [#913](https://github.com/CATcher-org/CATcher/pull/913)

For [EP], I made a PR to `nusmods`, to update the academic calendar and contact information for the Physics department.
The contribution was simple, but I originally intended to fix a [bug with the tooltip](https://github.com/nusmodifications/nusmods/issues/3205), but was unfortunately unable to find a fix.
- [Add Special Term start dates to academic-calendar.json](https://github.com/nusmodifications/nusmods/pull/2650)
- [Update Physics faculty email](https://github.com/nusmodifications/nusmods/pull/3382)


### Issues

| Week        | Achievements                                                                                                                    |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------- |
| 08 Mar 2022 | Submitted Issue: [Add localStorage logging for ParseErrors #902](https://github.com/CATcher-org/CATcher/issues/902)             |
| 26 Jan 2022 | Submitted Issue: [Speed up development builds #868](https://github.com/CATcher-org/CATcher/issues/868)                          |
| 09 Dec 2021 | Submitted Issue: [Disable creating transitive duplicate relations #851](https://github.com/CATcher-org/CATcher/issues/851)      |
| 12 Nov 2021 | Submitted Issue: [Apply formatting hook across the codebase #837](https://github.com/CATcher-org/CATcher/issues/837)            |
| 12 Nov 2021 | Submitted Issue: [Add better logging #836](https://github.com/CATcher-org/CATcher/issues/836)                                   |
| 30 Aug 2021 | Submitted Issue: [Add Dependabot to help manage outdated packages #771](https://github.com/CATcher-org/CATcher/issues/771)      |
| 13 Jul 2021 | Submitted Issue: [Reorder imports across codebase #737](https://github.com/CATcher-org/CATcher/issues/737)                      |
| 06 Jul 2021 | Submitted Issue: [Representing AuthComponent state using ADT #732](https://github.com/CATcher-org/CATcher/issues/732)           |
| 13 May 2021 | Submitted Issue: [Package versions causing type errors in CI workflows #695](https://github.com/CATcher-org/CATcher/issues/695) |

### Pull Requests

| Week        | Achievements                                                                                                           |
| ----------- | ---------------------------------------------------------------------------------------------------------------------- |
| 19 Mar 2022 | Submitted PR: [Remove lower priority check for duplicate issues #913](https://github.com/CATcher-org/CATcher/pull/913) |
| 24 Mar 2022 | Merged PR: [Remove lower priority check for duplicate issues #913](https://github.com/CATcher-org/CATcher/pull/913)    |
| 11 Feb 2022 | Submitted PR: [Viewchild fix 867 #879](https://github.com/CATcher-org/CATcher/pull/879)                                |
| 13 May 2021 | Submitted PR: [Fix graphql-tag version #696](https://github.com/CATcher-org/CATcher/pull/696)                          |
| 13 May 2021 | Merged PR: [Fix graphql-tag version #696](https://github.com/CATcher-org/CATcher/pull/696)                             |

### PR Reviews

| Week        | Achievements                                                                                                                                    |
| ----------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| 19 Mar 2022 | Reviewed PR: [Use coroutine parsers for Issue Templates #910](https://github.com/CATcher-org/CATcher/pull/910)                                  |
| 10 Mar 2022 | Reviewed PR: [Abstract code to find comments that match a template to the Template class #904](https://github.com/CATcher-org/CATcher/pull/904) |
| 08 Mar 2022 | Reviewed PR: [Use arcsecond parser to find comments that match template #900](https://github.com/CATcher-org/CATcher/pull/900)                  |
| 22 Feb 2022 | Reviewed PR: [Add launch.json to repository #888](https://github.com/CATcher-org/CATcher/pull/888)                                              |
| 25 Feb 2022 | Reviewed PR: [Tester Response: Show disagreements in Issues Responded #885](https://github.com/CATcher-org/CATcher/pull/885)                    |
| 15 Feb 2022 | Reviewed PR: [Fix #880 Clicking on open issue in new tab gives error #881](https://github.com/CATcher-org/CATcher/pull/881)                     |
| 15 Feb 2022 | Reviewed PR: [Add better logging #878](https://github.com/CATcher-org/CATcher/pull/878)                                                         |
| 18 Feb 2022 | Reviewed PR: [Show issues that are accepted during Tester Response Phase #877](https://github.com/CATcher-org/CATcher/pull/877)                 |
| 07 Feb 2022 | Reviewed PR: [Add launch.json to .gitignore #876](https://github.com/CATcher-org/CATcher/pull/876)                                              |
| 06 Feb 2022 | Reviewed PR: [Extract session-selection from auth component #874](https://github.com/CATcher-org/CATcher/pull/874)                              |
| 26 Jan 2022 | Reviewed PR: [Apply prettier on all ts/json/html files #869](https://github.com/CATcher-org/CATcher/pull/869)                                   |
| 01 Mar 2022 | Reviewed PR: [Add error handling for malformed issues / comments #866](https://github.com/CATcher-org/CATcher/pull/866)                         |
| 21 Jan 2022 | Reviewed PR: [Apply prettier across the codebase #864](https://github.com/CATcher-org/CATcher/pull/864)                                         |
| 22 Jan 2022 | Reviewed PR: [Fix error snackbar and team response assignee bugs #860](https://github.com/CATcher-org/CATcher/pull/860)                         |
| 15 Jan 2022 | Reviewed PR: [Fix response tag overlapping with assignee #859](https://github.com/CATcher-org/CATcher/pull/859)                                 |
| 12 Jan 2022 | Reviewed PR: [Merge Release V3.4.1 with Master #858](https://github.com/CATcher-org/CATcher/pull/858)                                           |
| 19 Dec 2021 | Reviewed PR: [Fix cursor position after pasting image #857](https://github.com/CATcher-org/CATcher/pull/857)                                    |
| 19 Dec 2021 | Reviewed PR: [Disable marking duplicated issue as duplicate #855](https://github.com/CATcher-org/CATcher/pull/855)                              |
| 19 Dec 2021 | Reviewed PR: [Improve viewing bug report on github ui #854](https://github.com/CATcher-org/CATcher/pull/854)                                    |
| 19 Dec 2021 | Reviewed PR: [Improve styling of no internet connection alert #853](https://github.com/CATcher-org/CATcher/pull/853)                            |
| 19 Dec 2021 | Reviewed PR: [Allow duplicate issues to inherit labels/assignees from parent issue #852](https://github.com/CATcher-org/CATcher/pull/852)       |
| 29 Dec 2021 | Reviewed PR: [Add confirmation dialog for cancelling edit #850](https://github.com/CATcher-org/CATcher/pull/850)                                |
| 05 Dec 2021 | Reviewed PR: [Team Response Phase: Fix Auto-ticking of mixed case user names #847](https://github.com/CATcher-org/CATcher/pull/847)             |
| 02 Dec 2021 | Reviewed PR: [Increase upload limit to 11 MiB and 6 Mib #845](https://github.com/CATcher-org/CATcher/pull/845)                                  |
| 14 Sep 2021 | Reviewed PR: [Fix Display for Dropdown Label Text #777](https://github.com/CATcher-org/CATcher/pull/777)                                        |
| 27 Aug 2021 | Reviewed PR: [Improve regex readability #770](https://github.com/CATcher-org/CATcher/pull/770)                                                  |
| 27 Aug 2021 | Reviewed PR: [Abstract checkbox logic #765](https://github.com/CATcher-org/CATcher/pull/765)                                                    |
| 14 Oct 2021 | Reviewed PR: [Allow HTML Tags in Markdown text #761](https://github.com/CATcher-org/CATcher/pull/761)                                           |
| 09 Oct 2021 | Reviewed PR: [AuthComponent: Extract Confirm Login Logic Into a New Component #760](https://github.com/CATcher-org/CATcher/pull/760)            |
| 13 Aug 2021 | Reviewed PR: [Update CATcher to V3.3.8 #759](https://github.com/CATcher-org/CATcher/pull/759)                                                   |
| 08 Aug 2021 | Reviewed PR: [Add Graphql Codegen to NPM Scripts #752](https://github.com/CATcher-org/CATcher/pull/752)                                         |
| 31 Jul 2021 | Reviewed PR: [Remove and Reorder Unused Imports and Variables #739](https://github.com/CATcher-org/CATcher/pull/739)                            |
| 17 Jul 2021 | Reviewed PR: [DataService: Add method return types #736](https://github.com/CATcher-org/CATcher/pull/736)                                       |
| 06 Oct 2021 | Reviewed PR: [Closed issue syncing #734](https://github.com/CATcher-org/CATcher/pull/734)                                                       |
| 13 Jul 2021 | Reviewed PR: [Fix assignees names displayed lowercase #733](https://github.com/CATcher-org/CATcher/pull/733)                                    |
| 02 Aug 2021 | Reviewed PR: [Make Download Log Button Appear Before Login #730](https://github.com/CATcher-org/CATcher/pull/730)                               |
| 06 Jul 2021 | Reviewed PR: [Increase number of labels in a single fetch #728](https://github.com/CATcher-org/CATcher/pull/728)                                |
| 01 Jul 2021 | Reviewed PR: [LoggingService: Improve tests by minimising implementation details #725](https://github.com/CATcher-org/CATcher/pull/725)         |
| 04 Jul 2021 | Reviewed PR: [AuthComponent: Perform version check after authentication #723](https://github.com/CATcher-org/CATcher/pull/723)                  |
| 22 Jun 2021 | Reviewed PR: [Bug report / Team response: Replace placeholder text with a hint #716](https://github.com/CATcher-org/CATcher/pull/716)           |
| 25 Jun 2021 | Reviewed PR: [Issue model: Remove unnecessary ternary operators #715](https://github.com/CATcher-org/CATcher/pull/715)                          |
