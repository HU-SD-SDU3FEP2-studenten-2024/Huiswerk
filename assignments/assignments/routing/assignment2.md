# Assignment 2: SCRUM and Github

In these assignments we will work on the `bike-repair-shop` case. The `bike-repair-shop` case is a simple case of an actual bike shop with a repair service. They don't have any processes automated yet and they experience a couple of issues with their current way of working. They want to automate their repair service, so that they can work more efficiently and provide better service to their customers.

Because this FEP2 course is not about requirements engineering, we will not go into details about how to gather and analyze requirements. We assume that you already have this knowledge. We also assume that you are familiar with the SCRUM framework, since that to is not the focus of this course.

In this assignment we will use Github to facilitate our SCRUM process, by creating issues for scrum stories and tasks. We will also use Github projects to manage our sprint board.

## Milestones

Analyzing the `bike-repair-shop` case, we can identify the following issues in with the current way of working:

1. The repair assignment are currently written down and the handwriting is not always readable for other bike-doctors working at the shop. This becomes a problem especially when a bike-doctor needs to contact a customer about the repair and can't read the phone number or email address.

2. The bike shop currently works with different physical queues of bikes. One queue for bikes that still needs to get repaired and one for bikes that are done. Each bike has a repaircard attached to the bike, on which we find the customer contact information as well as what needs to be done. There is also space for the bike-doctor to write down what has been done. So there is no way to track the progress of the repair, other than looking at the bike itself. This issue becomes a problem if we want to solve other issues of the repair process.

We could write down more issues, but for now we will focus on these two issues. In future iterations we still can add more issues with the current way of working.

Each of these issues with the current way of working is a milestone in our project. Because if we solve that single issue, we will have a working solution for that issue. And to solve it, we might need to define multiple scrum stories.

In Github we can define milestones for our project on the issues page. We can then assign SCRUM stories to a milestone. This way we can track the progress of our project. That is why we don't assign tasks to a milestone, because tasks are part of a story and a story is part of a milestone.
We further work only on stories that belong to a single milestone. This way we can stay focused on the current milestone issue we want to solve and don't get distracted by other issues.

Create now for each of the issues describe above a milestone in your Github repository for the `bike-repair-shop` project.

## SCRUM Stories

Next we will define the SCRUM stories. We will log these stories as Github issues in the repository of the `bike-repair-shop` project. Each story will get a label that indicates if it is a `userstory`, `learningstory`, `researchstory` or `enablerstory`. Since those labels are not predefined in Github, you can create them yourself from the issue page. Since we will write multiple stories that all have the same template, you can also create a custom issue template for the SCRUM stories in Github as well (you will find this feature on at the General Features of the Settings page of your repository).

Lets start with the stories we can come up with.
We could have started with an **enabler story** to setup the boilerplate of the project, but we already did that in the previous assignments.
So we can start with the first **user story** we can think of.

In our case that is the following user story:

```text
As a bike-doctor I want to do an intake of a bike, so that we know what needs to be done to repair the bike.

Definition of ready:
- [ ] We know how to create a lit element to display a form
- [ ] We know how to trigger a print to the printer
- [ ] We know how to control the visibility of the print.
- [ ] We know how to set up a backend to store the repair assignments.
- [ ] We know how to apply the architectural layers of our application, to store the repair assignments in the local storage of the browser

Definition of done:
- [ ] The home-page needs a link to the repair intake form
- [ ] The repair intake form is displayed and asks for the following information:
  - [ ] Name of the bike owner
  - [ ] Phone number of the bike owner
  - [ ] Email address of the bike owner
  - [ ] Description of the problem
  - [ ] All fields are required
  - [ ] A button to submit the form
    - [ ] Submitting the form will assign a unique repair assignment number to the repair.
    - [ ] Submitting the form will trigger a print of the form data to the printer, so that we can attach the repair assignment to the bike.
    - [ ] The print will show the repair number, name, phone number, email address and description of the problem, but not the submit button.
    - [ ] Submitting the form will store the repair assignment in the backend, so that we can retrieve the repair assignment later.
    - [ ] Submitting the form will clear the form.
    - [ ] Submitting the form will redirect the user to the home page, to show the list of repair assignments.
```

When we add this story as issue our Github repository, we can assign it to the milestone we created earlier. We can also add a `userstory` label to the issue.
We don't have defined a project yet, so we can't assign this story to a project yet.
We also don't assign team members to a stories we will do that using `task` issues during the daily standup.

We further can discuss if this story is to big. It might be that we think that the print functionality is a separate story. We can then create a new user story for that. We might also decide that we don't know how to print and controle the printout yet. In that case we can create a `learning story` for that. All those stories can be assigned to the same milestone.

Let's define another user story for the `bike-repair-shop` project:

```text
As a bike-doctor I want to see a list of all repair assignments, so that I can see what needs to be done.

Definition of ready:
- [ ] We know how to create a lit element to display a list
- [ ] We know how to apply the architectural layers of our application, to retrieve the repair assignments from the backend.

Definition of done:
- [ ] The repair assignment list is displayed and shows the following information:
  - [ ] A clickable Repair number
    - [ ] Clicking the repair number will show the repair assignment details (name, phone number, email address, description) in a modal dialog
    - [ ] Clicking the repair number will also offer a button to remove the repair assignment
  - [ ] A button to remove the repair assignment
    - [ ] Removing the repair assignment will remove the repair assignment from the local storage of the browser.
```

