## Defects

We've spent quite a bit of time up to this point learning how to find defects.  This makes sense, since one of the key goals of testing is to do exactly that.  However, we have not yet done is to rigorously define defects and think about what to do once they are found. 

#### What is a Defect?

A defect is any sort of error in a program which causes the system under test to do one of the following:

1. Not meet the specified requirements (functional or non-functional)
2. Return an incorrect result
3. Stop execution unexpectedly (system stability is an implicit requirement in all systems under test)

The most obvious kind of defect is a system failing to meet requirements.  If the requirements state that the program should do something, and the program does not do it, then that is a defect.  As discussed in the chapter on requirements, though, anything written in natural language is bound to have some ambiguities.  The developer who implemented the feature may have had a different understanding of what the requirement actually meant than the tester.  

An example of a system returning an incorrect result would be a spreadsheet program showing that the value of 2 + 3 is 23, or a drawing program where every time the user clicks on the color red, the drawing tool starts drawing in blue.  These may or may not be specified by the requirements (and thus may or may not overlap with the second kind of defect).  

The last kind of defect is one common to all programs - a program should not die unexpectedly.  That is, the intent of the user or the original writer or installer of the program is that the system should be running at some point in time, that program should be running at that point in time.  

Software may "shut down hard" without it necessarily being a defect.  For example, sending a SIGKILL (via "kill -9" or a similar command) to a Unix process causes the program to cease execution without running any of its shutdown routines.  However, it did not die unexpectedly - the user wanted it to do so and even sent it a message telling it to do exactly that!  The reason that the user had to send a SIGKILL to the process may be a defect, but the fact that it stopped running under these circumstances is not a defect.  If the system dies to a segmentation fault, an untrapped division by zero, or dereferencing a null pointer, these are all considered defects.  They should never happen in a program, even if the requirements do not specify that "the program shall run without any null pointer exceptions".

Finally, note that the word "bug" is often used interchangeably with the word "defect".  They mean exactly the same thing, but "bug" is much more colloquial.  In this book, the word "defect" will be used, except in those cases where author forgets to do so.  

#### Reporting a defect and the Defect Lifecycle

Upon discovering a defect, the tester (or whoever else finds the defect) should __report__ it.  Reporting may have different meanings depending on the organization and the severity of the defect.  At its root, it means that the specifics of the defect should be marked down in a location where it can be reviewed in the future, by stakeholders of the project - other testers, developers, management, etc.  In most cases, teams will have some sort of defect-tracking software, but defect reporting could be as simple as marking down the error with old-fashioned pen and paper.

When testing software, it is important to keep track of as much information as is practical, for a variety of reasons.  The first is that the more information availble, the more likely the defect will be reproducible.  If you have a system failing only when certain environment variables are set, but don't specify that in the defect report, then the developer assigned to fix the defect may just file it as "works for me", since the system did work as designed no matter what the developer did.  Without knowing about those environment variables, there's no way for the reproduction to take place.

On the other hand, it's possible to take this too far.  There is usually no need to mark down every process running on the system whenever a typo is discovered.  Exactly how much to write down for a defect will vary based on the defect and domain in which you are working.  However, there are certain pieces of data which are useful in many cases.  The following section will outline a defect template so that these items are not forgotten.  By reminding the filer of the defect to include certain information, the filer is much more likely to not miss any important steps or information, thus minimizing superfluous communication about the defect in the process of fixing it.  Checklists and templates like this are extremely powerful tools for situations where the costs of failing to do something are high; see __The Checklist Manifesto__ by Atul Gawande for a detailed explanation.

When a defect is reported, its life is just beginning.  Just as software development as a whole has a "software development lifecycle" - going from requirements, to design, etc., all the way to maintenance, support, and eventual end-of-lifing of the software, defects also have a "defect lifecycle".  The defect lifecyle is as follows:

1. Discovery
2. Reporting
3. Triaging / Assignment
4. Fixing
5. Verification

When a tester or other user first encounters and recognizes a defect, this is the "Discovery" stage.  In some cases, nothing can happen after this - the user ignores the defect or decides that it's not worth investigating further.  However, for a professional tester, this should not be the case - the job of a tester is to determine the quality of the software, and this includes finding and reporting defects.

The second stage is filing the defect, usually in some sort of standardized way.  This involves spending some time and figuring out exactly what the tester did to expose the issue, what was expected to happen, and what was observed to happen.  Rememember that the role of the tester is to discover the issue and how to reproduce it, not to figure out the root cause of the problem in the code.  This can be done - and often is, depending on the technical knowledge of the tester - but it is not the primary role.  If a tester looks too deeply into the codebase, he or she may be tempted to write what needs to be done to fix the defect as opposed to focusing on what the defect actually is.  Remember, similar to requirements, a defect should be focused on __what__ is wrong, not __how__ to fix it.

