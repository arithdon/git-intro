# More...

## Design lessons

There are many great design lessons we can learn from Git, created by Linus Torvalds:
1. **Focus on simplicity and usability**. Git has a simple set of commands that can be combined in powerful ways. It was designed with the everyday workflow of developers in mind. This simplicity made it much easier for people to adopt than other SCM tools at the time.
2. **Build for extensibility**. Git was designed with a flexible architecture that has allowed it to be extended and adapted in many ways. This is why there are so many Git frontends, integrations, workflows, and hosting options now. Linus built it with the expectation that others would build on top of it.
3. **Use a decentralized model when possible**. Rather than having a central repository with single source of truth like other tools, Git embraced a decentralized model where anyone can copy and modify the entire repository. This has enabled new collaboration workflows that weren't possible before Git.
4. **Give users full control and visibility**. Git provides granular control over history with commands like rebase, reset, cherry-pick and more. And the .git directory gives insight into all Git's metadata and objects, for those who want to understand the internals. Git treats the user as an expert, not trying to hide complexity under the hood.
5. **Release early and iterate**. Git was first released just 3 months after initial development. It has gone through constant improvement and evolution ever since based on user feedback and experience. Linus understood that "perfect" is the enemy of "good enough". Iteration and responsiveness to users has made Git much better than if it was developed for years in isolation.
6. **Solve problems for yourself**. Linus started Git to solve his own needs for a version control tool that could handle the Linux kernel development workflow. Developers will build the best tools that they themselves need. Solve problems that you deeply understand.
7. **Accept infinite diversity in infinite combinations**. Linus did not aim to dictate how people used Git; he gave them the flexibility to adapt it to their needs. This has led to an incredible diversity of Git-based workflows, tools, and hosting options. Support adaptability and customization in your designs.

[HOME](../README.md) | [PREV](./code_review.md)
