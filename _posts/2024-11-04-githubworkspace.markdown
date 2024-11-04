---
layout: default
title:  "Github Workspace"
date:   2024-11-03 15:07:10 -0400
categories: ChatGPT Parser
excerpt: As the number of applicants for jobs increase every year...
tag: Azure
---

## Github Workspace
[GitHub Workspace](https://githubnext.com/projects/copilot-workspace) is a cloud-based development environment that seamlessly integrates with GitHub. By bringing the full development lifecycle into a single, unified interface, GitHub Workspace eliminates the need to juggle multiple tools and applications. From writing and testing code to deploying and monitoring applications, everything can be managed directly within the GitHub ecosystem.

## How It Works
### Planning Phase
First you select and issue from your repository and open the issue with Workspace. You can either use the issue itself as a prompt for the Workspace or you can edit it for clarity. From there Github will handle the rest, while still giving you ample control to prevent any changes you don't want to the repo.

Upon running the prompt, the first thing the Workspace will do is to scan the code in your repo. It will look for what the current implementation regarding the issue is, and what it should look like once the issue is addressed. Below is an example from a basic app that will call an API to get stock information, but currently only prints 'hello world'.

![Planning Phase](/images/GHWS-Plan1.png)

Here we can see the current features, as well as what it would look like if the features in the issue were resolved. If you're happy with the proposed solution then you can move to the next step.

### Discovery Phase
After you verify that it has created a proposed solution to your issue, you can move on to asking the Workspace questions about your code and its implementation of its solution. You can ask your own questions or use any of the suggested questions to pin down exactly how this code will change and guide it in ways that work for you. 

![Question Phase](/images/GHWS-Question1.png)

Let's jump ahead to a more complex question. In this case I have a simple web page using Flask where the stock information is printed, however, I want to 'beautify' the site as it is currently just a text box. I have asked the workspace for ideas on how to improve user experience on the web page.

![Answer Phase](/images/GHWS-Answer1.png)

By selecting any of the suggestions, the AI will make sure to implement the changes in a way that aligns with your selection. In this case I selected the 'Enhance Visual Design' option. 

Once you have asked questions to make sure that the proposed changes align with your goals, the workspace will plan out each feature as a step. Each step is broken down by which file the changes will occur in. In this case it will create a new index.html file to hold the changes to the web page, and make some smaller edits to the stockquote.py file.

![Planning Phase 2](/images/GHWS-Plan2.png)

### Implementation Phase
Finally, once you have double checked the plan, you can proceed with implementation. The workspace quickly (depending on size) creates the changes for you and creates a merge request with a selected branch of the repo. 

![Implemntation Phase 1](/images/GHWS-Implementation1.png)

Following up from the question phase, the workspace has implemented the changes asked of it. From the style sheet for consistency, to the favicon symbol displayed. As Workspace is in beta, and that it has access to all files in your repo, be sure to double check any changes here. Once you are satisfied with the code, return to the issue and merge the branch!

### Final Thoughts
Github Workspace is an amazing new tool for any developers kit, however there is risk that comes with the magic. Be careful when making large changes to any file, especially when it comes to more esoteric problems. Being explicit and keeping changes small had more effective and predictable changes in my experience. 