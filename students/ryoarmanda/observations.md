# Project: Mattermost

Mattermost is an open source platform for secure communication, collaboration, and orchestration of work across tools and teams. Mattermost provides three key tools: Channels, a messaging service for direct and group communications, Playbooks, a task manager to plan and run repeatable tasks or processes on project situations such as campaigns or incidents, and Boards, a collaborative project manager.

Mattermost was originally proprietary, but have moved to open-source as the first version was published in 2015. Over the years, it has grown into a big player in its field, competing with proprietary team messaging applications such as Slack.

## My Contributions

I started out small with editing the data representation and display on user-facing features. My first contribution is updating the options text on a snooze timer for status updates such that it will be displayed as `for 60 minutes`, and so on with other durations. Although trivial at first glance, it turns out that I need to do more than just literal text substitution. The durations do not only come from literal string representation, but also procedurally generated through the playbook defaults and the user's previous choice of duration. I had to understand how the flow of data between the frontend components first to gather the function that generates the texts for the runtime durations.

## My Learning Record

### Project Workflow

Community contributors would need to go through the [contribution checklist](https://developers.mattermost.com/contribute/getting-started/contribution-checklist/) that outlines what to do and expect out of creating a code contribution.

Issue management for many aspects of the app is on the `mattermost-server` repository, although the issue may span across multiple repositories. For example, many issues about the Playbook plugin are posted there, even though it actually resides on a different repository.

For the review workflow, in my opinion it is largely the same as MarkBind, in that it is mostly a conventional merge workflow to the `master` branch. Although, I may need to experience more with the process to learn more about it.

### Lessons Learned

I had my first experience on running services with Docker as I have to run the server and the webapp at the same time in order for me to be able to deploy the Playbook plugin. As I am using Windows with Windows Subsystem for Linux (WSL), there were some setup involved before I can easily run the project, such as integrating Docker from the host OS to the WSL distros, creating links between the systems, and more. It was a good learning experience for me. Once I have set it up, running the server becomes a breeze. 

I also learned about Webpack and how it can help watch files and recompile for changes, especially in webapp files, which gives us ease of use to the development of frontend features.

### What can be adopted by MarkBind

One thing I found fascinating is that type-safety is important in Mattermost, as it is using Typescript for most of the webapp code. I saw types rigorously defined in the code files, which gives the developers a sense of security that the data that are passed around is of a certain type. I feel MarkBind can move towards being a type-safe project as well for the convenience of the developers and the system as well.