After a defect is filed, somebody must determine whether or not to spend resources to fix it, and if so, how to prioritize what may be a large number of defects.  This is called __triaging__.  The word triage comes from a medical term from prioritizing the order that patients should be treated based on how badly they've been wounded or how ill they are.  It is often done when there are large numbers of casualties at once, too many for medical personnel to take care of all of them.  For instance, after a major natural disaster, a hospital may be overrun with patients, some of whom have minor injuries, others with serious injuries, and some who are critically wounded and unable to be saved.  In such cases, the hospital may triage the incoming patients and immediately treat those with serious wounds, but deprioritize working on those with minor injuries and those whom they are unlikely to be able to heal.  

Although this particular scenario is rare in the medical world, in the software development world, it is extremely common.  There are usually far more defects known than the developers have time to fix.  Relevant stakeholders decide which defects can be fixed given the amount of resources available, and prioritize according to how quickly they can be fixed and how much pain they are causing to users of the software.  In some organizations, developers are responsible for choosing which defects to work on, determining for themselves which will be easiest to fix and resolve the most pain for the users of the software.  This kind of system works best when the developers deeply understand the needs of the user, for example when the project is itself a software development tool which they are users of.

After the defect is assigned to a developer or group of developers to work on, the developer fixes it.  Oftentimes, an automated test will be added to cover the case where the defect occurred, so as to avoid it happening again in the future.  As the developer is very close to the software being written, however, the final determination on whether or not a defect has been fixed is not up to him or her.  In the same way that a person has trouble seeing typos in a paper that they have written themselves, a developer may not see that a different error was introduced by the fix, or that some edge cases were not coered.  Mandatory code reviews can help ameliorate this somewhat, but usually a tester is used for the final verification that a defect has been fixed, as one of the key aspects of their job is to be an indepedent observer of the quality of the software under development.

Thus, after the defect has been fixed, the software should be returned to the test team to verify that it has been fixed.  The developer or developers may not have tested all the different edge cases, or they may have caused other issues with their fix.  The tester can independently verify that the fix did indeed resolve the defect without causing other defects in the process (or at least not ones worse than the one that was fixed).  In some cases, of course, other defects may arise, especially if the first one was occluding others.  For example, if there is a typo in the "welcome" page which appears after login, but a defect has prevented users from logging in, the tester should still verify a fix that allows users to log in.  The fact that there is a typo on the welcome page has been exposed by the ability to log in, but it is not related.  Even a defect which causes related defects may sometimes be verified.  Continuing the example of a login not working, if a fix comes in that allows users to log in, but never checks the password, this may be seen as an improvement and a second defect filed for the password issue.

#### A Standardized Defect Template

