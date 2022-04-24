## Project: Mattermost

Mattermost is an open source platform for secure communication, collaboration, and orchestration of work across tools and teams. Mattermost provides three key tools: Channels, a messaging service for direct and group communications, Playbooks, a task manager to plan and run repeatable processes on project situations such as campaigns or incidents, and Boards, a collaborative project manager.

Mattermost was originally proprietary, but have moved to open-source as the first version was published in 2015. Over the years, it has grown into a big player in its field, competing with proprietary team messaging applications such as Slack.

The Mattermost codebase is hosted in GitHub as the [Mattermost organization](https://github.com/mattermost). Some of the key repositories in the organization are [`mattermost-server`](https://github.com/mattermost/mattermost-server), which handles the core backend functionalities, [`mattermost-webapp`](https://github.com/mattermost/mattermost-webapp), which is the web frontend service for the platform, and [`mattermost-plugin-playbooks`](https://github.com/mattermost/mattermost-plugin-playbooks), which is the full-stack plugin for Playbooks.

### My Contributions

I took on specifically contributing to the Playbooks plugin as there are many community-friendly issues posted for the plugin. I started out small with editing the data representation and display on user-facing features. My first contribution is updating the options text on a snooze timer for status updates such that it will be displayed as `for 60 minutes`, and so on with other durations. Although trivial at first glance, it turns out that I need to do more than just literal text substitution. The durations do not only come from literal string representation, but also procedurally generated through the playbook defaults and the user's previous choice of duration. I had to understand how the flow of data between the frontend components first to gather the function that generates the texts for the runtime durations.

I then picked up an issue to specifically explore the interaction across services when Mattermost is run. The issue asks to match the description field limit on adding and editing a Playbook Task as currently, which seems quite straightforward, but it turns out that these two actions are displayed through vastly different endpoints: the edit task UI is rendered by the plugin webapp service, while the add task UI is rendered by the core webapp service. To discover this, I had to trace from the plugin webapp, to the plugin backend, cross over to the core backend, and end up at the core frontend. Through this, I became acquainted with the way the plugin service interacts with the core service. The knowledge becomes important as the fix is to understand the core backend API parameters and modify the communication body between the plugin backend to the core backend to override the default length limit at core's side.

**Links to Pull Requests**
- [[GH-19313] Update snooze options text to be "for" duration](https://github.com/mattermost/mattermost-plugin-playbooks/pull/1094)
- [[GH-19356] MM-41063, MM-40565: Increase new task dialog description length](https://github.com/mattermost/mattermost-plugin-playbooks/pull/1117)

## My Learning Record

### Project Workflow

Community contributors would need to go through the [contribution checklist](https://developers.mattermost.com/contribute/getting-started/contribution-checklist/) that outlines what to do and expect out of creating a code contribution.

Issue management for various areas of the app is done in the `mattermost-server` repository, although some issues may span across multiple repositories. For example, many issues related the Playbook plugin are posted there, even though it actually resides on a different repository. Another instance is that there are issues that require changes in multiple repositories at the same time, such as in `mattermost-server` and `mattermost-webapp`.

For the review workflow, in my opinion it is largely the same as MarkBind, in that it is mostly a conventional merge workflow to the `master` branch. Although, one key difference is that there is a sequence of reviews in the approval process, meaning a pull request must be approved by more than one person. I encountered a total of 2-3 reviews, each assuming different positions in the team: the first review is by the Developer reviewer, then the second is by User Experience (UX) reviewer, then the final one is by the Quality Assurance (QA) reviewer.

### Lessons Learned

I had my first experience on running services with Docker as I have to run the server and the webapp at the same time in order for me to be able to deploy the Playbook plugin. As I am using Windows with Windows Subsystem for Linux (WSL), there were some setup involved before I can easily run the project, such as integrating Docker from the host OS to the WSL distros, creating links between the systems, and more. It was a good learning experience for me. Once I have set it up, running the server becomes a breeze. 

I also learned about Webpack and how it can help watch files and recompile for changes, especially in webapp files, which gives us ease of use to the development of frontend features.

### What can be adopted by MarkBind

One thing I found fascinating is that type-safety is important in Mattermost, as it is using Typescript for most of the webapp code. I saw types rigorously defined in the code files, which gives the developers a sense of security that the data that are passed around is of a certain type. I feel MarkBind can move towards being a type-safe project as well for the convenience of the developers and the system as well.
