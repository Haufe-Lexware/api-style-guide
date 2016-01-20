## API Design Review Process

### Why

We believe in APIs and in the API Economy.
We are just at the beginning of our journey to an API driven company.
There is no clear roadmap how to reach our goal. For sure it will be a way were we stumble, sometimes fall and hopefully succeed.

There will be pros and cons how to handle and apply the style guide to real projects.
There are many things to learn and to share with another. 
We have to permanently improve the style guide, review process, tools and our knowledge and experience. 

To support the mutual experience exchange we decided to establish the **API Design Review Process**.
Each new or refactored API MUST run through this process.

### Feedback

We have now done a couple of API design reviews. 
> Please give us feedback how we can improve this process!

### Review process

#### Who is responsible for scheduling the review?
A representative of the project team that creates the service MUST contact the CTO office.
In most cases the representative is the API designer/architect.

#### How to plan the review?
It is the responsibility of the team to plan their resources for the API review and 
to talk with the team leaders of CTO office to organize reviewers.
Together they fix a time box for the review.

#### Tooling
Currently we use a **private GitHub repo** for each API review.
The initial page/readme contains information about

- Purpose of the API
- Links to the API descriiption
- Start date of the review
- End date of the review
- Mandatory reviewers including state
- State of the review itself
- Conclusion

#### API description
It is necessary to provide a formal description of the API to review it.
The API designer has to provide a Swagger description (prefered) or an equivalent description.
Providing a running system or a mock to work with the API is highly appreciated.

#### Reviewing the API
It is up to the API designer and reviewer how to perform the review.
It is a good idea to schedule a meeting and step through the API and talk with the reviewers about the functionality and intention.
That's a good way to cut down review times because questions and missunderstandings can be solved immediately.
Others prefer to review the API without a meeting.

Changes requests are stored as issues in the GitHub repo.
Each reviewer has to update his personal review state in GitHub.

#### Mandatory vs optional reviewers
During the review planning at least two reviewers will be fixed.
The mandatory reviewers are explicitly named in the GitHub review repo.

Each review is open for feedback from a wide group of other people.
That includes all interested architects, team members, clients that will use the API.
CTO office informs per default all architects. The team should inform CTO Office to add other optional reviewers.

#### How issues should be raised and addressed
We use the GitHub issue mechanism for a transparent communication during the review.
Each issue must be categorized:

- Must change   
  Must be changed in the API design and implementation
- Improvement   
  Should be changed. Depends on the team
- Question   
  For Questions/Discussions and clarifications

GitHub gives us the opportunity to customize the labels per project to achieve this.

#### Clearly time boxed reviews
Like mentioned above the whole review is time boxed.
It starts at a given date and more important it ends at a predefined date.
The outcome of the review is documented in the GitHub repo readme.md.

#### When is an API reviewed and can be implemented?
After finishing the API review there will be some change requests for the API.
Most of them might be easy to fix/change and the API designer will do that immediately during the review.
Other changes may need more effort and planning how to fix them.
There might be change request that are too expensive regarding the benefit.
The next steps and the the conclusion how to proceed with the change requests is written 
down in the **conclusion** section of the GitHub repo readme.md.
The implementation can start after finishig the review. 

#### Learnings from the review and feedback to the style guide
After each review the API designer/team should provide an feedback issue in the 
API style guide project and mark it with the label **feedback**.
The feedback has to cover at least the topics

- How helpful was the API style guide for your design process?   
  Which sections especially?
- How to improve the style guide itself?   
  Which information do you miss? Which guideline do you not agree with?
- How to improve the API review process itself?   
  What can we improve regarding tooling, planning etc.

#### How to measure success for the API review


