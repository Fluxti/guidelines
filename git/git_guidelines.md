# Git guidelines

## Index
### [Git commit guidelines](#commit_guidelines)
### [Branching strategy](#branching_strategy)
### [Pull requests](#pull_requests)
### [Tools](#tools)

## <a name="commit_guidelines"></a> Git Commit Guidelines

Each commit message consists of a header, a body and a footer. The header has a special format that includes a type, a scope and a subject:

```
<type>(<scope>): <subject>
<BLANK LINE>
<body>
<BLANK LINE>
<footer>
```

The header is mandatory and the scope of the header is optional.

Any line of the commit message cannot be longer 100 characters! This allows the message to be easier to read on GitHub as well as in various git tools.


### Revert

If the commit reverts a previous commit, it should begin with revert:, followed by the header of the reverted commit. In the body it should say: This reverts commit <hash>., where the hash is the SHA of the commit being reverted.

### Type

Must be one of the following:

* feature: A new feature
* fix: A bug fix
* docs: Documentation only changes
* style: Changes that do not affect the meaning of the code (white-space, formatting, missing semi-colons, etc)
* refactor: A code change that neither fixes a bug nor adds a feature
* performance: A code change that improves performance
* test: Adding missing or correcting existing tests
* chore: Changes to the build process or auxiliary tools and libraries such as documentation generation

### Scope 

The scope should be the name of the current branch :  feature / hotfix / release  .
e.g.

```
feature(login) addition of fields
fix(session) modification of session variables
docs(git) addition of git branching strategy
style(checkout) format of the checkout html
refactor(main_controller) refactor of login method
performance(database) modification of database connections
test(main) addition of login test
chore(maven) modification of dev profile 
```

### Subject 

The subject contains succinct description of the change:

* use the imperative, present tense: "change" not "changed" nor "changes"
* don't capitalize first letter
* no dot (.) at the end

Composed by 

```action (of|to) (target|element)[of|to] [targer|element]```

Is prefered to use the following as actions :
* addition
* modification
* deletion
* fix 
* refactor
* merge
* move 

For merges done with --no-ff the naming convention is 

```merge of (merging_branch) into (base_branch) ``` 

### Body

Just as in the subject, use the imperative, present tense: "change" not "changed" nor "changes".

 The body should include the motivation for the change and contrast this with previous behavior.

In general the body elements must follow the same pattern as the subject 

```action (of|to) (target|element)[of|to] [target|element]```

```
modification of size variables
addition of login fields
fix of input validation
refactor of variable names
deletion of unused files
move of resource files
```

### Footer

The footer should contain any information about Breaking Changes and is also the place to reference  issues that this commit closes.

Breaking Changes should start with the word BREAKING CHANGE: with a space or two newlines. The rest of the commit message is then used for this.

```
fix(underwriting): fix to promo code

fix to promo code session variable by deleting multiple
promo code variables and leaving only one

Closes #392
Breaks underwriting.java

```

## <a name="branching_strategy"></a>Branching strategy

