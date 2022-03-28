# Project: NUSMods

NUSMods is the official course catalogue, module search and timetable builder 
for the National University of Singapore (NUS). 

The main feature of NUSMods has to do with the timetable builder which provides
a stable and easy platform for NUS students to plan their timetable and 
examinations for that particular semester.

Currently, NUSMods is also developing another feature for undergraduate students to plan their modules over the 4 or 5 years of their student life in NUS. 

## My Contributions

During my time in NUSMods, I worked on a range of aspects with regards to the 
codebase for NUSMods. 

1. **PR**: [PR #3343 Reduce Code Duplication in utils/ical](https://github.com/nusmodifications/nusmods/pull/3343) <br/>**Tools Learnt**: Typescript <br/> **Issue Tackled**: [Issue #726 Reduce Code Duplication in utils/ical](https://github.com/nusmodifications/nusmods/issues/726) <br/><br/>For my first PR for NUSMods, I handled a `good first issue` which tackled code duplication in some of the utlity modules for the retrival of lesson time in hours and minutes. 

2. **PR**: [PR #3348 Fix Cropping Out of Dropdown menus](https://github.com/nusmodifications/nusmods/pull/3348) <br/>**Tools Learnt**: Typescript, React State Management <br/> **Issue Tackled**: [Issue #2747 Module Planner -  Dropdown for module card is cropped out](https://github.com/nusmodifications/nusmods/issues/2747) <br/><br/> While tackling issues with the Module Planner, one issue is that the dropdown menus for modules are cropped out and thus, cannot be deleted or edited. Thus, with the aid of React State Management and HTML / CSS, the bug was fixed to ensure that overflow did not happen.

3. **PR**: [PR #3374 Add Toggling of Prerequisites Checks for Planner](https://github.com/nusmodifications/nusmods/pull/3374) <br/>**Tools Learnt**: Typescript, React State Management, React Redux <br/> **Issue Tackled**: [Issue #3369 Allow removal of yellow warnings in Planner](https://github.com/nusmodifications/nusmods/issues/3369) <br/><br/>For my first feature developed in NUSMods, a simple toggle was created to allow students to turn off and on prerequisite checking for the modules that they plan to take, as NUS API can be outdated and does not include revamped modules. 

## My Learning Record

### Workflow in NUSMods

The workflow is the same as how CATcher operates, where we practice trunk-based development and do deployments via the `master` / `release` branch. 

Pull Requests get reviewed by NUSMods developers and need to pass checks via `Vercel` to ensure a stable system after the Pull Request has been merged. 

**Credits**: [Contributing Guide](https://github.com/nusmodifications/nusmods/blob/master/CONTRIBUTING.md)

### Lessons Learnt from contributing to NUSMods 

1. Feature Suggestions <br/><br/> For large-scale OSS Projects, it is important to consider about most students before proceeding with a feature. 

2. Large Codebases and Documentation <br/><br/>For large-scale codebases like NUSMods, it becomes much easier to understand the logic with documentation. This is something CATcher can learn from as well via their rich documentation in the codebase. 

### Possible Practices / Tools to be adopted by CATcher 

1. Incorporation of [Vercel](https://vercel.com/) for Website Preview<br/><br/>NUSMods allows for website preview for their website via Vercel. We can make use of the hobby plan such that we are able to preview the changes before merge. 

### Suggested areas of improvement for the external project 

1. Pull Request Reviews <br/><br/>
One possible area for improvement will be to get more developers onboard. As NUSMods becomes a staple in years to come, the team can get current NUS undergraduates onboard to review and discuss feature development.  

2. Workflow Explanations <br/><br/>
Currently, their workflow documentation is only available when talks occur via NUSHackers. In the future, it will be good to documentation their workflow via their GitHub Repository.  
