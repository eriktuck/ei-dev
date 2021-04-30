# User Testing Guide

User testing is essential to designing and building a solution that solves a real problem for a real person. This testing guide draws primarily from Alexander Cowan's [Customer Discovery Handbook](https://www.alexandercowan.com/customer-discovery-handbook).

## Four types of user testing

We define four types of user testing

1. **Persona** hypothesis testing
2. **Problem** hypothesis testing
3. **Value** hypothesis testing
4. **Usability** hypothesis testing

#### What is hypothesis testing?

We craft hypotheses to test so that we can better define useful tests.  A hypothesis is just your educated guess about the world. Specifically, a guess that is testable. A hypothesis can be crafted as an "if...then" statement, but we'll allow any statement that can be proven false.

### Persona hypothesis testing

Does our user exist and do we understand them? Make an educated guess about how they'll behave related to your problem/solution and test if it is true. If yes, you have evidence you understand them. If no, figure out what you're missing. You should also do discovery that is not necessarily "hypotheses testing" (e.g., talk to people, observe people in the wild). 

### Problem hypothesis testing

Does our user have the problem we think they do? Is it a priority problem? How do they currently solve it? You can't just ask your subject if they have the problem you think they do, because people are generally agreeable and will likely say 'yes'. Be more creative about confirming that they do have this problem *and* that it is a sufficient pain point to warrant a change in behavior (i.e., adoption of your solution). Make sure these problem hypotheses are falsifiable.

### Value hypothesis testing

Can we create sufficient value to drive adoption of our solution? Value hypotheses should be stated as 'if...then' statements, generally "If we do something for a specific user segment they will behave in a certain way." At this point, you won't have built your solution, so you'll need a creative way to test your value hypotheses. You can use technology (e.g., build a quick Google Site as example) or magic (e.g., physically do the thing you plan to automate) to see if you're on the right track.

This is also a good point to enumerate all of the assumptions in your Product Hypothesis (combination of persona, problem and value hypotheses). Unpack these assumptions, define which ones still need testing, and test them. 

### Usability testing

Can our user actually use elements of our solution to solve specific objectives? Usability testing will be tied to user stories (i.e., specific descriptions of problems to be solved or jobs to be done that are the focus of your solution). Usability testing often involves observing a user attempt to accomplish an objective using your solution. Bonus points if you can validate that they would use your solution instead of the next best alternative.

We divide usability testing into three phases: exploratory, assessment, and validation.

#### Exploratory

Use low-fidelity prototypes to A/B test alternative solution implementations. Which one delivers a better user experience? Use the results to decide on a design direction.

#### Assessment

Determine if users can meet topline objectives with your proposed solution. Use the results to continue refining your design or pivot to a new solution.

#### Validation

Generally validation occurs after release of your solution. Define and track metrics that help you continue to refine the solution or detect issues.

## Usability Testing Process

Before usability assessment testing, you should have good evidence to support your persona, problem and value hypotheses. Now you want to know if the solution you are building will deliver value against a real problem for a real user. Before testing, answer these four questions

1. What are we testing? (Usability hypothesis)
2. How will we test it? (The test)
3. How will we know we passed? (Grading; often includes metrics and acceptance criteria)
4. What will we do with the test results, good or bad? (Adaptation)

#### Testing Outline

Usability testing (especially exploratory and assessment testing) will often take place with one or more users. Use the outline below to develop a testing script that all testers can use to maintain consistency. You want to make sure you have the right user, take a moment to confirm (or better understand) their characteristics and problems, conduct the test, gather qualitative feedback, and finally capture takeaways.

1. Validation question: are you who we think you are?

2. Discovery
    - Do you have these characteristics (confirm persona hypotheses)?
    - Do you have this problem (confirm problem hypotheses)?
    - How do you solve it now (what is the next best alternative)?

3. Testing

    - Overview and instructions
    - Test
        - Example: Give the subject a goal (related to a problem hypothesis) and watch them attempt to solve it
    - Measure: track the metrics that were identified in your grading plan

4. User feedback: an opportunity for the user to share their qualitative experience (optional but helpful)

5. Capture takeaways
    - Persona and problem hypotheses
    - UI and user stores
    - Other notes

