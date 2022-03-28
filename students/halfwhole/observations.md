### Project: ToBeYou

ToBeYou is a Singaporean interactive fiction game that lets players experience the life of someone else.
The game features the stories of a cast of characters of different racial, religious or cultural backgrounds,
and in playing through these stories, players can answer mini-quizzes and pen down their personal reflections at the end of each chapter.
As a whole, the project aims to cultivate empathy and spark interest in issues surrounding diversity and multiculturalism in Singapore.

### My Contributions

Having worked on the project since July 2021, I’ve made a good number of contributions to the project.

For the main game, I optimized the front-end to scale with large amounts of data,
as the number of user responses increased significantly since the product launch.
I also improved the design of our database by introducing cloud functions and some denormalization to answer common queries faster.
Example: [#165 Add infinite scroll to reflections](https://github.com/bettersg/besomebody_v3/pull/165)

Separately, I’ve been responsible for creating two new pages/sub-projects in addition to the main game.
I created an [administrator dashboard](https://github.com/bettersg/tobeyou_admin)
for administrators to view and moderate responses by players of the game.
I also created a [facilitator dashboard](https://github.com/bettersg/tobeyou_facilitator)
where teachers can view the progress and data of their students in the main game.

### My Learning Record

As for the technical aspects, I’ve become more skilled with technologies such as React and Firebase,
and learned general techniques for dealing with NoSQL databases.

I’ve also learned more about collaborating with UI/UX designers when creating a new product,
and involving myself in making important technical or design decisions.
Personally, I’ve also learned that working on something meaningful is more rewarding than working on just ‘any’ open-source project.

### Possible practices/tools that can be adopted

1. GitHub actions to automatically preview on staging upon making a PR.
This saves us the trouble of having to check out and run the code locally if it's just a minor cosmetic change that we want to preview quickly.

2. Using code formatters, in particular Prettier with ESLint.
This us saves a lot of time worrying about code quality and fixing lint errors, and enforces a standard coding style in our team.

### Suggested areas of improvement for the external project

ToBeYou is a less ‘polished’ or ‘professional’ project compared to most well-regarded open source projects of a technical nature.
We’re mostly a bunch of student developers with little experience, and our project is much more product-focused.
We could definitely improve our code review practices, which are optional at the moment (sometimes we even push directly to the main branch).
We could also improve on our public-facing works, such as READMEs and documentation,
and make greater use of GitHub issues and PRs as a means of communication and project organisation.


