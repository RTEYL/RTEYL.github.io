---
layout: post
title:      "Maintaining an Open Source Project on GitHub"
date:       2021-03-13 22:10:38 +0000
permalink:  maintaining_an_open_source_project_on_github
---


Open source license:

​	The first step to creating an open source project on GitHub is choosing the right license. GitHub recommends [choosealicense.com](https://choosealicense.com/) to help you make an informed decision on which license best suits your needs and in the [Gihub Licensing Guide](https://docs.github.com/en/github/creating-cloning-and-archiving-repositories/licensing-a-repository) there's a helpful [blog](https://opensource.guide/legal/#which-open-source-license-is-appropriate-for-my-project) link that goes into more discussion on open source specifically. choosing a license is extremely important for any project “otherwise your project is just code that you shared online” -Dodds. 

Code of Conduct:

​	Adding a Code of Conduct is important to set the stage for others and provide a safe and seamless way for the community to interact with you, the project, and each other. It’s also important to touch on what your project was created to do (what problem are you solving?), what your project is and is not. Define the scope of your project. A single project (especially for beginners) is not going to solve all the problems in the world. Solve one problem at a time and if need be, allow other projects and middlewares utilize it to increase functionality that may otherwise be out of scope.

Automation:

​	Automation is a key factor in efficiency  and ease of use. It can give comfort to beginners and be less stressful for maintainers. Use [GitHub Actions](https://github.com/features/actions) to automate alerts, Continuous Integration, and Continuous Deployment ([heres](https://youtu.be/cP0I9w2coGU) a great video from GitHub Docs). Integrate scripts for developers to run tests (and clearly document them), add actions to run checks, pipelines, deployments, releases, analytics, etc. Automation is your friend, learn and use it well to create a better workflow and experience for everyone.

community:

​	Having and creating a strong community is Important is focus on. Create a [COMMUNITY_GUIDELINES.md](https://docs.github.com/en/github/building-a-strong-community) to set the tone,  “treat others as you would want to be treated” seems like a given but the actions you take and the guidelines you set can have a positive (or negative) effect in your community. Encourage beginners and welcome them to be apart of your project by being considerate. Structure the documentation in a “beginner friendly” way and add labels to help beginners identify areas they can work on and contribute with confidence not fear.

Issues/Pull Requests:

​	Use labels accurately and specifically to help contributors ascertain the type of problem that needs solving. Provide an [Issue Template](https://docs.github.com/en/github/building-a-strong-community/configuring-issue-templates-for-your-repository) to format new issues in a way that helps the flow of information travel. This will also help beginners communicate and gather more specific/accurate details about the problem. Include a well documented `CONTRIBUTING.md` that guides developers (from Fork to PR) that is both beginner friendly and sets the course of how other developers should format, document, test, commit, and submit PR’s. It’s also very helpful to have a specific syntax for commits and PR’s that make it easy to find the issue and track the history of changes in the project. Add a [Pull Request Template](https://docs.github.com/en/github/building-a-strong-community/creating-a-pull-request-template-for-your-repository) to gather more details on changes, notifications for code review, and track the corresponding issue.

​	Be thoughtful and interact with contributors if you can. You may want to look into [Saved Replies](https://docs.github.com/en/github/writing-on-github/using-saved-replies), people are busy and large project can bring lots of issues and contributors. Thank people for contributing, It’s not just code they offer but, it‘s their valuable time and it was spent helping to better the project and fix issues.

Maintaining:

​	Keep a `MAINTAINING.md` and have it depict how the project is maintained. Try to update it on your process, what's been successful and what's failed. Help others understand how and what you’re doing to keep the project and community alive. Eventually you may want to move on or the project has finished so leaving a record behind to show other contributors what you’ve done can increase the longevity of the project.

Conclusion:

​	Be kind, friendly and welcome to all developers regardless of skill level. Help each other to create, build, and communicate. Welcome beginners, we all start somewhere and helping newer developers learn and create is very impactful. I did not cover `README.md` in this blog but, here's [some great examples](https://github.com/matiassingers/awesome-readme), [GitHub Docs](https://docs.github.com/en/github/creating-cloning-and-archiving-repositories/about-readmes). Another large aspect I did not cover is API Documentation, here's [a great article](https://www.altexsoft.com/blog/api-documentation/) to get you started.



References:

GitHub:

​	[Community Profile](https://docs.github.com/en/github/building-a-strong-community/about-community-profiles-for-public-repositories)

​	[Licensing Guide](https://docs.github.com/en/github/creating-cloning-and-archiving-repositories/licensing-a-repository)

​	[Code of Conduct](https://docs.github.com/en/github/building-a-strong-community/adding-a-code-of-conduct-to-your-project)

​	[Community Profile](https://docs.github.com/en/github/building-a-strong-community/about-community-profiles-for-public-repositories)

​	[Actions](https://github.com/features/actions)

​	[Community Guidlines](https://docs.github.com/en/github/building-a-strong-community)

​	[Issue Template](https://docs.github.com/en/github/building-a-strong-community/configuring-issue-templates-for-your-repository)

​	[Pull Request Template](https://docs.github.com/en/github/building-a-strong-community/creating-a-pull-request-template-for-your-repository)

​	[Readme](https://docs.github.com/en/github/creating-cloning-and-archiving-repositories/about-readmes)

Misc:

​	[Kent C. Dodds - UtahJS - YT](https://www.youtube.com/watch?v=EGp38jqDpHA) - Highly reccomend watching

​	[Jordi Boggiano - Neos CMS - YT](https://www.youtube.com/watch?v=Ci_I0ATr748)

​	[choosealicense](https://choosealicense.com/)

​	[altexsoft - blog - API Docs](https://www.altexsoft.com/blog/api-documentation/)

​	[matiassingers/auesome-readme](https://github.com/matiassingers/awesome-readme)

​	[opensource.guide](https://opensource.guide/legal/#which-open-source-license-is-appropriate-for-my-project)