This is not an industry standard defect reporting template, but I have found it to be very useful.  It ensures that all of the major aspects of a defect, once found, have been reported.  By acting as a kind of checklist for what to put down for a defect, it helps to ensure that a tester will not forget anything (that big blank space after a line has a way of showing up in most people's visual fields).  Note that although there is no identifier specifically listed here, it will often be automatically added by whatever defect tracking software you are using.  If you are not using defect tracking software, and don't want to use defect  tracking software, then you can add an IDENTIFIER field.

Without further ado, here is the template.

```
SUMMARY:
DESCRIPTION:
REPRODUCTION STEPS:
EXPECTED BEHAVIOR:
OBSERVED BEHAVIOR:
IMPACT:
SEVERITY:
WORKAROUND:
NOTES:
```


##### Summary

The __summary__ is a one sentence or so summary of the defect found.  It's useful when people are scrolling through long lists of defects, or to make sure that people understand the gist of the defect.  Some examples of good summaries would be:

1. Page background is red, should be blue
2. In-app calculator displays -1 for result of sqrt(-1)
3. System exits with "SEGFAULT" error when user selected
4. Adding three or more items to a cart at the same time clears out other items

All of these describe the problem succinctly, without going into much detail.  Remember that the summary will often be glanced at along with perhaps hundreds of other bug summaries, especially if you are presenting to management.  There is a reason why most books written today have snappy titles, as opposed to titles such as __The Egg, Or The Memoirs Of Gregory Giddy, Esq: With The Lucubrations Of Messrs. Francis Flimsy, Frederick Florid, And Ben Bombast. To Which Are Added, The Private Opinions Of Patty Pout, Lucy Luscious, And Priscilla Positive. Also The Memoirs Of A Right Honourable Puppy. Conceived By A Celebrated Hen, And Laid Before The Public By A Famous Cock-Feeder.__  Yes, that is a real book, written in 1772.

##### Description

In the description, the defect is described in more detail.

##### Reproduction Steps

Remember that once you have found a defect, that somebody is probably going to try to fix it.  It may be you, or it may be somebody else, but the first step to fixing something is being able to reproduce it.  The __reproduction steps__ are the specific steps that one must take in order for the defect to manifest itself.

When someone brings a problem to a developer to be fixed, the order of operations is as follows:

1. Developer attempts to replicate defect.
2. If developer cannot replicate defect, will write on defect report that "it works on my machine" and stop working on defect.
3. Otherwise, the developer will write some code hoping to fix it.
4. Developer replicates original behavior that caused defect.
5. If defect is fixed, write on defect report that it has been fixed.  Otherwise, go to step 3.

You will notice that there is a bit of a premature exit at line 2 if the developer cannot replicate the defect.  You will also note the first appearance of the bane of many testers, the excuse that "it works on my machine!"  Development and production machines are often very different - in installed programs, libraries, perhaps even operating systems.   A defect appearing in production or in a test environment but not a developer's system is a very real and very common problem.  Just as often, though, the tester has simply not specified the reproduction steps as precisely as possible.  As a tester, you can help minimize the problem of hearing "it works on my machine!" by writing down reproduction steps in a detailed and unambiguous way.

This includes writing down any relevant steps and preconditions before the actual steps which directly caused the defect to occur.  For example, if a certain environment variable needs to be set before starting the program, or a particular garbage collection flag was passed in, or even that the system was running for six hours before the particular test steps occurred.  Determining what is and is not relevant is a difficult process.  As you gain experience testing projects in your particular domain, and understand the system under test more, you will be able to use your testing "sixth sense" to determine this.  Before you have hit that point, it usually makes sense to err on the side of "too much information" rather than too little.

It is imperative to write down specific values and steps, especially if there is more than one way to do things.  Never write that one should "Enter an invalid value"; instead, write that one should "Enter -6 once the > prompt appears."  Note that in the latter example, the tester is providing a specific instance of an invalid value, as well as saying where and when it should be entered.  Just like when writing test cases, you should imagine that the person executing these is an automaton with a general understanding of the system.  Tell that automaton exactly what to do and remove as much ambiguity as is possible.

##### Expected Behavior

The core concept behind testing, and one which we will refer to again and again throughout this book, is checking that the expected behavior of a system matches the observed behavior.  It makes little sense to test something if you don't know what should happen.  In the __expected behavior__ field, the tester should note what the system was expected to do after the execution steps are executed.

Once again, the more specific the behavior listed, the easier it will be for developers and other testers to reproduce the issue, and eventually fix it.  One should never write down that "the system should return the correct value"; instead, write that the "the system should return 6".  

Aside from saving people from calculating the correct value themselves, another benefit of writing down the expected behavior so precisely is that the tester or test case may not be expecting the correct behavior!  In this case, someone can view the defect and determine that it's actually invalid - the software is performing as intended.  This can happen when the requirements are ambiguous or the reporting tester simple makes a mistake.

##### Observed Behavior

Opposed to the expected behavior field is the __observed behavior__ field.  This describes what actually happened after the execution steps were executed.  Just as in all of the fields of the defect report template, this should be filled in as precisely as possible.

##### Impact

The tester should have some idea of what impact this will have on the user.  In the __Impact__ field, this can be specified.  

Be careful not to overgeneralize or editorialize in this section.  Let's say that a video game background color changes from blue to purple when the player jumps.  A factual way of describing the impact is that "User will see background color change, but gameplay is unaffected."  An editorialized version would be "User will hate this game and everybody who made it, because the stupid background becomes a stupid color when the stupid user does something stupid."  In general, if you find yourself using the word "stupid" or other derogatory words in your defect reports, there is probably a better way to express the issue.

##### Severity

Related to the Impact field is the __Severity__ field, which contains the defect reporter's initial impression of how severe the issue is.  This can either be explained qualitatively (e.g., "this is really, really, bad.  Really, really, really bad." or "This isn't much of an issue, to be honest") or a standardized scale can be used in order to be more quantitative.  The latter is far more popular, but will usually vary from organization to organization.  It may be something as simple as a numeric scale, with "1" being a very minor issue and "10" being an absolute showstopper.  However, oftentimes there will be gradations with descriptors.

An example rating system is explained below.  

1. __Blocker__ - This is a defect so severe that the system cannot reasonably be released without either fixing it or devising a workaround.  Examples of blocker bugs would be 

2. __Critical__ - Although the system could still be released with a bug of this magnitude, it severely impacts the core functionality of the program or makes it almost unusable.  Alternatively, a defect that normally would be marked as a blocker, but that has some sort of workaround, could be classified as critical.

3. __Major__ - This is a defect which causes a relatively severe problem, although not so severe as to entirely hobble a system.  It has a good change of being noticed by any user using the feature where the defect exists.

4. __Normal__ - This is a defect which inconveniences the user, or a more severe bug with an easy and straightforward workaround.

5. __Minor__ - The defect might or might not be be noticed, but does not cause much issue for the user, or has a simple and straightforward workaround.

6. __Trivial__ - The defect would probably not even be noticed, unless someone is looking specifically for it.  An example would be a small typo, such as a transpositioning of letters, in a large block of text.

7. __Enhancement__ - Sometimes a defect isn't really a defect; it's something that the users want, but is not specified by the requirements or is otherwise not in the scope of the current project.  In such a case, the defect can be filed as an enhancement.  Alternatively, a tester may think that a problem is a defect, but management may overrule him or her and mark it as an enhancement.

Grading the severity of bugs can be art, not a science, although there are some heuristics for determining how severe an issue is.  Depending on the domain of software in which the tester is working, as well as the organization's own rules for testing, the severity of a defect may be marked dramatically differently.  Even within organizations, there can be conflicts between engineers, managers, testers, and other stakeholders on exactly how severe a defect is.

Severity should not be mistaken for priority.  Severity is how severe the problem is, whereas priority marks what order the organization would like to see the problem fixed.  For example, a typo on the home page of a web application is of low severity, as it does not impact the functionality of the software.  However, it reflects poorly on the company that created it, and will require only a modicum of developer time to fix.  In such a case, its priority would be very high.  Alternatively, a defect could of low priority, but high severity.  An example would be a serious performance issue with database reads, where they are so slow that it makes the system completely unusable to the user after a half-hour of work.  This is a very severe problem.  However, if fixing it will be a very time-consuming process, and database optimization is already planned for later in the project, it may be of low priority.  With all else being equal, though, defects of a higher severity are usually of higher priority than ones of a lower severity.

##### Workaround

The __workaround__ field describes how the defect can be avoided, or at least ameliorated.  Assuming a defect where special characters don't work in passwords, the workaround is to only use alphanumeric characters in passwords.  It is important to note that workarounds are not always **good** workaround; they may involve not using certain functionality.  For example, if the word count feature of an editor is not working, the workaround may be to not use word count, or to use a different program (e.g., "wc -w" in Unix systems).

In some cases, there may not be any sort of workaround, or at least not a known one.  If the system is crashing at seemingly nondeterministic times, it would be impossible to list a workaround except the trivial case of "don't use the software", which is not generally accepted as a workaround.  This does not mean that a workaround does not exist, only that it's not known; there may be a setting or input value which is causing the issue, but the testing team just has not uncovered it,  If no user can log in to a web application, or the server refuses to start, or a system consistently gives wrong results to every query, then there may be no workaround, as the system is entirely unusable.  In such cases, the severity of the bug is inevitably BLOCKER or its equivalent.

##### Notes

I like to think of this field as the "miscellaneous" field.  It's where everything which might be useful for tracking down a bug, or that may or may not be releant, goes.  It is also a good place for putting data which is too long to fit in any of the other sections, which should be relatively short and easy to grasp for developers, managers, other testers, and anyone else who may have to look at this defect and try to understand it, but may not be as familiar with that section of the software as the original tester.

What exactly goes here will vary by the type of software you are testing, but it will usually be technical details of the program or its environment, or factors which may or may not be relevant.  Some examples include:

1. Stack traces
2. Copies of relevant sections of the program log
3. System or environment variables
4. Technical specifications of where it was discovered (e.g., operating system, CPU, amount of RAM, etc.)
5. Other applications running the computer at the same time
6. Particular settings, flags, or arguments passed into the program
7. Suspicious or notable behavior of other programs
8. Copied error message text

##### Exceptions to the Template

For some defects, fields may be intentionally left blank.  For example, the defect may be relatively simple and not include any extra detail, so the Notes field may be marked "N/A".  In many cases where the expected behavior is obvious, this field can also be marked "N/A"; if the Observed Behavior is "System crashes", it would be obvious to most reviewers that the Expected Behavior is "System does not crash."  As a best practice, it's better to write "N/A", "None" or some other signifier, so that others who look at the defect report are not under the impression that the tester just forgot to fill in some fields.  

In other cases, the field may be marked by "Unknown" or a similar marker that indicates that the filer of the defect does not know what the field shoud be.  For example, if the system crashed randomly, with no error message or other output, and the tester is unable to reproduce it, the Reproduction Steps field may be marked "Unknown".  If requirements are ambiguous or contradictory, the expected behavior field may be marked "Unknown".  If the tester is not sure what impact the defect would have on the end user, e.g., because the problem is deep in the infrastructure of the system, the Impact field may be marked "Unknown".

Depending on how much focus is spent on testing in an organization, some of the fields may be omitted, or some added.  For example, in some organizations, testers may not have the domain knowledge to be able to determine the impact or severity of a defect.  At a minimum, any defect report should at a minimum include reproduction steps, the expected behavior, and the observed behavior.  Anything beyond that is the icing on the bug cake.

#### Defect Examples

In this section, several examples of defects will be provided.

Note that a defect error provides a description of the defect, not what should be done to fix it.  In the first example, there are numerous ways that the defect could be resolved - the default error message for a 500 screen may be changed to include a message such as "Press the back button to retry", or the a null check could be added to the code so that the exception is never thrown, or additional error handling could occur inside the exception handler.  From the point of view of someone filing a defect, it doesn't matter; the problem is that the defect exists, and it is the job of the filer to report it accurately, not to say how it should be fixed.  This is similar to writing requirements, which are supposed to say what should be done, not how to do it.

SUMMARY: Site returns 500 error when username is blank

DESCRIPTION: When a user attempts to log in and fills in a password, but does not fill in a username, the system returns a 500 error page (see NOTES section for text of page).

This was attempted with numerous passwords.  It does not occur if no password is filled in.

REPRODUCTION STEPS:
Preconditions - User is not logged in

1. Using any web browser, navigate to root page of app
2. In the textbox labeled "Password", type "foo"
3. Ensure that the textbox labeled "Username" is blank
4. Click the "Login" button

EXPECTED BEHAVIOR:

User sees error page with "Please fill in both username and password" message

OBSERVED BEHAVIOR:

User sees 500 error. 

IMPACT:

A user who forgets to type in their username, but does type in the password, will not see the expected error page.

SEVERITY:

Normal - This is an edge case, but the user may not know to hit the back button to retry logging in.

WORKAROUND:

Ensure username field is not blank when logging in.

NOTES:

The text of the page shown is

```
500 Internal Server Error

We're sorry, something went wrong.  Please try again later.
```

The server logs report the following error:

```
Caught NullPointerException in LoginProcedure, Line 38
```

----

SUMMARY: "Invisible wall" on level 12 of Amazing Bulgarian Plumber 

DESCRIPTION: 

On level 12, three blocks to the right from the first Anaconda Plant, there are three blocks stacked on top of each other.  However, they are the same color as the background and thus invisible to the user.  Per the requirements, there should be no invisible obstacles for players of the game.

REPRODUCTION STEPS:

Start on level 12
Go to the right three screens
Jump over Anaconda Plant
Move three blocks to the right
Attempt to move one further block to the right

EXPECTED BEHAVIOR:

Player can move onto block

OBSERVED BEHAVIOR:

Player cannot move, as invisible blocks are in the way

IMPACT:

Player of game may be confused by inability to move

SEVERITY:

MAJOR - This is specifically called out by the requirements, and is quite annoying to the player of the game.

WORKAROUND:

Jump over invisible blocks, once you know that they are there

NOTES:

None

----

SUMMARY: Users cannot log in to system

DESCRIPTION:

When attempting to log in to the system with all user and administrator accounts, the same error message is given: "Login failed, perhaps you mistyped your password?"

REPRODUCTION STEPS:

Go to login page of system
Type "TestUser1" in "Username" textbox
Type the password for TestUser1 in "Password" textbox
Click the "Login" button

EXPECTED BEHAVIOR:

Welcome page is shown

OBSERVED BEHAVIOR:

Error message is displayed "Login failed, perhaps you mistyped your password?"

IMPACT:

No users will be able to use the system

SEVERITY:

BLOCKER - As it is, the system is unusable to all users.

WORKAROUND:

None

NOTES:

If you do not know the password for TestUser1, please see the QA Lead.

The logs do not show any unusual messages when attempting to log in.
