## quality loops
a collection of loops to run in a project to keep an eye on slop and complecity

### paranoid bunch
Can you review the project code from a softwate architecture standpoint? I want you to analyze it and look for paranoid, backwards compatibility, obsesive thoroughness complexity. I want to be able to understand how data flows thru the system in no more than 4 paragraphs. Complexity makes things fragile, simplicity is antifragile.

### tests aren't free
Go through the test suites. Evaluate the tests for usefulness, relevance, whether they are serving their stated purpose and only their stated purpose, and whether they are actually testing live code paths or are just testing mocks. Look out for tests that are testing code we do not own. We do not want to test packages, frameworks, SDKs, browsers, databases, or other third-party code—only our final integration with them. Prepare a digest in CSV format with a recommendation column containing keep, delete, or fix, but do not add tests that you recommend keeping. Be super, super critical. We do not want useless code running in our test suites. We do not want tests that preserve a one-time decision or document a change that happened in the past and is no longer relevant. We want to be minimal. Our tendency should be to decrease, not increase, complexity, including testing complexity.