Branching strategy is based on [gitflow](https://danielkummer.github.io/git-flow-cheatsheet/)

The Gitflow Workflow still uses a central repository as the communication hub for all developers. Developers work locally and push branches to the central repo.

### Historical Branches

Instead of a single master branch, this workflow uses two branches to record the history of the project. The master branch stores the official release history, and the develop branch serves as an integration branch for features. It's also convenient to tag all commits in the master branch with a version number.

![historical_branches](../images/gitflow_1.svg)

The rest of this workflow revolves around the distinction between these two branches.

The naming convention for both branches is :
For development branch : ```develop```
For master ```master``` is the default created branch

### Feature Branches

![feature_branches](../images/gitflow_2.svg)

Each new feature should reside in its own branch, which can be pushed to the central repository for backup/collaboration. But, instead of branching off of master, feature branches use develop as their parent branch. When a feature is complete, it gets merged back into develop. Features should never interact directly with master.

Feature branches are created with the following naming convention : 
```feature/feature_name```

Example : ``` feature/git_guidelines ```

### Release Branches

![release_branches](../images/gitflow_3.svg)

Once develop has acquired enough features for a release (or a predetermined release date is approaching), you fork a release branch off of develop. Creating this branch starts the next release cycle, so no new features can be added after this point—only bug fixes, documentation generation, and other release-oriented tasks should go in this branch. Once it's ready to ship, the release gets merged into master and tagged with a version number. In addition, it should be merged back into develop, which may have progressed since the release was initiated.

Using a dedicated branch to prepare releases makes it possible for one team to polish the current release while another team continues working on features for the next release. It also creates well-defined phases of development (e.g., it's easy to say, “this week we're preparing for version 4.0” and to actually see it in the structure of the repository).

The release branch is created as ```release/release_release_number``` 

Example : ```release/release_1.2```

### Maintenance Branches

![maintenance_branches](../images/gitflow_4.svg)

Maintenance or “hotfix” branches are used to quickly patch production releases. This is the only branch that should fork directly off of master. As soon as the fix is complete, it should be merged into both master and develop (or the current release branch), and master should be tagged with an updated version number.

Having a dedicated line of development for bug fixes lets your team address issues without interrupting the rest of the workflow or waiting for the next release cycle. You can think of maintenance branches as ad hoc release branches that work directly with master.

Hotfix branches are created with the following naming convention :
```hotfix/hotfix_release_version_number```

Example : ``` hotfix/hotfix_1.2```

## Pull requests

### Pull requests guidelines

* Pull requests to merge into develop and master require at least 2 reviewers
* One of this reviewers must be the  technical lead developer of the technology
* The lead team developer must be included in the review
* Users cannot approve their own pull requests unless there is only 1 dev in the company 
* The default is merge is done with --no-ff

### How to write a good pull request

* Include the purpose of this Pull Request. For example:
```
This is a spike to explore…
This simplifies the display of…
This fixes handling of…
```
* Consider providing an overview of why the work is taking place (with any relevant links); don’t assume familiarity with the history.
* Remember that anyone in the company could be reading this Pull Request, so the content and tone may inform people other than those taking part, now or later.
* Be explicit about what feedback you want, if any: a quick pair of 👀 on the code, discussion on the technical approach, critique on design, a review of copy.
* Be explicit about when you want feedback, if the Pull Request is work in progress, say so. A prefix of “[WIP]” in the title is a simple, common pattern to indicate that state.
* @mention individuals that you specifically want to involve in the discussion, and mention why. (“/cc @jesseplusplus for clarification on this logic”)
* @mention teams that you want to involve in the discussion, and mention why. (“/cc @github/security, any concerns with this approach?”)

### Offering feedback 

* Familiarize yourself with the context of the issue, and reasons why this Pull Request exists.
* If you disagree strongly, consider giving it a few minutes before responding; think before you react.
* Ask, don’t tell. (“What do you think about trying…?” rather than “Don’t do…”)
* Explain your reasons why code should be changed. (Not in line with the style guide? A personal preference?)
* Offer ways to simplify or improve code.
* Avoid using derogatory terms, like “stupid”, when referring to the work someone has produced.
* Be humble. (“I’m not sure, let’s try…”)
* Avoid hyperbole. (“NEVER do…”)
* Aim to develop professional skills, group knowledge and product quality, through group critique.
* Be aware of negative bias with online communication. (If content is neutral, we assume the tone is negative.) Can you use positive language as opposed to neutral?
* Use emoji to clarify tone. Compare “✨ ✨ Looks good 👍 ✨ ✨” to “Looks good.”

### Responding to feedback 

* Consider leading with an expression of appreciation, especially when feedback has been mixed.
* Ask for clarification. ("I don’t understand, can you clarify?")
* Offer clarification, explain the decisions you made to reach a solution in question.
* Try to respond to every comment.
* Link to any follow up commits or Pull Requests. (“Good call! Done in 1682851”)
* If there is growing confusion or debate, ask yourself if the written word is still the best form of communication. Talk (virtually) face-to-face, then mutually consider posting a follow-up to summarize any offline discussion (useful for others who be following along, now or later).

## <a name="tools"></a> Tools

Linux / OSX : Command line
Windows : git bash 

### Merging 

Merging tools : kdiff3

### Repository management services

The current repository management is VSTS