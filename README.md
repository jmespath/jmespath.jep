# JMESPath Enhancement Proposals

The JMESPath Enhancement Proposals (JEP) process is used to modify the
JMESPath language and specification.  There are implementations of JMESPath
in over 10 languages, and this process ensures stakeholders and community
members have the opportunity to review and provide feedback before it's
officially part of the specification.

You can see the list of accepted JEPs at:

https://jmespath.github.io/jmespath.jep/


## Things that need a JEP

Any functional change that would require an update to the
[specification](http://jmespath.org/specification.html) requires a JEP.

This includes, but is not limited to:

* New syntax
* New functions
* New semantics

You can review the existing JEPs in this repo to get a sense of the type
of changes that require a JEP.

## Things that do not need a JEP

Anything that is specific to a JMESPath library does not need a JEP.  You
should defer to the specific library's contributing guide.  This can include
additional language specific APIs, extension points (e.g. adding custom
functions), configuration options, etc.

## Guidelines for proposing new features

First, make sure that the feature has not been previously proposed.  If it has,
make sure to reference prior proposals and explain why this new proposal should
be considered despite similar proposals not being accepted.

Writing a JEP can be a lot of work, so it can help to get initial guidance
before going too far.  A well thought out, high quality JEP helps its chance
of acceptance and helps ensure a productive review process.

Before writing a JEP, you can create an issue for initial high level feedback
in order to get a sense of the likelihood of a JEP being accepted.  You
can also use that issue to gauge interest in the feature.

## The JEP Process

1. Fork [this repository](https://github.com/jmespath/jmespath.jep).
2. Copy `0000-jep-template.md` to `proposals/0000-feature-name.md`,
   where `feature-name` is a high level descriptive name of the
   proposal.  You don't need to add a JEP number, one will be
   assigned during the review process.
3. Fill in all sections of the JEP template.  Be mindful of the
   "Motivation" and "Rationale" sections.  These are an important
   part of driving consensus for a JEP.
4. Submit a pull request to this repo.
5. The JEP will be reviewed and feedback will be provided.  Proposals
   often go through several rounds of feedback, this is a normal and
   expected part of the process.
6. As you incorporate feedback, do not rebase your commits.  This ensures
   the history and evolution of the proposal remains visible.
7. The discussions will eventually stabilize to one of several states:

   * The JEP has consensus for both the functionality and the
     proposed specification and is ready to be accepted.
   * The JEP has consensus for the feature but there is not consensus
     with the specification.
   * The JEP does not have consensus for the feature.
   * The JEP loses steam and the discussions go stale.  This will result
     in the PR being closed, but is subject to being reopened by anyone
     that wants to continue working on the JEP.

8. Once the JEP is approved by the JMESPath core team the pull request
   will be merged and the JEP will be assigned a number.

9. The relevant parts of the "Specification" section will be added to the
   JMESPath specification, and the tests cases from the "Test Cases" section
   of the JEP will be added to the
   [jmespath.test](https://github.com/jmespath/jmespath.test) repo.

10. JMESPath libraries can now implement the accepted JEP.

### Tenets of JMESPath

When proposing new features, keep these tenets in mind.  Adhering to
these tenets gives your proposal a higher likelihood of being accepted:

* JMESPath is not specific to a particular programming language.  Avoid
  constructs that are difficult to implement in another language.
* JMESPath strives to have one way to do something.
* Features are driven from real world use cases.