This story is related to the second milestone we created earlier. We can assign this story to that milestone and add a `userstory` label to the issue.

## Tasks

We now add a tasks section to the stories. Realizing the story can be a single task or multiple tasks. A task should not take longer than 4 hours to complete. If a task takes longer, we should split the task into smaller tasks. We can add a checklist to the issue to define the tasks. We can also create a custom issue template for the tasks. Doing so will make it easier to see the procress of the story and to account for the time spend on the story.
We than will convert tasks to issues (can be done from the issue in Github itself) and assign them a label `task`.
We can than at the daily standup assign a task to a team member. This way we can track the progress of the story. We can also assign a task to a project. This way we can track the progress of the project.
But be aware that we don't assign tasks to a milestone, because tasks are part of a story and a story is part of a milestone.

## Project Board

Github offers a project board feature, where we can keep track of the progress of our project.
Create a new project from the repository page. On the dialog that appears select on the left start from scratch with a table. At the next dialog give the project a name. A good name would be `Bike-Repair-Shop`, but that might lead to a problem in this educational setting since this project will be created at the Github organisation level. This is because normally an organisation would only have a single project for the `Bike Repair Shop`. But in our educational setting each student team works on their own bike repair shop project. So in order to avoid project name conflicts please choose either `{teamname} - Bike Repair Shop` or `{studentname} - Bike Repair Shop` as project name.

We now see have an empty View1 tab. Rename the tab to `Repository`.

Next get back to the issues page in your repository and assign each story and task to the project you just created. They will appear in the `Repository` tab of the project board. What we are missing is the assigned label. We can add this label using the fields options of the `Repository` tab. Don't forget to save the changes.

Now we can create a new view via the duplicate view option of the repository tab. Rename this new view to `Backlog`. The `Backlog` view should only show SCRUM stories but not tasks and will provide us with an overview over the whole project. We can filter the issues by label.
If we add the milestone to the view, we can also sort the issues by milestone (it is therefore good to start the milestone with a prefix like `M{number}`). This way we can see the progress of the project.
Finally we want to switch the `Backlog` view from table view to board view. This way we can see the progress of the stories in the backlog.

The next view we want to create is a `Sprint` view. The `Sprint` view should only show task issues. We start by duplicating the `Repository` again and renaming it to `Sprint {number}`, where the {number} is the number of the sprint. 
We can now filter the issues by the `task` label. The problem now is that also tasks from other sprints are shown. To solve this we add a new field `sprintnr` to the view. That field will become a property of the issue and we can now filter the issues by the sprint number.
We can also add a column `status` to the view. This way we can see the progress of the sprint. Finally we want to switch the `Sprint` view from table view to board view. This way we can see the progress of the tasks in the sprint.

Note that you can apply rules to automate the update of the project board using commit and pull messages. This way you can move issues from one column to another by just adding a message to a commit or pull request. This is a very powerful feature of Github projects.

## Daily Standup

The daily standup is a meeting where the team members discuss the progress of the tasks. Each team member will tell which task(s) they have worked on since the last daily standup and what the status of the task(s) is. Since a task should not take longer than 4 hours to complete, we can expect that a team member has completed a least one task. If not, the team member should have added a comment to the task issue, explaining why the task is not completed and what the team member needs to complete the task. Splitting the task up into smaller tasks might be a solution in case the task was to big. The team member can also ask for help from other team members to complete the task. The unfinished task will be discussed shortly on how to proceed, and a comment about that discussion will be added to the task issue.

To account the process we can also keep a markdown file in the repository, where we write down a summary of the daily for each tasks and story. Since we comment on tasks themself within the issue, the summary can be very short.
An example of this logboek could look like this:

---
# Daily Standup Logbook

## Date: 2021-09-01

| Task Issue | Old Status | New Status | Comment |
|------------|------------|------------|---------|
| [#42](link-to-issue) | In Progress | Done | - |
| [#43](link-to-issue) | In Progress | In Progress | Needs help from team member X |
| [#44](link-to-issue) | In Progress | In Progress | Was to big, has been split up |
| [#45](link-to-issue) | Done | Done | Story merged from story branch to review branch |

Working on [Milestone x](link-to-milestone)

| Story Issue | Old Status | New Status | Comment |
|-------------|------------|------------|---------|
 [#1](link-to-story) | In Progress | Done | - |
 [#2](link-to-story) | In Progress | In Progress |

---

Because this logbook markdown file is in the repository, our logbook is part of the versioning of the project (if we have a good branch strategie).
And because we are linking to the issues and milestones, we can easily navigate to the issues and milestones from the logbook and see the comments and progress.

---

:house: [Home](../../README.md) | :arrow_backward: [Assignment 1](./assignment1.md) | :arrow_up: [Assignments](./README.md) | [Assignment 3](./assignment3.md) :arrow_forward:
