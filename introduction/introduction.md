# Introduction

Haufe-Lexware's software architecture centers around decoupled (micro)services that provide functionality via RESTful APIs. Our APIs most purely express what our systems do, and are therefore highly valuable business assets. Designing high-quality, long-lasting APIs has become even more critical for us since we started developing our new open platform strategy, which transforms Haufe-Lexware from individual and closely coupled silos into business enablementplatform. Our strategy emphasizes using API's to expose and make accessible our services for our product teams and external business partners.

With this in mind, we’ve adopted “API First” as one of our key engineering principles. (Micro)Service development begins with API definition outside the code and ideally involves ample peer-review feedback to achieve high-quality APIs. API First encompasses a set of quality-related standards and fosters a peer review culture including a lightweight review procedure.
We encourage our teams to follow them to ensure that our APIs:

- are easy to understand and learn
- are general and abstracted from specific implementation and use cases
- are robust and easy to use
- have a common look and feel
- follow a consistent RESTful style and syntax
- are consistent with other teams’ APIs and our global architecture

Ideally, all of our APIs will look like the same author created them.

## Haufe-Lexware specific information

The purpose of our “RESTful API guidelines” is to define standards to successfully establish “consistent API look and feel” quality. The CTO Team drafted and owns this guidelines. Teams are responsible to fulfill these guidelines during API development and are encouraged to contribute to guideline evolution via pull requests.

Furthermore, teams should take part in [API review process](../api-design-review-process/api-design-review-process.md).

Note: These guidelines will, to some extent, remain work in progress as our work evolves, but teams can confidently follow and trust them.

---
This chapter was adopted from the [Zalando API Styleguide](https://github.com/zalando/restful-api-guidelines/blob/master/Introduction.md)