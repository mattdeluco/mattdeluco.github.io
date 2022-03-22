---
title: "Continuous Integration"
description: "My thoughts on the practice of continuous integration in software development."
ogimage: ""
date: 2022-03-22T10:00:00-04:00
hideComments: true
tags: [
    "Software Development",
    "Practice"
]
---

# References
* [The Linux Kernel as a Case Study on Rapid Development for Complex Software](https://thenewstack.io/the-linux-kernel-as-a-case-study-on-rapid-development-for-complex-software/)
* [Martin Fowler - Continuous Integration](https://martinfowler.com/articles/continuousIntegration.html)

# Notes
* Software development has followed the linux kernel path because of the adoption of git - we branch because we can
* Little critical analysis done of whether we ought to build software the same way
* Git was developed for linux kernel development
* Branching facilitates peer review via pull requests
* Pull requests (vs. commit to main) are necessary in kernel development because of the disparate developer base - it is a "low trust environment"
* CI could be a better choice in a high-trust environment (within an organization)
* CI + pairing work great because PR is unnecessary, making commits to main the obvious path
